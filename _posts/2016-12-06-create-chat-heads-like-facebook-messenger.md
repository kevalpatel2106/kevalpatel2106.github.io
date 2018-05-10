---
layout: post
title: Create Chat Heads Like Facebook Messenger
category: Android
image: https://kevalpatel2106.github.io/img/blog/chat-heads/image.png 
---

# Create Chat Heads Like Facebook Messenger

We all love the chat heads (or chat bubbles) from the popular [Facebook Messenger](https://www.messenger.com/). This provides very handy and easy access to the chat conversation screen no matter on which screen you are. Chat heads are very convenient for multitasking as a user can work and chat at the same time. That means if you are using the calculator app on your phone and if any new message arrives, you can open the conversation and replay the message without switching app. Pretty cool!!!

In this tutorial, we are going to learn how to create simple chat head and allow user to drag them across the screen. So that user can adjust the position of the floating widget in the screen.

So, let’s get started.

* * *

### **Understanding the basics:**

  * Chat heads are nothing but a floating view that is drawn over other application. Android system allows applications to draw over other application if the application has [android.permission.SYSTEM_ALERT_WINDOW](https://developer.android.com/reference/android/Manifest.permission.html#SYSTEM_ALERT_WINDOW) permission. We are going to use the background service to add the floating widget into the view hierarchy of the current screen. So, this floating view is always on top of application windows.
  * To, drag the chat head across the screen we are going to override [OnTouchListener()](https://developer.android.com/reference/android/view/View.OnTouchListener.html) to listen to drag events and change the position of the chat has in the screen.

* * *

### **Implementation:**

**Step-1:**
Add _[android.permission.SYSTEM_ALERT_WINDOW](https://developer.android.com/reference/android/Manifest.permission.html#SYSTEM_ALERT_WINDOW)_ permission to the _AndroidManifest.xml_ file. This permission allows an app to create windows , shown on top of all other apps.

<script src="https://gist.github.com/kevalpatel2106/3d525fa2ff9c91892668e2e7f8bcaa0d.js"></script>

**Step-2:**
Create the layout of the chat head you want to display.

<script src="https://gist.github.com/kevalpatel2106/b82d39b961012bb520c765921fbc3e6e.js"></script>

#### **Step-3: Add music control widget to the window and handle dragging:**

  * Now create a service called ChatHeadService. Whenever you want to display a chat head, start the service using startService() command. In onCreate() of the service we will add the layout of the chat head at the top-left corner of the window.

	<script src="https://gist.github.com/kevalpatel2106/42340f32188ecbf677cb14ef7194f1a6.js"></script>

  * To drag the chat head along with the user’s touch, we have to override _[OnTouchListener()](https://developer.android.com/reference/android/view/View.OnTouchListener.html)_. Whenever the user touches the chat head, we will record the initial x and y coordinates, and when the user moves the finger, the application will calculate the new X and Y coordinate and move the chat head.

  * Also, implement click listener to close the chat head by stopping the service when the user clicks on the close icon at the top-right of the chat head.

	<script src="https://gist.github.com/kevalpatel2106/d75224c690acfa0b05327dede24d374f.js"></script>

  * So finally your _ChatService.java_ will look like below:

	<script src="https://gist.github.com/kevalpatel2106/0a2533181ae1f27ec502f5fcc8611326.js"></script>

#### **Step-4: Handle Overdraw permission:**

  * Now one final step is remaining. To add display chat head, you need to start the _ChatService.java_.
  * Before that, we need to check if the application has _android.permission.SYSTEM_ALERT_WINDOW_ permission or not? For android version <= API22, this permission is granted by default. But for the android versions running API>22 ,we need to check for the permission run-time. If the permission is not available, we will open permission management screen to allow the user to grant permission using _[Settings.ACTION_MANAGE_OVERLAY_PERMISSION](https://developer.android.com/reference/android/provider/Settings.html#ACTION_MANAGE_OVERLAY_PERMISSION)_ intent action. This will open below screen facilitate user to grant _android.permission.SYSTEM_ALERT_WINDOW_ permission.

	![permission](https://kevalpatel2106.github.io/img/blog/chat-heads/image2.png)

  * Here is the code snippet for the _MainActivity _that will display the chat head when the button is clicked by checking the _SYSTEM_ALERT_WINDOW_permission.

	<script src="https://gist.github.com/kevalpatel2106/ea66370ae2d1c53665f0fd0212b65367.js"></script>

* * *

That’s it. Now build and run the project to see the results.

Here is the sample of how our final application should look like. ![sample.gif](https://kevalpatel2106.github.io/img/blog/chat-heads/image3.gif)

Don’t worry if you have any problems while building the project. Full source code is also available on [GitHub](https://github.com/kevalpatel2106/android-samples/tree/master/Facebook%20Chat%20Heads). Go ahead, download and run it. Still, if you have any queries let me know in comments below or hit me on [Twitter](https://twitter.com/Kevalonly77).
