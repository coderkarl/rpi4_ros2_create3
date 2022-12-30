# rpi4_ros2_create3

This repo documents the setup for a raspberry pi 4 with ros2 pre-installed. For now, it will NOT include the following, but users should be aware of the following pre-reqs (already done on the provided sd card):

## Pre-reqs already done (for reference)

- Flashing the sd card with the operating system  
- Adding a new network setup file in /etc/netplan/50-cloud-init.yaml  
- Enable ssh, i2c  
- Install ros2 and make your /home/<user>/ros2_ws/src/ directory for custom packages  
- Add the following ros2 custom packages to the ros2_ws/src/ directory and compile  
  git clone -b ros2 https://github.com/coderkarl/xv_11_laser_driver.git  
  git clone <raspicam2 url> (We will return to this /home/<user>/ros2_ws/src/raspicam/ package folder to see the repo and local changes
  
## Start here. Rpi 4 network setup for your router

### Determine your ubuntu laptop (or virtual box) ip address (OPTION 1)

Open a terminal in ubuntu (Ctrl + Alt + t) or click the bottom left square matrix and type "terminal"  
From the ubuntu programs menu, right click terminal and add to favorites  
In the terminal, type  
`ip addr`  
Find your wifi (or ethernet if wired) ip address, likely 192.168.x.y/z  
We will use the same x and z numbers for the rpi4, and a unique number for y  

### Determine your ubuntu laptop (or virtual box) ip address (OPTION 2)

Install the FING app on your smartphone and connect your phone to the wifi router you want to use.  
Click SCAN on the FING app and observe a list of devices and their ip addresses.
Use the 192.168.x.y to determine how to modify the rpi4 network settings.  
Again, we want the 192.168.x numbers to match, but need to assign unique y number for the rpi 4.  

### Update rpi4 /etc/netplan/50-cloud-init.yaml (OPTION 1)

Connect the usb c power supply with a switch to the rpi4 usb c port  
Note the power supply switch has a | symbol on it for ON  
Connect the micro hdmi to hdmi cable to the rpi4 micro hdmi port (next to usb c)  
Connect the hdmi cable to a monitor  
Connect a usb keyboard to the rpi4 usb port  
Note no mouse is needed. The rpi4 only has an ubuntu server installed (no gui dekstop)  
Turn on the rpi4 and observe the terminal output on the monitor  
Wait for the rpi4 to boot up and provide a terminal command prompt  
`sudo nano /etc/netplan/50-cloud-init.yaml`  
You will be asked to enter the password and you will not see any password characters appear. Hit Enter.  
Modify the 192.168.x.y addresses. You should be able to leave the last y digit unchanged unless another device on the network uses that.  
Update the ssid and password strings at the end of the wifis section.  

### Update rpi4 /etc/netplan/50-cloud-init.yaml (OPTION 2)

This does NOT require connecting a monitor and keyboard to the rpi4.  
Remove the micro sd card from the rpi 4.  
Using an sd card adapter, connect the micro sd card to your laptop.  
From an ubuntu machine, open the sd card writable partition and navigate to /media/<user>/writable/etc/netplan/  
Verify you have found the location of the 50-cloud-init.yaml file  
Use Ctrl + l from the file explorer (nautilus) to verify the absolute path of the netplan folder.  
From an ubuntu terminal:  
`sudo nano /media/<user>writable/etc/netplan/50-cloud-init.yaml`  
Make the same changes described in OPTION1. Note this is your user password, not the rpi4 password.  
After making the changes, save with Ctrl + x and Enter (yes to save).  
Close the file explorer and eject the sd card and re-insert it into to rpi 4.  

### Test rpi 4 network

If you used a monitor and keyboard in option 1, after saving the network file:  
`sudo reboot`  
Verify you are connected to the network with:  
`ping <your laptop ip address`  
`sudo poweroff`  
Unplug the monitor and keyboard.  

Testing headless (no monitor or keyboard):  
Connect the usb c power supply to the rpi 4 and turn it on.  
Observe the green activity light blinking next to the red power led.  
Wait for activity light to stop or slow down.  
On your ubuntu machine terminal type:  
`ssh <rpi4 user>@<192.168.x.y>`  
Enter rpi4 password (no feedback, press enter)  
Hopefully you successfully connected.
In ssh session terminal (<rpi user>@<rpi hostname> before the prompt) shutdown with:  
`sudo poweroff`  

### Always turn off raspberry pi using command line

Do not simply remove power or turn off the switch. There is a small chance this will damage the sd card.  
