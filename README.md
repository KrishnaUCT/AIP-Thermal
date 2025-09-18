Exploring low-cost thermal camera applications. 

# Hardware

- Processing: [Raspberry Pi 5](https://datasheets.raspberrypi.com/rpi5/raspberry-pi-5-product-brief.pdf)
- Hardware Interface: [FLIR Breakout Board](https://cdn.sparkfun.com/assets/8/c/a/9/6/DS-FLiR_Lepton_-_Breakout_Board_V2.pdf)
- Thermal Camera: [FLIR Lepton 3.1R](https://mm.digikey.com/Volume0/opasdata/d220001/medias/docus/5491/Lepton_Series_7-28-23.pdf)

## Interfacing

To connect the camera to the Raspberry Pi, refer to the following guides:
- [Breakout board pin connections](https://groupgets-files.s3.amazonaws.com/lepton/Resources/Getting%20Started%20with%20the%20Raspberry%20Pi%20and%20Breakout%20Board%20V2.0.pdf)
- [Raspberry Pi Pinout](https://pinout.xyz/)

**Take special note to not interchange GPIO pin numbers with header pin numbers.** Header pin numbers are sequential. GPIO pins are numbered for pins with GPIO functionality.

# Software

The Raspberry Pi is running Debian 12.

As this project is still in it's early phase, we are currently using https://github.com/tannerliu347/Pylepton to capture images from the thermal camera.
In the future, we may switch to a more actively maintained project or develop a custom solution.

## Networking

Changing the network proves quite a challenge. Assuming you are using NetworkManager, and have already connected to a network, the easiest method to change the network is to manually edit `/etc/NetworkManager/system-connections/Wi-Fi\ connection\ 1.nmconnection`. See [this StackOverflow thread](https://askubuntu.com/questions/615245/how-can-i-update-a-network-manager-connection-from-the-command-line) for more information.

To do this, you either need to be connected to the old network or have a system that can supports ext4 filesystems.
- If you are connected to the old network, simply ssh into the Pi and edit the file using your text editor of choice.
- If you are running an OS that supports the ext4 filesystem, power off the Pi and use a sdcard reader to view the filesystem.

Simply edit the "ssid" under \[wifi\] and "psk" under \[wifi-security\] to match the new network.

## Capturing Images

Connect to the Raspberry Pi using ssh:
``` bash
ssh <username>@<ip>
```

Clone the Pylepton software from https://github.com/tannerliu347/Pylepton:
``` bash
git clone https://github.com/tannerliu347/Pylepton
```
Note, you may need to install dependencies and drivers. Please refer to the repository above for that.

Capture an image:
``` bash
./pylepton_capture output-image-name.png
```

If there are multiple lines of "Garbage frames", there may be a problem with your hardware.

## Viewing Images

If you are running in headless mode, you cannot view the image directly on the Raspberry Pi.

To copy the file to your machine, use [rsync](https://github.com/RsyncProject/rsync.git). Most linux distributions include it in their repositories.
``` bash
# Copy all png files in /home/username/Pylepton/ to the current directory on the host
rsync <username>@<ip>:/home/username/Pylepton/*.png .
```
Replace \<username\> and \<ip\> with the username and IP address of the Raspberry Pi.

# Dataset

We are currently working on exploring the capabilites of the thermal camera. The images are available [here](https://drive.google.com/drive/folders/1-9IPE6QjecM_AsmYUu2huvvXA5-sMLge?usp=sharing). 

The plan is to eventually curate an application specific dataset.

