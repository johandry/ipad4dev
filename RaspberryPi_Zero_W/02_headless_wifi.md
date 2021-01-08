# Configure WiFi

It's possible to enable and configure WiFi headless, having just the Raspberry Pi Zero, without keyboard or monitor.

Having the microSD card with the OS image and SSH enabled (read [Installation](./01_Installation.md)), create the file `wpa_supplicant.conf` with the WiFi configuration either using the Mac or the iPad.

For more information refer to [Setting up a Raspberry Pi headless](https://www.raspberrypi.org/documentation/configuration/wireless/headless.md).

## Using macOS

1. Insert the microSD card with the OS image into the card reader

2. Open Terminal, to change directory to `/Volumes/boot`

3. Create the file `wpa_supplicant.conf` using any text editor with the following content

   ```bash
   country=US
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1
   network={
   	ssid="<your WiFi name here>"
   	psk="<your WiFi password here>"
   	key_mgmt=WPA-PSK
   }
   ```

4. Save the file and eject properly the microSD card

## Using iOS from the iPad

1. Insert the microSD card with the OS image into the card reader

2. Open iSH to create the file `wpa_supplicant.conf` , executing:

   ```bash
   cat <<EOF >wpa_supplicant.conf
   country=US
   ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
   update_config=1
   network={
   	ssid="<your WiFi name here>"
   	psk="<your WiFi password here>"
   	key_mgmt=WPA-PSK
   }
   EOF
   ```

3. Open File, you should see `boot` and `iSH` in the locations.

4. Go to `iSH` then go to `root` or the directory where you created the `wpa_supplicant.conf` file.

5. Select the file `wpa_supplicant.conf` , then the 3 dots next to Open or press on the file name to open a menu, and select **Move**.

6. Select the `boot` location/folder to copy the file there

7. (Optional) Go to `boot` to verify the file `wpa_supplicant.conf` is there with more than 130 bytes

8. Eject manually the microSD card

## Login to the Raspberry Pi Zero W

1. Insert the microSD card to the Raspberry Pi Zero W

2. Turn it on (plug in the power cable) and wait a few minutes until the system is up and running

3. Open Terminal to execute the following command:

   ```bash
   ssh pi@raspberrypi.local
   ```

4. If you get a `Host not found` , `Connection refused` or any other error, wait a few more minutes and try again the same command.

5. Enter `yes` when warn about the authenticity 

6. Type the password `raspberry`

From the iPad, follow similar instructions but using a SSH application such as Terminus on step #3. 

You cannot use the `ssh` on iSH using `raspberrypi.local` because mDNS or Avahi cannot work on Alpine. To use iSH get the IP address once connected then SSH to the Raspberry Pi Zero using the IP address. To find the Raspberry Pi Zero IP address, follow any of the remote headless methods mentioned in [IP Address](https://www.raspberrypi.org/documentation/remote-access/ip-address.md).

From macOS to get the IP address of the RaspberryPi Zero, execute:

```bash
arp -n raspberrypi.local
```

Once you are logged into the Raspberry Pi Zero, and using the same WiFi, get and write the IP Address so you can use  `ssh` from iSH.

```bash
ifconfig
# OR
ip addr show wlan0
```



## Initial setup

Once logged in, you should inmediately change the hostname and the password of the user `pi`. Then update and upgrade the system.

1. Execute `sudo raspi-config` . Use the arrow keys, tab, enter and escape to navigate through the application

2. Go to `System Options` > `S4 Hostname`

3. Replace the `raspberrypi` hostname for your own hostname, i.e. `rpizerow`

4. Go to `System Options` > `S3 Password`

5. Enter the new password twice

6. Go to `Finish` then reboot

7. Wait a few minutes to login back to the Raspberry Pi Zero

8. Execute the `ssh` command now using the new hostname and password, for example:

   ```bash
   ssh pi@rpizerow.local
   ```

9. Update the system executing:

   ```bash
   sudo apt-get update -y
   sudo apt-get full-upgrade -y
   ```

The IP Address of the RaspberryPi Zero should not change when connected to the same WiFi, but the hostname is different now. Identify the IP Address using the `arp` command on a Mac using the new hostname:

```bash
arp -n rpizerow.local
```

## Enable VNC

Enable VNC is a good idea if you'd like to use the graphical interface.

1. Execute `sudo raspi-config` . Use the arrow keys, tab, enter and escape to navigate through the application
2. Go to `Interface Options` > `P3 VNC` and follow the instructions
3. Go back, then go to `Finish` or press Escape





