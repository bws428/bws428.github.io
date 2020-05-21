---
title: Headless Raspberry Pi Zero W Setup
image: assets/images/pi-zero-hd.png
categories: electronics
tags: ["raspberry pi", featured]
---

The [Raspberry Pi Zero W](https://www.raspberrypi.org/products/raspberry-pi-zero-w/) is the smallest standalone RPi board, and includes built-in WiFi and Bluetooth connectivity. It has the same 40-pin GPIO pinout as the full-featured Raspberry Pi 4, but without the male headers soldered onto the board. This makes it great for use in embedded projects.

Now, while the Pi Zero board does have a Mini HDMI port and Micro USB ports for power and peripheral devices, it is ideally used in a "headless" mode, with nothing connected to it except the power cable. All access to the module is via Secure Shell ([SSH]({% post_url 2008-05-29-beginning-ssh %})) from a remote terminal.

What follows is my step-by-step process for getting a Pi Zero up and running out of the box in headless mode, using a Linux (or Mac) computer as the remote terminal for communicating with and configuring the Pi.

## Required Hardware

Here is the list of hardware I used to get started. Although you can buy everything "a la carte," so to speak, it often makes sense to just buy a starter kit from a place like [PiShop.us](https://www.pishop.us/), [Adafruit](https://www.adafruit.com/), [SparkFun](https://sparkfun.com), and others which will include everything you're likely to need. That's what I did.

- Raspberry Pi Zero W (\$10)
- 16GB Micro SD Card, Class 10 (\$8)
- Micro-USB Power Supply, 5V 2.5A (\$5)
- [Pi Zero Case](https://flirc.tv/more/flirc-raspberry-pi-zero-case) (\$5--\$15, optional)

{% include image.html file="pi-zero-starter.jpg" description="Pi Zero W Starter Kit" credit="PiShop.us" %}

You'll also need some type of [SD card reader](https://www.amazon.com/Anker-Portable-Reader-RS-MMC-Micro/dp/B006T9B6R2) or hub attached to your computer in order to write the Raspbian OS to the SD card.

## Install Raspbian Lite

Raspbian is a derivative of the Debian Linux distrubution, and is the standard operating system for all the Raspberry Pi boards. Since I'll be using the Pi Zero in headless mode, accessing it only from a remote computer, there's no need to install the complete desktop version. [Raspbian Lite](https://www.raspberrypi.org/downloads/raspbian/) is a stripped-down version of the OS, and without the need for a graphical user interface, it's less than half of the size of the desktop image.

The excellent Raspberry Pi documenation contains a thorough description of the various methods of [installing operating system images](https://www.raspberrypi.org/documentation/installation/installing-images/). These days, however, the simplest way to get Raspbian onto your SD card is to install the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) program, available for Windows, MacOS, and Ubuntu.

Just insert your Micro SD card into your reader, launch the Raspberry Pi Imager, select your OS and SD card, and click `WRITE`. Then grab a coffee.

`Operating System > Choose OS > Raspbian (other) > Raspbian Lite`

{% include image.html file="rpi-imager-write.png" %}

<div class="alert alert-primary text-center" role="alert">
  <b>STOP!</b> Do not remove the SD card from your computer yet.
</div>

We've got some configuring to do first. Since we want to be able to access the Pi Zero remotely, rather than plugging in a keyboard, we'll need to set up the WiFi networking and a few other things _prior to the first boot_ of the Pi Zero board. So leave the SD card inserted into your computer for now.

## Raspbian Boot Configuration

If the Raspian Lite OS image was successfully burned to the SD card, you should have gotten a "Write Successful" popup from the Raspberry Pi Imager program. In your file browser's "Devices" menu, where previously you would have seen your blank SD card and other mounted drives, you should see two new devices named `boot` and `rootfs`. If you don't see them, try removing and reinserting your SD card into your computer.

{% include image.html file="boot.png" description="New devices 'boot' and 'rootfs'" %}

We are only concerned with the [`/boot` partition](https://www.raspberrypi.org/documentation/configuration/boot_folder.md), so open up that directory in your file browser as in the image above. In the next sections, we are going to set up Raspbian for its first boot, by creating and/or editing the following three text files in the `boot` directory:

1. &nbsp; `wpa_supplicant.conf` - to configure WiFi
2. &nbsp; `ssh` - to enable SSH for remote shell access
3. &nbsp; `config.txt` - to enable UART for backup serial access

Go ahead and fire up your [favorite text editor](https://code.visualstudio.com/) and let's get started.

### Configure WiFi

On startup, Raspbian looks in the `boot` directory for a file called `wpa_supplicant.conf`, if it exists, and uses that to configure its WiFi interface. This can be used to enable our headless setup without the need to plug in a keyboard and display. Using your text editor, go ahead and create that file in the SD card's `boot` directory now, and paste in the following:

```bash
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
    ssid="SSID"
    psk="PASSWORD"
    scan_ssid=1
}
```

Be sure to:

- Replace `US` with your two-letter [country code](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) if you're in not the U.S.
- Replace `SSID` with the SSID name of your WiFi network.
- Replace `PASSWORD` with the WPA2 password for your network.

That's it! Close the file and save it.

### Enable SSH

For security reasons, all recent Raspbian OS images have SSH [disabled by default](https://www.raspberrypi.org/blog/a-security-update-for-raspbian-pixel/). Since we need this to be able to access the Pi Zero remotely, we'll want to enable it on first boot.

In the `boot` directory, simply create an empty file named `ssh`. You can do this with your text editor, or from the command line by typing:

```bash
touch ssh
```

### Enable UART

In case for some reason we can't SSH in to the Pi when we first boot it up, we'll want to enable [UART](https://en.wikipedia.org/wiki/Universal_asynchronous_receiver-transmitter) on the GPIO header pins so we have a backup means of communicating with it. You'll also need a [USB to TTL Serial "Console" Cable](https://www.adafruit.com/product/954) to attach to the appropriate GPIO UART pins. If you bought one of the Pi Zero starter kits, you may also have the dongles to simply connect a keyboard and HDMI display. Either way, this step is just a backup.

{% include image.html file="pi-zero-w-gpio-pinout.png" description="Pi Zero W GPIO diagram - UART is on GPIO 14 and 15 (pins 8 & 10)" %}

Again in the `boot` directory, open the `config.txt` file in your text editor. This file should already exist, so we'll just need to scroll to the end of the file and add the following text:

```bash
# Enable UART
enable_uart=1
```

Close the file and save it.

## Preflight Check

Before removing the SD card from your computer, double check that you have the following highlighted files in your `boot` directory, and that they've been configured as discussed above.

{% include image.html file="boot-files.png" description="Required files in the 'boot' directory" %}

## Light It Up

At this point we're ready to fire up the Pi. Safely eject the SD card from your computer and insert it into the slot on the Pi Zero. Plug the power adapter into a wall socket and insert the Micro USB male end into the PWR IN port as shown below.

{% include image.html file="raspberry-pi-zero-pwr.png" description="Raspberry Pi Zero ports, note the PWR IN on the right" %}

There's no power light on the Pi Zero, but the green LED should be flickering at this point, indicating that the device is booting up.

## Communicate with the Pi Zero

Give the Pi a few minutes to finish booting, then try to ping the device and/or SSH into it. The default username is `pi` and the default password is `raspberry`.

```bash
ping -c 3 raspberrypi.local
```

```bash
ssh pi@raspberrypi.local
```

If these don't work, you can also try to use the Pi's LAN IP address instead of `raspberrypi.local`. You may need to use `nmap` or some similar utility to [discover the devices](https://www.raspberrypi.org/documentation/remote-access/ip-address.md) on your local network. Failing that, it may be time to plug in a keyboard and monitor, or [use the console cable](https://learn.adafruit.com/raspberry-pi-zero-creation/give-it-life) to troubleshoot.

## Housekeeping

Once you're logged into the Pi for the first time, it's a good idea to update the installed software prior to performing any other tasks.

```bash
sudo apt update
sudo apt upgrade
```

This could take quite a while on the first run, so it might be a good time to grab another beverage.

## Initial Configuration

Once you've updated all the software, you should run:

```bash
sudo raspi-config
```

This will brin up the Raspberry Pi Software Configuration Tool, which will allow you to compolete most of the configuration you'll need to do before starting a project or installing other software.

{% include image.html file="config.png" description="Raspberry Pi Software Configuration Tool" %}

It's usually a good idea to change the password, configure localization, and even change the hostname from `raspberrypi` to something unique, especially if you plan to have several devices running on the network.

## Conclusion

I hope this article helps you get started using your Raspberry Pi Zero W. Check out the References section below to read more details about this setup process, and to get ideas about where to go next. Best of luck!

---

### References

- Raspberry Pi Docs, [Installing Operating System Images](https://www.raspberrypi.org/documentation/installation/installing-images/)
- Raspberry Pi Docs, [Setting up a Raspberry Pi Headless](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md)
- Raspberry Pi Docs, [Remote Access](https://www.raspberrypi.org/documentation/remote-access/README.md)
- Raspberry Pi Docs, [Setting up a Wireless LAN via the Command Line](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
- Raspberry Pi Docs, [The boot folder](https://www.raspberrypi.org/documentation/configuration/boot_folder.md)
- Raspberry Pi Docs, [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md)
- Raspberry Pi Docs, [config.txt](https://www.raspberrypi.org/documentation/configuration/config-txt/README.md)
- Stack Exchange, [How to setup Raspbian Networking](https://raspberrypi.stackexchange.com/questions/37920/how-do-i-set-up-networking-wifi-static-ip-address/37921#37921)
- Adafruit, [Raspberry Pi Zero Headless Quick Start](https://learn.adafruit.com/raspberry-pi-zero-creation?view=all)
