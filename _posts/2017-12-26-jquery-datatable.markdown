---
layout: post
title:  "Spring Boot & Datatable"
subtitle: "Tuning tables with jquery datatable"
---
[Jquery DataTables](https://datatables.net/) table plugin for jquery is really easy-to use tool for tuning your tables. It provides paging, sorting, searching and many more useful features.

Usage of datatables with Spring Boot thymeleaf templates is straigthforward. You just have to link javascript and css libraries to your template and have a rest controller to fetch data.

Let's change our todo sample application's bootstrap table to use Jquery DataTables plugin. You can clone the application from the Github.

{% highlight java %}
git clone https://github.com/juhahinkula/todoList.git
{% endhighlight %}

First we have to add rest controller method to return our todos in the JSON format. Add following method to TodoController class. You can now strart the application and call endpoint http://localhost:8080/gettodos and see that all todos are returned in the JSON format.

{% highlight java %}
	// Return all todos as JSON
    @RequestMapping(value="/gettodos")
    public @ResponseBody List<Todo> getTodos(Model model) {	
        return (List<Todo>)repository.findAll();
    }   
{% endhighlight %}

Then we will change our Thymeleaf view called todos.html. First add following dependencies to the view.

{% highlight html %}
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
<link type="text/css" rel="stylesheet" th:href="@{https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css}" />
<link type="text/css" rel="stylesheet" th:href="@{https://cdn.datatables.net/1.10.16/css/dataTables.bootstrap.min.css}" />
<script type="text/javascript" src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>
<script type="text/javascript" src="https://cdn.datatables.net/1.10.16/js/dataTables.bootstrap.min.js"></script>
{% endhighlight %}

Then give an unique id to the table and remove the existing row part.

{% highlight html %}
    <table id="todotable" class="table table-striped">
		<thead>
		<tr>
		  <th>Description</th>
		  <th>Status</th>
		  <th>CreatedDate</th>
		  <th>CreatedBy</th>
		</tr>  
		</thead>
    </table>
{% endhighlight %}

Finally add following javascript to the end of the body section. The script calls getTodos method from our controller and populates the table with returned data.

{% highlight html %}
	<script>
	$(document).ready( function () {
		 var table = $('#todotable').DataTable({
			"sAjaxSource": "/gettodos",
			"sAjaxDataProp": "",
			"order": [[ 0, "asc" ]],
			"columns": [
				{ "data": "description"},
				{ "data": "status"},
				{ "data": "createdDate"},
				{ "data": "createdBy"},				    
			]
		 })
	});	
	</script>
{% endhighlight %}

That's it and now you have todo table with paging, sorting and instant search. It is also easy to get excel and pdf export functionality. 

![screenshot]({{ site.baseurl }}/img/todo_datatable.png)

See the full source code from the [repository](https://github.com/juhahinkula/todoList.git) datatable branch.

