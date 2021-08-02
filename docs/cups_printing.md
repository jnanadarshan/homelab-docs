# How to Install CUPS Print Server on Raspberry Pi and use Ricoh SP111 as a Network Printer

?> This is a beginner friendly tutorial

CUPS is an open-source printing system developed by Apple for UNIX like operating systems. Running CUPS on a Raspberry Pi and attaching a USB printer to the device can make the printer accessible to all the devices within the network, without having any WiFi or Ethernet interface on the printer. I use CUPS to use my ultra-cheap Ricoh SP111 Laser printer as a network printer for my Home.

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

Now you can turn on your Printer and connect to Raspberry Pi via USB. Then head to the CUPS web interface using the IP and Port as mentioned above. However, if you have a old printer you might need to go few extra steps. Below are the steps for my Ricoh SP111 which I follow to get the printer working with CUPS.  


# Add Ricoh SP111 Printer to CUPS
There are couple of dependencies that needs to be installed before we can add the CUPS PPD files which are like drivers for the Ricoh SP111 printer. Though I have SP111 the system should be work for any SP100 series printer. 

Now head over to the terminal with SSH and run the below commands.

```bash
sudo apt-get install jbigkit-bin
```

```bash
sudo apt-get install inotify-tools
```

We need to get the PPD files from Github. In case you have never installed Git use the below command to install Git on Raspberry Pi.

```bash
sudo apt install git
```

After installing Git use the below command to download the necessary files from the repository. 
```bash
sudo git clone https://github.com/droidzone/ricoh-sp100.git
````
I have seen sometimes the old respos are removed, so in the unlikely case the above repo has been removed I have made it a fork on below which can be used alternatively.

```bash
sudo git clone https://github.com/jnanadarshan/ricoh-sp100.git
```

After download, verify that the folder `ricoh-sp100` is there using `ls` command though it is optional.

Now `cd` into the downloaded repo to copy necessary files to CUPS folder in the Pi.

```bash
cd ricoh-sp100
```

Use the below command to copy the necessary files to CUPS folder.

```bash
sudo cp pstoricohddst-gdi /usr/lib/cups/filter
```

Then we need to modiy the permissions before we can use the new PPD or drivers.

```bash
sudo chown root:root /usr/lib/cups/filter/pstoricohddst-gdi
```


Now at this point everything is complete and you can head over to the CUPS interface to add printer and if your printer is powered on and connected via USB to the Pi you should be able to add it to CUPS. However I saw this issue of printing blank pages which quite some other people have also faced. To fix printing blank pages on Raspbian or Ubuntu you can install the below library and it will start working.

```bash
sudo apt-get install imagemagick-6.q16
```

I suggest you restart the `cups.service` just to make sure all the latest libraries are being used.
```bash
sudo systemctl restart cups.service
```

Everything is finished and now you can head over to `192.168.1.20:631` and `Add Printer` where you can see `Ricoh SP111` listed and when you hit add you should also see the `Ricoh SP111 DDST` drivers available already. 