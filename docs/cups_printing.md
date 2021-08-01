# How to Install CUPS Print Server on Raspberry Pi

CUPS is an open-source printing system developed by Apple for UNIX like operating systems. Running CUPS on a Raspberry Pi and attaching a USB printer to the device can make the printer accesiable to all the devices within the network, without having any WiFi or Enthernet interface on the printer. I use CUPS to use my ultra-cheap Ricoh SP111 Laser printer as a netwrok printer for my Home.

Here are the steps.

## Step 1
 Log in to your Pi ssh using ssh username@<ip-addr> example my Pi username is still Pi and my Pi's IP address is so I will use  ssh pi@192.168.1.20 ant enter your Pi password when prompted
 using your Windows Powershell, Mac Terminal or Linux Bash

## Step 2
Though not mandatory it is advisable to update the system 

```bash
sudo apt-get update
```
Then, proceed to install CUPS

```bash
sudo apt install cups
```
