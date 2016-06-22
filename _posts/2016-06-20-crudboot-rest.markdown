---
layout: post
title:  "REST service with Spring Boot"
---
It is very easy to implement REST service to StudentList example that was made in previous [blog](/2016-06-16-crudboot).

Just add following method to controller class. Spring uses Jackson JSON library to automatically marshal instances of model class Student into JSON.

{% highlight java %}
    @RequestMapping(value = "getstudents", method = RequestMethod.GET)
    public @ResponseBody List<Student> getStudents() {
            return (List<Student>)repository.findAll();
    }  
{% endhighlight %}

Run the application and navigate to localhost:8080/getstudents

![screenshot]({{ site.baseurl }}/img/bootrest.png)

This example is Open REST API which is not secure. It can be secured by using Spring Security. 



