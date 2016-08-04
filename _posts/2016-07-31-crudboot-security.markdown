---
layout: post
title:  "Secured Spring Boot CRUD application"
---
Simple CRUD application with Spring Boot and Spring Security

![screenshot]({{ site.baseurl }}/img/bootcrud_secure.png)

List of technologies used


- Spring Boot
- Spring security
- H2 database (Java based in-memory db)
- Thymeleaf
- Bootstrap

The starting point was the example crud application made in the previous [blog](/2016-06-16-crudboot).

Authentication was added by using Spring Security. The first step is to add Spring Security dependency to your pom.xml file.

{% highlight xml %}
<dependencies>
    ...
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
    ...
</dependencies>
{% endhighlight %}

WebSecurityConfig class contains Spring Security configurations. The configure(HttpSecurity) method defines which URL paths are secured and which are not. Only authenticated users can access student list and create or delete users. This example uses inMemoryAuthentication where users are created at runtime. Note! You have to enable static/css foleder in your configuration. Otherwise css styling is not working without authentication (login page).

{% highlight java %}
@Configuration
@EnableWebSecurity
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
	@Override
    protected void configure(HttpSecurity http) throws Exception {
        
      http
        .authorizeRequests().antMatchers("/css/**").permitAll() // Enable css when logged out
          .and()
        .authorizeRequests()
          .antMatchers("/", "add", "delete/{id}", "save", "students")
            .permitAll()
            .anyRequest().authenticated()
            .and()
        .formLogin()
            .loginPage("/login")
            .defaultSuccessUrl("/students")
            .permitAll()
            .and()
        .logout()
            .permitAll()
            .and()
        .httpBasic()
            .and()
        .csrf().disable(); //Disable CSRF
    }
     
    @Autowired
    public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {  
    	auth.inMemoryAuthentication().withUser("user").password("user").roles("USER");
    }
}
{% endhighlight %}

The login page is done with Thymeleaf. Spring Security provides a filter that intercepts the request and authenticates the user. If the user fails to authenticate, the page is redirected to "/login?error" and login page page displays the appropriate error message.  

{% highlight html %}
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <link type="text/css" rel="stylesheet" href="/css/bootstrap.min.css" th:href="@{css/bootstrap.min.css}" />
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />	
    <title>Studenlist login</title>
  </head>
  <body>
  <div class="col-md-4 col-md-offset-4">
    <div class="alert alert-danger" th:if="${param.error}">
        Invalid username and password.
    </div>
    <div class="alert alert-warning" th:if="${param.logout}">
        You have been logged out.
    </div>
    <form th:action="@{/login}" method="post">
      <div><label> User Name : <input type="text" name="username" class="form-control"/> </label></div>
      <div><label> Password: <input type="password" name="password" class="form-control"/> </label></div>
      <div><input type="submit" value="Sign In" class="btn btn-success"/></div>
    </form>
  </div>
  </body>
</html>
{% endhighlight %}

Student listpage contains logout functionality and shows current auhenticated username.

{% highlight html %}
<form th:action="@{/logout}" method="post">
    <input type="submit" value="Sign Out" class="btn btn-danger"/>
</form>
{% endhighlight %}


Project also contains testdata which are inserted at runtime by using Spring Boot CommandLineRunner.

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentListSecure.git)

## Part II: Reading users from database & password encoding

Next step is to save users to database and use this entity for authentication. 

The user entity class 

{% highlight java %}
import javax.persistence.*;

@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id", nullable = false, updatable = false)
    private Long id;

    // Username with unique constraint
    @Column(name = "username", nullable = false, unique = true)
    private String username;

    @Column(name = "password", nullable = false)
    private String passwordHash;

    @Column(name = "role", nullable = false)
    private String role;

    ... getters and setters
{% endhighlight %}

Username is defined to be unique by using unique constraint. It is not good idea to store password as a plain-text therefore it is saved as encrypted.
In this example I am using BCrypt password hashing which is really easy to use with Spring framework.

We also have to implement UserDetailsService interface which is Srping core interface for user specific data. The interface requires only one read-only method.

{% highlight java %}
/**
 * This class is used by spring controller to authenticate and authorize user
 **/
@Service
public class UserDetailServiceImpl implements UserDetailsService  {
	private final UserServiceImpl userService;

	@Autowired
	public UserDetailServiceImpl(UserServiceImpl userService) {
		this.userService = userService;
	}

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException
    {   
    	User curruser = userService.getUserByUsername(username);
    	
        UserDetails user = new org.springframework.security.core.userdetails.User(username, curruser.getPasswordHash(), true, 
        		true, true, true, AuthorityUtils.createAuthorityList("USER"));
        
        return user;
    }   
} 
{% endhighlight %}

Then we have to do modification to configureGlobal method in WebSecurityConfig class.

{% highlight java %}
@Autowired
public void configureGlobal(AuthenticationManagerBuilder auth) throws Exception {
    auth.userDetailsService(userDetailsService).passwordEncoder(new BCryptPasswordEncoder());
}
{% endhighlight %}

Now the authentication is done against database user entity with crypted passwords.

Inserted testdata now contains one user with crypted password (password = user).

{% highlight java %}
// Create user with BCrypt encoded password
User user1 = new User("user", "$2a$06$3jYRJrg0ghaaypjZ/.g4SethoeA51ph3UD4kZi9oPkeMTpjKU5uo6", "USER");
urepository.save(user1);
{% endhighlight %}

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentListFinal.git)
