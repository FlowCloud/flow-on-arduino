---
layout: post
title: Geofence Application&#58; Android Client 
---

What we have so far can do all the logging and alert triggering, but it's pretty useless without a client application to use it.

I have produced an Android client based on MakeItFlow, which you can get [here](https://github.com/IMG-FlowCloud/flow-on-arduino/raw/gh-pages/downloads/app-geofence-debug.apk). I have replaced the command interface with a map-based GUI. This branch of MakeItFlow is available in the github mirror of MakeItFlow [here](https://github.com/IMG-FlowCloud/make-it-flow-arduino/tree/geofence). The apk can also be downloaded from the [release page](https://github.com/IMG-FlowCloud/make-it-flow-arduino/releases/tag/geofence).

The client doesn't have to be Android. You could use any of the [Flow SDKs](http://flow.imgtec.com/developers/develop). For example you could make a web-based client using the [Linux python SDK](http://flow.imgtec.com/developers/develop/desktop/linux/python-sdk) and Django. Using Vagrant will allow you to create an environment for using both the python and C SDKs on linux, instructions for setting this up can be found [here](http://flow.imgtec.com/developers/develop/desktop/linux/setup)


# Android application demo 

The final application looks like this.

<img src="/flow-on-arduino/images/demo.jpg" width="350"></img>

The blue circle is the current geofence, the purple marker is the wifire and the blue marker is the location of the phone running the Android app. The top-right box shows the information from the latest reading and a line of the location history is shown. Along the left side the controls allow the logging period and geofence to be configured. The individual waypoints can be toggled on and the history can be replayed. The location can be updated manually, and also updates every 30s.  

### The Geofence
<img src="/flow-on-arduino/images/set_fence.png" width="350"></img>

<video src="/flow-on-arduino/images/fence_a_4.webm" width="350" controls></video>

### Test walk

In order to test the application in a larger area I connected the WiFire and Bluetooth adapter to a battery.

<img src="/flow-on-arduino/images/shoebox.jpg" width="600"></img>

I then took this, along with the android phone and GPS data logger for a walk.
You can see a x5 speed video of this walk below.

At `0:35` the phone lost its connection, but I could reconnect it and it was able to recover by fetching the current status back from the WiFire and from the datastore.

<video src="/flow-on-arduino/images/walkdemo_1_3gcutout.webm" width="350" controls></video>

<br><br>
### Path waypoints

Points go from red to green based on their age with red being the oldest and green being the newest.
The line uses a spline between waypoints rather than straight segments for a more natural look.

<img src="/flow-on-arduino/images/path_1.png" width="350"></img>

<img src="/flow-on-arduino/images/path_2.png" width="350"></img>


## Conclusion

That's it. I had lots of fun making this project and enjoyed experimenting with the features provided by FlowCloud. There are lots of ways to extend this demonstration, really only limited by your imagination. 

When developing your own projects don't forget the FlowCloud Developer website has lots of information to help if you get stuck when using FlowCloud, and the [FlowCloud developer Forum](http://forum.imgtec.com/categories/flow-developers) is a great way to get help from the community and to share your ideas.

Thanks very much for reading and best of luck with your own projects using FlowCloud on Arduino!
