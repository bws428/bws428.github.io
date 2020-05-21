---
title: Pi-hole on Google WiFi
image: assets/images/rpi-large.jpg
categories: electronics
tags: ["raspberry pi", featured]
---

The [Pi-hole](https://pi-hole.net/)<sup>®</sup> is a DNS sinkhole that protects your devices from unwanted content, without installing any client-side software." (like browser-plugin adblockers, etc.)

Pi-hole® - A black hole for Internet Ads

<img class="img-fluid" src="/assets/images/pi-zero-5.png" alt="Demo Image">
<span class="caption text-muted">A Raspberry Pi Zero W.</span>

Here is the [Pi-hole](https://pi-hole.net/) three-step setup:

1. Install a supported operating system
2. Install Pi-hole
3. Configure Pi-hole as your DNS server

There's also a step four, VNC, but we ain't doing that at this point.

Here's the link to the [Pi Zero setup]({% post_url 2020-05-20-raspberry-pi-zero-w-setup %})!

## Documentation

- https://docs.pi-hole.net/

- Adafruit, [Pi Hole Ad Blocker with Pi Zero W](https://learn.adafruit.com/pi-hole-ad-blocker-with-pi-zero-w/install-pi-hole)

- [Pi-hole Setup on Raspberry Pi Zero W](http://blog.deadlypenguin.com/blog/2019/02/11/pi-hole-setup-raspberry-pi-zero/)

- https://raspberrypi.stackexchange.com/questions/37920/how-do-i-set-up-networking-wifi-static-ip-address

- [Google WiFi (1st gen)](https://store.google.com/product/google_wifi_first_gen)

- Simultaneous dual-band Wi-Fi (2.4GHz / 5GHz)

- https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md

## Required Hardware

TODO

### parts list

- Raspberry Pi Zero W
- MicroSD Card
- Ethernet Adapter
- Case [Optional]
- Power Supply

## Install Pi-hole

The simplest way to get up and running is to use Pi-hole's one-step automated install.

**A word of caution:** as noted in the Pi-hole documentation, [curling and piping to bash](https://pi-hole.net/2016/07/25/curling-and-piping-to-bash) is controversial, so if you'd like to inspect the script before you run it, you may want to follow the instructions for [alternate installation methods](https://github.com/pi-hole/pi-hole/#alternative-install-methods) from the Pi-hole GitHub repo.

Otherwise, copy and paste the following into a terminal session:

```bash
curl -sSL https://install.pi-hole.net | bash
```

## Configure DNS Server

## Pi-hole Built-in DHCP Server

```bash
http://pi.hole/

or

192.168.86.XX
```

![Screenshot](https://piholenet.b-cdn.net/wp-content/uploads/2018/12/Screenshot-2018-12-19-17.39.58.png)

## When the Pi Zero loses WiFi

https://weworkweplay.com/play/rebooting-the-raspberry-pi-when-it-loses-wireless-connection-wifi/

---

### References

- [How to run Pi-Hole with Google WiFi](https://www.mbreviews.com/pi-hole-google-wifi-raspberry-pi/)
- [Setting up Pi-hole and PiVPN on Google WiFi](https://medium.com/@patrikmarin/setting-up-pi-hole-and-pivpn-on-google-wifi-2e8a86947931)
- [Configuring Your Device's DNS Server](https://discourse.pi-hole.net/t/how-do-i-configure-my-devices-to-use-pi-hole-as-their-dns-server/245)
- [How to use Pi-hole’s Built-in DHCP Server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026)
