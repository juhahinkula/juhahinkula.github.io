---
layout: post
title:  "Cordova App with RESTful API call"
date:   2016-06-02 17:38:45 +0300
categories: cordova
---
Cordova App

This Cordova example app will make REST API calls to NASA's Open API ([NASA Open APIs]: https://api.nasa.gov/).

The first API call use APOD API (Astronomy Picture Of the Day). The API returns url to image and descriptive information about it (like copyright, description etc.).

The second API call use same API with date parameter. By passing the date as a parameter you get the picture of the given date.

Example query
{% highlight ruby %}
https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY
{% endhighlight %}

Check out the [Jekyll docs][jekyll-docs] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll Talk][jekyll-talk].

[jekyll-docs]: http://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
