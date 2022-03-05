---
layout: post
title:  "Customizing Jekyll and Minima"
date:   2022-03-04 19:50:00 -0600
categories: blog
tags: March 2022 Personal
author: Jeff
comments: true
published: true
---
I thought I would sneak one in as I did a little tweaking on the site today. I use a nice font on my computer, [hack](https://sourcefoundry.org/hack/). I set this as the default for vsCode and Visual Studio, and I've been using it for several years now. Last week I thought I would try my hand at customizing the [minima](https://github.com/jekyll/minima) theme, nothing major, but I thought it would be nice to have the code snippets rendered in my favorite font. I found an [article](https://simonkjohnston.life/code/2019/12/20/Minima-Typography.html) that discussed one method of making changes, and as I dug into the article, I realized there was a slightly more straightforward method.

So he starts with talking about the font he likes and that to make the theme aware of it, you would need to pull down the _includes/head.html and make changes. Minima allows you to extend much of its configuration without downloading everything. I took this approach and modified my [_includes/custom-head.html](https://github.com/jekyll/minima/blob/master/_includes/custom-head.html) to have the link to the font download location.

The following section talks about CSS Modifications, and here he talks about pulling down the _sass/minima.scss file. That file doesn't technically exist in the current (as of the version at this point in time) version. But again, we can leverage the custom files that Minima has; there is a [_sass/minima/custom-variables.scss](https://github.com/jekyll/minima/blob/master/_sass/minima/custom-variables.scss) file. I created that locally and added the one line I wanted to change:

``` css
$code-font-family: "Hack", "Inconsolata", "Consolas", "Roboto Mono", "Ubuntu Mono", "Liberation Mono", "Courier New", monospace;
```

My approach was a more straightforward method of making the changes I was after, I was concerned that if I started customizing deeper into the theme, I would wind up with a one-off, and that's not a situation I would like to be in.

Anyway, I wanted to get this down if someone else found this approach easier to follow.

Have a great evening!
