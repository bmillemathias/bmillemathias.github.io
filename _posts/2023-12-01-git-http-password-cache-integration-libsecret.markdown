---
layout: post
title:  "Your git http passwords stored safely in libsecret (GNOME or KDE)"
date:   2023-12-01 16:24:04 +0000
categories: technology
---

Git supports several transport protocols and one of these is http. I would not use as a default method to access git repository but sometimes,
 you have to use this. Http authentication in git is by default frustrating, you've to provide your password (or the token) each time you want to push.

{% highlight shell %}
$ git push http://gitlab.domain.tld/repo.git
Username: <type your username>
Password: <type your password>

[work for 5 more minutes]
$ git push http://gitlab.domain.tld/repo.git
Username: <type your username>
Password: <type your password>
{% endhighlight %}
not really fun 😢 

### git http password cache

To reduce the password fatigue, git has a [helper mechanism][git-helper] able to store your password for certain duration (which lasts by default 15 min) in memory. It's a bit like [AutoAuth][auto-auth] extension in Firefox.

{% highlight shell %}
$ git config credential.helper cache
$ git push http://gitlab.domain.tld/repo.git
Username: <type your username>
Password: <type your password>

[work for 5 more minutes]
$ git push http://gitlab.domain.tld/repo.git
[your credentials are used automatically]
{% endhighlight %}

But still, once you log out, or after a while (if you did not set a large time-out), you need to enter your password again. I'm a lazy person so I want not to have to enter my password again.

### Much better: git password storage securely in your session keyring (GNOME or KDE)

git has another more helpful helper that use [libsecret][libsecret] to store safely your http passwords. Libsecret is a library interface to access secrets (being password, token, ...), and is used by bother GMOME and KDE.

Let's see how to enable it:
{% highlight shell %}
# Example for Fedora 39
$ sudo dnf install git-credential-libsecret
$ rpm -ql git-credential-libsecret
/usr/libexec/git-core/git-credential-libsecret
$ git config --global credential.helper /usr/libexec/git-core/git-credential-libsecret
$ git push http://gitlab.domain.tld/repo.git
Username: <type your username>
Password: <type your password>

[5 days later]
$ git push http://gitlab.domain.tld/repo.git
{% endhighlight %}

<figure>
    <img src="/assets/images/202301201-gnome-keyring.png"
         alt="Seahorse windows">
    <figcaption>Seahorse showing our github http credentials</figcaption>
</figure>
How does it work? When you log in your desktop session, the keyring daemon (GNOME or KDE) unlocks the secret storage, then git can access the user and the password for the domain.

[git-helper]: https://git-scm.com/docs/git-credential-cache
[auto-auth]: https://addons.mozilla.org/fr/firefox/addon/autoauth/
[libsecret]: https://gitlab.gnome.org/GNOME/libsecret
