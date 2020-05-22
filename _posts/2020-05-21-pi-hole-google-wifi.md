---
title: Pi-hole on Google Wifi
image: assets/images/pi-hole-bg.jpg
categories: electronics
tags: ["raspberry pi", featured]
---

One of the first projects I wanted to try with my new Raspberry Pi was running [Pi-hole](https://pi-hole.net/). Pi-hole is a software tool that blocks Internet ads before they ever download -- not just on a single web browser, but _on your entire network_. It's billed as "a black hole for Internet ads."

Since my home network is a first-generation [Google Wifi](https://store.google.com/product/google_wifi_first_gen) mesh network, Pi-hole setup is slightly different than with a garden-variety router.

## Pi-hole Basics

Pi-hole operates as a [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_sinkhole), essentially filtering every DNS request made by devices on your network. Requests for valid web services are returned, while requests for ads are blocked. Pi-hole filters the DNS requests for ads by comparing each request to a blacklist of known ad servers.

But unlike traditional browser-based adblock plugins, Pi-hole doesn't just _hide_ the ads a web page returns -- it never requests them in the first place! This makes page loads quicker and reduces network traffic, making everything just feel _zippier_.

On top of all that, Pi-hole comes with a nifty admin dashboard that shows you in real time all the requests made on your network, which devices are making the queries, and how many queries have been blocked.

{% include image.html file="pi-hole-admin.png" description="Pi-hole Admin Dashboard" %}

## Getting Pi-hole

It's a simple process to get Pi-hole running on your Google Wifi network:

1. Setup the Raspberry Pi
1. Assign the Pi a static IP address
1. Install the Pi-hole software
1. Configure Pi-hole as your DNS server

### 1. Setup the Raspberry Pi

Required equipment for this setup is as follows:

- Raspberry Pi Zero W (\$10)
- 16GB Micro SD Card, Class 10 (\$8)
- Micro-USB Power Supply, 5V 2.5A (\$5)
- [Pi Zero Case](https://flirc.tv/more/flirc-raspberry-pi-zero-case) (\$5--\$15)

I set up my Pi Zero in "headless" mode, so there was no need for plugging in keyboards or displays. I installed the latest Raspbian Lite image since I'd only be accessing the Pi-hole via SSH or the web-based admin dashboard. For more details, see the [Headless Raspberry Pi Zero W Setup]({% post_url 2020-05-20-raspberry-pi-zero-w-setup %}) article.

After I had the Pi Zero up and running, I used the `raspi-config` tool to change the the hostname from `raspberrypi` to `pi-hole` to differentiate it from other Raspberry Pi devices on the network.

### 2. Assign the Pi-hole a Static IP Address

Since Pi-hole is a server, it needs a static IP address to work properly. By default, Google Wifi uses [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol) to dynamically assign IP addresses to devices on the network. Using the Google Wifi app on your phone, you can manually assign a static IP address to any device you choose.

Open the Google Wifi app, and from the home network status screen select `Devices > pi-hole > ⋮ > Reserve IP`, where `pi-hole` is whatever hostname you chose for your Raspberry Pi.

{% include image.html file="wifi-ip-reservation.png" description="Assigning Pi-hole a Static IP address" %}

You can leave the existing IP address as your static IP, or you can enter something different -- just make sure that whatever you choose is not already assigned to another device. When you're done, reboot your Raspberry Pi.

### 3. Install Pi-hole

The simplest way to get Pi-hole up and running is to use the one-step automated installer. As noted in the documentation, [curling and piping to bash](https://pi-hole.net/2016/07/25/curling-and-piping-to-bash) is controversial, so if you'd like to inspect the script before you run it, you may want to follow the instructions for [alternate installation methods](https://github.com/pi-hole/pi-hole/#alternative-install-methods).

Otherwise, paste the following command into a terminal session on your Pi:

```bash
curl -sSL https://install.pi-hole.net | bash
```

This will launch the automated installer. Just follow the prompts and you'll be on your way. It takes a few minutes to install, so be patient, and the installer will occasionally need some input from you.

{% include image.html file="pi-hole-install.png" %}

The installer will remind you that Pi-hole needs a static IP address. Since you've already assigned one to your Raspberry Pi, the current network settings should be correct. Trust, but verify.

{% include image.html file="pi-hole-static.png" %}

When the installer finishes, you should get a message like the one below. Be sure to **save your login password** before closing this screen!

{% include image.html file="pi-hole-complete.png" description="Installation complete!  Don't forget to save your password!"%}

### 4. Configure Pi-hole as your DNS Server

The first thing you'll want to do once you get Pi-hole installed is to access the Admin dashboard from another computer on the network, just to make sure everything is working properly. Point your web browser to the static IP address you created for the Pi-hole, and you should see the web interface. For example, in my case Pi-hole lives at `http://192.168.86.10/admin/`. (Note: The domain name `http://pi.hole/admin/` won't work until after we finish the DNS setup.)

Next, open the Google Wifi app again and navigate to:

`Settings > Network & General > Advanced networking > LAN`

In the DHCP Address Pool section, set _both_ the "Starting IP" and the "Ending IP" address to the static IP address of the Pi-hole. We're going to use the Pi-hole's built-in DHCP server instead of Google Wifi's.

{% include image.html file="wifi-lan.png" %}

Finally, return to the Pi-hole web dashboard, and navigate to:

`Settings > DHCP`

In the DHCP Settings section, check the box next to "DHCP server enabled." Then, under the "Range of IP addresses to hand out," select a suitable range that's either above or below the static IP address of your Pi-hole server. Make sure the range is broad enough to include all of the devices on your network.

{% include image.html file="pi-hole-dhcp-settings.png" description="Enabling the DHCP server on the Pi-hole" %}

After making these changes, you should restart the Google Wifi network. Once all devices have reconnected with new DHCP leases, you can test everything by pointing a web browser at [http://pi.hole/admin](http://pi.hole/admin), which should now display the Pi-hole dashboard.

## Troubleshooting

If at any point during setup your phone or computer loses its connection to the network such that you can no longer communicate with the Pi-hole, you may need to temporarily change the DNS settings on your device to point directly at the Pi-hole rather than `8.8.8.8` or whatever the default setting is. Incidentally, this method also allows you to configure individual devices to either use or bypass the Pi-hole DNS server should it become necessary.

## Conclusion

Hopefully this article helped you get up and running with Pi-hole on your Google Wifi network, and that the two devices are living harmoniously together. For more information on Pi-hole setup and operation, check out the [Pi-hole Docs](https://docs.pi-hole.net/), or the links in the References section below.

{% include image.html file="pi-hole-google.jpg" description="Pi-hole and Google Wifi living in harmony" %}

---

### References

1. Adafruit, [Pi Hole Ad Blocker with Pi Zero W](https://learn.adafruit.com/pi-hole-ad-blocker-with-pi-zero-w/install-pi-hole)
1. MBR Reviews, [How to run Pi-Hole with Google Wifi](https://www.mbreviews.com/pi-hole-google-wifi-raspberry-pi/)
1. Medium, [Setting up Pi-hole and PiVPN on Google Wifi](https://medium.com/@patrikmarin/setting-up-pi-hole-and-pivpn-on-google-wifi-2e8a86947931)
1. Pi-hole Docs, [Configuring Your Device's DNS Server](https://discourse.pi-hole.net/t/how-do-i-configure-my-devices-to-use-pi-hole-as-their-dns-server/245)
1. Pi-hole Docs, [How to use Pi-hole’s Built-in DHCP Server](https://discourse.pi-hole.net/t/how-do-i-use-pi-holes-built-in-dhcp-server-and-why-would-i-want-to/3026)
