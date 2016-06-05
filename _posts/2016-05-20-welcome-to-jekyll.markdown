---
layout: post
title:  "Cordova App with REST API call"
date:   2016-06-02 17:38:45 +0300
categories: cordova
---
Cordova App

This Cordova example app will make REST API calls to [NASA's Open API](https://api.nasa.gov/).

The first API call use APOD API (Astronomy Picture Of the Day). The API returns url to image and descriptive information about it (like copyright, description etc.).

The second API call use same API with date parameter. By passing the date as a parameter you get the picture of the given date.

Example query
{% highlight ruby %}
https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY
{% endhighlight %}

The user interface is made by using JQuery Mobile. UI has two buttons in the header for API calls.

![screenshot]({{ site.baseurl }}/img/Screenshot_20160602.png)

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/NasaApp.git)


