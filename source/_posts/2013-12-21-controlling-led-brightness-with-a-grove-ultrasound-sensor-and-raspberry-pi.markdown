---
layout: post
title: "Controlling LED Brightness With a Grove Ultrasound Sensor and Raspberry Pi"
date: 2013-12-21 15:26:13 +0100
comments: true
categories: 
---

I recently started hacking my Raspberry Pi, and as someone who feels most at home in software, I was delighted to find myself controlling hardware components over an SSH connection and having the entirety of the web at my disposal. 

I decided to build a simple project that would use a Grove Ultrasound sensor I had bought. I decided on controlling the brightness of an LED by moving my hand up and down above the ultrasound sensor. To do this, I started with the [Grove Ultrasound Sensor python code](https://github.com/yexiaobo-seeedstudio/Grove-RaspberryPi "Grove Ultrasound sensor github") off Github as well as a [software PWM (pulse width modulation) library](http://marks-space.com/2013/09/23/software-pwm-on-a-raspberry-pi/ "software PWM (pulse width modulation) library") for Raspberry Pi GPIO pins by Mark Williams. 

{% img center /../images/raspberrypi_ultrasound_brightness.png 500 335 %}


### Getting started

For this project you will need:

- 1 Raspberry Pi
- 1 LED
- 1 100 Ohm resistor 
- 1 Grove Ultrasound Sensor
- 5 Male-to-Female Jumper Wires
- 1 Breadboard

### Step 1.

Build a simple LED circuit on a breadboard with the 100 Ohm resistor. <!--more--> Once you have done this, hook up the ground and power of the breadboard to the ground pin and GPIO pin 7 of the Raspberry Pi respectivley. The pins below are labeled using the orientation of the Raspberry Pi in the image above. 

{% img center /../images/raspberrypi_gpio_pins.png 500 335 %}

### Step 2.

Hook up the Grove Ultrasound sensor connectors with the following labels:

 - VCC to the 5V pin on the Raspberry Pi
 - GND to the ground line on the breadboard
 - SIG to Raspberry Pi GPIO 11  
 - NC remains unconnected

 {% img center /../images/grove_sensor.png 400 %}

### Step 3.

Now we need to install Mark Williams PWM software on our Raspberry Pi. 

{% codeblock lang:bash %}
pi@raspberrypi ~ $ git clone https://github.com/mwilliams03/pi-blaster.git
pi@raspberrypi ~ $ cd pi-blaster
pi@raspberrypi ~ $ make pi-blaster
{% endcodeblock %}

Once we've done this, we need to enable PWM on GPIO pin 7 in order to control the LED brightness. We do this by specifying pin 7 as an argument in the following call:

{% codeblock lang:bash %}
pi@raspberrypi ~ $ sudo ./pi-blaster/pi-blaster 7
{% endcodeblock %}

As Mark explains, this starts the pi blaster as a background job. If you ever want to stop it from running, you run the following:

{% codeblock lang:bash %}
pi@raspberrypi ~ $ sudo killall pi-blaster
{% endcodeblock %}

### Step 4

Now we need to grab the code I wrote based on the Grove Ultrasound Sensor python script. I've modified the script to adjust the LED brightness based on the distance measured by the sensor. The closer ones hand is, the dimmer the LED becomes. In order to control the brightness of the LED, we need to write to /dev/pi-blaster specifying the pin and the brightness between 0 (off) and 1 (full brightness). 

{% codeblock lang:bash %}
pi@raspberrypi ~ $ git clone git@github.com:fabioberger/raspberrypi-ultrasound-led-brightness.git
pi@raspberrypi ~ $ cd ./raspberrypi-ultrasound-led-brightness
pi@raspberrypi ~ $ sudo python main.py
{% endcodeblock %}

Given that the pi-blaster was setup in the previous step, this should begin the measurements by the ultrasound sensor. Move your hand up and down above the sensor and watch the LED brightness adjust!

I hope you enjoyed this walk through!

Questions? Comments? Always happy to connect.
