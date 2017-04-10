---
layout: post
title:  "Spring Boot & JPA Auditing"
---

Database auditing means logging the persistent entities. In the business applications we are often interested in who created the record and when. JPA auditing allows you to record who, what and when for each entity object. 

If you have existing Spring Boot JPA application you need following steps to enable auditing.

1.) Entity annotations

Auditing attributes can be defined by using @CreatedDate, @CreatedBy, @LastModifiedDate and @LastModifiedBy annotations in the entity class.

{% highlight java %}
@Entity
@EntityListeners(AuditingEntityListener.class)
public class Todo {
  @Id
  @GeneratedValue(strategy=GenerationType.AUTO)
	private long id;
    
	private String description, status;
	
	@CreatedDate
	private Date createdDate;	

	@CreatedBy
	private String createdBy;

	@LastModifiedDate 
	private Date lastModifiedDate;
	
	@LastModifiedBy 
	private String lastModifiedBy;
	
	public Todo() {
	}
  ...
{% endhighlight %}

2.) Auditing configuration

Add a new configuration class which enables JPA auditing ( @EnableJpaAuditing annotation )

AuditorAware<T> interface is used to get Spring Security user info for createdby and modifiedby fields.

{% highlight java %}
@Configuration
@EnableJpaAuditing
class AuditConfig {
    @Bean
    public AuditorAware<String> createAuditorProvider() {
        return new SecurityAuditor();
    }

    @Bean
    public AuditingEntityListener createAuditingListener() {
        return new AuditingEntityListener();
    }

    public static class SecurityAuditor implements AuditorAware<String> {
        @Override
        public String getCurrentAuditor() {
            Authentication auth = SecurityContextHolder.getContext().getAuthentication();
            String username = auth.getName();
            return username;
        }
    }	
}
{% endhighlight %}

Below is the screenshot from the demo todo application with auditing enabled. Source code can be found from the [repository](https://github.com/juhahinkula/todoList.git)

![screenshot]({{ site.baseurl }}/img/jpa_auditing.png)

