title: Continuous Integration and Code Coverage in Android using Travis CI
link: https://kevalpatel2106.wordpress.com/2017/01/23/continuous-integration-and-code-coverage-in-android-using-travis-ci/
author: kevalpatel2106
description: 
post_id: 280
created: 2017/01/23 22:42:48
created_gmt: 2017/01/23 17:12:48
comment_status: open
post_name: continuous-integration-and-code-coverage-in-android-using-travis-ci
status: publish
post_type: post

# Continuous Integration and Code Coverage in Android using Travis CI

Testing has always been a trending topic in the world of Android. It becomes a time consuming task in large android projects where multiple developers are working together, maintaining the code quality and resolving bugs; which is a very difficult task.

This is a very serious issue in the open source projects where multiple developers from different environment and different level of expertise contribute to a single code base. Everyone is not the perfect and someone can commit changes that break the application code and we should have something that can alert us if someone is doing something wrong as soon as possible.

It would be great if we can find out that whether the code we are going to push onto our master branch of repository contains any possible bugs that can break the system. Here, continuous integration comes into play.

* * *

## What is Continuous Integration?

> Continuous Integration (CI) is a development practice that requires developers to integrate code into a shared repository several times a day. Each check-in is then verified by an automated build, allowing teams to detect problems early.

In simple words, each time you commit and push your code to the remote repository, continuous integration will check if everything is working correctly in the code before you merge it into your master (assuming you are using git for version control).

With continuous integration, you can:

  *  Automated build process. Every commit/push to a git repo will automatically trigger a new build.
  * Find out Android Lint errors.
  * Coding styles to ensure that the new code is matching the code format that your team has decided.
  * Get the code coverage percentage. This will be really helpful to figure out if any part of the code is remaining to test.
  * Run Unit tests located on your /test directory of project.
  * Run Instrumentation tests located in your /androidTest project.

* * *

## How to configure Continuous Integration?

![](https://kevalpatel2106.files.wordpress.com/2017/01/81b6b-1ity49flzepw8sjv-t0xk-q.gif)As the title of this post suggests, here I am going to use [Travis CI](https://travis-ci.org/). You can use other services available to the market like CircleCI, Codeship, etc. But here, I will stick to Travis CI as it is most used CI service for GitHub projects and also it is free for Open Source projects.

### Prerequisites:

  * An account on GitHub. I believe nowadays every developer has it.
  * An account in Travis CI. If you don’t have it yet you can create it from [here](https://travis-ci.org/).

Let’s start with the integration. For the demo purpose, I am going to use API 25 as the targeted Android version in our Gradle and we are going to test our build on API 21 and API 19.

> We have skipped the description of some lines as we don’t want to make this article too long and boring. We will concentrate more on the lines which are more important and difficult to understand.

### **Step-1 :** Creating .yml file

First, create a .travis.yml file into your project’s root directory. This file will contain the configuration script for the automated build testing.

![1fshca7ehyvosixq8sczwca](https://kevalpatel2106.files.wordpress.com/2017/01/1fshca7ehyvosixq8sczwca.png)

### Step-2 : Basic setup:

[gist]9fcbcf4f55d6cbf39da47bcc80353303[/gist]

Add above lines at the starting of the file. This basically tells Travis that this repository is an android application source code and it has to use Java 8 for compilation. The notification section will provide email notifications on the email you provided whenever a build gets completed successfully or fails due to some error.

### **Step-3 : Defining android targets:**

[gist]4b816a9f3d0784d52df075d6cb0c52b2[/gist] Next, add above lines to your .travis.yml file. The line under the matrix section defines the Android API version and the processor architecture, on which the code will be tested. These lines re-executes the script for each variable. So if I write another line with: “_\- ANDROID_TARGET=android-16 ANDROID_ABI=armeabi-v7a_”, it will execute the whole script with the 21, 19 and 16 APIs. We can say it is really useful to test different API levels. So, each commit you push to the repository will start two separate builds and test prepossess. You can see this in Travis dashboard like below. ![1xrxjad5hawop4pgcypscpa](https://kevalpatel2106.files.wordpress.com/2017/01/1xrxjad5hawop4pgcypscpa.png)