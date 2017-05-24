# Getting wireless working on the beagle bone black wireless
How to enable wifi on the beagle bone black wiress  from the command line

The beagle bone black wireless is a great little maker tool, but the documentation can be frustrating to navigate, especially for getting up and running with the wireless chip. The beagle bone black wireless getting started page doesn't have a lot of info on it, which is sort of frustrating, given that the whole point of having the BBBW is to use wifi!

Anyway, rant out of the way, here's how to get it working:

First you need to get access to the command line on the BBBW. There are two easy ways to do this.

Regardless which method you use, you'll have to get network access over USB working. If you're using windows, the BONE_64.exe driver works reasonably well one you jump through all the hoops to get it installed.

For some reason the BBBW devs haven't signed the BONE_64.exe usb driver so that it can be installed easily on windows 10. It's not particularly easy to install it, unfortunately just saying "Run as Administrator" won't work.

You have to reboot into advanced mode, I find the easiest way to do this is to hold down the shift button when you click restart.

After you've restarted in advanced mode, click on "Troubleshoot", then "Advanced Options", and finally "Startup Settings". This will take you to a screen where the only thing you can do is restart *again*. Thanks windows for the super intuitive process.

Once you have restarted, a list should come up that says Startup Settings. Hitting F7 will select "Disable driver signature enforcement", and will finally boot into windows. Once you've done this, you can run the BONE_64.exe driver installer and everything should install properly.

On linux and mac, you don't need to do any of this, they work a lot better with network over usb.

Now that you have network access to the BBBW, pick one of the following methods to get command line access:

# Method 1:

ssh root@192.168.7.2

This will just do a normal ssh into the magic IP address assigned to the BBBW.

# Method 2:

Open chrome and browse to http://localhost:3000/ide.html

This will open up the cloud9 development IDE. Down at the bottom of the IDE there's a tab that says bash - "beaglebone". That'll give you command line access.

# Running connmanctl

The next thing you want to do is run the linux application connmanctl. This is the command line application that manages, amongst other things, wifi settings. So, from the command line just type:

`connmanctl`

When that launches, you'll first have to disable tethering or nothing will work (this was a stumbling block for me for a while):

`connmanctl> tether wifi disable`

Next, enable wifi:

`connmanctl> enable wifi`

Now that wifi is enabled, we want to scan for all available wifi connections:

`connmanctl> scan wifi`

You should see it respond with "Scan complete". If not, something is wrong and you'll have to seek expert advice. When the scan has completed successfully, list out all of the services you just found:

`connmanctl> services`

This should display a list of all the access points in range. Locate the access point that you want to connect to, and copy it's identifier it'll look something like "wifi_506583d4dace_556d6272656c6c6120436f72706f726174696f6e_managed_psk". Once you've done that, you want to turn the agent on with:

`connmanctl> agent on`

With the agent running, you have to tell it to connect to the access point identifier you copied earlier:

`connmanctl> connect [paste access point identifier]`

Make sure you don't include the brackets above, it should just be something like connect wifi_506583d4dace_556d6272656c6c6120436f72706f726174696f6e_managed_psk 

That's it! To exit connmanctl, just type quit: 

`connmanctl> quit`

Test out your connection by pinging google:

`ping google.com`


