---
title: Arduino-Python 4-Axis Servo Control
date: 2008-04-08
image: assets/images/arduino-1.jpg
categories: electronics
tags: arduino, featured
---

Although the [Arduino](http://arduino.cc/) platform is ideal for standalone applications, it really comes to life when interfaced with a PC. Connect Arduino to a personal computer and you instantly add a ton of versatility and processing power to your project.

This tutorial will describe how to use Arduino to control a bank of four independent RC servos with your PC (or Mac, or \*nix Box), using a USB cable and a modular Arduino-Python software stack.

The following discussion builds upon concepts presented in two previous articles, “[Arduino Serial Servo Control](/notes/arduino-serial-servo-control/)” and “[Joystick Control of a Servo](/notes/joystick-control-of-a-servo/).” As always, comments, critiques, or suggestions for improving or adapting this code are welcome and appreciated.

<a class="btn btn-dark" href="https://github.com/bws428/arduino-python-servos" >View on GitHub &nbsp; <span class="fa-stack"><i class="fab fa-github fa-stack-2x fa-inverse"></i></span></a>

## Project Outline

The primary goal for this project was to create a software stack that allows simple and flexible control of multiple servos from any type of Python script.

The solution has two basic components: (1) an Arduino sketch that waits for serial input from a connected PC, then moves each servo to its commanded position, and; (2) a Python module on the PC that opens the serial connection and formats the data packets expected by the Arduino.

Any other Python program written to sit on _top_ of these two layers need not worry about the messy details of serial communication, but rather can just say something like, “Move servo #2 to 90 degrees.” Or, more precisely:

```python
servo.move(2,90)
```

Easy, right? Let’s get started.

## Part I: Smoke, Mirrors, and Hand-Waving

If you just want to get things up and running quickly, start here. These instructions will get your servos connected and obeying every whim of your PC in no time.

### Hardware Setup

Hardware for this project consists of an Arduino module, four [JR Sport ST47](https://servodatabase.com/servo/jr/st47) standard servos, and a breadboard to create the circuit.

The servos each have three wires: Ground (brown), Power (red) and Control (yellow). Each of the Control wires will connect to a different digital pin on the Arduino board (pins 2 through 5 in our setup), and _all_ of the Power and Ground wires will need to connect somehow to the 5V and Gnd pins.

[![](https://web.archive.org/web/20121229113135im_/http://principialabs.com/wp-content/uploads/arduino-breadboard.thumbnail.jpg "Breadboard: click to enlarge")](http://principialabs.com/wp-content/uploads/arduino-breadboard.jpg)

The simplest way to accomplish this is to create a “bus” bar along one of the breadboard’s edges, as shown in the photo above. Simply route the Arduino’s 5V and Gnd to a convenient area on the breadboard, and connect all the servos.

### Required Software: The Lower Layers

To get the effects seen in the video above, you’ll need at least the following two programs. Although this code is designed to control four servos, it also works as-is with _fewer_ servos, and — with a few modifications — as many as twelve (or 48 with the [Arduino Mega](http://arduino.cc/en/Main/ArduinoBoardMega)!).

**Download the code:**

**MultipleServos.pde**: This is the Arduino sketch. Copy and paste this code into your Arduino software and upload it to the board.

[**MultipleSerialServoControl.pde**](http://principialabs.com/wp-content/uploads/MultipleSerialServoControl.pde): \[Updated 12/23/09\] This is the NEW Arduino sketch, which uses the Arduino [Servo library](http://arduino.cc/en/Reference/Servo) to make alterations _much_ simpler. You can control up to 12 servos using this code (with modifications) and most Arduino boards, or _up to 48 servos_ using an Arduino Mega! (See the code comments for specific details.) This code should also solve most of the servo “jitter” issues mentioned in the comments below. Copy and paste this code into your Arduino software and upload it to the board. (Requires [Arduino 0017](http://www.arduino.cc/en/Main/Software) or greater.)

[**servo.py**](http://principialabs.com/wp-content/uploads/servo.py): This is the Python module which talks directly to the above Arduino sketch. This script requires the [pyserial](http://sourceforge.net/projects/pyserial/) module, available from Sourceforge. Save this script on your PC wherever you like, just be sure to name it “servo.py”.

**Important!** The code presented here has been tested and verified to work on Windows, Mac, and Linux using [Python 2.6](http://www.python.org/download/releases/2.6.6/), [PySerial 2.5](http://pypi.python.org/pypi/pyserial) for Python 2.x, and [PyGame 1.9.1](http://www.pygame.org/download.shtml) for Python 2.6. Python 3.x is not supported. For users with 64-bit systems, the 32-bit version of all software is recommended. Other versions of Python will probably work, but getting all the modules working together is up to you.

**New to Python?** Welcome! Python is a versatile and fun language to learn, and it’s used by just about everyone, from newbies to NASA! Check out the [Beginner’s Guide](http://wiki.python.org/moin/BeginnersGuide) to get your bearings, or get the full skinny at [python.org](http://python.org/).

**Customize the code:**

Depending on your computer system and Arduino hardware setup, you may need to make a few modifications to the code.

**Arduino:** In the “MultipleSerialServoContro” sketch, take note of the following variables and make adjustments as necessary for your setup. See “[Arduino Serial Servo Control](/web/20121229113135/http://principialabs.com/arduino-serial-servo-control/)” for more details regarding the `minPulse` and `maxPulse` variables. (If you’ve got standard RC servos attached to pins 2-5, you probably won’t have to change anything.)

    // Common servo setup values
    int minPulse = 600;   // minimum servo position, us (microseconds)
    int maxPulse = 2400;  // maximum servo position, us

    // Attach each Servo object to a digital pin
    servo1.attach(2, minPulse, maxPulse);
    servo2.attach(3, minPulse, maxPulse);
    servo3.attach(4, minPulse, maxPulse);
    servo4.attach(5, minPulse, maxPulse);

**Python:** In the “servo.py” script, you’ll most likely need to change the value of the `usbport` variable, which tells Python how to find your Arduino (On Windows, it’ll be something like ‘COM5′. On a Mac, ‘/dev/tty.usbserial-xxxxx’. On Linux, ‘/dev/ttyUSB0′.). Try running `ls /dev/tty*` from a Mac or Linux terminal for a list of available ports. \[ToDo: Modify the script to make this step unnecessary.\]

**Test the code:**

Once your hardware is set up and the software is installed, you can test the system’s basic functionality from the [Python interactive interpreter](http://docs.python.org/tutorial/interpreter.html) or “shell,” like so:

    ~/path/to/servo.py$ python
    >>> import servo
    >>> servo.move(2,150)

The `servo.move()` method takes two arguments, both integers. The first is the servo number you wish to move, 1-4 (or whatever). The second is the commanded angular position of the servo horn, from 0-180 degrees. So, if you want to move Servo #3 fully clockwise (180 degrees), you’ll type `servo.move(3,180)`. Cake, baby!

### Optional Software: From Totally Geek to Totally Chic

The following scripts are designed to leverage the functionality of the `servo.move()` method for simple and readable code. Make sure these files reside in the same directory as “servo.py”.

- [**servodance.py**](http://principialabs.com/wp-content/uploads/servodance.py): A cascading effect that feels like watching a quarter spiral down one of those funnel-shaped wishing wells.

- [**servorandom.py**](http://principialabs.com/wp-content/uploads/servorandom.py): The final servo sequence seen in the video, with individual servos moving to random positions and then waving “goodbye” in unison.

- [**servomarch.py**](http://principialabs.com/wp-content/uploads/servomarch.py): This one’s not in the video, but it’s a fun script to test individual and simultaneous movement of multiple servos. Just set the number of servos to march (default is 4), and send them off!

- [**multijoystick.py**](http://principialabs.com/wp-content/uploads/multijoystick.py): Allows joystick control of four servos, with each joystick axis controlling a single servo. This script is the most complex, so try getting the first three working, then graduate to this one — it’s easier to troubleshoot problems that way.  
  \[Note: This script also requires installation of the [pygame](http://www.pygame.org/) module.\]

With any luck, you should now have everything up and running just like in the video!

### Adding Servos: When 4 Axes Just Isn’t Enough

\[Updated 12/23/09\] If you’re using the [new Arduino sketch](http://principialabs.com/wp-content/uploads/MultipleSerialServoControl.pde), you can easily add servos to your project by making a few simple additions to the code. The beauty of this system is that `servo.py` can remain unchanged, and all of the higher-level Python scripts just need simple alterations to include whatever number of servos you decide to add. This segment will outline how to change the Arduino sketch; changes to the Python scripts will be up to you!

There are three places in the `MultipleSerialServoControl` sketch where you’ll need to make additions, if you want to control more than four servos. Each section of the code contains the comment “TO ADD SERVOS:” followed by a suggestion on what to add.

First, add a Servo object for each additional servo:

1.  // Create a Servo object for each servo
2.  Servo servo1;
3.  Servo servo2;
4.  Servo servo3;
5.  Servo servo4;
6.  // TO ADD SERVOS:
7.  //   Servo servo5;
8.  //   etc...

// Create a Servo object for each servo
Servo servo1;
Servo servo2;
Servo servo3;
Servo servo4;
// TO ADD SERVOS:
// Servo servo5;
// etc...

Second, assign a digital pin to each additional servo:

1.  // Attach each Servo object to a digital pin
2.  servo1.attach(2, minPulse, maxPulse);
3.  servo2.attach(3, minPulse, maxPulse);
4.  servo3.attach(4, minPulse, maxPulse);
5.  servo4.attach(5, minPulse, maxPulse);
6.  // TO ADD SERVOS:
7.  //   servo5.attach(YOUR_PIN, minPulse, maxPulse);
8.  //   etc...

// Attach each Servo object to a digital pin
servo1.attach(2, minPulse, maxPulse);
servo2.attach(3, minPulse, maxPulse);
servo3.attach(4, minPulse, maxPulse);
servo4.attach(5, minPulse, maxPulse);
// TO ADD SERVOS:
// servo5.attach(YOUR_PIN, minPulse, maxPulse);
// etc...

Third, create a new switch case for each additional servo:

1.  // Assign new position to appropriate servo
2.  switch (servo) {
3.  case 1:
4.  servo1.write(pos);    // move servo1 to 'pos'
5.  break;
6.  case 2:
7.  servo2.write(pos);
8.  break;
9.  case 3:
10. servo3.write(pos);
11. break;
12. case 4:
13. servo4.write(pos);
14. break;
15. // TO ADD SERVOS:
16. //     case 5:
17. //       servo5.write(pos);
18. //       break;
19. // etc...
20. }

    // Assign new position to appropriate servo
    switch (servo) {
    case 1:
    servo1.write(pos); // move servo1 to 'pos'
    break;
    case 2:
    servo2.write(pos);
    break;
    case 3:
    servo3.write(pos);
    break;
    case 4:
    servo4.write(pos);
    break;
    // TO ADD SERVOS:
    // case 5:
    // servo5.write(pos);
    // break;
    // etc...
    }

After making changes, be sure to click the “Verify” button on your Arduino software to make sure there are no errors, then upload it to the board. Test your changes by calling the `servo.move()` method from the Python interpreter. That’s it!

### Joystick Button Digital On/Off Demo

\[Updated 02/18/2011\] So, you’ve got your joystick merrily controlling your servos, but dammit, you want to be able to press a button and magically activate your \[insert cool/evil feature here\] … and you just can’t figure out how to get started. Well, today is your lucky day. Due to the high demand in the comments field for this feature, I’ve released updated versions of `multijoystick.py` (v.0.4) and `MultipleSerialServoControl.pde` (v.1.1).

This simple little demo will allow you to control Arduino’s built-in LED on Pin 13 with your joystick Button 1 (the trigger, hopefully, but your joystick may require code-tweaking). Depressing and holding the trigger should set Pin 13 to HIGH (LED on), and releasing the trigger should set the pin back to LOW (LED off). The `multijoystick.py` script now has built-in skeleton support for 6 joystick buttons — which simply means the code structure is there, and you’ll be able to see in the Python interpreter window which button PyGame thinks you are pressing.

The new joystick code also adds support for the “Hat” or POV Switch. The hat switch toggles L/R/UP/DN and then centers, and the code supports events for each position. The included demo will drive servo #4 full left with a “hat left” command, full right with a “hat right” command, and then center the servo when the hat springs back to center — one possible method to control the “pan” function of a pan/tilt platform, for example.

Here are a couple of snippets from the new `multijoystick.py` script:

1.  # Assign actions for Button DOWN events
2.  elif e.type == pygame.JOYBUTTONDOWN:
3.  # Button 1 (trigger)
4.  if (e.dict\['button'\] == 0):
5.  print "Trigger Down"
6.  # Set pin 13 LED to HIGH for digital on/off demo
7.  servo.move(99, 180)
8.  # Button 2
9.  if (e.dict\['button'\] == 1):
10. print "Button 2 Down"


    # Assign actions for Button DOWN events
    elif e.type == pygame.JOYBUTTONDOWN:
        # Button 1 (trigger)
        if (e.dict\['button'\] == 0):
            print "Trigger Down"
            # Set pin 13 LED to HIGH for digital on/off demo
            servo.move(99, 180)
        # Button 2
        if (e.dict\['button'\] == 1):
            print "Button 2 Down"

1.  # Assign actions for Button UP events
2.  elif e.type == pygame.JOYBUTTONUP:
3.  # Button 1 (trigger)
4.  if (e.dict\['button'\] == 0):
5.  print "Trigger Up"
6.  # Set pin 13 LED to LOW for digital on/off demo
7.  servo.move(99, 0)
8.  # Button 2
9.  if (e.dict\['button'\] == 1):
10. print "Button 2 Up"


    # Assign actions for Button UP events
    elif e.type == pygame.JOYBUTTONUP:
        # Button 1 (trigger)
        if (e.dict\['button'\] == 0):
            print "Trigger Up"
            # Set pin 13 LED to LOW for digital on/off demo
            servo.move(99, 0)
        # Button 2
        if (e.dict\['button'\] == 1):
            print "Button 2 Up"

I know, I know, you’re saying, “What’s with this `servo.move(99, 180)` crap? I don’t even _have_ 99 servos!” Well, since the Arduino sketch is already listening for a servo number and position, it’s a trivial matter to just “pretend” that our LED (or relay, or nerf turret, etc.) is just another servo. And since you’re unlikely to have more than fifty servos in your project, 99 seemed like a safe number to pick. The servo positions — in this case 180 and 0 — simply substitute for “on” and “off” respectively, but you could easily come up with your own communication scheme.

Here are some snippets from the new Arduino sketch that were added to accommodate the LED demo:

In the header, with the other variable assignments:

1.  // LED on Pin 13 for digital on/off demo
2.  int ledPin = 13;
3.  int pinState = LOW;

// LED on Pin 13 for digital on/off demo
int ledPin = 13;
int pinState = LOW;

In the `void setup()` block:

1.  // LED on Pin 13 for digital on/off demo
2.  pinMode(ledPin, OUTPUT);

// LED on Pin 13 for digital on/off demo
pinMode(ledPin, OUTPUT);

In the `switch(servo)` block:

1.  // LED on Pin 13 for digital on/off demo
2.  case 99:
3.  if (pos == 180) {
4.  if (pinState == LOW) { pinState = HIGH; }
5.  else { pinState = LOW; }
6.  }
7.  if (pos == 0) {
8.  pinState = LOW;
9.  }
10. digitalWrite(ledPin, pinState);
11. break;

// LED on Pin 13 for digital on/off demo
case 99:
if (pos == 180) {
if (pinState == LOW) { pinState = HIGH; }
else { pinState = LOW; }
}
if (pos == 0) {
pinState = LOW;
}
digitalWrite(ledPin, pinState);
break;

So basically, the Arduino code just receives the `servo.move(99, x)` from the serial buffer, looks in the `switch` block to see if there is, in fact, a Servo #99 (which is an LED in our case), and then executes the code in that block, which sets the output of Pin 13 to either HIGH or LOW. You could conceivably replicate this code for as many digital pins as you like, and a handful of 5V solid-state relays could switch on or off just about any electronic device imaginable. Have fun!

## Part II: Getting Down to Brass Tacks

Next, let’s take a look under the hood to see how it all works. If you’re the type that just wants to get things working and damn the details, **STOP HERE**. Otherwise, continue on, and I’ll do my best to explain how the code “do what it do.”

### The Problem Set

Asynchronous serial communication is not perfect. Sometimes there are errors, dropped packets, confusion. Sometimes the mail does _not_ get through. In both of the previous two serial/servo projects, the Arduino expected only one byte from the PC, and in both cases that byte represented a commanded servo position — and nothing more. If a byte was missed or skipped, it wasn’t a big deal, another one was sure to come along, and it was impossible to misinterpret.

This project presents a couple of new challenges. First, we are controlling more than one servo, so the Arduino needs more than one command element for each move. As we’ve seen above, it needs to know (at least) _which_ servo to move, and _how much_ to move it. Secondly, we have the problem of communication. This time, we’re sending _two_ command elements for each move (servo number & position), and these elements are clearly _not_ interchangable. That is, if we want to send `servo.move(4,90)`, we need to make sure that Arduino knows that the ’4′ means “Servo #4″ and the ’90′ means “90 degrees.”

Tom Igoe’s article, “[Interpreting Serial Data](http://www.tigoe.net/pcomp/code/communication/interpreting-serial-data-bytes),” contains an excellent discussion of some of the problems involved in serial communication, and lists several issues that need to be addressed in every project, namely:

> 1.  How many bytes am I sending? Am I receiving the same number?
> 2.  Did I get the bytes in the right order?
> 3.  Are my bytes part of a larger variable?
> 4.  Is my data getting garbled in transit?

The Arduino’s `Serial.read()` function reads one byte of data at a time from its _serial buffer_. Think of the serial buffer as a mailbox. It’s a small (128 bytes) area of memory where incoming serial messages are stashed until the Arduino is ready to read them. Every character we send from the PC to Arduino is one byte. So, while we could send the Arduino something very unambiguous like, “Yeah, hi, Arduino, it’s the Linux Box again. What’s happening? If you could go ahead and move Servo #4 to the 90-degree position, that would be great. Thaaanks,” (163 bytes) it’s obviously better if we can come up with something a little more terse.

However, as we’ve seen, if we just send over the characters ’4′, ’9′, and ’0′ — remember, each character is a byte — the Arduino might get confused. This problem is amplified when more commands start stacking up in the buffer. Let’s say now we command `servo.move(2,180)` and `servo.move(3,120)`. Now the buffer should hold {4,9,0,2,1,8,0,3,1,2,0}, except–OOPS!–one of the bytes got dropped along the way, so now it holds {4,9,0,2,1,8,3,1,2,0}. “Wait, which servo did you want me to move?” You can clearly see a problem developing.

### Solution: Data Packets and Start Bytes

Luckily, part of the solution is handled in the way Arduino communicates. Arduino uses [ASCII](http://en.wikipedia.org/wiki/ASCII) encoding to represent alphanumeric characters. Each character sent over the wire is converted to the binary equivalent of a decimal value from 0 to 255. \[See this [conversion chart](http://www.arduino.cc/en/Reference/ASCIIchart) for specifics.\]

So, for example, if we send over the character ‘A’, Arduino recognizes this as its decimal value, ’65′. We won’t get too deep into this concept except to say that the implementation is _great_ for our application, because as long as the values we’re sending are less than 255, they’ll fit neatly into one byte. Since the largest value we send is 180, we only have to send two bytes per command.

Now, if you’ve looked at the ASCII conversion chart, you’ll recognize that doing this every time you want to send a command would be a real pain. Also, trying to teach Python this chart would take up a lot of unnecessary code. Thankfully, this problem is already solved for us with Python’s `chr()` function. Wrap any decimal value from 0-255 in `chr()`, and you get back its ASCII equivalent. A few examples:

    ~$ python
    >>> chr(65)
    'A'
    >>> chr(110)
    'n'
    >>> chr(13)
    'r'
    >>> chr(9)
    't'

You get the idea, but notice that ASCII doesn’t just represent letters and numbers, but also symbols and other “control” or “non-printing” characters like line-feeds, returns, and tabs.

So, now if we want to send `servo.move(4,90)`, we only need _two_ bytes, the ASCII equivalents of ’4′ and ’90′, represented in Python as `chr(4)` and `chr(90)`, and interpreted by the Arduino sketch as, once again, simply ’4′ and ’90′. Easy! \[Seriously, if your brain explodes at this point, or you're bleeding from the ears, it's understandable. I don't like it any more than you do, but stick with me, it'll all work out neatly in the end.\]

#### Packets, Headers, and Payloads

Okay, great, now instead of just digits in Arduino’s serial buffer, we have meaningful values. Part of the problem is solved, but we still haven’t addressed the issue of dropped or missing bytes. That is, how will the Arduino know that a ’4′ is “Servo #4″ and not “4 degrees” when pulling values out of a crowded buffer like {4,90,2,180,3,0,1,110} ?

The answer is [data packets](http://computer.howstuffworks.com/question525.htm). Very simply, instead of just sending a long string of numbers to Arduino, we’ll send a very specific ordered message, a _packet_ of values, that is intended to be read and interpreted **as a whole** and **in order**, or else discarded completely.

Now, the structure of a packet can be as simple or as complex as we need it to be, as you might have noticed if you followed that last link. But all we really need is some means of ensuring that Arduino doesn’t confuse one value for another.

Essentially, our Python script needs to tell Arduino three things:

1.  Here comes a new servo command.
2.  Servo number to move.
3.  Commanded servo position.

We’ve already been sending the last two elements, the servo number and position. Here, we’re adding a third element, which we’ll call the _header_ or the _start byte_. Our header, like the rest of the data in our packet, will be just one byte long, and contain no real information other than the conceptual message, “I am a header.”

The _order_ of this message is important. Every packet sent over the wire, or read from the serial buffer, will now have the following format:

    (Header, Servo Number, Servo Position)

or, more tersely:

    (startbyte, servo, angle)

What to use as a startbyte? Well, we’re only using the values from 0-180 as either our Servo Number or Servo Postion. Any value from 181 to 255 would be unique. We’ll use ’255′ just to make it obvious. So, every packet will now look something like one of the following:

    (255, 1, 90)
    (255, 2, 180)
    (255, 3, 0)

And the Arduino’s serial buffer would look something like:

    {255,1,90,255,2,180,255,3,0}

Now, instead of reading byte after byte and hoping for the best, Arduino will _wait_ until a minimum of three bytes arrive in the buffer, and then read the first byte to determine whether or not it is, in fact, a header (255). If it’s not, Arduino skips that value, and moves on to the next byte without touching the servos. When it finally sees a header, Arduino continues reading the next two bytes, in order, and assigning them to the Servo Number and the Servo Position, respectively. If either of _those_ two values is ’255′, Arduino assumes something is wrong, and skips everything until it reads a new header.

#### Side Note: Authoritarian Flow Control

“Now just a minute!” you’re saying. “If that is the case, then _some_ of the commands Python sends to the Arduino will be totally ignored!” And you’re right. This method of serial flow control is definitely one-way, with no error-checking. Other methods, such as “call-and-response” or “[handshaking](http://itp.nyu.edu/physcomp/Labs/Serial)” are much better at ensuring accuracy, since there’s a back-and-forth arrangement that can call for data to be re-sent in the event of dropped packets. But the two-way protocol this method requires is _much_ slower.

We have to make an engineering decision. In our application, which is more important, accuracy or quick response? It depends on exactly how you are using the servos, but if you consider say, a joystick-controlled robot or RC vehicle application, then clearly an immediate response and quick visual feedback is preferable to perfect accuracy. If you command “turn right” with a joystick, and your vehicle doesn’t respond appropriately, you’ll just instinctively add more right stick input.

Perfect accuracy is not required.

### Writing the Code

Very briefly, let’s look at how the above concepts are implemented in both the Python and Arduino software.

#### Python Implementation

Whenever we call the `servo.move()` method, the Python script `servo.py` handles the serial communication details using the `pyserial` module, and formats the arguments into the data packet outlined above. The bare-bones version looks like this:

1.  #!/usr/bin/env python

2.  import serial
3.  usbport = '/dev/ttyUSB0'
4.  ser = serial.Serial(usbport, 9600, timeout=1)

5.  def move(servo, angle):
6.  if (0 <= angle <= 180):
7.  ser.write(chr(255))
8.  ser.write(chr(servo))
9.  ser.write(chr(angle))
10. else:
11. pass

#!/usr/bin/env python

import serial
usbport = '/dev/ttyUSB0'
ser = serial.Serial(usbport, 9600, timeout=1)

def move(servo, angle):
if (0 <= angle <= 180):
ser.write(chr(255))
ser.write(chr(servo))
ser.write(chr(angle))
else:
pass

#### Arduino Implementation

Arduino opens its own serial connection, waits for at least three bytes to fill the buffer, then starts reading:

1.  /\*\* MultipleSerialServoControl.pde (bare bones) \*\*/

2.  void setup() {
3.  // Open the serial connection, 9600 baud
4.  Serial.begin(9600);
5.  }

6.  void loop()
7.  {
8.  // Wait for serial input (min 3 bytes in buffer)
9.  if (Serial.available() > 2) {
10. // Read the first byte
11. startbyte = Serial.read();
12. // If it's really the startbyte (255) ...
13. if (startbyte == 255) {
14. // ... then get the next two bytes
15. for (i=0;i<2;i++) {
16. userInput\[i\] = Serial.read();
17. }
18. // First byte = servo to move?
19. servo = userInput\[0\];
20. // Second byte = which position?
21. pos = userInput\[1\];
22. // Packet error checking and recovery
23. if (pos == 255) { servo = 255; }

/\*\* MultipleSerialServoControl.pde (bare bones) \*\*/

void setup() {
// Open the serial connection, 9600 baud
Serial.begin(9600);
}

void loop()
{
// Wait for serial input (min 3 bytes in buffer)
if (Serial.available() > 2) {
// Read the first byte
startbyte = Serial.read();
// If it's really the startbyte (255) ...
if (startbyte == 255) {
// ... then get the next two bytes
for (i=0;i<2;i++) {
userInput\[i\] = Serial.read();
}
// First byte = servo to move?
servo = userInput\[0\];
// Second byte = which position?
pos = userInput\[1\];
// Packet error checking and recovery
if (pos == 255) { servo = 255; }

If Arduino gets a complete packet with header, servo, and angle values, it assigns the new position to the appropriate servo. If the value of `servo` is not between 1 and 4 (or whatever maximum number of servos you specify), the loop exits without assigning any new values. That's it! The Arduino Servo library really makes servo control easy and painless.

26. // Assign new position to appropriate servo
27. switch (servo) {
28. case 1:
29. servo1.write(pos);    // move servo1 to 'pos'
30. break;
31. case 2:
32. servo2.write(pos);
33. break;
34. case 3:
35. servo3.write(pos);
36. break;
37. case 4:
38. servo4.write(pos);
39. break;
40. }

        // Assign new position to appropriate servo
        switch (servo) {
          case 1:
            servo1.write(pos);    // move servo1 to 'pos'
            break;
          case 2:
            servo2.write(pos);
            break;
          case 3:
            servo3.write(pos);
            break;
          case 4:
            servo4.write(pos);
            break;

    }
    }

## Whew! We're Done

Well, if you've made it this far, congratulations: you're totally insane. I hope the above dissertation helps at least one person better grasp these concepts, since it took me across many web pages and into several late nights to find the answers. Good luck!

---

### References

- Tom Igoe, _[Making Things Talk: Practical Methods for Connecting Physical Objects](http://www.oreilly.com/catalog/9780596510510/)_
- Tom Igoe, "[Serial Communication](http://www.tigoe.net/pcomp/code/communication/serial-communication)"
- Tom Igoe, "[Interpreting Serial Data](http://www.tigoe.net/pcomp/code/communication/interpreting-serial-data-bytes)"
- Society of Robots, "[Actuators and Servos](http://www.societyofrobots.com/actuators_servos.shtml)"
- ITP Physical Computing, "[Servo Lab](http://itp.nyu.edu/physcomp/Labs/Servo)"
- ITP Physical Computing, "[Serial Lab](http://itp.nyu.edu/physcomp/Labs/Serial)"

### Supercool Projects Based on this Code

- [MIT’s Fab Lab Active Extrusion Machine](https://web.archive.org/web/20121229113135/http://fab.cba.mit.edu/classes/MIT/961.09/projects/active_extrusion/)
- [Pan/Tilt Webcam Robot](https://web.archive.org/web/20121229113135/http://dtostillwell.com/?p=215)
- [Walking Biped Robot](https://web.archive.org/web/20121229113135/http://auzieman.blogspot.com/2010/08/testing-code-style-blocks.html)
- [Wireless XBee Tank-Steer Rover](https://web.archive.org/web/20121229113135/http://www.somethingrisky.com/?p=38)
- [Exuro Kinect/Arduino Gimbaling Robot Eyes](https://web.archive.org/web/20121229113135/https://github.com/pkropf/exuro)
- [Wiimote-Controlled Webcam Platform](https://web.archive.org/web/20121229113135/http://www.westy92.com/?p=24)
- [AtomBot Netbook Rover](https://web.archive.org/web/20121229113135/http://sites.google.com/site/airwavershr/Home/atombot)
- [Automated NERF Vulcan “Portal” Turret](https://web.archive.org/web/20121229113135/http://negativeacknowledge.com/2010/06/automated-nerf-turret/)
- [Maya 3D/Arduino/Python Servo Control](https://web.archive.org/web/20121229113135/http://blog.makezine.com/archive/2008/12/maya-and-arduino-control.html)
- [Android/Arduino LED Light Bar](https://web.archive.org/web/20121229113135/http://blog.mawqey.com/2011/01/24/experimenting-with-android-and-arduino/)

Add a link to your project in the comments below!

### Author's Note

This article was originally published on [Principia Labs](https://web.archive.org/web/20121229113135/http://principialabs.com/arduino-python-4-axis-servo-control/).
