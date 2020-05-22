---
title: Pi-hole on Google Wifi
image: assets/images/pi-hole-bg.jpg
categories: electronics
tags: ["raspberry pi", featured]
---

One of the first projects I wanted to try with my new Raspberry Pi was running [Pi-hole](https://pi-hole.net/). Pi-hole is a software tool that blocks internet ads before they ever download -- not just on a single web browser, but _on your entire network_. It's billed as a "black hole for Internet ads."

## Pi-hole Basics

Pi-hole operates as a [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_sinkhole), essentially filtering every DNS request made by devices on your network. Requests for valid web services are returned, while requests for ads are blocked. Pi-hole filters the DNS requests for ads by comparing each request to a blacklist of known ad servers.

But unlike traditional browser-based adblock plugins, Pi-hole doesn't just _hide_ the ads a web page returns -- it never requests them in the first place! This makes page loads quicker and reduces network traffic, making everything just feel _zippier_.

On top of all that, Pi-hole comes with a nifty admin dashboard that shows you in real time all the requests made on your network, which devices are making the queries, and how many queries have been blocked.

{% include image.html file="pi-hole-admin.png" description="Pi-hole Admin Dashboard" %}

## Installing Pi-hole

It's a simple process to get Pi-hole running on your Google Wifi network:

1. Setup the Raspberry Pi
1. Assign the Pi a static IP address
1. Install the Pi-hole software
1. Configure Pi-hole as your DNS server

### 1. Setup the Raspberry Pi

Required equipment for my setup is as follows:

- Raspberry Pi Zero W (\$10)
- 16GB Micro SD Card, Class 10 (\$8)
- Micro-USB Power Supply, 5V 2.5A (\$5)
- [Pi Zero Case](https://flirc.tv/more/flirc-raspberry-pi-zero-case) (\$5--\$15)

I did all the setup of my Pi Zero W in "headless" mode, so there was no need for plugging in keyboards or displays. I installed the latest Raspbian Lite image rather than a full distro with the desktop GUI, since I'd only be accessing the Pi-hole via SSH or the web-based admin dashboard.

Read the full details in my [Headless Raspberry Pi Zero W Setup]({% post_url 2020-05-20-raspberry-pi-zero-w-setup %}) article.

After I had my new Pi Zero up and running, I used the `raspi-config` tool to change the the hostname from `raspberrypi` to `pi-hole` to differentiate it from other Raspberry Pi devices on the network.

### 2. Assign the Pi-hole a Static IP Address

> Option 2: DHCP enabled on Google Wifi, DHCP enabled on Pi-Hole: Google Wifi only gives an IP address to the Raspberry Pi, while the devices that connect to the Google Wifi will receive the IP address from the Raspberry Pi.

- Google Wifi app on your phone

`Devices > pi-hole > ⋮ > Reserve IP`

{% include image.html file="wifi-ip-reservation.png" %}

Restart your Pi.

### 3. Install Pi-hole

The simplest way to get up and running is to use Pi-hole's one-step automated install. As noted in the Pi-hole documentation, [curling and piping to bash](https://pi-hole.net/2016/07/25/curling-and-piping-to-bash) is controversial, so if you'd like to inspect the script before you run it, you may want to follow the instructions for [alternate installation methods](https://github.com/pi-hole/pi-hole/#alternative-install-methods).

Otherwise, paste the following command into a terminal session on your Pi:

```bash
curl -sSL https://install.pi-hole.net | bash
```

This will launch the automated installer. Just follow the prompts and you'll be on your way. It takes a few minutes to install, so be patient, and the installer will occasionally need some input from you.

{% include image.html file="pi-hole-install.png" %}

Pi-hole needs a static IP address to function correctly. You can let the installer use whatever dynamic IP address your router has assigned to it, or you can set a different unique address yourself. Since

{% include image.html file="pi-hole-static.png" %}

When the installer finishes, you should get a message like the one below.

{% include image.html file="pi-hole-complete.png" %}

<div class="alert alert-primary" role="alert">
  <b>Note:</b> Be sure to save your login password before closing this screen!
</div>

### 4. Configure DNS

`Network & General > Advanced networking > LAN`

{% include image.html file="wifi-lan.png" %}

#### Pi-hole Built-in DHCP Server

```bash
http://pi.hole/

or

192.168.86.XX
```

{% include image.html file="pi-hole-dhcp-settings.png" %}

{% include image.html file="pi-hole-google.jpg" description="Pi-hole and Google Wifi living in harmony" %}

---

### References

1. [Pi-hole Documentation](https://docs.pi-hole.net/)
1. Adafruit, [Pi Hole Ad Blocker with Pi Zero W](https://learn.adafruit.com/pi-hole-ad-blocker-with-pi-zero-w/install-pi-hole)
1. [Google Wifi (1st gen)](https://store.google.com/product/google_wifi_first_gen)
1. MBR Reviews, [How to run Pi-Hole with Google Wifi](https://www.mbreviews.com/pi-hole-google-wifi-raspberry-pi/)
1. Medium, [Setting up Pi-hole and PiVPN on Google Wifi](https://medium.com/@patrikmarin/setting-up-pi-hole-and-pivpn-on-google-wifi-2e8a86947931)
1. Discourse, [Configuring Your Device's DNS Server](https://discourse.pi-hole.net/t/how-do-i-configure-my-devices-to-use-pi-hole-as-their-dns-server/245)
1. Discourse, [How to use Pi-hole’s Built-in DHCP Server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026)
