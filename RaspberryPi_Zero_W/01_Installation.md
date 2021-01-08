# How to install a Raspberry Pi Zero W from macOS

This is the procedure I followed to install Raspberry Pi OS Lite on a Raspberry Pi Zero W from macOS Big Sur.

## Requirements

* Raspberry Pi Zero W
* MicroSD Card with more than 8GB  (i.e. SanDisk Ultra 64GB, XC 1). Check the [microSD Card Requirements](https://www.raspberrypi.org/documentation/installation/sd-cards.md)
* SD or microSD card reader

## Raspberry Pi OS Lite Installation using Raspberry Pi Imager

1. Dowload Raspberry Pi Imager from the [Raspberry Pi OS](https://www.raspberrypi.org/software/) page
2. Insert the microSD card to the SD/microSD card reader. Verify the content, it will be erased in the next step.
3. Execute it, then:
   1. Choose the OS, in this case it is: Raspberry Pi OS (Other) > Raspberry Pi OS Lite (32 Bit)
   2. Choose the SD Card selecting the inserted microSD card
   3. Click on Write, click on Yes, and wait a few minutes to have the image on the microSD card
4. Eject manually the microSD card, insert it again and open Finder to view the content
5. Open a Terminal to change directory to `/Volume/boot` 
6. Try to write the file `ssh` , if you get a `Read-only file system`  error or cannot write the file, try the manual installation. Otherwise, proceed to setup SSH connectivity

```bash
cd /Volume/boot
touch ssh
```

## Raspberry Pi OS Lite Manual Installation

1. Download the Raspberry Pi OS flavor from [Operating system images](https://www.raspberrypi.org/software/operating-systems/). In this case I choose [Raspberry Pi OS Lite](https://downloads.raspberrypi.org/raspios_lite_armhf/images/raspios_lite_armhf-2020-12-04/2020-12-02-raspios-buster-armhf-lite.zip)

2. Unzip the downloaded image (if not done by Safari) to get the `.img` image file. Example: `2020-12-02-raspios-buster-armhf-lite.img`

3. Identify the device name using Disk Utility application or `diskutil list` command. The device name would be something like `/dev/diskN` , in this example it is `/dev/disk2`.

4. Unmount the partition (do not eject it) either using Disk Utility application or the command `diskutil unmountDisk /dev/diskN`

5. Execute the following command to copy the image:

   ```bash
   sudo dd bs=1m if=path_of_your_image.img of=/dev/rdiskN; sync
   ```

6. Eject the microSD card using Disk Utility application or the command: `sudo diskutil eject /dev/rdiskN`

```bash
cd ~/Downloads

FName=2020-12-02-raspios-buster-armhf-lite
wget https://downloads.raspberrypi.org/raspios_lite_armhf/images/raspios_lite_armhf-2020-12-04/${FName}.zip
unzip ${FName}.zip

diskutil list
N=2
diskutil unmountDisk /dev/disk${N}

sudo dd bs=1m if=${FName}.img of=/dev/rdisk${N}; sync
```

In case of any error or for more information, refer to the following pages:

* [Installing operating system images](https://www.raspberrypi.org/documentation/installation/installing-images/)
* [Copying an operating system image to an SD card using Mac OS](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)

## Other OS Installation

If you are using Raspberry Pi Imager choose the OS you'd like to install, some of the options are:

* Raspberry Pi OS Desktop (default)
* Raspberry Pi OS Lite: It's Raspberry Pi OS with no Desktop environment. Choose Raspberry Pi OS (other) > Raspberry Pi OS Lite
* Raspberry Pi OS Full: It's Raspberry Pi OS with desktop and recommended applications. Choose Raspberry Pi OS (other) > Raspberry Pi OS Full

If you are installing manually, go to the following pages to download the desired OS to install:

* [Operating system images](https://www.raspberrypi.org/software/operating-systems/)

Some OS are not recommended on the Raspberry Pi Zero, verify if it's supported before download it.

## Enable SSH

To enable SSH the easy way, just create the file `ssh` in the new image. This can be done on macOS or on iOS from the iPad.

### Enable SSH on macOS

1. Insert the microSD card into the SD card reader.
2. Open Terminal, to change directory to `/Volumes/boot`
3. Create an empty file named `ssh`

```bash
touch /Volumes/boot/ssh
```

### Enable SSH from the iPad

1. Insert the microSD card into the SD card reader connected to your iPad
2. Open iSH to create the empty file `ssh` , executing `touch ssh` 
3. Open File, you should see `boot` and `iSH` in the locations.
4. Go to `iSH` then go to `root` or the directory where you created the `ssh` file.
5. Select the file `ssh` , then the 3 dots next to Open or press the file, and select **Move**.
6. Select the `boot` location/folder to copy the file
7. (Optional) Go to `boot` to verify the file `ssh` is there with size 0.