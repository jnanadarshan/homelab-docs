# How to Install CUPS Print Server on Raspberry Pi

?> This is a beginer friendly tutorial

CUPS is an open-source printing system developed by Apple for UNIX like operating systems. Running CUPS on a Raspberry Pi and attaching a USB printer to the device can make the printer accesiable to all the devices within the network, without having any WiFi or Enthernet interface on the printer. I use CUPS to use my ultra-cheap Ricoh SP111 Laser printer as a netwrok printer for my Home.

Here are the steps.

## Step 1 - SSH into your System
 Log in to your Pi ssh using `ssh username@<ip-addr>` example my Pi username is still `pi` and my Pi's IP address is `192.168.1.20` so I will use  `ssh pi@192.168.1.20` and hit `Enter`. Then input your Pi `password` when prompted
 This can be done using your Windows Powershell, Mac Terminal or Linux Bash

## Step 2 - Update system and install CUPS
Though not mandatory it is advisable to update the system 

```bash
sudo apt-get update
```
Then, proceed to install CUPS

```bash
sudo apt install cups
```
## Stpe 3 - Add Default user to Print Group
Then you need to add your default user to the Print group so that it can access printer. Use the below format
`sudo usermod -aG lpadmin <usename>` replace `username` with your pi user name, my Raspberry Pi username is  `pi` so I will run
```bash
sudo usermod -aG lpadmin pi
```
## Step 4 - Enable Print Server access outside Host System
Now that CUPS is installed and you have added the user to Print Group, you can access the print server by loging in to your RaspberyPi using VNC or RDP. However the main goal of a home print server is to access it across the home network using any device like Laptops, Desktops and Mobile Devices. To enable this we need to enable remote access to the CUPS server from any device.

Run the below command for this
```bash
sudo cupsctl --remote-any
```
Now you can access your CUPS server using any system in your network on Pi's IP Address and port `:631`
In my case I can access the server at

`http://192.168.1.20:631`