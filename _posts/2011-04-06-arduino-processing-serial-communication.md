---
title: Arduino-Processing Serial Communication
date: 2011-04-06
image:
categories: electronics
tags: arduino, featured
---

Over the last few weeks I’ve been messing around quite a bit with the [Processing](http://processing.org/) programming language. I was drawn to it initially because it is a close cousin to the [Arduino](http://arduino.cc/) language and programming environment (which is based on Processing), so naturally I wanted to find a way to combine the two in a project.

<div class="video">
<iframe src="http://player.vimeo.com/video/22055593?byline=0&amp;portrait=0" width="380" height="280" frameborder="0"></iframe>
</div>

## Background

What I had in mind, of course, was an outlandishly-complicated, full-screen Processing user interface, with dials and button and knobs and sliders and big maps and graphs of data readouts and a little videoport showing the view from the Kinect-based eyes of some hardy Arduino-powered robot as it trundled bravely into the caldera of a smoking volcano.

<!-- ## Rip and Burn Checklist

Already know your way around Processing and Arduino? Here you go.

1. Download and install [controlP5 GUI Library](http://www.sojamo.de/libraries/controlP5/#installation) for Processing.
2. Download [SliderSerial_Arduino](http://principialabs.com/wp-content/uploads/SliderSerial_Arduino.zip).
3. Download [SliderSerial_Processing](http://principialabs.com/wp-content/uploads/SliderSerial_Processing.zip). -->

But alas, I thought it would be better to start small — with one little graphical slider control, one Arduino, and one LED. The slider would command a brightness level for the LED, serial communication would do the talking, and pulse-width modulation (PWM) would do the legwork. In my quest for the ultimate ready-made user interface, however, I did stumble across the awesome-looking and fully-featured [controlP5 GUI library](http://www.sojamo.de/libraries/controlP5/) for Processing.

[![controlP5 GUI Library](https://web.archive.org/web/20121228185657im_/http://principialabs.com/wp-content/uploads/controlP5.jpg "controlP5 GUI Library")](http://principialabs.com/wp-content/uploads/controlP5.jpg)

## Arduino Setup

As you can see in the video, I used an [Adafruit Boarduino](http://www.adafruit.com/index.php?main_page=index&cPath=19) and a breadboard for this little experiment, and both are great!, but neither are necessary. You need to have your Arduino plugged into your PC’s USB port (or serial, if you’re old school), and you need to have an LED and an appropriate resistor plugged into Pin 9 (or any PWM pin) and GND on the board (pictured below). That’s it!

[![Arduino LED Setup](https://web.archive.org/web/20121228185657im_/http://principialabs.com/wp-content/uploads/arduino-led-setup.png "Arduino LED Setup")](http://principialabs.com/wp-content/uploads/arduino-led-setup.png)

## Software Setup

You’re going to be running both the Arduino and Processing development environments side-by-side, so be careful not to confuse one for the other. Open the `SliderSerial_Arduino` sketch (download it below) in the Arduino environment, and open `SliderSerial_Processing` in Processing.

[![Arduino and Processing](https://web.archive.org/web/20121228185657im_/http://principialabs.com/wp-content/uploads/screenshot-1-880-550x375.png "Arduino and Processing")](http://principialabs.com/wp-content/uploads/screenshot-1-880.png)

If you don’t already have the controlP5 GUI library for Processing, download that as well and install it in your Processing `Libraries` folder (should be in the same directory where all your sketches are saved — see below for help installing libraries).

Upload the Arduino sketch to your Arduino board, then run the Processing sketch. A small window should open, revealing the slider control, which should control the brightness of the LED.

## Serial Communication

If the Processing sketch doesn’t work the first time, you may need to make a small adjustment to properly locate the port to which your Arduino is connected. In the Processing sketch, look for this code block and follow the instructions in the comments:

```arduino
// =======================================================
// SERIAL COMMUNICATION SETUP:
// =======================================================

// CHANGE THE FOLLOWING VARIABLE to match the port
// to which your Arduino is connected.

// SEE THE LIST of available ports in the black debugging
// section at the bottom of the Processing window. (It will
// appear after the first time you run the sketch.

// THE LIST LOOKS LIKE THIS on Windows:
// \[0\] "COM1"
// \[1\] "COM3"
// \[2\] etc...
// or like this on a Mac:
// \[0\] "/dev/tty.usbserial-somenumbers"
// \[1\] "/dev/tty.usbserial-othernumbers"
// \[3\] etc...

// TYPE THE NUMBER (inside the brackets) of the desired
// port after the equals sign.

int serialPortNumber = 0;

// =======================================================
```

## Quick Reference Checklist

1. Download and install the [Arduino software](http://arduino.cc/en/Main/Software).
2. Download and install [Processing](http://processing.org/download/).
3. Download the [controlP5 GUI Library](http://www.sojamo.de/libraries/controlP5/#installation) for Processing.
4. Install the controlP5 library. ([Here’s how](http://www.learningprocessing.com/tutorials/libraries).)
5. Download and unzip the [SliderSerial_Arduino](http://principialabs.com/wp-content/uploads/SliderSerial_Arduino.zip) sketch.
6. Download and unzip the [SliderSerial_Processing](http://principialabs.com/wp-content/uploads/SliderSerial_Processing.zip) sketch.
7. Configure your Arduino and LED as pictured above.
8. Open `SliderSerial_Arduino` in your Arduino software environment.
9. Open `SliderSerial_Processing` in your Processing environment.
10. Upload `SliderSerial_Arduino` to your Arduino.
11. Run `SliderSerial_Processing` and make sure the LED will dim with the slider.
12. If it works, congratulations! If not, try changing the `serialPortNumber` variable as described above.

I hope this little project helps people get Arduino and Processing working together. This code could be the jumping off point for bigger things and I think Processing is a natural choice for interfacing Arduinos with PCs. Enjoy!

### Author's Note

This article was originally published on [Principia Labs](https://web.archive.org/web/20121228185657/http://principialabs.com/arduino-processing-serial-communication/).
