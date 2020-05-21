---
title: Rocket-Mounted Digital Video Camera
date: 2007-10-11
image: assets/images/arduino-2.jpg
categories: rocketry
tags: featured
---

The idea comes from [MAKE Magazine, Vol. 7](https://www.makezine.com/): Hack a \$30, “single-use” camcorder and fly it on a model rocket. The project involves disassembly of the plastic camera housing, soldering a stripped USB cable onto the camera’s circuit board, hacking the board’s embedded software to make it reusable, then mounting it in the nosecone of an Estes rocket kit. With persistence, the project can be completed in a long weekend, and the results are spectacular.

## The CVS “Single-Use” Camcorder

[![Camera Front](/web/20130105060430im_/http://principialabs.com/images/cvs_front_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/cvs_front.jpg) [![Camera Rear](/web/20130105060430im_/http://principialabs.com/images/cvs_back_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/cvs_back.jpg)

The [Pure Digital One-Time Use Video Camcorder](https://web.archive.org/web/20130105060430/http://www.puredigitalinc.com/products/otucvideo.html) is marketed by CVS and Rite-Aid pharmacies as an inexpensive and user-friendly device for capturing family memories, vacation outings and the like. With only three buttons, it is simple enough for anyone to use. A 1.5-inch color LCD serves as a viewfinder, and allows you to watch a playback of the most recent clip. The camera’s firmware and data are stored on a Samsung 128MB non-volatile flash memory chip which holds roughly 20 minutes of digital video.

The palm-sized camera costs less than thirty bucks, but there’s a catch. When your 20 minutes are up, you take the camera back to CVS, where they charge you a \$13 processing fee to download the video data and burn it onto a DVD for you. The camera’s memory is then cleared, but you don’t get to keep it; it gets sent back to the manufacturer for repackaging and resale. Well, with a little tinkering and some steady-handed soldering, we’ll make our own camera interface, turning this product into a compact, reliable and — best of all — reusable digital video platform.

## Camera Hacking 101

[![CVS Interface](/web/20130105060430im_/http://principialabs.com/images/interface_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/interface.jpg)

Peeling off the sticker on the top end of the camera reveals its secret: a proprietary, card-edge connector that only fits the plug behind the counter at CVS. Luckily, it uses standard USB protocol to communicate, so interfacing with the camera is simply a matter of soldering a cable onto the correct contacts on the circuit board.

To disassemble the camcorder, first remove the battery cover from the rear of the case, then unlock the gray battery holder inside and remove it. Unpeeling the four corners of the sticker on the back of the case reveals four tiny Phillips-head screws. Remove them and snap off the back of the case to expose the printed circuit board (PCB).

Looking at the PCB, you’ll notice two more tiny black Phillips-head screws near the lower left and right edges of the board. Remove these to release the PCB from its standoff mounts. There is a small, four-pin connector on the lower left (underside) corner of the PCB that connects the circuit board to the batteries. Pull the board straight up from this corner, and it will come free of the case.

[![PCB Rear](/web/20130105060430im_/http://principialabs.com/images/pcb_rear_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/pcb_rear.jpg)

[![PCB Front](/web/20130105060430im_/http://principialabs.com/images/pcb_front_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/pcb_front.jpg)

Now the tricky part: We’ll solder a USB cable to the PCB’s card-edge contacts so we can connect the camcorder to a PC.

## Rigging the USB Interface

Find yourself an old USB cable (maybe from an unused USB mouse or other device you won’t miss) and snip off the far end of the cord. Strip the insulation to reveal four smaller wires (and possibly some metal mesh [RF shielding](https://web.archive.org/web/20130105060430/http://en.wikipedia.org/wiki/Electromagnetic_shielding)), colored red, black, green and white.

If you can get your hands on a [decent soldering iron](https://web.archive.org/web/20130105060430/http://www.testequipmentdepot.com/weller/images/wes51.jpg) with a fine-pointed tip, the following procedure will be much easier. The contacts on the PCB are very close together, and any solder that accidentally flows between contacts will foul the connection — or worse. You can see from the photos that an iron that’s too large (as mine was) can quickly make a mess of the job. Also, using needle-nose pliers or (preferably) a small [hemostat](https://web.archive.org/web/20130105060430/http://www.sparkfun.com/commerce/advanced_search_result.php?keywords=hemostat) to hold the wires to the PCB while soldering will make things go much more smoothly.

[![Bad Solder](/web/20130105060430im_/http://principialabs.com/images/bad_solder_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/bad_solder.jpg)

[![USB Connected](/web/20130105060430im_/http://principialabs.com/images/video_pcb_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/video_pcb.jpg)

Notice that there are ten gold contacts on the PCB’s edge connector. With the connector facing you and the camera lens pointing at you, label the connectors 1-10 from left to right. Now, solder the wires to the circuit board’s edge connector in this order: red, black, green and white, to contacts 6 through 9 respectively. (If the computer indicates a problem later, try swapping the green and white wires.) Take your time.

## Connecting the Camcorder to a PC

The forums at [CameraHacking.com](https://web.archive.org/web/20130105060430/http://camerahacks.10.forumer.com/index.php?c=5) are the primary resource for the most current information on hacking the Pure Digital cameras. These bright folks have _legally_ reverse-engineered the hardware and software in both still and video camera models, allowing them to be reused for a variety of applications. \[For more detailed information on how all this was done, visit [John Maushammer's website](https://web.archive.org/web/20071212020550/http://www.maushammer.com/systems/cvscamcorder/index.html).\]

As you might imagine, Pure Digital updates the firmware and security software in each new model of camera, and your workaround solution may vary slightly from this description, depending on your camera’s model number and firmware version. The following applies to Model 220, firmware version 33.04.

The camera’s model number can be found on the FCC sticker on the bottom of the camera case exterior. To discover your firmware version, re-connect the camcorder PCB to the battery pack and verify that the camera is turned off. Hold down the “Record” and “Delete” buttons, then press the power button to turn the camera on. This should bring up the camera’s Configuration Screen which looks something like:

```bash
FW-VERSION: 33.04
CAMERA ID: GD5060\*\*\*\*\*\*
PCB VER: B1
```

If you haven’t already, shoot a few minutes of test footage. Make sure everything still works, and that you can save and play back a short video clip on the camera. Now plug the camera into your PC’s USB port. Windows should recognize the camera as a “Saturn”, and ask to install a driver. This message indicates that you have connected the USB wires correctly. Click “Cancel”, since you have no driver yet. (See CameraHacking.com for MacOS and Linux support.)

[![Saturn Ops Software](/web/20130105060430im_/http://principialabs.com/images/Saturn_Ops_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/Saturn_Ops.jpg)

We will manage our Saturn camera using a small graphical client called “Ops” (see screenshot, right). \[In Roman mythology, Ops was the wife of Saturn.\] The Ops software can be used to download videos stored on the camera’s flash chip and save them to your PC. You can also use Ops to alter the camera’s firmware or increase the video resolution. The entire Saturn file tree is accessible using Ops.

To convince Windows to interface with Saturn, we’ll need to download a hardware driver (just like any other device) and we’ll need Ops. You can scour CameraHacking.com to find these items, or you can simply use Ryan David’s [One-Click Setup](https://web.archive.org/web/20130105060430/http://ryandavid773.googlepages.com/ocs). This will install the most current driver and the Ops client automatically. Be sure and get the latest version from the download page.

Once all the software and drivers are installed, we’ll connect the camera to the PC with the USB cable and fire up the Ops client. Complete instructions for using Ops are available in the FAQ section of CameraHacking.com, but the basic procdeure is: Open Camera -> Unlock -> Download All Movies -> Close Camcorder. That’s it! \[If you have problems unlocking your camera, you may want to check out [this site](https://web.archive.org/web/20130105060430/http://freelowell.com/downloads/camcorderpage/Camerahacking.html).\]

### Playing the Videos

Videos from the Saturn camera are saved as .AVI files and are encoded using the open-source [XviD codec](https://web.archive.org/web/20130105060430/http://www.xvidmovies.com/info/), which, like DivX, is an imlementation of the MPEG-4 standard. In order to play these clips on a Windows PC, we’ll need to [download](https://web.archive.org/web/20130105060430/http://www.xvidmovies.com/codec/) this codec. Once we’ve installed the XviD codec, we will be able to play XviD videos using Windows Media Player or any other XviD-enabled player.

### Building the Rocket

The model rocket we’ll use to fly the camcorder is the [Estes X-Prize Canadian Arrow](https://web.archive.org/web/20130105060430/http://www.estesrockets.com/products.php?number=2188). The design is essentially a stretched version of the classic WWII German V-2 rocket, but our main concern is to find a kit with a nosecone diameter large enough to accommodate the video PCB. The Estes Fat Boy kit, while quite a bit shorter, would also serve our purposes. Since we are going to modify the kit slightly from it’s original configuration, we’ll use [Rocksim](https://web.archive.org/web/20130105060430/http://www.apogeerockets.com/rocksim.asp) design and simulation software to ensure our modifications won’t affect the flight characteristics of the rocket.

Next: [Rocket Stability and Performance](/web/20130105060430/http://principialabs.com/rocket-stability-and-performance)

[![Rocksim](/web/20130105060430im_/http://principialabs.com/images/rocksim_3d_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/rocksim_3d.jpg)

[![Building the Kit](/web/20130105060430im_/http://principialabs.com/images/v2_build_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/v2_build.jpg)

[![Lens Cutout](/web/20130105060430im_/http://principialabs.com/images/the_eye_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/the_eye.jpg)

[![Standoffs - Top View](/web/20130105060430im_/http://principialabs.com/images/standoffs_top_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/standoffs_top.jpg)

[![Mounted Camera](/web/20130105060430im_/http://principialabs.com/images/mounted_cam_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/mounted_cam.jpg)

[![Making Progress](/web/20130105060430im_/http://principialabs.com/images/progress2_sm.jpg "Click to enlarge")](/web/20130105060430/http://principialabs.com/images/progress2.jpg)

---

### Author's Note

This article was originally published on [Principia Labs](https://web.archive.org/web/20130105060430/http://principialabs.com/rocket-mounted-digital-video-camera/).
