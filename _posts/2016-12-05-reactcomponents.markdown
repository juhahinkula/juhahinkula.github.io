---
layout: post
title:  "Using 3rd party React.js components"
---
Let's fine tune our Studentlist front end by using some 3rd party React components. Good place to find components is [js.coach](https://js.coach).

It would be nice to change student form to be modal. We will use react [Skylight](http://marcio.github.io/react-skylight/) component for that.

First step is to install Skylight. Go to the project root and type following npm command.

{% highlight html %}
npm install --save react-skylight
{% endhighlight %}

Import Skylight to your application by adding following statement to App.jsx file

{% highlight javascript %}
import SkyLight from 'react-skylight';
{% endhighlight %}

Now we are ready to use it. We will put our form inside Skylight tags. 'New student'- button will be outside and it will be shown in the listpage. The modal form is opened when button is pressed. 

{% highlight html %}
  <div>
    <SkyLight hideOnOverlayClicked ref="simpleDialog">
        <div className="panel panel-default">
        <div className="panel-heading">Create student</div>
        <div className="panel-body">
        <form className="form">
        <div className="col-md-4">
          <input type="text" placeholder="Firstname" className="form-control"  
       name="firstname" onChange={this.handleChange}/>    
        </div>
        <div className="col-md-4">       
          <input type="text" placeholder="Lastname" className="form-control" 
       name="lastname" onChange={this.handleChange}/>
        </div>
        <div className="col-md-4">
          <input type="text" placeholder="Email" className="form-control" 
       name="email" onChange={this.handleChange}/>
        </div>
        <div className="col-md-2">
          <button className="btn btn-primary" 
          onClick={this.handleSubmit}>Save</button>   
        </div>       
      </form>
    </div>      
    </div>
  </SkyLight>
  <div className="col-md-2">
    <button className="btn btn-primary" 
    onClick={() => this.refs.simpleDialog.show()}>New student</button>
  </div>
</div>   
{% endhighlight %}

The form is then hidden in the handleSubmit function where student is first saved.

{% highlight javascript %}
this.refs.simpleDialog.hide();    
{% endhighlight %}

That is only thing we need to transfer it to modal form.

![screenshot]({{ site.baseurl }}/img/modal.png)

The other component we are going to use is [react-s-alert](https://github.com/juliancwirko/react-s-alert). We are using it to show toast message after student is deleted.

To install react-s-alert go to the project root and type following npm command.

{% highlight html %}
npm install --save react-s-alert
{% endhighlight %}

Import component and stylings by adding following statement to App.jsx file

{% highlight javascript %}
import Alert from 'react-s-alert';
import 'react-s-alert/dist/s-alert-default.css';
import 'react-s-alert/dist/s-alert-css-effects/slide.css';
{% endhighlight %}

Add Alert to App components render() function with 2 sec timeout.

{% highlight html %}
 render() {
    return (
       <div>
          <StudentTable deleteStudent={this.deleteStudent} 
            students={this.state.students}/> 
          <StudentForm createStudent={this.createStudent}/>
          <Alert stack={true} timeout={2000} />
       </div>
    );
  }
{% endhighlight %}

Alert is shown in deleteStudent function afer succesful deletion.

{% highlight javascrip %}
  // Delete student
  deleteStudent(student) {
      fetch (student._links.self.href,
      { method: 'DELETE', 
        credentials: 'same-origin'})
      .then( 
          res => this.loadStudentsFromServer()
      )
      .then(() => { 
          Alert.success('Student deleted', {
            position: 'bottom-left',
            effect: 'slide'
          });
      })
      .catch( err => console.error(err))                
  }  

{% endhighlight %}

![screenshot]({{ site.baseurl }}/img/toast.png)

Source code can be found from the [repository](https://github.com/juhahinkula/SpringListReact.git)