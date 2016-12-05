---
layout: post
title:  "React.js + Spring Security"
---
We will now secure our [Spring Boot + React.js application](https://github.com/juhahinkula/SpringReactWebpack.git) by using Spring security. Follow the steps from [older post](/2016-07-31-crudboot-security) to include user entity and Spring Security.

Spring security configuration class will allow access to public folder because our bundled bundle.js file is there. All other endpoints needs authentication.  Csrf is not covered here therefore it is disabled.

{% highlight java %}
  @Override
  protected void configure(HttpSecurity http) throws Exception {
		http
		.authorizeRequests()
			.antMatchers("/public/**").permitAll()
			.anyRequest().authenticated()
			.and()
		.formLogin()
			.defaultSuccessUrl("/", true)
			.permitAll()
			.and()
		.httpBasic()
			.and()
		.csrf().disable()
		.logout()
	    .logoutSuccessUrl("/")
			.invalidateHttpSession(true)
			.deleteCookies("JSESSIONID");
    }
  }   
{% endhighlight %}

After Spring security is configured the rest api is also secured. Rest api endpoint is /api and now everything under /api/** is only accessible to authenticated users.

Now when the enduser login to application, Spring security send back a cookie that contains JSESSIONID parameter. This parameter must be sent back in every request that our application knows we are logged in.

Spring security configuration defines also that session is destroyed and cookie is deleted after user logout from the application.  

We use fetch() to send requests to the rest api from React frontend. Fetch [documentation](https://github.com/github/fetch) says that credentials option must be provided to be able to send cookies. 

{% highlight javascript %}
fetch('/URI', {
  credentials: 'same-origin'
}) 
{% endhighlight %}

After that we can access secured rest api with react.js frontend. We can also see that now JSESSIONID is included to requests.

![screenshot]({{ site.baseurl }}/img/cookie.png)

Finally  we can fetch students from the backend with following function.

{% highlight javascript %}
  loadStudentsFromServer() {
      fetch('http://localhost:8080/api/students', 
      {credentials: 'same-origin'}) 
      .then((response) => response.json()) 
      .then((responseData) => { 
          this.setState({ 
              students: responseData._embedded.students, 
          }); 
      });
  } 
{% endhighlight %}

Source code can be found from the [repository](https://github.com/juhahinkula/SpringListReact.git)