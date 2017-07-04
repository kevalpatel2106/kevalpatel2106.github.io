---
layout: post
section-type: post
title: Measure the core temperature of your Raspberry Pi.
category: Raspberry Pi
image : https://kevalpatel2106.github.io/img/blog/monitor-core-tepmrature/image1.jpg
---

Raspberry Pi is a pretty powerful device. That is why people are using their raspberry pi for performing some intensive tasks that squeeze last drop of CPU power from the Raspberry Pi. You can also overclock the Raspberry Pi to make it more powerful. 

Raspberry Pi consumes very less power compare to your desktop CPU. But just like every other computer, while performing heavy tasks, it also gets hot.

### Why do you need to monitor core temperature?

Many times, you want to keep an eye on the core temperature. You want to constantly measure the core temperature of your raspberry pi. In my last project, I was building Tensorflow on my raspberry pi. It was long running CPU intensive task. I also overclocked my raspberry pi to run on 1300 MHz. So, it was going to fry my CPU. (And nobody wants to do that with their little Pi.) I was looking for a way to measure core temperature constantly. 

### What happens if your Raspberry Pi gets too hot?

- Raspberry Pi is a low-end mobile computer. So it doesn't have fans to cool it down like other desktop/laptop CPUs. 

- If your temperature rises above 80°C, you will see a little thermometer on you Raspbian desktop. That indicates that your Pi is getting hot. 

![1*8ITi0D6JrpibvAC9iTG2rA.png](https://kevalpatel2106.github.io/img/blog/monitor-core-tepmrature/image2.png)

- As the core temperature rises, the thermometer gets to fill. Then at 85°C, it changes to a full thermometer. However, that is the maximum recommended operating temperature. After this temperature, your CPU starts throttling and reduces the clock to cool down the temperature. This will decrease the performance.

So, I decided to write a small python script to monitor core temperature. Let's get started.

### How can you measure the core temperature?

You can measure the core temperature by issuing following command in your Raspberry Pi terminal.

<script src="https://gist.github.com/kevalpatel2106/dd1cca23c04a9cf0ccebc8e485a94566.js"></script>

It will give you core temperature in Celcius.

<script src="https://gist.github.com/kevalpatel2106/bb4ab988179850f6d530b5137143e381.js"></script>

Great. Let's write a python script to monitor and print the temperature every second.

### Constantly monitor the core temperature.

- Login to your Raspberry Pi terminal by SSH or using VNC.

- Create new python file monitor-temp.py by issuing following command.

<script src="https://gist.github.com/kevalpatel2106/b74e01b1a54dad005323e7126b366668.js"></script>

- Write down below code in that file. This script issues the command <prev>/opt/vc/bin/vcgencmd measure_temp</prev> every second and print the formatted temperature in the console.

<script src="https://gist.github.com/kevalpatel2106/ac79e08e6362e246e757895d0e9aa1f6.js"></script>

- Press Ctl+X and then Y to save that file.

That's it. Now run the python script by issuing below command: 

<script src="https://gist.github.com/kevalpatel2106/4952e7257e2fa73c81c200f0fa4c8509.js"></script>

Volla!!! You will see updated core temperature every second. You can stop the script anytime by issuing Ctl+C in the terminal.

### Conclusion: 
So, now you can measure and monitor the temperature. You can easily modify the script to play a buzzer or start a cooling fan if the core temperature is more than the particular limit. 