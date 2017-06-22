---
layout: post
section-type: post
title: How you can decrease application size by 60% (In only 5 minutes)?
category: Android
tags: [ 'android', 'application size', 'optimization']
image: https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image.png 
---

# How you can decrease application size by 60% (In only 5 minutes)?

Mobile devices always have limited resources. They have a limited amount of battery, limited storage, limited processing power, limited RAM, limited internet connectivity … and the list goes on. This doesn’t matter if you are targeting Android or iOS. This is the universal truth.

In past couple of months, I was developing an android application “[Anti-Theft Screen Lock](https://play.google.com/store/apps/details?id=com.antitheftlock)”. This application capture image using device’s front camera, play alert whenever someone entered the wrong password to unlock the device. If you want to know more here is the play store page, go ahead and read the description:

Here, I am going to tell you about some simple the tweaks and tricks I applied to decrease the application size. These tricks are very simple and easy to apply. These may help you in your current/future applications too.

* * *

## Smaller is always batter:

As a developer we always care about the application performance, design and user experience the most. But, most of the developer forget (or underestimate) one thing: Application Size. This is one of the key factors if you want to target next billion users with your application.

There are more than 11000 android powered devices available on the market and most of them are having low-end configurations, limited storage capacity (1GB to 8GB of internal storage) and even 2G or 3G connectivity. These devices are having major market share in developing countries like India, Bazil or countries in Africa etc, where you can find your next billion users.

Keeping your application size optimum becomes very important. _The less your application uses storage, the more user gets more free space to store their videos and images._ Let’s be honest, you don’t want your user to uninstall your application because of “Storage out of space” notification.

![1*cjMsR_IEniBq3SXQ3YtJ6g.jpeg](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image1.jpeg)

These developing countries are also having the limited 2G/3G connectivity. So, if your application is large in size, it will take the larger time to download the application (And more chances that user won’t download it in the first place). Also, most of the users are having a limited amount of internet data. Every byte user uses to download the application, will affect your user’s wallet.

So, it is clear that in the world of mobile application,

> Smaller is always better.

* * *

## Breakdown your APK using APK Analyser

Android studio provides a handful tool: [APK Analyser](https://developer.android.com/studio/build/apk-analyzer.html). APK Analyser will tear down your application and let you know which component in your .apk file is taking the larger amount of space? Let’s have a look in Anti-Theft screen lock apk file without any optimization applied.

![1*qwluezWR7KE9-raJVkAc9A.png](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image2.png)

From the apk analyzer output, you can see that the application raw size is about 3.1MB. After applying play store compressions, the application size is roughly 2.5 MB.

As you can see from the screenshot, there are main 3 folders that are consuming most of the application size.

  * **classes.dex** — This is the dex file which contains all the bytecode files of your java code that will run on your DVM or ART.
  * **res** — This folder includes all the files under your res folder. Most of the time this will contain all the images, icons and raw files, menu files, and layouts.
![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image3.png)

  * **resources.arsc** — This file holds all the value resources. This file contains all the data you have under your different value folders. This resource contains strings, dimensions, styles, integers, ids etc.
![1*B1MMigEQSVfKIJRmujeIag](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image4.png)

* * *

So, now you know what an APK is made of. Let’s see, how can we decrease the application size by optimising one by one component.

## Reduce classes.dex:

As we discussed, this contains all the java code. While you build your application, gradle will combine all of your .class files from all the modules and convert those .class files to .dex files and combine them in single classes.dex file.

> If you are curious, how the compilation process works, visit my another blog post : [The Jack and Jill: Should you use in your next Android Application?](https://blog.mindorks.com/the-jack-and-jill-should-you-use-in-your-next-android-application-ce7d0b0309b7#.gq31gtrdj)

Single classes.dex file can accommodate, approximately 64K methods. If you exceed this limit, you have to enable [multidexing](https://developer.android.com/studio/build/multidex.html) in your project. Which will create another classes1.dex file to include all remaining methods. Thus the number of classes.dex file depends on the number of methods you have.

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image5.png)

As you can see currently “Anti-Theft Screen Lock” contains 4392 classes and 29897 methods. This result is without applying proguard. You have two default proguard file you can apply.

- proguard-android-optimize.txt
- proguard-android.txt

As the name suggests “proguard-android-optimize.txt” is the more aggressive progurard configuration.
We used this as the default proguard configuration. You can add you custom proguard configurations in `proguard-rules.pro` file in your `/app` directory.

<script src="https://gist.github.com/kevalpatel2106/b414528c6480508cdb77c82a962762cb.js"></script>

By setting minifyEnabled to true, yo are telling proguard to remove all the unused methods, instructions and slim down the classes.dex file.

Here is the result from minify enabled APK,

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image6.png)

As you can see by enabling the proguard in every module of our project we can we are able to reduce the classes.dex file size almost by 50%. Also, you can see method count decreased from 29897 to 15168 (almost 50%). Hurray…!!!

> Size decreased from 3.1 MB to 1.98MB. (~50% decrease)

* * *

## Reduce res:

The second largest component in your apk size is your res folder, which contains all the images, raw files, and XML. You cannot add/remove/modify your XML, as they are containing your layouts. But we can decrease the image files.

- **“shrinkResources”** attribute will remove all the resources, those are not used anywhere in the project. Enable this in your build.gradle file by adding below line:
<script src="https://gist.github.com/kevalpatel2106/8503bd3a0982b33cc6bca138e21b6291.js"></script>

- **“resConfigs”** attribute will remove all the other localized resources while building the application. In our case, “Anti-Theft Screen Lock” only supports the English language. While all the support libraries may have localized folders for other languages. Which we don’t need. So, add following line to allow only English resources to be added in APK file.
<script src="https://gist.github.com/kevalpatel2106/c121eb77f57e7b5fb328d42d9945d9eb.js"></script>

- **Android Studio 2.3**, and you application minimum version is 18 or above, you should use webp instead of png. webp images are having less size than png files and also it retains the original image quality. More to that webp images are natively supported in Android. So you can load webp images in ImageView same as other raster images. So, you don’t have to change your layouts. You can select drawable and mipmap folders from you project, right click and select convert to webp. This will open below configuration dialog.

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image7.png)

Press ok and it will convert all the png images to webp format one-by-one. If the webp image is having the larger size than png, Android Studio will automatically skip that file.

Let’s see the final result:

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/decrease-apk-size/image8.ong)

Volla!!! res folder size decrease from 710KB to 597KB.

> Size decrease by 105KB. (Decrease by 16%)
> You can also convert the images to vector drawable. But for that, you have to make some changes to make it backward compatible. If you want to learn more on vectors see [this great post by Chris Banes](https://medium.com/@chrisbanes/appcompat-v23-2-age-of-the-vectors-91cbafa87c88#.ust6pssbk).

* * *

## TL;DR:

- Enable **proguard** in your project to your release build type.
- Enable **shrinkResources**.
- Strip down all the unused locale resources by adding required resources name in **“resConfigs”**.
- **Convert all the images** to the webp or vector drawables.
	
## Conclusion:

By applying above simple tricks the application size decreases from 3.19 MB to 1.89 MB.

These are the most simple tricks. There are many other tweaks that can reduce your application size. But, you should always apply above simple tricks to your android applications to make sure you reduce the application size as much as you can.

For more tips and tricks you can read this from Android Developers website.

> Remember: Smaller is Alway Better. 
