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

The UI is really simple with default stylings. There is button for adding new todos. FlatList is used to show fetched todos. Add button opens a modal form for entering new todos. Modal is standard React Native component. 

The datepicker component used for this project is [react-native-datepicker](https://github.com/xgfe/react-native-datepicker) and toast component for the messages is [react-native-easy-toast](https://github.com/crazycodeboy/react-native-easy-toast). See the installation and usage from their sites.

{% highlight html %}
render() {
    return (
      <View style={styles.maincontainer}>
        <Modal animationType="slide" transparent={false} visible={this.state.modalVisible}
        onRequestClose={() => {}} >
        <View style={styles.inputcontainer}>
          <TextInput
          style={{height: 40, width: 200, borderColor: 'gray', borderWidth: 1, marginBottom: 7}}
          onChangeText={(description) => this.setState({description})}
          value={this.state.text}
          placeholder="description"
          />
          <DatePicker
          style={{width: 200, marginBottom: 7}}
          date={this.state.date}
          mode="date"
          placeholder="select date"
          format="YYYY-MM-DD" 
          customStyles={{
            dateIcon: {
              position: 'absolute',
              left: 0,
              top: 4,
            },
          }}
          onDateChange={(date) => {this.setState({date: date})}}
          />         
          <Button onPress={this.saveData} title="Save" /> 
        </View>
        </Modal>
        <View style={styles.headercontainer}>                  
          <Text style={{fontSize: 20, marginRight: 40}}>ALL TODOS</Text>   
          <Button title="Add" onPress={() => this.setState({modalVisible: true})} />
        </View>
        <View style={styles.listcontainer}>
          <FlatList
            data = {this.state.todos}
            keyExtractor = {this.keyExtractor}
            renderItem = {this.renderItem}
            style={{marginTop: 20}}
            />
        </View>
        <Toast ref="toast" position="top"/>        
      </View>
    );
  }
}
{% endhighlight %}

Button will call saveData method when it is clicked. The database listener has push function which takes new todo object as a parameter. New todo object will then be saved to firebase realtime db and it gets an unique key. Todo get two attributes description and date. Description and date are kept in react state.

{% highlight javascript %}
saveData = () => {
  this.itemsRef.push({ description: this.state.description, date: this.state.date});
  this.refs.toast.show('Todo saved');
  this.setState({date: '', modalVisible: false});
};
{% endhighlight %}

The todos are shown in the FlatList component. We have to add listener for all todo items. The listener will fetch todos from the database and save those to the react state called todos. Todos are saved as an array of JSON objects. The benefit with Firebase realtime database is that whenever items are added, edited or deleted we will get new result back as a snapshot.

{% highlight javascript %}
// Fetch todos
listenForItems(itemsRef) {
  itemsRef.on('value', (snap) => {
    var items = [];
    snap.forEach((child) => {
      items.push({
        id: child.key,
        description: child.val().description,
        date: child.val().date,
      });
    });

    this.setState({todos: items });
  });
}
{% endhighlight %}

For the flatlist we have to define keyExtractor and renderItem properties. One listitem will show todo's description and date.
{% highlight javascript %}
  keyExtractor = (item) => item.id;

  renderItem = ({item}) =>
  <View >
    <Text>{item.description}, {item.date}</Text>   
  </View>;
{% endhighlight %}

And then flatlist can be added to render() function.

{% highlight html %}
<FlatList
  data = {this.state.todos}
  keyExtractor = {this.keyExtractor}
  renderItem = {this.renderItem}
/>
{% endhighlight %}


App screenshot
![screenshot]({{ site.baseurl }}/img/todolist.png)

Firebase database
![screenshot]({{ site.baseurl }}/img/firebase_db.png)

See the full source code from the [repository](https://github.com/juhahinkula/NativeFirebase.git)

