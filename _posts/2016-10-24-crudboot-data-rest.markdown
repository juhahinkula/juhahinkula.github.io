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

curl -v localhost:8081

![screenshot]({{ site.baseurl }}/img/curl_v.png)

Spring Data REST returns keys with link objects using HAL as media type. HAL is a simple format that gives a consistent and easy way to hyperlink between resources in your API.

Resources are named according to pluralized entities (Example: Student entity --> students resource).

In our project the application is secured with Spring Security therefore the available resourses are also secured. 

There is one test user in the database which we can use to test resources

curl -v localhost:8081/students -u user

![screenshot]({{ site.baseurl }}/img/curl_students.png)
