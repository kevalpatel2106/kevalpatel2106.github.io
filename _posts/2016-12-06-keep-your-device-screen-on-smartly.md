---
layout: post
section-type: post
title: Keep your device screen on smartly!!!
category: Open Source
image : https://kevalpatel2106.github.io/img/blog/prevent-screen-off/image.gif
github : https://github.com/kevalpatel2106/Prevent-Screen-Off
---

# Keep your device screen on smartly!!!

Ideally, when you user is looking at the screen, your application should prevent the device screen from turning off. 

This is the huge deal for the blogging, messaging applications. These applications display textual content to their user. Reading those textual content takes more time to the user. While reading that content (let say an article) if the user does not interact with the display and the screen turns off because of the screen timeout that is frustrating to the user. It will destroy the concentration of the user and breaks the link.

If you are using Samsung devices you might know there is one feature called “Smart Stay”. When this feature is enabled, the device will prevent your screen from turning off regardless of your screen timeout settings. But, this technology is only available on some Samsung devices. What if we want to use that technology in our application to solve above problem? Today, I am going to present the solution of this problem.

* * *

### Here is the solution:

[**Prevent-Screen-Off**](https://github.com/kevalpatel2106/Prevent-Screen-Off) library handles screen on/off timing smartly. It prevents device display from turning off when the user is looking at the screen and he/she might be reading some textual content provided by your application. As soon as the user stop looking at the screen it will allow phone screen to turn off.

### How this works?

  * This library uses the front camera to sense when you are looking at your device
  * This uses [Google Play Services Mobile Vision API](https://developers.google.com/vision/) to track users eye using the device front camera. This uses the same algorithm to detect user’s eyes used in “**[UserAwareVideoView**](https://github.com/kevalpatel2106/UserAwareVideoView)”. _(I wrote a whole article regarding how UserAwareVideoView works. You can find it from __[here_](https://kevalpatel2106.github.io/open%20source/2016/12/04/User-Aware-Video-View.html)_.)_
  * It keeps the screen from turning off regardless of the screen timeout setting if the user is looking at the screen by detecting user’s eyes.

* * *

### Integrating the library in your application:

Let’s dive into some technical stuff and see how you can integrate this library in your application and use it.

**Add Gradle dependency:**

  * To start with the integration, first provide the Gradle dependency of the library by entering below lines in your module level _build.gradle_.

	<script src="https://gist.github.com/kevalpatel2106/547fa53bd14719574404f14e4d6b0892.js"></script>

> This library adds android.permission.CAMERA and android.permission.WAKE_LOCK permission in your applications AndroidManifest.xml file.

**Initialize in your activity:**

  * First, you need to inherit `AnalyserActivity` in the activity which you want to control screen on/off automatically. The library will synchronize with your activity life-cycle and start and stop eye tracking based on your activity state.
  * Library will start tracking user eyes as soon as activity comes into the foreground. If the user is looking at the screen this will prevent the screen from turning off.
  * When library detects that the user is not looking at the screen, this will turn off the screen after some time.
  * It will stop eye tracking when your activity goes into the background to preserve the battery.
  * Implement `ScreenListener` to receive the callbacks from the library.

	<script src="https://gist.github.com/kevalpatel2106/5868baeec08b84bf493183e1b6599b96.js"></script>

  * Handle the callbacks and errors received from `ScreenListener`.
	
	<script src="https://gist.github.com/kevalpatel2106/2dcff2e3d11890971114e23ebaf0e5be.js"></script>

  * That’s it. You successfully integrated the library. By the way, if you want to try our sample application before integration, you can find it from this [link](https://github.com/kevalpatel2106/Prevent-Screen-Off#demo).

### **This library won’t work if,**

  * When camera fails to initialize or your front camera is being used by another application. In that case, you will receive Errors.UNDEFINED error code in onErrorOccured event.
  * When the light source is behind the user or there is low light in the surrounding environment, so that eye tracking is not possible. In that case, you will receive Errors.LOW_LIGHT error code in onErrorOccured event. At this point, you can show a message to your user to go to the place where enough light available.
  * Google Play Service not available. In that case, you will receive Errors.PLAY_SERVICE_NOT_AVAILABLE error code in onErrorOccured event. In this case, the library will automatically display the dialog to install or update Google Play Services application from the play store, if the device supports Google Play Service.
  * If the device does not have the front camera. In that case, you will receive Errors.FRONT_CAMERA_NOT_AVAILABLE error code in onErrorOccured event. In this case, we are helpless. :-(

* * *

### Where can you use this feature?

  * Tracking user’s eye using device camera consumes more battery. So, it is advisable that you don’t integrate automatic screen control in every screen of your application.
  * You can integrate this features in your application activity, which has more textual content to read. An e.g. activity that shows chat conversation in messaging app or activity that displays full article in your blogging application. _(This list can be extended for may other use cases. Let me know if you have more ideas in comments.)_