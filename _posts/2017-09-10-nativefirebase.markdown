---
layout: post
title:  "React Native & Firebase"
---
How to create simple Todo app with React Native and Firebase realtime database.

Firebase offers you realtime NoSql database where items are saved as JSON objects. To get that you have to sign-in to Firebase and create project for your app .

First you have to go to Firebase and create new firebase project. You need certain configuration paramters for your app. Navigate to firebase console and click 'Add firebase to your web app' and parameters are shown to you.

![screenshot]({{ site.baseurl }}/img/firebase_params.png)

You also have to modify rules for your database to be able to test it without authentication (not covered in this blog). Copy following lines to your database's rules tab in Firebase console and press Publish button.

{% highlight html %}
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
{% endhighlight %}

Now everyone has rights to read and write our database so this really only for testing/development purposes.

When the Firebase project has been created it is time to create React Native app (This example is done for Android).

Create initial React native app

{% highlight javascript %}
react-native init yourAppName
{% endhighlight %}

Install Firebase
{% highlight javascript %}
npm install --save firebase
{% endhighlight %}

Now we are going to modify *index.android.js* file

Import firebase module to your app
{% highlight javascript %}
import * as firebase from 'firebase';
{% endhighlight %}

Add configuration parameters (fill with your own ones) and intialize the app.

{% highlight javascript %}
const firebaseConfig = {
  apiKey: "YOUR_API_KET",
  authDomain: "YOUR_DOMAIN",
  databaseURL: "YOUR_DB_URL",
  storageBucket: "YOUR_BUCKET"
};

const firebaseApp = firebase.initializeApp(firebaseConfig);
{% endhighlight %}

Add realtime database reference inside constructor.

{% highlight javascript %}
  constructor(props) {
    super(props);
    //realtime db reference
    this.itemsRef = firebaseApp.database().ref('todos');
    this.state = { description: '', todos: [] };
}
{% endhighlight %}

The UI is really simple with default stylings. The upperpart has one TextInput for typing description for the todo and one Button for saving the todo. FlatList is used to show fetched todos.

{% highlight html %}
  render() {
    return (
      <View style={styles.maincontainer}>
        <View style={styles.inputcontainer}>
          <TextInput
          style={{height: 40, width: 200, borderColor: 'gray', borderWidth: 1}}
          onChangeText={(description) => this.setState({description})}
          value={this.state.text}
          />
          <Button onPress={this.saveData} title="Save" /> 
        </View>
        <View style={styles.listcontainer}>
          <FlatList
            data = {this.state.todos}
            keyExtractor = {this.keyExtractor}
            renderItem = {this.renderItem}
            />
        </View>        
      </View>
    );
}
{% endhighlight %}

Button will call saveData method when it is clicked. The database listener has push function which takes new todo object as a parameter. New todo object will then be saved to firebase realtime db. Todo get two attributes description and date. Description is kept in state when TextInput is changed. Date is created when new todo is going to be saved.

{% highlight javascript %}
 saveData = () => {
    let dat = new Date();
    let datString = (dat.getMonth() + 1) + "-" + dat.getDate() + "-" + dat.getFullYear();
    this.itemsRef.push({ description: this.state.description, date: datString});
};
{% endhighlight %}


App screenshot
![screenshot]({{ site.baseurl }}/img/nativefirebase.png)

Firebase database
![screenshot]({{ site.baseurl }}/img/firebase_db.png)

See the full source code from the [repository](https://github.com/juhahinkula/NativeFirebase.git)

