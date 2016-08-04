---
layout: post
title:  "Spring Boot testing"
---
Adding tests to simple CRUD application with Spring Boot and Spring Security

The starting point was the example crud application made in the previous [blog](/2016-07-31-crudboot-security). The goal was to create JPA integration testing.

Spring boot version used in this project was 1.3.6. There is lot of improvements coming for testing in the next Spring boot version 1.4.

First thing to do is to add Spring Boot test starter to dependencies. Spring Boot test starter is done for testing Spring Boot applications with libraries including JUnit, Hamcrest and Mockito 

{% highlight xml %}
<dependencies>
    ...
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
    ...
</dependencies>
{% endhighlight %}

Spring Initializr auomatically creates package and one class for tests which you can use as a starting point. The testing is really straighforward if you are familiar with JUnit.
We need Spring4JUnit4ClassRunner (SpringRunner in Spring Boot 1.4) for application context to be created (@RunWith annotation). @SringApplicationConfiguration annotation is used to specify which application contexts are used in tests.

In the test class we have access to application context and we can inject any Spring bean by using @Autowire.

Test methods are defined by using @Test annotation. The following test class creates user and student to database and test that insertion is succeeded.

{% highlight java %}
@RunWith(SpringJUnit4ClassRunner.class)
@SpringApplicationConfiguration(classes = {CrudbootApplication.class, WebSecurityConfig.class })
public class CrudbootApplicationTests {

	private UserRepository userRepository;

    @Autowired
    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    private StudentRepository studentRepository;

    @Autowired
    public void setStudentRepository(StudentRepository studentRepository) {
        this.studentRepository = studentRepository;
    }    
    
    @Test
    public void addUser() {
    	User user = new User("testuser", "testuser", "USER");

    	assertNull(user.getId());
    	userRepository.save(user);
    	assertNotNull(user.getId());
    }
    
	@Test
    public void addStudent() {
		Student student = new Student("Test", "Student", "IT", "test@test.com");
		
		studentRepository.save(student);
		Student findStudent = studentRepository.findOne(student.getId());
		assertNotNull(findStudent);
    }   
}
{% endhighlight %}

You can run tests by using maven or with IDE. Eclipse contains nice JUnit plugin which shows test results after run.

![screenshot]({{ site.baseurl }}/img/springboot_test.png)

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentListFinal.git)
