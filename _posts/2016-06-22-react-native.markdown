---
layout: post
title:  "Getting known React Native"
---
Getting known React Native

My goal was to test React Native when implementing similar Nasa app that was made with Cordova in older [post](/2016-05-20-welcome-to-jekyll).

The installation of React Native is really straigthforward. The project was quickly created by using React Native CLI. There was now problems to run first Hello World app with Android emulator. I used Android Studios AVD.

The problems started when I made changes to code and Reloaded JS to emulator. The app changes were not uploaded to emulator. The reason seemed to be BlueStacks Android emulator which was started automatically. When I unistalled BlueStacks everything started to work correctly and I was able to upload chages to emulator.

The REST API call was really easy to implement and worked straigth away. The GUI also contains a button which excecutes API call. 

{% highlight html %}
 render() {
    return (
        <View style={styles.container}>
            <View style={styles.header}>
                <TouchableHighlight style={styles.button} underlayColor='#99d9f4' onPress={this.onLoadPressed.bind(this)}> 
                    <Text style={styles.buttonText}>Load image</Text>
                </TouchableHighlight>
            </View>                   
            <View style={styles.image}>        
                <Image source={{uri: imgUrl}} style={{width: 400, height: 400}} />
            </View>
        </View>
    );
  }
  
  onLoadPressed() {
      fetch('https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY')
      .then((response) => response.json())
      .then((responseData) => {
          var imgSrc = responseData.url;
          console.log(responseData.url)
          imgUrl = responseData.url;
          console.log('url=' + imgUrl);
      })
      .done();
  }
{% endhighlight %}

The proto worked when I set the image URL as a static text but does not work if I try to take it from the variable. I have tried to find out what is going wrong but...Have to continue.

![screenshot]({{ site.baseurl }}/img/reactnative.png)



