---
layout: post
lang: en
title: Solving IPv6 DNS resolution issue with CoreDNS
date: 2023-02-24 00:00:00 +0000
tags: technology kubernetes
---

(I'm so bad finding a good post title)

A story about PGBouncer not able to resolve the name of a service in the same namespace, with a bit of c-ares and Core DNS.

## The DNS problem

> “it's alway DNS”

my colleague, Feb 2024

### Overall issue
While deploying the [Percona Operator for PostgreSQL][postgresql-operator] in Kubernetes, we discovered [PGBouncer][pgbouncer] pod was not able to connect the Primary PostgreSQL service because of a DNS resolution issue.
Our attempts done in the container of PGBouncer were working fine, using `getent hosts postgresql-primary` either with `A` or `AAAA` type.

Strange thing is inside minikube on my colleague's laptop it was working fine. So it went against the core principe of Kubernetes :)

### Going deeper into the rabbit hole

After some investigation from a colleague (with `strace` and `tcpdump`), he found out that PGBouncer provided in the container does not rely on the venerable [GNU libc][glibc] to resolve hostname but actually use lib [c-ares][c-ares], an asynchronous resolver.

During the DNS resolution
1. PGBouncer [creates a packet `AF_UNSPEC`][dns-resolution] which means it does not specify to use IPv4 or IPv6 and do a `ares_gethostbyname` on `postgresql-primary`
2. c-ares receives the request, and it processes first using IPv6, so it sends a AAAA query.
3. The `AAAA` reaches coredns (the inside-Kubernetes resolver) which at some point, is forwarded to the host
4. The resolution on the host is performed by systemd-resolved, which returns a `SERVFAIL` answer.
5. PBBouncer receives the `SERVFAIL` answer and returned the error to the client. Final stop.

### A quick look to the CoreDNS configuration

The default CoreDNS configuration looks like more or less this
```
.:53 {
    errors
    health {
        lameduck 5s
    }
    ready
    kubernetes cluster.local in-addr.arpa ip6.arpa {
        pods insecure
        fallthrough in-addr.arpa ip6.arpa
        ttl 30
    }
    prometheus :9153
    forward . /etc/resolv.conf
    cache 30
    loop
    reload
    loadbalance
    import config/*.config
}
```

The important points are
```
kubernetes cluster.local in-addr.arpa ip6.arpa {
    pods insecure
    fallthrough in-addr.arpa ip6.arpa
    ttl 30
}
```
which implement the [service discovery DNS resolution][coredns-kubernetes] inside kubernetes cluster.

and
```
forward . /etc/resolv.conf
```
which means query are resolved using the content of `/etc/resolv.conf` in the CoreDNS pod.

### Why systemd-resolved returns a SERVFAIL ?

As I understand it, `systemd-resolved`, for the sake of security, does not forward what it considers local queries over the internet resolvers, and answers `SERVFAIL` over `NXDOMAIN`.
In our case, PBBouncer stops any further DNS query.

## Resolving the resolution

The solution we needed is at some point was to make PGBouncer resolves using a `A` type instead of `AAAA`.
We skipped any solution that would have required do configuration on the hosts because we don't want to manage them.

### Attempt #1: Reply a `A` record on a `AAAA` query

CoreDNS provides a plugin [`rewrite`][coredns-rewrite], which allows as its name implies allow to rewrite DNS queries.
Skipping over our test and investigation, we eventually added this new config as a `configMap`.

```
rewrite AAAA A
```

Which means, in the event of a `AAAA` query, return a `A` answer if the `A` answer exists. Full stop.
This is kind of bulldozer solution because, whatever the namespace or the pod, CoreDNS replies `A`.
It worked fine, at least until we discovered the pod `loki-gateway` which sports a nginx process as a reverse-proxy to the various loki components, was not working anymore because it was not happy receiving a A answer on a AAAA query.

### Attempt #2: Return an empty reply om a `AAAA` query for the specific record

So back to work; we need to limit the rewrite the record `postgresql-primary` only else we could break other pods.

But as `rewrite` did not allow a regexp for `TYPE` we used [`template`][coredns-template] plugin that permit to construct an DNS answer.

We ended up with such easy snipplet

```
template IN AAAA {
    match `^postgresql-primary.*`
    rcode NOERROR
    fallthrough
}
```

for AAAA query resolving `^postgresql-primary.*` answers nothing, else continue the resolution.

So in our case, once PGBouncer receive the empty answer with `NOERROR` for `AAAA`, a new DNS query is done, this time using `A` type, and it is resolved without any issue.

Problem solved.

[postgresql-operator]: https://docs.percona.com/percona-operator-for-postgresql/2.0/index.html
[pgbouncer]: https://www.pgbouncer.org/
[glibc]: https://www.gnu.org/software/libc/
[c-ares]: https://c-ares.org/
[dns-resolution]: https://github.com/pgbouncer/pgbouncer/blob/e913a15592c5d03008fc60b15be692e310e37d21/src/dnslookup.c#L595C2-L595C4
[coredns-kubernetes]: https://coredns.io/plugins/kubernetes/
[coredns-rewrite]: https://coredns.io/plugins/rewrite/
[coredns-template]: https://coredns.io/plugins/template/
[coredns-log]: https://coredns.io/plugins/log/
