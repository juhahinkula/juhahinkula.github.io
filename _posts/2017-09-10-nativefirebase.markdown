---
layout: post
title:  "React Native & Firebase"
---
How to create simple Todo app with React Native and Firebase realtime database.

First you have to go to Firebase and create new firebase project.

Create React native app

{% highlight javascript %}
react-native init yourAppName
{% endhighlight %}

Install Firebase
{% highlight javascript %}
npm install --save firebase
{% endhighlight %}

Import firebase to your app
{% highlight javascript %}
import * as firebase from 'firebase';
{% endhighlight %}

Add configuration parameters (fill with your own ones). How to get your configuration parameter values? Navigate to firebase console and click 'Add firebase to your web app'

![screenshot]({{ site.baseurl }}/img/firebase_params.png)

{% highlight javascript %}
const firebaseConfig = {
  apiKey: "YOUR_API_KET",
  authDomain: "YOUR_DOMAIN",
  databaseURL: "YOUR_DB_URL",
  storageBucket: "YOUR_BUCKET"
};

const firebaseApp = firebase.initializeApp(firebaseConfig);
{% endhighlight %}


{% highlight javascript %}
{% endhighlight %}

App screenshot
![screenshot]({{ site.baseurl }}/img/nativefirebase.png)

Firebase database
![screenshot]({{ site.baseurl }}/img/firebase_db.png)

See the full source code from https://github.com/juhahinkula/NativeFirebase.git)

