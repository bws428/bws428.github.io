---
title: Pi-hole on Google WiFi
image: assets/images/pi-hole-bg.jpg
categories: electronics
tags: ["raspberry pi", featured]
---

One of the first projects I wanted to try with my new Raspberry Pi was running [Pi-hole](https://pi-hole.net/). Pi-hole is a software tool that blocks internet ads before they ever download -- not just on a single web browser, but _on your entire network_.

## Pi-hole Basics

Pi-hole operates as a [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_sinkhole), essentially filtering every DNS request made by devices on your network. Requests for valid web services are returned, while requests for ads are blocked. Pi-hole filters the DNS requests for ads by comparing each request to a blacklist of known ad servers

But unlike traditional browser-based adblocking plugins, Pi-hole doesn't just _hide_ the ads a web page returns -- it never requests them in the first place! This makes page loads quicker and reduces network traffic, making everything just feel _zippier_.

On top of all that, Pi-hole comes with a nifty admin dashboard that shows you in real time all the requests made on your network, which devices are making the queries, and how many queries have been blocked.

{% include image.html file="pi-hole-admin.png" description="Pi-hole Admin Dashboard" %}

## Installing Pi-hole

It's a simple three-step process to get Pi-hole running on your home network:

1. Install a supported OS
2. Install the Pi-hole software
3. Configure Pi-hole as your DNS server

I'll be using a Raspbery Pi for my Pi-hole server, and running it on a Google WiFi mesh network, which adds a few challenges to the setup.

### 1. Installing an Operating System

Required equipment for my setup is as follows:

- Raspberry Pi Zero W (\$10)
- 16GB Micro SD Card, Class 10 (\$8)
- Micro-USB Power Supply, 5V 2.5A (\$5)
- [Pi Zero Case](https://flirc.tv/more/flirc-raspberry-pi-zero-case) (\$5--\$15)

I did all the setup of my Pi Zero W in "headless" mode, so there was no need for plugging in keyboards or displays. I installed the latest Raspbian Lite image rather than a full distro with the desktop GUI, since I'd only be accessing the Pi-hole via SSH or the web-based admin dashboard.

Read the full details in my [Headless Raspberry Pi Zero W Setup]({% post_url 2020-05-20-raspberry-pi-zero-w-setup %}) article.

### 2. Install Pi-hole

The simplest way to get up and running is to use Pi-hole's one-step automated install. As noted in the Pi-hole documentation, [curling and piping to bash](https://pi-hole.net/2016/07/25/curling-and-piping-to-bash) is controversial, so if you'd like to inspect the script before you run it, you may want to follow the instructions for [alternate installation methods](https://github.com/pi-hole/pi-hole/#alternative-install-methods) from the Pi-hole GitHub repo.

Otherwise, copy and paste the following into a terminal session:

```bash
curl -sSL https://install.pi-hole.net | bash
```

This will launch the automated installer. Just follow the prompts and you'll be on your way. It takes a few minutes to install, so be patient, and the installer will occasionally need some input from you.

{% include image.html file="pi-hole-install.png" %}

Pi-hole needs a static IP address to function correctly. You can let the installer use whatever dynamic IP address your router has assigned to it, or you can set a different unique address yourself.

{% include image.html file="pi-hole-static.png" %}

When the installer finishes, you should get a message like the one below.

{% include image.html file="pi-hole-complete.png" %}

<div class="alert alert-primary" role="alert">
  <b>Note:</b> Be sure to copy and save your login password before closing this screen!
</div>

### 3. Configure DNS

#### Pi-hole Built-in DHCP Server

```bash
http://pi.hole/

or

192.168.86.XX
```

![Screenshot](https://piholenet.b-cdn.net/wp-content/uploads/2018/12/Screenshot-2018-12-19-17.39.58.png)

---

### References

- https://docs.pi-hole.net/

- Adafruit, [Pi Hole Ad Blocker with Pi Zero W](https://learn.adafruit.com/pi-hole-ad-blocker-with-pi-zero-w/install-pi-hole)

- [Pi-hole Setup on Raspberry Pi Zero W](http://blog.deadlypenguin.com/blog/2019/02/11/pi-hole-setup-raspberry-pi-zero/)

- https://raspberrypi.stackexchange.com/questions/37920/how-do-i-set-up-networking-wifi-static-ip-address

- [Google WiFi (1st gen)](https://store.google.com/product/google_wifi_first_gen)

- Simultaneous dual-band Wi-Fi (2.4GHz / 5GHz)

- https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md

1. MBR Reviews, [How to run Pi-Hole with Google WiFi](https://www.mbreviews.com/pi-hole-google-wifi-raspberry-pi/)
1. Medium, [Setting up Pi-hole and PiVPN on Google WiFi](https://medium.com/@patrikmarin/setting-up-pi-hole-and-pivpn-on-google-wifi-2e8a86947931)
1. Discourse, [Configuring Your Device's DNS Server](https://discourse.pi-hole.net/t/how-do-i-configure-my-devices-to-use-pi-hole-as-their-dns-server/245)
1. Discourse, [How to use Pi-hole’s Built-in DHCP Server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026)
