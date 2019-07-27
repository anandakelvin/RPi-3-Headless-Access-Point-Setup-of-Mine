# RPi-Headless-Access-Point-Setup-of-Mine
I decided to make this tutorial repo considering this project solves one of my main problems.
I live in a campus dormitory for internet connectivity, I rely on campus WiFi which is unstable for regular laptop built-in wireless card because of it's coverage range, so I managed to get 802.11AC USB Wireless Adapter. But there are only Windows driver available for this chipset, and it sucks to always use a dongle because my MacBook Pro does not has any USB ports. Because there is not any workoaround to this problem and with some brainstorming of what could be possible, I managed to buy a Raspberry Pi 3B and planned to set this as my backup server and an Access Point.

Please note that this project workability has been tested with Raspberry Pi 3B (2015 model) with Raspbian Stretch OS.

### Overview
###### This helps someone who:
 - Does not know how to configure his RPi without monitor to set it as headless (without peripherals), and
 - Wants to make his RPi as an Access Point with 2 wireless interfaces

###### My prerequisites:
 - Raspberry Pi 3B
 - 8 GB Micro SD card for Raspbian OS
 - Micro SD card Adapter
 - 5V 2,5A Power Adapter (equivalent to a smartphone charger)
 - USB Cable
 - USB WiFi dongle (as the second WiFi interface for connecting long range WiFi available, 802.11AC is recommended as its capability of simultanous 2.4/5 GHz bands)

###### How it works:
The RPi acts as an Access Point which will rely on its both wireless interfaces, the built-in one will act as a Hotspot which users will connect to, and the USB one will connect to any available WiFi router to gain internet connectivity. The concept is similar to what people call as "Bridging".

### Instructions
###### Installing Raspbian OS for RPI in the Micro SD Card
1. Go to https://www.raspberrypi.org/downloads/raspbian/ to download the zip file of Raspbian OS ISO image (Stretch is recommended)
2. Unzip and burn the ISO image into the Micro SD Card. Follow [this](https://www.raspberrypi.org/documentation/installation/installing-images/) instruction
3. To be able to control your RPi headlessly without peripherals, you need to make your RPi can be connected via SSH. To do that, plug in your SD Card adapter with your pre-installed Raspbian OS Micro SD Card attached, go to 'boot' partition, put an empty file named 'ssh' in it
4. In order to control your RPi over SSH, your RPi and computer must be on the same network. To set your RPi to connect to available WiFi automatically, put a file named 'wpa_supplicant.conf' and type in like  [this](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md) instruction inside the file.
###### Connecting to your RPi from another computer
1. Connect to the same network and open CMD if you're on Windows or Terminal if you're on Linux/MAC OS, type 'ssh pi@raspberrypi.local'
2. When it prompts for password, type 'raspberry'.
3. You should see something like 'pi@raspberrypi:~ $' in your console to determine you're connected to RPi.
4. If not, google around how to perform SSH.
###### Installing the USB Wireless Adapter driver for your RPi
1. Plug in your USB WiFi adapter to the RPi
2. In the SSH session, type 'ifconfig' to see your network interfaces.
3. If you see interface 'wlan1', skip this step. Otherwise, from your computer find the driver of your card chipset in [here](http://downloads.fars-robotics.net/wifi-drivers/) and download it
4. Send the driver file to your RPi using SCP, google around how to perform SCP
5. In the SSH session, extract the driver file. google around how to perform tar.gz extraction.
6. You should see file named 'install.sh', and run that file. Google around running .sh file as root
7. Restart your RPi
8. Connect to your RPi and in console, run 'ifconfig'
9. You should see an interface named 'wlan1'

The interface 'wlan0' refers to the built-in wireless card of your RPi, and 'wlan1' refers to the external USB Wireless Adapter. But the problem is, sometimes the interface name would be exchanged during boot, and to distinguish which one is one is by take a look at its MAC address, when your run 'ifconfig', the interface MAC address is next to the word 'ether'.

To solve this problem, I'll leave it with you. There are many ways of method to solve this.
###### Set your RPi as an Access Point
1. On this step, I'll just reference you to the instructions from the Raspberry Pi official documentation [here](https://www.raspberrypi.org/documentation/configuration/wireless/access-point.md). Follow the instructions until 'Configuring the access point host software (hostapd)'
2. That tutorial addresses to setting up the Raspberry Pi to bound internet traffic from wlan0 to eth0, but in this case we're trying to bound the internet traffic from wlan0 to wlan1. So the instruction 'Add routing and masquerade' for us would be slightly different
3. What I mean by that is, replace the word 'eth0' to 'wlan1' in each step.

###### If you're trying to bound internet traffic with a Captive Portal login
My workaround to this problem is MAC Spoofing. As my campus WiFi is passwordless but requires user to log into its Captive Portal webpage until it renew its leases, and it sorta block handshakes from my RPi, and only allowing students to log in using Mac/Windows. I trick the router by changing the wlan1 MAC address to my laptop MAC address, and log in to the network using my laptop when I attend lectures by the day. For more details, google around MAC Spoofing.

If you're following this tutorial, feel free to open issue if you've experienced any trouble. And my apologies for this less effort writings.
