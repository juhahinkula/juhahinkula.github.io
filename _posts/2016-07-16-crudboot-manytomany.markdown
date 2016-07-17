---
layout: post
title:  "Spring Boot application with JPA many-to-many relationship"
---
Studentlist application from the previous blogs will be used to add many-to-many relation by using JPA. The starting point application can be found from previous [here](/2016-06-16-crudboot-security).

The goal is to add new Course entity which is linked to student with many-to-many link. User should also be able to add new courses to students.

The listpage cointains two actions for deleting students and adding courses to students.

![screenshot]({{ site.baseurl }}/img/manytomany_list.png)

List page also shows studen courses in one column. This column is implemented by using thymeleaf iterator and iteration status. It shows all coruses separetad by comma. See more info about iteration status from [thymelead pages](http://www.thymeleaf.org/doc/tutorials/2.1/usingthymeleaf.html#keeping-iteration-status).

{% highlight html %}
<tr th:each = "student : ${students}">
    <td th:text="${student.firstName} + ' ' + ${student.lastName}"></td>
    <td th:text="${student.email}"></td>
    <td th:text="${student.department}"></td>
    <td>
        <span th:each="course,iterStat : ${student.courses}">
        <span th:text="${course.name}"/><th:block th:if="${!iterStat.last}">,</th:block>
        </span>    		
    </td>
    <td>
        <a th:href="@{/addStudentCourse/{id}(id=${student.id})}" class="btn btn-success btn-xs">Add Course</a>
        <a th:href="@{/delete/{id}(id=${student.id})}" class="btn btn-danger btn-xs">Delete</a>
    </td>
</tr>
{% endhighlight %}

Course can be added to students by using the following form.

![screenshot]({{ site.baseurl }}/img/manytomany_add.png)

Many-to-many relationship is defined in Student and Course entity classes. @JointTable annotation creates join table. 

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

It is good to set Spring Boot property for logging sql statements. Then you can see sql statements for table creations in the console.

{% highlight java %}
spring.jpa.show-sql=true
{% endhighlight %}

![screenshot]({{ site.baseurl }}/img/manytomany_console.png)




List of technologies used


- Spring Boot
- Spring security
- H2 database (Java based in-memory db)
- Thymeleaf
- Bootstrap


Project also contains testdata which are inserted at runtime by using Spring Boot CommandLineRunner.

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentCourseList.git)

