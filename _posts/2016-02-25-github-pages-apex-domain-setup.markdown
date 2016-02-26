---
layout: post
title:  "Github Pages Apex Domain Setup"
date: 2016-02-25 14:10:08
categories: beginner blogs
tags: github jekyll domain
excerpt_separator: ...
---
Here's where you point your DNS' A record file if you want your snazzy custom domain name to point to a github pages project. 

[192.30.252.153, 192.30.252.154]
...
Wait 10 minutes for your DNS to get its shit together.

Then check it like this, as per [Github's Apex domain setup guide](https://help.github.com/articles/setting-up-an-apex-domain/)
{% highlight bash %}
$ dig nooblesoup.com +nostats +nocomments +nocmd
;nooblesoup.com.
nooblesoup.com.   73  IN  A 192.30.252.153
nooblesoup.com.   73  IN  A 192.30.252.154
{% endhighlight %}

Now, you might have a tiny side concern about base urls and stylesheet links if you tried putting your snazzy new Jekyll size right up on your github pages. Chances are you may have looked at the [Jekyll Github Pages documentation](https://jekyllrb.com/docs/github-pages/) and got wondering about exactly whether or not and where you were going to stick things like this 

```html
<link href="{{ site.github.url }}/path/to/css.css" rel="stylesheet">
```

Well, if you're using a custom domain, don't. Just don't worry about it. 

If you check out `your-app-root/_includes/header.html` you'll find this

```html
<link rel="stylesheet" href="{{ "/css/main.css" | prepend: site.baseurl }}">
```

And that's what you want: to prepend your site.baseurl, and not your github site url (which would be like `irstacks.github.io/nooblesoup`, or whatever).