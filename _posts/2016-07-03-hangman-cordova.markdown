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

The game logic is implemented inside checkGuess function.

In this version the wordlist are done with array. 

{% highlight javascript %}
var words = ['policeman', 'tobacco', 'university', 'neighborhood', 'explanation', 'accident', 'understand']; 
{% endhighlight %}

This is not an optimal solution. Better would be read words from some API service or from a file or database. I will test cordova file plugin to implement file based wordlist later.

The theme is done with [ThemeRoller](https://themeroller.jquerymobile.com/) which is tool for creating your own themes for JQuery Mobile. You can download your own theme as a zip file and extract css files to your project.

The source code of the Hangman can be found from [GitHub](https://github.com/juhahinkula/HangMan.git).

Screenshot of the game.

![screenshot]({{ site.baseurl }}/img/hangman.png)




