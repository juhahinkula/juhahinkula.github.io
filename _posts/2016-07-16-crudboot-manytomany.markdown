---
layout: post
title:  "Spring Boot application with JPA many-to-many"
---
Studentlist application from the previous blogs will be used to add many-to-many relation by using JPA. The application will now contains also Course entity which is linked to student with many-to-many link.

The starting point application can be found from previous [blog](/2016-06-16-crudboot-security).

![screenshot]({{ site.baseurl }}/img/manytomany_list.png)

List of technologies used


- Spring Boot
- Spring security
- H2 database (Java based in-memory db)
- Thymeleaf
- Bootstrap


Project also contains testdata which are inserted at runtime by using Spring Boot CommandLineRunner.

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentCourseList.git)

