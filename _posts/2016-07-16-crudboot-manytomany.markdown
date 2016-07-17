---
layout: post
title:  "Spring Boot application with JPA many-to-many"
---
Studentlist application from the previous blogs will be used to add many-to-many relation by using JPA. The starting point application can be found from previous [here](/2016-06-16-crudboot-security).

The goal is to add new Course entity which is linked to student with many-to-many link. User should also be able to add new courses to students.

The listpage cointains two actions for deleting students and adding courses to students.

![screenshot]({{ site.baseurl }}/img/manytomany_list.png)

Course can be added to students by using the following form.

![screenshot]({{ site.baseurl }}/img/manytomany_add.png)

Many-to-many relation is defined in Student and Course entity classes. @JointTable annotation creates join table. 

{% highlight java %}
// Student entity
@ManyToMany(cascade = CascadeType.MERGE)
@JoinTable(
    name = "student_course", 
    joinColumns = { @JoinColumn(name = "id") }, 
    inverseJoinColumns = { @JoinColumn(name = "courseid") })
public Set<Course> getCourses() {
    return this.courses;
}
{% endhighlight %}

{% highlight java %}
// Course entity
@ManyToMany(mappedBy = "courses")    
private Set<Student> students;  
    
public Set<Student> getStudents() {
    return students;
}
{% endhighlight %}

List of technologies used


- Spring Boot
- Spring security
- H2 database (Java based in-memory db)
- Thymeleaf
- Bootstrap


Project also contains testdata which are inserted at runtime by using Spring Boot CommandLineRunner.

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentCourseList.git)

