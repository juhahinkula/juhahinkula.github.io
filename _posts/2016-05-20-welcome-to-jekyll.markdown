---
layout: post
title:  "Cordova App with REST API call"
---
Cordova App

This Cordova example app will make REST API calls to [NASA's Open API](https://api.nasa.gov/).

The first API call use APOD API (Astronomy Picture Of the Day). The API returns url to image and descriptive information about it (like copyright, description etc.).

The second API call use same API with date parameter. By passing the date as a parameter you get the picture of the given date.

Example query
{% highlight ruby %}
https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY
{% endhighlight %}

**Note!** This example is using the special DEMO_KEY API key. This API key can be used for initially exploring APIs prior to signing up, but it has much lower rate limits, so if you are creating your own apps you should signup for your own API key.

The user interface is made by using JQuery Mobile. UI has two buttons in the header for API calls.

![screenshot]({{ site.baseurl }}/img/Screenshot_20160602.png)

*How to implement this simple app? This is rough description and I am not going to Cordova and JQuery basics.*

**1. Install Cordova, create a project and install platform**
Follow the [Get Started](https://cordova.apache.org/#getstarted) instructions from Cordova site

**2. Include JQuery, JQuery CSS and JQuery Mobile to your index.html file**

{% highlight html %}
<link rel="stylesheet" type="text/css" href="css/jquery.mobile-1.4.5.min.css">
<script type="text/javascript" src="js/jquery-1.12.3.js"></script>
<script type="text/javascript" src="js/jquery.mobile-1.4.5.js"></script>
{% endhighlight %}

**3. I have also used responsiveImg library which should be also included.** Check more info from their [web site](http://responsiveimg.com/)

{% highlight html %}
<script type="text/javascript" src="js/responsiveImg.js"></script>
{% endhighlight %}

**4. Add UI elements**

{% highlight html %}
<div data-role="header">
    <h1>NasaApp</h1>
    <button class="ui-btn-left ui-btn ui-btn-inline ui-mini ui-corner-all ui-btn-icon-left ui-icon-action" id="loadImage">Load 
    image</button>
    <button class="ui-btn-right ui-btn ui-btn-inline ui-mini ui-corner-all ui-btn-icon-left ui-icon-action" id="randomImage">Random image</button>
</div>
<div data-role="content" id="imgcontent" style="width:95%;">
    <img id="spaceimage" style="max-width:100%;"/>
</div>

<div data-role="content" id="textcontent">
    <p name="copyright" id="copyright"></p> 
    <p name="desc" id="desc"></p> 
</div>
{% endhighlight %}

**5. Modify index.js file, Add listener to the first button which will load the image of the current day**
API call is done by using [JQuery ajax call](http://api.jquery.com/jquery.ajax/).

{% highlight javascript %}
var url = "https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY";

$("#loadImage").click(function(){
    $.ajax({
        url: url,
        success: handleResult
    });

    function handleResult(result){
        $("#spaceimage").attr("src", result.url);

        // Using http://responsiveimg.com library
        $("#spaceimage").responsiveImg();

        $("#copyright").text("Copyright: " + result.copyright) ;
        $("#desc").text(result.explanation);
    }
});
{% endhighlight %}

**6. Modify index.js file, Add listener to the second button which will load the random image.** randomDate function is modified from [miguelmota's](https://gist.github.com/miguelmota/5b67e03845d840c949c4) function from GitHub. It will return random date between given two dates. The date is formatted according to NASA APIs date format.

{% highlight javascript %}
$("#randomImage").click(function(){
    $.ajax({
        url: url + "&date=" + randomDate(new Date(2015, 0, 1), new Date()),
        success: handleResult
    });

    function handleResult(result){
        $("#spaceimage").attr("src", result.url);

        //http://responsiveimg.com/
        $("#spaceimage").responsiveImg();

        $("#copyright").text("Copyright: " + result.copyright) ;
        $("#desc").text(result.explanation);
    }
});  

// based on https://gist.github.com/miguelmota/5b67e03845d840c949c4
function randomDate(start, end) {
    var date = new Date(start.getTime() + Math.random() * (end.getTime() - start.getTime()));
    
    var dat = date.getDate();
    var month = date.getMonth() + 1;
    var yr = date.getFullYear();
    
    return yr + "-" + month + "-" + dat;
}
{% endhighlight %}

**7. Run the project in emulator (for example using command *cordova emulate android*)**

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/NasaApp.git)

TODO: Whitelisting
