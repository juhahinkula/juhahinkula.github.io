---
layout: post
title:  "Cordova plugins"
subtitle: "Sample app with Camera and Geolocation plugins"
---
Cordova plugins are add-on codes that provides JavaScript interface to native components. They allow your app to use native device capabilities. 
Lot of Cordova plugins can be found from the following [page](https://cordova.apache.org/plugins/?q=camera).

Following sample app uses two plugins: Camera and Geolocation plugins

Camera plugin defines a global navigator.camera object, which provides an API for taking pictures 
and for choosing images from the system's image library. Plugin can be added to Cordova project by running the following command

{% highlight javascript %}
cordova plugin add cordova-plugin-camera
{% endhighlight %}

Geolocation plugin provides information about the device's location, such as latitude and longitude. Plugin can be added to Cordova project by running the following command

{% highlight javascript %}
cordova plugin add cordova-plugin-geolocation
{% endhighlight %}

The sample app have button which opens device camera and shows taken picture. The current location is shown in the footer.

![screenshot]({{ site.baseurl }}/img/Screenshot_plugin.png)


{% highlight java %}
// Open camera and show taken picture
function openCamera() {
    navigator.camera.getPicture(onSuccess, onFail, { quality: 50,
        destinationType: Camera.DestinationType.FILE_URI });

    function onSuccess(imageURI) {
        console.log(imageURI);
        $("#camimage").attr("src", imageURI);
        $("#camimage").responsiveImg();
    }

    function onFail(message) {
        app.showAlert('Error taking picture', 'Error');
    }     
}

// Get current position and show it in the footer
function getPosition() {
	navigator.geolocation.getCurrentPosition(onSuccess, onError, {enableHighAccuracy: false});

    function onSuccess(position) {
        $("#loc").text("Lat:"  + position.coords.latitude + " Long: "  + position.coords.longitude);
    }

    function onError(err) {
        console.log('Error:' + err);
    }
}
{% endhighlight %}

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/CordovaPluginTest.git)

