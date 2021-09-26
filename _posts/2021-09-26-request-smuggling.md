---
layout: post
title: Stealing Cookies through Request Smuggling leading to Account Takeover
subtitle: How you could mass exploit users through Request Smuggling!
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---

Ever heard of Request Smuggling? It was recently popularized by James Kettle - Director of PortSwigger. A very dangerous critical vulnerability - if exploited can lead to mass arbitrary account takeovers, PII information leakage and what not.

In this article, I'd like to demostrate how I was recently able to discover, exploit and reveal victim's cookies through Request DeSync Attack/Request Smuggling. 


## Application Recon & Discovery

Before testing for certain bug types, my methodology when auditing an application always revolve around using the application as an intended user. I'd like to overall assess what requests are being sent, if any interesting header is being leaked, any interesting parameters to play with and then go down that rabbithole! 


Performing some reconnaissance and looking for open ports, I saw this application had http port 5000 open with Gunicorn 20.0.0.


![app](https://imgur.com/EGd6oXH)








This is a demo post to show you how to write blog posts with markdown.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](https://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

**Here is some bold text**

## Here is a secondary heading

Here's a useless table:

| Number | Next number | Previous number |
| :------ |:--- | :--- |
| Five | Six | Four |
| Ten | Eleven | Nine |
| Seven | Eight | Six |
| Two | Three | One |


How about a yummy crepe?

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg)

It can also be centered!

![Crepe](https://s3-media3.fl.yelpcdn.com/bphoto/cQ1Yoa75m2yUFFbY2xwuqw/348s.jpg){: .mx-auto.d-block :}

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

## Boxes
You can add notification, warning and error boxes like this:

### Notification

{: .box-note}
**Note:** This is a notification box.

### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
