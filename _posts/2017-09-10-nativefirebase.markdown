---
layout: post
title:  "React Native & Firebase"
---
How to create simple Todo app with React Native and Firebase realtime database.

Firebase offers you realtime NoSql database where items are saved as JSON objects. To get that you have to sign-in to Firebase and create project for your app .

First you have to go to Firebase and create new firebase project. You need certain configuration paramters for your app. Navigate to firebase console and click 'Add firebase to your web app' and parameters are shown to you.

![screenshot]({{ site.baseurl }}/img/firebase_params.png)

You also have to modify rules for your database to be able to test it without authentication (not covered in this blog). Copy following lines to your database's rules tab.

{% highlight html %}
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
{% endhighlight %}

Now everyone has rights to read and write our database so this really only for test purposes.

When the Firebase project has been created it is time to create React Native app (This example is done for Android).

Create React native app

{% highlight javascript %}
react-native init yourAppName
{% endhighlight %}

Install Firebase
{% highlight javascript %}
npm install --save firebase
{% endhighlight %}

Now we are going to modify index.android.js file

Import firebase module to your app
{% highlight javascript %}
import * as firebase from 'firebase';
{% endhighlight %}

Add configuration parameters (fill with your own ones) and intialize the app.

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

See the full source code from https://github.com/juhahinkula/NativeFirebase.git

