# SSH over USB connectivity

SSH on WiFi is a good option but not easy to setup when you are not in your home or office WiFi. In situations where you are in a unknown or public WiFi it's recommended to use SSH with the Raspberry Pi Zero connected to the iPad with a USB cable.

Make sure VNC is enabled, if not, follow the instructions in [Enable VNC](./02_headless_wifi.md#enable_vnc).

Execute the following commands to enable SSH over USB:

```bash
sudo cp /boot/config.txt /boot/config.txt.bkp
echo "dtoverlay=dwc2" | sudo tee -a /boot/config.txt
sudo sed -i 's/#framebuffer_width=.*/framebuffer_width=1024/' /boot/config.txt
sudo sed -i 's/#framebuffer_height=.*/framebuffer_height=768/' /boot/config.txt

sudo cp /boot/cmdline.txt /boot/cmdline.txt.bkp
echo "modules-load=dwc2" | sudo tee -a /boot/cmdline.txt

sudo cp /etc/modules /etc/modules.bkp
echo "libcomposite" | sudo tee -a /etc/modules

sudo cp /etc/dhcpcd.conf /etc/dhcpcd.conf.bkp
echo "denyinterfaces usb0" | sudo tee -a /etc/dhcpcd.conf

sudo apt install dnsmasq -y

cat <<EOF | sudo tee /etc/dnsmasq.d/usb
interface=usb0
dhcp-range=10.55.0.2,10.55.0.6,255.255.255.248,1h
dhcp-option=3
leasefile-ro
EOF

IPAddress=10.55.0.1

cat <<EOF | sudo tee /etc/network/interfaces.d/usb0 
auto usb0
allow-hotplug usb0
iface usb0 inet static
  address $IPAddress
  netmask 255.255.255.248
EOF

cat <<EOS | sudo tee /root/usb.sh
#!/bin/bash
cd /sys/kernel/config/usb_gadget/
mkdir -p pi4
cd pi4
echo 0x1d6b > idVendor # Linux Foundation
echo 0x0104 > idProduct # Multifunction Composite Gadget
echo 0x0100 > bcdDevice # v1.0.0
echo 0x0200 > bcdUSB # USB2
echo 0xEF > bDeviceClass
echo 0x02 > bDeviceSubClass
echo 0x01 > bDeviceProtocol
mkdir -p strings/0x409
echo "fedcba9876543211" > strings/0x409/serialnumber
echo "Ben Hardill" > strings/0x409/manufacturer
echo "PI4 USB Device" > strings/0x409/product
mkdir -p configs/c.1/strings/0x409
echo "Config 1: ECM network" > configs/c.1/strings/0x409/configuration
echo 250 > configs/c.1/MaxPower
# Add functions here
# see gadget configurations below
# End functions
mkdir -p functions/ecm.usb0
HOST="00:dc:c8:f7:75:14" # "HostPC"
SELF="00:dd:dc:eb:6d:a1" # "BadUSB"
echo $HOST > functions/ecm.usb0/host_addr
echo $SELF > functions/ecm.usb0/dev_addr
ln -s functions/ecm.usb0 configs/c.1/
udevadm settle -t 5 || :
ls /sys/class/udc > UDC
ifup usb0
service dnsmasq restart
EOS
sudo chmod +x /root/usb.sh

sudo cp /etc/rc.local /etc/rc.local.bkp
sudo sed -i '/^exit 0$/i /root/usb.sh' /etc/rc.local

sudo shutdown -h now
```

When the RaspberryPi Zero is turned off, disconnect from the power source and plug it to the iPad Pro on the port labeled **USB** (not the port labeled **PWR**, the second port) using a USB C to mini USB cable.

## Verify connectivity

On the iPad go to **Settings**, wait for the Raspberry Pi to boot and you should see the **Ethernet** option in the left menu between WiFi and Bluetooth. Selecting the **PI4 USB Device**, you'll see the Raspberry Pi Zero network settings.

### Using SSH

Open iSH or any other SSH application to connect to the Raspberry Pi Zero IP with the user `pi`, in this case `10.55.0.1`, like this on iSH:

```bash
ssh pi@10.55.0.1
```

If you didn't changed the user `pi` password, the default one is `raspberry`. Make sure to change it for security reasons.

### Using VNC

Open your VNC client (i.e. **VNC Viewer** or **Screens**), add a new host using the IP address `10.55.0.1`. If the client allows it, enter the username `pi` and the password. If the VNC client allows it, you may also assign a name to the server.

## Clean up

If everything is working as expected, you may delete all the backup files created. This action is optional and it may be a good idea to keep the backup files, at least, for a while, just in case something goes wrong.

```bash
rm -f /boot/config.txt.bkp \
  /boot/cmdline.txt.bkp \
  /etc/modules.bkp \
  /etc/dhcpcd.conf.bkp \
  /etc/rc.local.bkp
```

