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

Authentication was added by using Spring Security. The first step is to add Spiring Security dependency to your pom.xml file.

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

WebSecurityConfig class contains Spring Security configurations. This example uses inMemoryAuthentication where users are created at runtime.

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

Project also contains testdata which are inserted at runtime by using Spring Boot CommandLineRunner.

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/StudentListSecure.git)

