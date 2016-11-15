---
title: Getting Started
---


## Setup ##
The easiest way to program the bean is using the Arduino IDE and Bean Loader to send our Arduino sketches to the LightBlue Bean. Get those here: [Arduino](https://www.arduino.cc/en/Main/Software), [Bean Loader](https://punchthrough.com/bean/docs/guides/getting-started/intro/). You can also use the Bean Loader app for your mobile device, get that at the [App Store](https://itunes.apple.com/us/app/bean-loader-lightblue-bean/id936509473?mt=8) or the [Google Play Store](https://play.google.com/store/apps/details?id=com.punchthrough.bean.loader).


Once we get that set up we will need to link our Arduino App to the Bean Loader. This should be prompted automatically when you first open the Bean Loader, but if not, select "Associate" in the main menu or press &#8984; + A. Then, **navigate to the Arduino.app:**
![associate](/images/arduino_associate.png)



**Now you should be able to find the LightBlue Bean as an available board in the Arduino IDE:**
![associate](/images/choose_board.png)


## Loading an Arduino Sketch ##
There are some example programs already in the Arduino app, I like the `AccelerationLed` example! This program takes data from the Bean's 3-axis accelerometer and converts each to a RBG channel to display on the Bean's LED. ** Let's try it: **
1. Select it by navigation to `File -> Examples -> LightBlue-Bean -> Acceleration -> AccelerationLed`.
2. Now click the upload button at the top of the Arduino IDE, and it should compile the sketch and send it to the Bean Loader.
3. Now in Bean Loader, make sure your LightBlue Bean has its battery in and look for it in the available Bean window.
4. Click `Refresh` at the bottom-right of the window if it does not show up. Once it appears, right click it and select connect. * It may prompt you to update the Bean's firmware, in which case you should *
5. After it connects, check that the `AccelerationLed` appears in the bottom-left corner
6. Right click the Bean in the Bean Loader and select `Program Sketch` to send the sketch to the Arduino
7. After it uploads, move the Bean around and watch the colors change!


![beanloader](/images/beanloader.png)


#### Check out the [example project](/en/i/lunch.html) ####
