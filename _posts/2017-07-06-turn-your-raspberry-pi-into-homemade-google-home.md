---
layout: post
section-type: post
title: Turn your Raspberry Pi into homemade Google Home
category: Raspberry Pi
image : https://kevalpatel2106.github.io/img/blog/homemade-google-home/home_banner.jpg
---
[Google Home](https://madeby.google.com/intl/en_us/home/) is a beautiful device with built-in [Google Assistant](https://assistant.google.com/) - A state of the art digital personal assistant by Google -. which you can place anywhere at your home and it will do some amazing things for you. It will save your reminders, shopping lists, notes and most importantly answers your questions and queries based on the context of the conversations. 

In this article, you are going to learn to turn your Raspberry Pi into homemade Google Home device which is,

- Powered by Google Assistant.

- Voice activated. No need to press any button, just say "Ok Google" or "Hey Google" and ask your question.

- There will be a LED indicator which will stay on whenever the conversation between the user and the Google Assistant it in progress. 

- It can initialize on boot so no need to login and run the script from terminal after reboots. 

So, let's get started.

* * *

### What things will you need?

- Raspberry Pi model 2 or 3.

- MicroSD card with Raspbian on it (Minimum 8GB recommended). 

- Power supply to feed your raspberry pi. (Any USB mobile charger with minimum 5V, 2A output will work.)

- USB mic (As Raspberry Pi doesn't have an inbuilt mic. I used MI-305).  

- A speaker.

- A LED.

- A couple of wires to connect LED.

Once you have all these things, login to Raspbian desktop and go to the following steps one by one.

* * *

### Step -1: Setting up USB mic.

- Raspberry Pi doesn't have inbuilt microphones. If you want to record audio, you need to attach a USB microphone. 

- Plug your USB mic into any of the USB slots of your Raspberry Pi.

- Go to the terminal and type following command.

<script src="https://gist.github.com/kevalpatel2106/abbfce002c4436ac0309ea8908aae1c8.js"></script>

- This command will list all the available audio record devices. You should get below output.

<script src="https://gist.github.com/kevalpatel2106/b0015076e96ac806d683a3e43875c111.js"></script>

As you can see your USB device is attached to card 1 and the device id is 0. Raspberry Pi recognizes card 0 as the internal sound card (which is bcm2835) and other external sound cards as external sound cards.

- Now, let's change the audio configs. Type below command to edit the asound.conf file.

<script src="https://gist.github.com/kevalpatel2106/38241a69c81cf33acdfc43996dcd06ac.js"></script>

- Add below lines in the file. Then press Ctrl+X and after that Y to save the file.

<script src="https://gist.github.com/kevalpatel2106/a0b6e792cc59e059d17584568ed7636c.js"></script>

This will set your external mic (see pcm.mic) as the audio capture device (see in pcm!.default) and your inbuilt sound card (card 0) as the speaker device. 

- Create a new file named .asoundrc in the home directory (/home/pi) by issuing following command.

<script src="https://gist.github.com/kevalpatel2106/d8be72f1e642269fd19737d5c08cc1dd.js"></script>

Paste above confugrations to this file.

### Step -2:  Setting up your speaker output.

- Connect your speaker to 3.5mm headphone jack of the Raspberry Pi.

- Run below command to open raspberry pi configuration screen.

<script src="https://gist.github.com/kevalpatel2106/97f90407eab4ff161135f8734a6f906a.js"></script>

- Go to Advanced Options > Audio and select the desired output device.

### Step -3: Test the mic and speakers.

- To test your speaker run below command in the terminal. This will play a test sound. Press Ctrl+C when done. If you are not able to hear the test sound check your speaker connection.

<script src="https://gist.github.com/kevalpatel2106/d5799f261573cf62bdf5bc7014292962.js"></script>

- To test your mic run following command. This will record a short audio clip. If you get any error check step 1 again.

<script src="https://gist.github.com/kevalpatel2106/6072d1e6ae1b1e5a69573c557e21a9b8.js"></script>

- Play the recorded audio and confirm everything works correctly by issuing following command.

<script src="https://gist.github.com/kevalpatel2106/4fc3be14fb57365f79437b8f16d84305.js"></script>

Okay. Our hardware is set.

### Step -4: Download required packages and configure Python environment:

- First, update your operating system.

<script src="https://gist.github.com/kevalpatel2106/9692e0dc545466dcdce2f82174936fbf.js"></script>

- Run below command one by one in the terminal. 

<script src="https://gist.github.com/kevalpatel2106/fafb148b483dba7ce9f5309ca0af16d0.js"></script>

This will create Python 3 environment (As the Google Assistant library runs on Python 3.x only) in your raspberry pi and install required dependencies.

- Activate the python environment.

<script src="https://gist.github.com/kevalpatel2106/6e405e137b12893ab977e20332572a8b.js"></script>

- Now, install the Google Assistant SDK package, which contains all the code required to get the Google Assistant running on the Raspberry Pi.

<script src="https://gist.github.com/kevalpatel2106/adeffe0bb2435feca7802080d82dbc93.js"></script>

It should download the Google Assistant Library and the demo.

### Step -5: Enabling the Google Assistant cloud project.

- Open the Google Cloud Console (https://console.cloud.google.com/) and create a new project. (You can name it whatever you want.) The account with which you sign in will be used to send queries to Google Assistant and get your personalized response.

![homemade-google-home](https://kevalpatel2106.github.io/img/blog/homemade-google-home/image1.png)

- Head over to API manager(https://console.cloud.google.com/apis/api/embeddedassistant.googleapis.com/overview) and enable the Google Assistant API.

- Make sure that you enable Web & App Activity, Device Information and Voice & Audio Activity in activity controls (https://myaccount.google.com/activitycontrols) for the account.

- Go to "[Credentials](https://console.cloud.google.com/apis/credentials)" and set up OAuth Content Screen.

![homemade-google-home](https://kevalpatel2106.github.io/img/blog/homemade-google-home/image2.png)

- Go to "Credentials" tab and Create new OAuth client ID.

![homemade-google-home](https://kevalpatel2106.github.io/img/blog/homemade-google-home/image3.png)

- Select application type as "Other" and give the name of the key.

![homemade-google-home](https://kevalpatel2106.github.io/img/blog/homemade-google-home/image4.png)

- Download the JSON file that stores the OAuth key information and keep it safe.

![homemade-google-home](https://kevalpatel2106.github.io/img/blog/homemade-google-home/image5.png)

### Step -6: Authenticating your Raspberry Pi.

- Install authorization tool by running below command.

<script src="https://gist.github.com/kevalpatel2106/f24adf3379af3837c2fc09c7d31be513.js"></script>

- Run the tool by running following command. Make sure you provide correct path for the JSON file you downloaded in step 5.

<script src="https://gist.github.com/kevalpatel2106/f43a009d7a50b519dd095fdebaafc70d.js"></script>

- It should display as shown below. Copy the URL and paste it into a browser (this can be done on your development machine, or any other machine). After you approve, a code will appear in your browser, such as "4/XXXX". Copy this and paste this code into the terminal.

<prev>
Please go to this URL: https://...
Enter the authorization code:
</prev>

If instead, it displays: <code>InvalidGrantError</code> then an invalid code was entered. Try again.

### Step -7: Setting up the LED indicator.

- Connect your LED between GPIO pin 25 and ground.

- The idea here is simple. We are going to set the GPIO pin 25 as the output pin. Google Assistant SDK provides a callback EventType.ON_CONVERSATION_TURN_STARTED when the conversion with the Google Assistant begins. At that point, we are going to set the GPIO 25 to glow the LED. Whenever the conversation terminates EventType.ON_CONVERSATION_TURN_FINISHED callback will be received. At that point, we will reset the GPIO 25 to turn off the LED.

![homemade-google-home](https://kevalpatel2106.github.io/img/blog/homemade-google-home/circuit-diagram.png)

### Step -8: Initialise on boot complete:

- Whenever your Raspberry Pi completes boot process, we will run a python script that will authenticate and initialize the Google Assistant on boot.

- Go to the user directory. Create new python file <code>main.py</code>.

<script src="https://gist.github.com/kevalpatel2106/ee36364d6de1e6c360fcdc7192e4882e.js"></script>

- Write following script and save the file.

<script src="https://gist.github.com/kevalpatel2106/44bbb610093e02641835d4a99e8e25cd.js"></script>
	
- Now create one shell script that will initialize and run the Google Assistant.

<script src="https://gist.github.com/kevalpatel2106/a7df14fc0143d9e159e73ae93a8481e8.js"></script>

- Paste below lines into the file and save the file.

<script src="https://gist.github.com/kevalpatel2106/b1cc1468618605629af196f170d26a1f.js"></script>

- Grant the execute permission.

<script src="https://gist.github.com/kevalpatel2106/74af4bd4d03a61209011824f492134b3.js"></script>

You can run google-assistant-init.sh to initiate the Google Assistant any time. Let's see how you can start the Google Assistant while booting.

- To enable Google Assistant on Boot there are two ways. Let's see each of them.

#### 1. Autostart with Pixel Desktop on Boot: 

- This will start the Google Assistant as soon as Pixel desktop boots up. Make sure you have "Desktop" boot selected in Raspberry Pi configurations.

- Type below command

<script src="https://gist.github.com/kevalpatel2106/81ce5aac8b15188eaad170eb72c46c9d.js"></script>

- Add the following after @xscreensaver -no-splash

<script src="https://gist.github.com/kevalpatel2106/626fa5af3cee752eb105b3cc57ba23b8.js"></script>

- Save and exit by pressing "Ctrl+X" and then "Y".

#### 2. Autostart with CLI on Boot:

- This will start the Google Assistant if you have set CLI boot. Make sure you have "CLI" boot selected in Raspberry Pi configurations.

- Type below command

<script src="https://gist.github.com/kevalpatel2106/a3fb98f5dbf0db844822859a73a897cb.js"></script>

- Add below line at the end of the file.

<script src="https://gist.github.com/kevalpatel2106/e21bd770f4c35d94abb4e8ebf582adf8.js"></script>

- Save and exit by pressing "Ctrl+X" and then "Y".

That's all!!! You "Homemade Google Home" is now ready. Reboot the device and ask your first question to your Google Assistant.

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">First question to my home made <a href="https://twitter.com/hashtag/googlehome?src=hash">#googlehome</a> .😂😂 <a href="https://twitter.com/hashtag/RaspberryPi?src=hash">#RaspberryPi</a> <a href="https://twitter.com/hashtag/LearnEveryday?src=hash">#LearnEveryday</a> <a href="https://twitter.com/hashtag/GoogleAssistant?src=hash">#GoogleAssistant</a> <a href="https://t.co/yTCwFYuhtG">pic.twitter.com/yTCwFYuhtG</a></p>&mdash; Keval Patel (@kevalpatel2106) <a href="https://twitter.com/kevalpatel2106/status/881518697533194240">July 2, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

* * *

### Conclusion:

You can do many daily stuf with your Google Home. If you want to perform your custom tasks like turning off the light, opening the door, you can do it with integrating [Google Actions](https://developers.google.com/actions/) in your Google Assistant. If you have any trouble with starting the Google Assistant, leave a comment below. I will try to resolve them.

*If you liked the article, share it so more people can see it! Also, you can follow me on [Medium](https://medium.com/@kevalpatel2106) or on [Twitter](https://twitter.com/kevalpatel2106), so you get updates regarding my future articles!!*

