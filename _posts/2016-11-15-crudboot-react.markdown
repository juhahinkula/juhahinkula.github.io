---
layout: post
title:  "Spring Boot + React.js"
---
I decided to try React.js in frontend with Spring Boot Data REST back end ([see previous post](/2016-10-24-crudboot-data-rest)).

Greg Turnquist has written excellent [tutorials](https://spring.io/guides/tutorials/react-and-spring-data-rest/) about Spring Boot and React.js and these are good starting point.

The example application uses H2 runtime database and has functionalities to list, add and delete students from the database. Stylings are made with Bootstrap.

![screenshot]({{ site.baseurl }}/img/springreact.png)

Students are fetched from the database by executing GET request to Spring Boot backend students endpoint. Students array is react state which means that everytime when the array is updated the render() function is called and ui is updated.

{% highlight javascript %}
  componentDidMount() {
    this.loadStudentsFromServer();
  }
  
  // Load students from database
  loadStudentsFromServer() {
      fetch('http://localhost:8080/api/students') 
      .then((response) => response.json()) 
      .then((responseData) => { 
          this.setState({ 
              students: responseData._embedded.students, 
          }); 
      });     
  } 
{% endhighlight %}

The React.js frontend is built by using following React components: 

- StudentTable
- Student
- StudentForm

StudentTable component renders all students in the table. 

{% highlight react %}
class StudentTable extends React.Component {
    constructor(props) {
        super(props);
    }
    
    render() {
      var students = this.props.students.map(student =>
        <Student key={student._links.self.href} student={student} deleteStudent={this.props.deleteStudent}/>
    );

    return (
      <div>
      <table className="table table-striped">
        <thead>
          <tr>
            <th>Firstname</th><th>Lastname</th><th>Email</th><th> </th>
          </tr>
        </thead>
        <tbody>{students}</tbody>
      </table>
      </div>);
  }
}
{% endhighlight %}

Student component renders one student row in the table. It will also add delete button to each row with onClick listener. Delete button will call component's deleteStudent function which will then  pass student object to App component's delete function.

{% highlight react %}
class Student extends React.Component {
    constructor(props) {
        super(props);
        this.deleteStudent = this.deleteStudent.bind(this);
    }

    deleteStudent() {
        this.props.deleteStudent(this.props.student);
    } 
 
    render() {
        return (
          <tr>
            <td>{this.props.student.firstname}</td>
            <td>{this.props.student.lastname}</td>
            <td>{this.props.student.email}</td>
            <td>
                <button className="btn btn-danger btn-xs" onClick={this.deleteStudent}>Delete</button>
            </td>
          </tr>
        );
    } 
}
{% endhighlight %}

App component's deleteStudent function execute DELETE request to Spring Boot backend.

{% highlight javascript %}
  // Delete student
  deleteStudent(student) {
      fetch (student._links.self.href,
      { method: 'DELETE',})
      .then( 
          res => this.loadStudentsFromServer()
      )
      .catch( err => cosole.error(err))                
  }  
{% endhighlight %}

StudentForm component renders student form which is used to create new students.

{% highlight react %}
class StudentForm extends React.Component {
    constructor(props) {
        super(props);
        this.state = {firstname: '', lastname: '', email: ''};
        this.handleSubmit = this.handleSubmit.bind(this);   
        this.handleChange = this.handleChange.bind(this);     
    }

    handleChange(event) {
        console.log("NAME: " + event.target.name + " VALUE: " + event.target.value)
        this.setState(
            {[event.target.name]: event.target.value}
        );
    }    
    
    handleSubmit(event) {
        event.preventDefault();
        var newStudent = {firstname: this.state.firstname, lastname: this.state.lastname, email: this.state.email};
        this.props.createStudent(newStudent);        
    }
    
    render() {
      return (
        <div className="panel panel-default">
        div className="panel-heading">Create student</div>
            div className="panel-body">
            <form className="form-inline">
                <div className="col-md-2">
                    <input type="text" placeholder="Firstname" className="form-control"  
                        name="firstname" onChange={this.handleChange}/>    
                </div>
                <div className="col-md-2">       
                    <input type="text" placeholder="Lastname" className="form-control" 
                        name="lastname" onChange={this.handleChange}/>
                </div>
                <div className="col-md-2">
                    <input type="text" placeholder="Email" className="form-control" 
                        name="email" onChange={this.handleChange}/>
                </div>
                <div className="col-md-2">
                    <button className="btn btn-success" onClick={this.handleSubmit}>Save</button>   
                </div>        
            </form>
            </div>      
        </div>
    );
  }
}
{% endhighlight %}

handleSubmit creates new student object from the values and pass it to App component createStudent function. handleChange change states when user type something to the input fields. 

createStudent function sends POST request to backend with created student in the body as a JSON object.

{% highlight react %}
  // Create new student
  createStudent(student) {
      fetch('http://localhost:8080/api/students', 
      {   method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(student)
      })
      .then( 
          res => this.loadStudentsFromServer()
      )
      .catch( err => cosole.error(err))
  }
{% endhighlight %}

The complete project code can be found from GitHub [repository](https://github.com/juhahinkula/SpringBootReact.git)

