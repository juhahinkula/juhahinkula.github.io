---
layout: post
title:  "Spring Boot Data REST"
---
Spring Data REST builds on top of the Spring Data and it allows you to get discovarable REST API to your Spring Boot application. In this blog we use our student example ([repository](https://github.com/juhahinkula/StudentListFinal.git)) project by adding Spring Data REST to it.

To get started add Spring Data REST to dependencies.

{% highlight xml %}
<dependencies>
    ...
    <dependency>
        <groupId>org.springframework.data</groupId>
        <artifactId>spring-data-rest-webmvc</artifactId>
        <version>2.5.3.RELEASE</version>
    </dependency>
    ...
</dependencies>
{% endhighlight %}

As a default Spring Data REST uses public repositories to detect and create REST resourses. In out student Spring Boot application there is three public repositories for students, course and users.

When Spring Boot application is up and running you can see the available resources by using GET request to application root.

{% highlight xml %}
curl -v localhost:8081
{% endhighlight %}

![screenshot]({{ site.baseurl }}/img/curl_v.png)

Spring Data REST returns keys with link objects using HAL as media type. HAL is a simple format that gives a consistent and easy way to hyperlink between resources in your API.

Resources are named according to pluralized entities (Example: Student entity --> students resource). You can use @RepositoryRestResource annotation to change resourse endpoint.

In our project the application is secured with Spring Security therefore the available resourses are also secured. 

There is one test user in the database which we can use to test resources

{% highlight xml %}
curl -v localhost:8081/students -u user
{% endhighlight %}

![screenshot]({{ site.baseurl }}/img/curl_students.png)

You can also use POST messages to add data using Spring Data REST

{% highlight xml %}
curl -i -X POST -H "Content-Type:application/json" -d '{  "firstName" : "Matt",  "lastName" : "Wilder" }' http://localhost:8081/students -u user
{% endhighlight %}

-i parameter will show response message. -X POST defines message to be POST for creating new entity. -H sets the contentype.

![screenshot]({{ site.baseurl }}/img/curl_add.png)

This will add new student to database.

You can also use all queries defined in the repository to search entities. Available query reources can be found with following request. Request will return query URIs.

{% highlight xml %}
curl -v localhost:8081/students/search -u user
{% endhighlight %}

![screenshot]({{ site.baseurl }}/img/curl_search.png)


