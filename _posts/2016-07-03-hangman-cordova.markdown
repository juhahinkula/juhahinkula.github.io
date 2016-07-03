---
layout: post
title:  "Hangman game with Phonegap"
---
[Phonegap](http://http://phonegap.com/products/) is the open source distribution of Apache Cordova. Phonegap has made desktop application that makes easier to create new project and run that in browser. With Phonegap's developer app you can quickly see how your mobile app looks on real device.

This is simple example of the hangman game was made by using following tecnologies


- Phonegap / Cordova
- JQuery Mobile

The alphabet buttons are implemented by using JQuery Mobile controlgroup. The buttons are added to controlgroup dynamically. Following code also attach click listeners to buttons.

{% highlight javascript %}
for (var i = 0; i < alphabets.length; i++) {
        $('#letters').controlgroup("container").append('<Button class="ui-btn ui-enabled" id=' + alphabets[i] + '>' + alphabets[i] + '</Button>');

    $( '#' + alphabets[i] ).bind( "click", function() {
        $(this).addClass('ui-disabled'); 
            
        checkGuess($(this).attr('id'));
    });            
}
{% endhighlight %}

The source code of the Hangman can be found from [GitHub](https://github.com/juhahinkula/HangMan.git).

Screenshot of the game.

![screenshot]({{ site.baseurl }}/img/hangman.png)




