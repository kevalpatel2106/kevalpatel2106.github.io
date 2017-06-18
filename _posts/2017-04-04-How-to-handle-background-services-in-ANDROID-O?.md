---
layout: post
section-type: post
title: How to handle background services in ANDROID O?
category: Android
tags: [ 'android','android o']
image: https://kevalpatel2106.github.io/img/blog/android-o-background-service/image.png 
---

> Nothing makes an android developer more crazy than a new version of Android.

Google has just revealed the DP1 of the next iteration of android: [Android O](https://developer.android.com/preview/index.html). There are many new exciting features and under the hood performance improvements in the newest version of android.

While others talk about what will be the name of Android O, let’s analyze this flavor of android from developer’s perspective. For android developers there are **four groundbreaking** changes:

*   Background execution limits
*   Location updates limit
*   Removing of implicit broadcasts
*   Notification channels

Here, in this article let’s talk about background execution limitation. Background execution limitations mainly apply to two major components:

*   Service
*   Wakelocks

Let’s talk about the limitations applied on services in this article.

* * *

## What is the service in android?

Let’s first refresh what is the service in Android? As per the android [documentation](https://developer.android.com/guide/components/services.html):

> A [`Service`](https://developer.android.com/reference/android/app/Service.html) is an application component that can perform long-running operations in the background, and it does not provide a user interface.

So, fundamentally Service is the same thing as the activity but it doesn't have the UI component in it. So, it doesn't have to perform smooth animation at 60 fps. That’s why it can run perform any task for the longer period of time than the activity.

There are **three** types of the service:

*   **Started Service** — A service is _started_ when an application component (such as an activity) calls [`startService()`](https://developer.android.com/reference/android/content/Context.html#startService%28android.content.Intent%29).
*   **Bound Service** — A service is _bound_ when an application component binds to it by calling [`bindService()`](https://developer.android.com/reference/android/content/Context.html#bindService%28android.content.Intent,%20android.content.ServiceConnection,%20int%29).
*   **Scheduled Service** — A service is _scheduled_ when an API such as the [`JobScheduler`](https://developer.android.com/reference/android/app/job/JobScheduler.html).

* * *

## Background vs Foreground applications:

To learn background execution changes, we need to know the difference between background and foreground application first.

_Rule of thumb_, your application will be considered as a foreground service if any of the below three cases are true:

1.  Your application has currently visible activity.
2.  Your application has foreground service running.
3.  Your application is connected to another foreground app by binding the service or by consuming their content providers.

If any of the above scenarios is not true in the current instance, your application is considered to be in the background.

* * *

## Why do we need to restrict the use of background services?

Whenever your applications run in the background using services, your application consumes two precious resources: **1) Memory** and **2) Battery**.

These two are limited resources on the mobile devices and most of the low to mid-range devices doesn’t have plenty of memory or battery inside it.

Suppose, if your application is doing some very intensive tasks in the background and using the larger amount of RAM to perform that task, then this will create the very junky user experience, especially if the user is using another resource-intensive app, such as playing a game or watching a video in the foreground.

As per the documentation for the started service the best practice is,

> When the operation is complete, the service should stop itself.

But, many applications have long running background services, which basically runs for the infinite time to either maintain the socket connection with the server or monitor some tasks or user activity. These services create battery drain and also they constantly consume memory.

From past couple of releases of android (Starting from Marshmallow), Google is trying very hard to increase the battery life and reduce the memory consumption used by the applications by introducing the doze mode and app standby by delaying the background execution by some amount of time if the phone is ideal.

But most of the time despite of knowing the downsides of the long-running services developers still use them. (Mostly because it is easy to implement and maintain rather than using other workarounds.)

* * *

## What are the limitations on services starting from Android O?

Starting from Android O, if your application is in the background (check above three conditions), your application is allowed to create and run background services for some minutes.

After some minutes passed, your application will enter in the ideal stage. When your application enters in the ideal stage, the system will stop all the background services just like your service calls [_Service.stopSelf()_](https://developer.android.com/reference/android/app/Service.html#stopSelf%28%29).

### And here comes the fun part…

As I discussed above, the problem of battery drain and memory consumption are mainly caused by started services. To eliminate this, Android O completely prevents the use of _startService()_ method to start the service. If you call _startService()_ on Android O, you will end up getting _IllegalArgumentException_ 😲.

![image2.gif](https://kevalpatel2106.github.io/img/blog/android-o-background-service/image2.gif)

There are some exceptions in these scenarios when your application is whitelisted temporarily for some time window. During this period, your application can create background services freely. The application will put into temporary whitelist under below situations:

*   When high priority FCM message received
*   Receiving a broadcast.
*   Executing a PendingIntent from a notification.

* * *

## How can you run background tasks?

If you are building a very large android application, there might be some genuine scenarios where you need to perform some tasks in background. Since starting a service using _startService()_ command is not an option, we need to find out other ways to perform the tasks in background.

### Scheduling your tasks using Job Scheduler API:

*   [`JobScheduler`](https://developer.android.com/reference/android/app/job/JobScheduler.html) API introduced in API21 to perform background tasks.
*   This API allows you to run scheduled service and the android system will batch all the services from different applications and run them together in some particular timeframe. The reason behind this is to reduce the amount of time your phone’s CPU and radio wakes up by batching the tasks together. This will consume less battery and maintains system health.
*   What if your application has minSdkVersion < 21? In this situation, the official way to schedule the job is to use [`Firebase Job Dispatcher`](https://github.com/firebase/firebase-jobdispatcher-android). Firebase Job Dispatcher is supported to all the way down up to API9.

### Use foreground service:

If you want some long running tasks to be performed in the background, consider using foreground services for that. None of the above background execution limitations applies to the foreground services.

This will also keep your user aware that your application is performing some background tasks by displaying the ongoing notification. This will increase transparency with your user.

Before Android O, if you want to create foreground service, you usually start a background service by calling _startService()_. Then after you can promote your service to the foreground service by assigning an ongoing notification using [_startForeground()_](https://developer.android.com/reference/android/app/Service.html#startForeground%28int,%20android.app.Notification%29) method. But starting from Android O, you cannot use _startService()_ anymore. <span class="markup--quote markup--p-quote is-me">So to create foreground service you have to use [_NotificationManager.startServiceInForeground()_](https://developer.android.com/reference/android/app/NotificationManager.html#startServiceInForeground%28android.content.Intent,%20int,%20android.app.Notification%29). This method is equivalent to creating background service and promoting it to the foreground service combine.</span>

![image1](https://kevalpatel2106.github.io/img/blog/android-o-background-service/image1.gif)

* * *

## **Conclusion:**

These limitations applied to the background service will definitely provide extended battery life and also lower RAM usage. Ultimately it will make your applications smooth and your user happy.

### Should you make changes in your application code now?

Android O is still in DP1\. There are 3 more developer previews to be release before the final version of Android O gets released. There might be some API changes in upcoming releases. So, right now it’s time to think the effects of these changes in your application and think about the alternative solution for them. Once developer preview 3–4 gets released, apply those changes to your applications and make your application Android O compatible.