---
layout: post
title:  "Getting known React Native"
subtitle: "Simple Android app"
---

My goal was to test React Native when implementing similar Android Nasa app that was made with Cordova in older [post](/2016-05-20-welcome-to-jekyll).

The installation of React Native is really straigthforward. The project was quickly created by using React Native CLI. There was now problems to run first Hello World app with Android emulator. I used Android Studio AVD emulators. I started by going through Facebook's [starting guide](https://facebook.github.io/react-native/docs/getting-started.html#content) and good reading about React Native layouts can be found [here](http://moduscreate.com/react-native-layout-system/). 

The problems started when I made changes to code and Reloaded JS to emulator. The app changes were not uploaded to emulator. The reason seemed to be BlueStacks Android emulator which was started automatically. When I unistalled BlueStacks everything started to work correctly and I was able to upload chages to emulator.

The REST API call was really easy to implement and worked straigth away. The GUI contains two buttons for showing the daily image and random image. 

Constructor contains intial state nasaData which is used to bind REST API call JSON response. This is also used to determine whether the data has been loaded.

{% highlight javascript %}
  constructor(props) {
    super(props);

    this.state = {
      nasaData: null,
    };
  } 
{% endhighlight %}

Render check if the data has been loaded by using nasaData state. 

{% highlight javascript %}
  render() {
    if (!this.state.nasaData) {
        return this.renderInitialView();
    }
    else {
        return this.renderMainView();
    }
  }
{% endhighlight %}

The first view is shown when app is started and data has not been loaded yet. When load button is pressed the API call is excecuted and the main view will be shown.

{% highlight html %}
renderInitialView() { 
     return (        
         <View style={styles.container}> 
            <View style={styles.mainheader}>
                <Text style={styles.headertext}>NasaApp</Text>
                <TouchableHighlight style={styles.button} underlayColor='#99d9f4' 
                onPress={this.onLoadPressed.bind(this)}> 
                    <Text style={styles.buttonText}>Load Image</Text>
                </TouchableHighlight>    
                <TouchableHighlight style={styles.button} underlayColor='#99d9f4' 
                onPress={this.onRandomPressed.bind(this)}> 
                    <Text style={styles.buttonText}>Load Random</Text>
                </TouchableHighlight>                     
             </View>            
         </View> 
    ); 
 }  
  
renderMainView() {
	return {
    <View style={styles.container}>
        <View style ={styles.header}>
        <View style={styles.header}>
            <TouchableHighlight style={styles.button} underlayColor='#99d9f4' 
            onPress={this.onLoadPressed.bind(this)}> 
                <Text style={styles.buttonText}>Load Image</Text>
            </TouchableHighlight>    
            <TouchableHighlight style={styles.button} underlayColor='#99d9f4' 
            onPress={this.onRandomPressed.bind(this)}> 
                <Text style={styles.buttonText}>Load Random</Text>
            </TouchableHighlight>                     
         </View> 
        </View>
        <View style={styles.copyright}>
            <Text>Copyright: {this.state.nasaData.copyright}</Text>
        </View>
        <View style={styles.image}>        
            <Image source={{uri: this.state.nasaData.url}} style={{width: 400, height: 400}} />
        </View>
        <ScrollView ref='scrollView' keyboardDismissMode='interactive' style={styles.scrollView}>
            <Text style={styles.footer}>{this.state.nasaData.explanation}</Text>
        </ScrollView>
    </View>
	};
}
{% endhighlight %}

The source code of the Android app can be found from GitHub

Screenshot of the main view. 'Load Image' - button loads the image of the day and 'Load Random' - button loads the random image from the past. I would also like to test using Android date picker for selecting date of the image to be loaded.

![screenshot]({{ site.baseurl }}/img/reactnative.png)




