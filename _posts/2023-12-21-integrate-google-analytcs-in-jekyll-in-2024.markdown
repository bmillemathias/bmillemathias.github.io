---
layout: post
lang: en
title: Google Analytics integration in Jekyll in 2024
date: 2023-12-21 20:00:04 +0000
tags: technology
---

Recently I created this site just to write from time to time some info mainly related to computing and technology, I chose to use jekyll because it was made of static files and natively integrated with girhub, so it involves no maintenance and there is not surface attack.

I was looking to get some stats if people get interested by the two posts I wrote before :) so I started searching over the interweb to install google analytics script on the pages.

I first tried the [jekyll-analytics plugin][jekyll-analytics-plugin], but it did not work except on my laptop, whatever the configuration I tweaked.

Then a quick search on ["jekyll google analytics"][ddg-search] sends me to various sites ([example][example]), where you had to create template, add includes and modify layout, and whatsoever, which were too time consuming for the task.

Actually in 2024, there is a simpler version I discovered by grepping the [source code of minima][gh-minima-analytics] the default theme provided by Jekyll.
Fist you need to use the branch `master` from git repository of the theme "minima". To do this edit the file `Gemfile` in you site repository and replace the line

`ruby
gem "minima", "~> 2.5"
`

by

`ruby
gem "minima", git: 'https://github.com/jekyll/minima'
`

Then define your Google Analytics identifier, edit the file `_config.yml` to add a variable `google_analytics: \<your-id\>`

Now you need to edit Jekyll config file `_config` and add this

```yaml
google_analytics: G-XXXXXXXXXX
```

Build your site, and you should see this piece of javascipt code in the `<head>` of each page.

```html
<script>
if(!(window.doNotTrack === "1" || navigator.doNotTrack === "1" || navigator.doNotTrack === "yes" || navigator.msDoNotTrack === "1")) {
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');

  ga('create', 'G-XXXXXXXXXX', 'auto');
  ga('send', 'pageview');
}
</script>
```

note the script respects "DoesNotTrack" option set by browsers.

[jekyll-analytics-plugin]: https://github.com/hendrikschneider/jekyll-analytics
[ddg-search]: https://duckduckgo.com/?q=jekyll+google+analytics
[example]: https://michaelsoolee.com/google-analytics-jekyll/
[gh-minima-analytics]: https://github.com/search?q=repo%3Ajekyll%2Fminima+analytics&type=code
