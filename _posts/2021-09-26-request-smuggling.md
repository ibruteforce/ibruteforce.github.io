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

Before testing for certain bug types, my methodology when auditing an application always revolve around using the application as an intended user. I'd like to overall assess what requests are being sent, if any interesting header is being leaked, any interesting parameters to play with and then go down the rabbithole! 


Performing some reconnaissance and looking for open ports, I noticed, that this application had http port 5000 open with Gunicorn 20.0.0.


![app](https://i.imgur.com/cIxMtmC.png)


We notice there's a sign in functionality. Trying some quick default credentials do not seem to work, but it seems like we can register see what's inside.

![app](https://i.imgur.com/4AOlqkX.png)


Quickly signing up, assessing the features, one of the main features you could do is to leave comments on a public post or create your own notes that's private to you under the notes tab.

![app](https://i.imgur.com/BovsRPc.png)

As the functionality of the app is really limited, trying a very basic injection on the comments do not seem to be rendered.

![app](https://i.imgur.com/79cTBSL.png)

Playing with the app back and forth, and trying to access notes that aren't authorized to us doesn't seem to work and throws an error. I kept testing other parameters to yield out some interesting behavior but couldn't find anything worth investigating. Decided to take a break and hit back later.


## Digging Deeper

Quick break and decided to spin up the app again. This time, I clicked all the buttons on the app as much as I can and intercepted the request through Burp. I spent time going through each and every request, but while looking at the response header something stood out.

![app](https://i.imgur.com/mAkHVmS.png)

We can see this response header called `Via: haproxy ` which seems to be a front-end load balancer. We also know that from previous reconnaissance that it's running `Gunicorn 20.0.0`


Let's investigate Gunicorn futher. Going through the changelogs of Gunicorn, we can see that Gunicorn 20.0 - which is released on October 10th 2019 which is almost 2 years ago.

![app](https://i.imgur.com/WAyyWPm.png)

Now, there must be something in here. If we look at Gunicorn 20.1 for example, 

![app](https://i.imgur.com/0EdTYRO.png)

We notice that they've fixed *chunked encoding support to prevent any request smuggling*. Now that rings a bell, which confirms that the 20.0.0 Gunicorn version is vulnerable to Request Smuggling.

Request Smuggling attacks are not the easiest attacks to pull off, doing a quick google search, there's a writeup on haproxy request smuggling by Nathan Davison. https://nathandavison.com/blog/haproxy-http-request-smuggling

I'll demonstrate how you can apply this technique over here.

## Introduction to Request Smuggling

I'll briefly explain why Request Smuggling occurs in the first place. A lot of these examples are taken from PortSwigger's Request Smuggling article: https://portswigger.net/web-security/request-smuggling - for more detailed understanding, I'd highly recommend checking this article out.

Basically, the front-end server wants to send several HTTP Request over the same backend connection. This is primarly done due to efficiency reaons. It's much faster to send them over the same connection.

In order for this to work, the frontend and backend server needs to agree on some kind of boundary between the request so they both have this understanding of where requests starts and where it stops.

In order to define this boundary between the request and the HTTP Specification, there's two ways -- Content Length Header & Transfer Encoding header which kind of does the same thing but in a different way. 

Let's look into how both of them ways.

Content length is just saying how many bytes does the body of the message have. 

![app](https://i.imgur.com/cnSiYN2.png)

Transfer encoding is pretty much the same, but the format is a bit different.




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
