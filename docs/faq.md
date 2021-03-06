Title: Frequently Asked Questions
Authors: Nigel Bowden

# FAQ

## Where can I get the latest WLAN Pi image?

The latest WLAN Pi image is available [here][wlanpi_releases]

(avoid the alpha releases, they are unstable, development builds, Beta are usually pretty good but may have the odd bug)

## How do I find which image version is currently installed on my WLAN?

If you look on the top-level "home" page of the front panel display, the version number of the image should be shown on the top left of the display.

If you see no version number, it may be that you have quite an old version of image. If you have network connectivity to the WLAN Pi, browse to it  (192.168.42.1 if you're on the USB OTG connection) and look at the top of the web page to see the image version.

## How do I burn a WLAN Pi image?

If you'd like to burn a WLAN Pi image on to a kit you've bought or update the image of your existing WLAN Pi, here is a video explaining the process:

<iframe width="560" height="315" src="https://www.youtube.com/embed/sD4WlNyyWDs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## How do I change the password for the wlanpi user

Changing the default password for the wlanpi user is highly recommended to help secure your unit.

To change from the default password (which is 'wlanpi'), SSH to the WLANP Pi and use the 'passwd' command as follows:

```
wlanpi@wlanpi:~$ passwd
Changing password for wlanpi.
(current) UNIX password: wlanpi
Enter new UNIX password: <enter new pwd>
Retype new UNIX password: <enter new pwd again>
passwd: password updated successfully
wlanpi@wlanpi:~$
```

**If connecting the WLAN Pi to any network, changing the default password should be a top priority. If the WLAN Pi becomes compromised as default credentials have been left in place, this could have very serious consequences.**

## How do I change the hostname of my WLAN Pi

By default, the hostname of your WLAN Pi is : wlanpi

If you'd like to change this to a more meaningful hostname, then you will need to SSH to your WLAN Pi and update the ```/etc/hostname``` and ```/etc/hosts``` files, followed by a reboot of the WLAN Pi:

Edit the /etc/hostname file using the command:

```
 sudo nano /etc/hostname
```

There is a single line that says 'wlanpi'. Change this to your required hostname. Then hit Ctrl-X  and "y" to save your changes.

Alternatively, you may also use the following CLI command to achieve the same result:

```
sudo hostnamectl set-hostname <name>
```

Whichever method is used to update the hostname file, next edit the /etc/hosts file:

```
 sudo nano /etc/hosts
```
Change each instance of 'wlanpi' to the new hostname (there are usually two instances). Then hit Ctrl-X  and "y" to save your changes.

Finally, reboot your WLAN Pi:

```
 sudo reboot
```



## How do I set the timezone on my WLAN Pi?

From the CLI of your WLAN Pi:

```
sudo dpkg-reconfigure tzdata
```

## How do I connect my WLAN Pi to my wireless network?

Assuming your network uses a PSK, simply edit the following files:

- /etc/network/interfaces
- /etc/wpa_supplicant/wpa_supplicant.conf


### /etc/network/interfaces 

Edit the file as follows:

```
sudo nano /etc/network/interfaces
```

Edit the following section of the file for wlan0:

```
# Wireless adapter #1
# Armbian ships with network-manager installed by default. To save you time
# and hassles consider using 'sudo nmtui' instead of configuring Wi-Fi settings
# manually. The below lines are only meant as an example how configuration could
# be done in an anachronistic way:
#
allow-hotplug wlan0
iface wlan0 inet dhcp
#address 192.168.0.100
#netmask 255.255.255.0
#gateway 192.168.0.1
#dns-nameservers 8.8.8.8 8.8.4.4
#   wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
# Disable power saving on compatible chipsets (prevents SSH/connection dropouts over WiFi)
wireless-mode Monitor
#wireless-power off
```

Remove the "#" character from the start of the line:

```
#   wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

Save the file by hitting CTRL-x

### /etc/wpa_supplicant/wpa_supplicant.conf

Edit the file as follows:

```
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
<make your changes>
CTRL-x
```
Enter your SSID and PSK in the areas indicated in the file. For the new settings to take effect, you have 3 options:

* Unplug and re-insert your USB wireless NIC
* Issue the following command on the WLAN Pi CLI : ```sudo ifdown wlan0; sudo ifup wlan0```
* Reboot your WLAN Pi (remove the power or issue the ```sudo reboot``` command on the CLI) 

(For other security methods, look in the following file for configuration examples: /home/wlanpi/wiperf/conf/etc/wpa_supplicant/wpa_supplicant.conf)

## Which wireless adapters are supported on the WLAN Pi

Up to image version (but not including) v1.9, the only WLAN chipset supported was the Realtek 8812au. The Comfast CF-912AC was the only adapter that had been tested to provide support for all WLAN Pi features.

From v1.9 onwards, support for the Realtek 8812au chipset continued, but a driver for the MediaTek mt7610u chipset was also added. Testing was performed with a wide range of adapters using both chipsets. The range of features supported varied quite widely and the CF-912AC is still the only adapter that supports all features. However, other adapters do work to varying degrees and an alternative adapter may meet your needs. The [testing results][adapter_sheet] showing the adapters tested and the WLAN Pi features that work with each one are shown in this sheet: [link][adapter_sheet]

(In a future version of the WLAN Pi image, there will be far more extensive support of features for MediaTek adapters that will be provided by a kernel update within the image.)

## Where can I get help support with my WLAN Pi?

Support is on a volunteer/best efforts basis by project volunteers. Try [here][support]

## How do I suggest a new feature for the WLAN Pi?

If you have a feature suggestion for the WLAN Pi, please get along to the GitHub site for the project and open an issue ticket with your suggestion: [link][suggestions] (this will need a (free) GitHub account to create an issue)

<!-- Link list -->
[support]: support.md
[wlanpi_releases]: https://github.com/WLAN-Pi/wlanpi/releases
[burn_image]: https://youtu.be/sD4WlNyyWDs
[adapter_sheet]: https://docs.google.com/spreadsheets/d/1yAjO2vZuIfJ9BwI5cQ_qu72HpyEuETj4Zd7bWBnskDM/edit#gid=0
[suggestions]: https://github.com/WLAN-Pi/wlanpi/issues

