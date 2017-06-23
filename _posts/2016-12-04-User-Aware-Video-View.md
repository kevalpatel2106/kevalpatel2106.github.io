---
layout: post
section-type: post
title: UserAwareVideoView
category: Open Source
image : https://kevalpatel2106.github.io/img/blog/user-aware-video-view/image.png
github : https://github.com/kevalpatel2106/UserAwareVideoView
---

# User Aware Video View

These days all new smartphones are packed with lots of sensors and most of the smartphone contains the front (selfie) camera. So, why don’t we make use of them to improve the user experience? While watching the video on your device is the most important feature for many social sharing, messaging and media applications. **UserAwareVideoView is a customized video viewer that smartly play and pause the video based on your user is looking at the video or not.** If the user is not looking at the screen then this will automatically pause the video, so your user does not miss any part of the video. UserAwareVideoView inherits the android framework’s default [VideoView](https://developer.android.com/reference/android/widget/VideoView.html)class. So, it has all the features that video viewer has. More than that it makes it easy to convert your all the default video views to UserAwareVideoView in your existing application. 

### How does it work?

At its core UserAwareVideoView uses Google Play Service Mobile Vision API for the image processing. It uses front camera of the device to track user’s eyes.

When _VideoView.start()_ is called, it starts the eye tracking algorithm and starts tracking the user’s eyes. It also starts monitoring surrounding light intensity and if the light intensity is not enough for processing camera output, it stops tracking the user’s eyes.

If the user is not looking at the device screen, UserAwareVideoView will pause the video and it will resume the video as soon as the user starts looking at the screen. Pretty amazing huh!!!

Whenever _VideoView.stopPlayback()_ or _VideoView.pause()_ is called, this will UserAwareVideoView will release the camera and stop eye tracking to consume the battery.

### How to integrate?

To start with the integration, first provide the Gradle dependency of the library by entering below lines in your module level _build.gradle_.

<script src="https://gist.github.com/kevalpatel2106/f196a5afc77f7128947a00eef8ddeb2e.js"></script>

> This library automatically adds `android.permission.CAMERA` permission in your applications AndroidManifest.xml file.

Add the UserAwareVideoView in you xml layout of your activity or fragment. You can use UserAwareVideoView just like you use the default VideoView in your layout.

<script src="https://gist.github.com/kevalpatel2106/9ca7ac07409b6b2cc6725544538556fb.js"></script>

Initialize the UserAwareVideoView in your activity/fragment by following three steps:

  * **Step-1:** Check for the Camera permission in run-time.
  * **Step-2:** Register `UserAwarenessListener` to get the callbacks from the `UserAwareVideoView` whenever error occurs in eyes detection or eye detection starts stops.
  * **Step-3:** Handle errors that may occur while eyes detection. There are main 4 errors. You can detect those errors by error code. Below is the snippet showing how you can handle different errors.

<script src="https://gist.github.com/kevalpatel2106/e96c22d61812f161fb8c5b204e62f4f9.js"></script>

That’s it. You are ready with UserAwareVideoView. 

### Conclusion:

By using the UserAwareVideoView, you can make your application smart that responds to the user’s activity and uses the phone’s sensors to control the video playback so that your user don’t miss any part of the video.