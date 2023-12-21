---
layout: post
lang: en
title: Google Analytics integration in Jekyll in 2024
date: 2023-12-21 20:00:04 +0000
tags: technology
---

Recently I created this site just to write from time to time some info mainly related to computing and technology, I chose to use jekyll because it was made of static files and natively integrated with girhub, si have nothing to maintain on my own.

I was looking to get some stats if people get interested by the two posts I wrote before :) so I started searching over the interweb to install google analytics. 
A quick search on "jekyll google analytics" lead me to some site, that asked me to add includes and modify layout, and whatsoever.

Actually in 2024, there is a simpler version, because the default theme `Minima` got natively support of.

Fist we need to use the git version of the theme "minima", edit the file `Gemfile` and replace the line

`ruby
gem "minima", "~> 2.5"
`

by

`ruby
gem "minima", git: 'https://github.com/jekyll/minima'
`

Edit the file `_config.yml` to add a variable `google_analytics: \<your-id\>`

Now you need to edit Jekyll config file `_config` and add this

```yaml
google_analytics: G-5GKGWBFLQJ
```

so you get the javascript snipplet added to each page

```html
<script>
if(!(window.doNotTrack === "1" || navigator.doNotTrack === "1" || navigator.doNotTrack === "yes" || navigator.msDoNotTrack === "1")) {
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'G-5GKGWBFLQJ', 'auto');
  ga('send', 'pageview');
}
</script>
```

note the script respects "DoesNotTrack" option set by browsers.
