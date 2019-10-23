# LoRa-Gateway-using-RAK831

![alt-text](https://github.com/vyshakpadinjarote/LoRa-Gateway-using-RAK831/blob/master/Images/gateway.png)

Recently I started working with Lora, specifically LoraWAN. But the area I live in has no gateway around so I had to buy my own. This video is all about setting up a cheap Lora Gateway using the RAK831 Lora Concentrator and Raspberry Pi. The gateway is configured with The Things Network.

## Procedures

The first thing we need to do is assemble the Raspberry Pi and the gateway if it is not already assembled. It is pretty much straightforward, but you can check the video for that.

After that, we need to prepare the SD card by flashing the Raspbian Lite. You can download the latest version of the OS from [here](https://www.raspberrypi.org/downloads/raspbian/). For flashing the OS you can use [Etcher](https://www.balena.io/etcher/). After OS get flashed to the SD card (I have used 16 GB one) don’t forget to create a file named SSH (without any extension) to the SD card, which will enable SSH connection to the Pi.

After booting the Pi with the SD card and having an SSH connection, next is to install the required packages for the gateway,

First, we need to update the pi, followed by installation of git. After that SPI has to be enabled from raspi-config.

```
sudo apt-get update
sudo apt-get install git
sudo raspi-config
```

After git is installed use the command below to clone the spi branch of the ttn-zurich ic880a gateway repository. This will enable the communication to the gateway from the Pi.

```
git clone -b spi https://github.com/ttn-zh/ic880a-gateway.git
```

Now navigate to the cloned folder ```cd ~/ic880a-gateway``` and edit the start.sh by replacing the pin from 25 to 17. 
![alt text](https://github.com/vyshakpadinjarote/LoRa-Gateway-using-RAK831/blob/master/Images/Screenshot%20(55).png)

For that use the command ```sudo nano start.sh``` and now run the install.sh script to proceed with the installations.

```
sudo ./install.sh
```

It will prompt for remote configuration setup or local. You can select anyone but if you are going for the remote you need to push config file to [github.com/ttn-zh/gateway-remote-config](github.com/ttn-zh/gateway-remote-config) repository. You can do so by adding your config file to the repository and making a pull request, and it might take up to 24 hours for them to approve. You need to copy the EUI from here which will be used while registering the gateway with TTN.

After that just fill up the details as prompted.

Now the gateway has to be registered to TTN and for that get into TTN Console. If you don’t have an account you will be prompted to create one.  Click on Gateways and select add a new gateway.

![alt text](https://github.com/vyshakpadinjarote/LoRa-Gateway-using-RAK831/blob/master/Images/Screenshot%20(53).png)

You will be prompted to fill in some details so fill it up. The important thing is to fill the Gateway ID (Paste the copied device EUI)and select the legacy packet forwarder.

![alt text](https://github.com/vyshakpadinjarote/LoRa-Gateway-using-RAK831/blob/master/Images/Screenshot%20(54).png)

That’s it, your gateway will be shown as connected and ready to go.
