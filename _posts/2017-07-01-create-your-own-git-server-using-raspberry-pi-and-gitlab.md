---
layout: post
section-type: post
title: Create your own Git server using Raspberry Pi and GitLab.
category: Raspberry Pi
image : https://kevalpatel2106.github.io/img/blog/local-gitlab-server/image1.jpg
---

If you are a software developer or you are linked to software development you might know what is **Git** and **GitHub**.

For those who don’t know what is GitHub, GitHub is a web service based on the software Git (which is a versioning software written by Linus Torvald, creator of Linux). Git allows you to store the history of your software code base and easily control versioning. GitHub host all those Git repositories and also provides many other features like bug tracking, milestone creation and also the creation of Wiki for your project.

In this article you will learn how to host your own git server like GitHub on your raspberry pi using GitLab.

### Why do your want to host your local Git server?

GitHub is pretty amaizing. It may satisfy all your code hosting requirements. But there are three main reasons to host your own git server.

- GitHub is free only for the public projects only. If you don't want to make your software open source than you have to buy the GitHub subscription plans. Now you mmay making project for hobby or you have a start up company with relatively small team. In that case of you don't want to buy those expensive subscription plans, you have to host your git server.

- If you want to use GitHub or any other git servers, you have to trust the third party for hosting your data. If you want to keep your data/source code secure and also want to control over where your data is beign host, you have to host a local Git server.

- It's really fun. Really!!!

The easyiest and the cheapest way to create these type of Git server is by using the most versetile and cheap Raspberry Pi. 

------
### What do you need?

- A Raspberry Pi 3. (Raspberry Pi 3 is recommended as it has 64 bit CPU and GitLab requires a 64-bit architecture.)

- Power supply to feed your raspberry pi.

- 16 or 32GB microSD card to store the Git reposetory. (Make you have the latest version of Rasbpian Jessie flashed on the SD card and your SSH is enabled.)

-----

### How to install GitLab on Pi?

Installing GitLab on the Raspberry Pi is very simple. It's just 4 simple steps and that's it! You are all done. Let's get started. First, login to your raspberry pi terminal using SSH and flow below steps.

#### 1. Install required dependencies.

- Run the fllowing command in your SSH terminal to install and configure the necessary dependencies. 

<script src="https://gist.github.com/kevalpatel2106/a2de7305dde77ff35756095a79dc7853.js"></script>

This command will install postfix on your raspberry pi. If you want to send emails from your GitLab server please select 'Internet Site' during setup.

- Once it is done, run below command to add GPG keys. This will allow us ti easily update our GitLab server.

<script src="https://gist.github.com/kevalpatel2106/67be31ad43dc0427df422f5ed1e9c5c2.js"></script>

#### 2. Install GitLab CE (Community Edition) server.

- Run below command one by one to download and install GitLab. This may take time based on your internet speed.

<script src="https://gist.github.com/kevalpatel2106/b3f7eb2e36b6ab1530674c6e8f93283b.js"></script>

#### 3. Configure and start GitLab.

- Run below command to configure your gitlab server for the first time. This command will take couple of minutes to execute.

<script src="https://gist.github.com/kevalpatel2106/438fd62067035df153f754c0d37a0258.js"></script>

That's it. Congratulations!!! You sucessfully installed GitLab server on your Raspberry Pi. Now one final step is remaining.

#### 4. Set up your GitLab account.

- Get the IP of your raspberry pi using ifconfig command.
- Now in your computer, go to the browser. Open the IP you get from the previous step in your browser. Volla!!! You will have the GitLab server page on your computer screen right from the tiny raspberry pi.

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/local-gitlab-server/image2.png)

- On your first visit, you'll be redirected to a password reset screen to provide the password for the initial administrator account. Enter your desired password and you'll be redirected back to the login screen. Keep in mind that your default account's username is root.
- Login with your username (that is root bydefault) and the password you set in the previous step. You will have the home page of your GitLab server, where all your future projects will be listed.

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/local-gitlab-server/image3.png)

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Finally!!👾 Running <a href="https://twitter.com/gitlab">@gitlab</a> CE locally on my <a href="https://twitter.com/hashtag/RaspberryPi3?src=hash">#RaspberryPi3</a>. <a href="https://twitter.com/hashtag/Git?src=hash">#Git</a> <a href="https://twitter.com/hashtag/RaspberryPi?src=hash">#RaspberryPi</a> <a href="https://twitter.com/hashtag/LearnEveryDay?src=hash">#LearnEveryDay</a> <a href="https://t.co/SB4SO39Xe8">pic.twitter.com/SB4SO39Xe8</a></p>&mdash; Keval Patel (@kevalpatel2106) <a href="https://twitter.com/kevalpatel2106/status/881063654359093248">July 1, 2017</a></blockquote> 
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

------
### Conclusion:

So, now you learned how you can host your own Git server at your home. This will cost you less than $50. But,it will provide you complete controll over your codebase. 

Later if you want to migrate to any of online git server, you can easily change your Git hosting by easily adding a remote pointing to the online reposetory.

*If you liked the article, share it so more people can see it! Also, you can follow me on [Medium](https://medium.com/@kevalpatel2106) or on [Twitter](https://twitter.com/kevalpatel2106), so you get updates regarding my future articles!!*



