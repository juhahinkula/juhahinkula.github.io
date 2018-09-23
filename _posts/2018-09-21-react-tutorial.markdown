---
layout: post
title:  "React Frontend"
subtitle: "Create React frontend to existing REST API"
---

In this tutorial we will create a simple frontend using React. The backend that we will use  is done with Spring Boot and it can be found from Heroku cloud server ([https://carstockrest.herokuapp.com]). You should have basic knowledge of React and REST API.

The REST API has */cars* endpoint that can be used to get all cars from the database.

![screenshot]({{ site.baseurl }}/img/carlist.PNG)

Let's create a new React application by using Create React App startkit made by Facebook ([https://github.com/facebook/create-react-app]). You need to have Node version 6 or greater installed. You can create a new React application called carfront with following npx command.

{% highlight xml %}
npx create-react-app carfront
{% endhighlight %}

To start the application you have to move into carfront folder made by create-react-app and start the application using following commands.

{% highlight xml %}
cd carfront
npm start
{% endhighlight %}

When the application has started you should see the following ui in your browser.

![screenshot]({{ site.baseurl }}/img/create_react_app.PNG)

We will use Material-UI React component library to build a user interface. This can be intalled with the following command.

{% highlight xml %}
npm install @material-ui/core
{% endhighlight %}

Open your application folder with proper code editor like VSCode. Open the file called App.js in the editor. The first task to do is to show all cars from REST API in the table. For that we have to make a GET request to *https://carstockrest.herokuapp.com/cars* endpoint. We will use fetch API for that. Request to REST API can be done inside the *componentDidMount()* lifecycle method that is invoked after the component has been rendered first time. Remove code from the *render()* method return statement of the App.js file and add the *componentDidMount()* method. Now, your App.js file should look like the following example code.

{% highlight react %}
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  componentDidMount() { 
    // Here we will make a request
  }

  render() {
    return (
      <div className="App">

      </div>
    );
  }
}

export default App;
{% endhighlight %}

Next, we will create the constructor and introduce state called cars. That state is array and it is used to keep cars that we get from the response.

{% highlight react %}
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {cars: []}
  }
  componentDidMount() { 
    // Here we will make a request
  }

  render() {
    return (
      <div className="App">

      </div>
    );
  }
}

export default App;
{% endhighlight %}

Next, add following code inside your *componentDidMount()* method. The code send a request to the backend */cars* endpoint and parse list of cars from the json response. The cars from the response are then saved to cars state using *setState()* method. The ui is then automatically re-rendered. 

{% highlight javascript %}
  componentDidMount() { 
    fetch("https://carstockrest.herokuapp.com/cars")
    .then(response => response.json())
    .then(responseData => {
      this.setState({cars: responseData})
    })
  }
{% endhighlight %}

The user interface is still empty because we haven't add anything to the render() method yet. Let's add Material-UI table that will list cars in our user interface. First, add following component imports to your App.js file. 

{% highlight javascript %}
import Table from '@material-ui/core/Table';
import TableBody from '@material-ui/core/TableBody';
import TableCell from '@material-ui/core/TableCell';
import TableHead from '@material-ui/core/TableHead';
import TableRow from '@material-ui/core/TableRow';
{% endhighlight %}

Then we have to generate TableRow components from our cars using map() function. Finally, we render table rows inside the <Table> component. There is also table header defined using <TableHead> component. Below is the source code of the *render()* method.

{% highlight react %}
 render() {
   // Generate table rows
    const rows = this.state.cars.map((row, index) => { return(
      <TableRow key={index}>
        <TableCell>{row.brand}</TableCell>
        <TableCell>{row.model}</TableCell>
        <TableCell>{row.color}</TableCell>
        <TableCell>{row.fuel}</TableCell>
        <TableCell numeric>{row.year}</TableCell>
        <TableCell numeric>{row.price}</TableCell>
      </TableRow>
    )});

    return (
      <div className="App">
        <Table>
        <TableHead>
          <TableRow>
            <TableCell>Brand</TableCell>
            <TableCell>Model</TableCell>
            <TableCell>Color</TableCell>
            <TableCell>Fuel</TableCell>
            <TableCell numeric>Year</TableCell>
            <TableCell numeric>Price (€)</TableCell>
          </TableRow>
        </TableHead>
          <TableBody>
            {rows}
          </TableBody>
        </Table>
      </div>
    );
  }
{% endhighlight %}

Now, your user interface should look like following screenshot.

![screenshot]({{ site.baseurl }}/img/cartable.PNG)

CONTINUES SOON...
