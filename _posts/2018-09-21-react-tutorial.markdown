---
layout: post
title:  "React Frontend"
subtitle: "Create React frontend to existing REST API"
---

In this tutorial we will create a simple frontend using React. The backend that we will use  is done with Spring Boot and it can be found from Heroku cloud server ([https://carstockrest.herokuapp.com]).

The REST API has */cars* endpoint that can be used to get all cars from the database.

![screenshot]({{ site.baseurl }}/img/carlist.PNG)

Let's create a new React application by using Create React App startkit made by Facebook ([https://github.com/facebook/create-react-app]). You need to have Node version 6 or greater installed. You can create a new React application called carfront with following npx command.

{% highlight xml %}
npx create-react-app carfront
{% endhighlight %}

To start the application you have move into carfront folder made by create-react-app and the start the application.

{% highlight xml %}
cd carfront
npm start
{% endhighlight %}



