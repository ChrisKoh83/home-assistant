## Installing Home Assistant Supervised on Raspberry Pi with Debian 10

This guide will help you to install Home Assistant Supervised, on a Raspberry Pi with Debian 10. This guide has been tested on a Raspberry Pi 4 4gb model, with USB SSD as the boot drive. 

:warning: Using Debian 10 and following a strict set of guidelines available [HERE](https://github.com/home-assistant/architecture/blob/master/adr/0014-home-assistant-supervised.md) will give you a supported installation of Home Assistant Supervised. If you choose at anytime to install additional software to the Debian operating system, your installation will become officially unsupported. Community support via the forums is always available however.

While every effort has been made to ensure this guide complies with ADR-14, no guarantee can be made it does now, or in the future.

In this guide, you will be using Debian 10 as the operating system. This type of installation is what is called “headless” and after the installation is complete, you will not need to have a keyboard, mouse or monitor attached (although you can if you prefer).

*What is Home Assistant Supervised?*

Home Assistant is a full UI managed home automation ecosystem that runs Home Assistant Core, the Home Assistant Supervisor and add-ons. It comes pre-installed on Home Assistant OS, but can be installed on any Linux system. It leverages Docker, which is managed by the Home Assistant Supervisor plus the added benefit of dozens of add-ons (think app store) that work natively inside the Home Assistant environment.

If you are new to Home Assistant, you can now proceed to Section 1 if you need assistance with installing Debian 10. If you already have Debian 10 installed and wish to move on to installing Home Assistant, move on to Section 2. If you have an existing Home Assistant installation and need to know how to back up your current configuration, please see the document  *Backing up and Restoring your configuration* located  [HERE](https://github.com/Kanga-Who/home-assistant/blob/master/Backup%20and%20restore%20your%20config.md)


## Section 1 – Install Debian

**1.1)** Start by downloading the correct `xz-compressed image` for you Pi from [HERE](https://raspi.debian.net/tested-images/). 

**1.2)** While Debian is downloading, you will need some other programs to help with the setup and installation. To burn the Debian xz-compressed image to your SD Card / USB SSD, you will use a program called **balenaEtcher**, available [HERE](https://www.balena.io/etcher/). once the Debian image has downloaded, insert your SD Card / USB SSD into your PC and launch Etcher.

Click **Select Image** and navigate to the location you saved the Debian xz-compressed image, Click **Select Target** and then choose your SD card / USB SSD, and then click **Flash**. Depending on the speed of your card / drive this process can take between 2 and 20 minutes to complete.

**1.3)** Once the image has been written to the SD card / USB SSD, you can safely remove the SD card / USB SSD and plug it into your Pi. Before powering on the Pi, you will need to connect an HDMI cable and Monitor, keyboard and mouse. Once you have done this, you can connect the power cable to your Pi. The initial boot will take a few minutes to complete. When the Pi is ready to use, you will see a screen that lloks like this (or similar).
```
Debian GNU/Linux 10 rpi4-20201112 tty1
rpi4-20201112 login: [   35.807813] vcc-sd: disabling
```

When you see this, Press Enter. You will now be prompted to login. The default username is **root**.

**1.4)** First, you will make sure to OS is up to date by entering the following command
```
apt update && apt upgrade -y
apt install sudo -y
```

**1.5)** You will need to add a username and make that user part of the sudo group. To do this, run the following commands

```
adduser YOUR_USERNAME
```
You will now be asked to enter your first name, last name, phone number etc. You can skip through all this by pressing *Enter*. Now you have added your user, you will need to make it part of the `sudo` group by running the following command.

```
usermod -aG sudo YOUR_USERNAME
```
You will now be able to connect to Pi via SSH to copy and paste all the commands needed to install Home Assistant Supervised. Check your router for the IP address of your Pi. To connect to your Pi via SSH you will use a piece of software called PuTTY, available [HERE](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). Putty is a free and open-source terminal emulator, serial console and network file transfer application. You can use Terminal on a Mac.

**1.6)**Open Putty and in the HOST NAME (OR IP ADDRESS) box, enter the IP of your Pi, then select OPEN. You will now be prompted to enter your username and password. This will be the username and password you just setup in step 1.5.

## Section 2 – Install Home Assistant Supervised

With Debian installed, you can move on to installing Home Assistant Supervised.

**2.1)** First you will start by updating the Debian OS to make sure all the latest updates and security patches are installed. To do this, log into the terminal of your machine, enter the following command and press enter.

```
sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y
```

Depending on the speed of your internet connection, this could take anywhere from 30 seconds to 20 minutes to complete. When finished, you will see the prompt.

**2.2)** Now the operating system is up to date, you can install Home Assistant Supervised. Enter each line of the below commands into the terminal and execute them one at a time.

```
sudo -i

apt-get install -y software-properties-common apparmor-utils apt-transport-https avahi-daemon ca-certificates curl dbus jq network-manager

systemctl disable ModemManager

systemctl stop ModemManager

curl -fsSL get.docker.com | sh

curl -sL "https://raw.githubusercontent.com/Kanga-Who/home-assistant/master/supervised-installer.sh" | bash -s
```

**2.3)** The installation time is generally under 5 mins, however it can take longer so be patient. You can check the progress of Home Assistant setup by connecting to the IP address of your machine in Chrome/Firefox on port 8123. (e.g. http://192.168.1.150:8123) 

Once you can see the login screen, the setup has been completed and you can set up an account name and password. If you are new to Home Assistant you can now configure any smart devices that Home Assistant has automatically discovered on your network. If you have an existing Home Assistant install and you have a snapshot or YAML files you wish to restore, refer to the document *Backing up and Restoring your configuration.*

You have completed the installation of Home Assistant Supervised on your Debian machine. It is recommended that you log into your machine at least once a month and use the following command to download security patches and keep the OS up to date.

```
sudo apt update && sudo apt upgrade -y && sudo apt autoremove –y
```

You can do this directly on the machine itself, or, if you wish to install Open-SSH so you can remotely connect to your Home Assistant machine from another PC, run the following from console. 

```
sudo apt install openssh-server -y
```

:warning: If you install Open-SSH you will not be adhering to the guidelines of [ADR-14](https://github.com/home-assistant/architecture/blob/master/adr/0014-home-assistant-supervised.md) and therefore will not have an officially supported installation, however, installing Open-SSH will not break your machine. If you do choose to install and use Open-SSH, you can then use software called PuTTY, available [HERE](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).

Putty is a free and open-source terminal emulator, serial console and network file transfer application. You can use Putty to execute commands on the Debian machine from your Windows PC. (Use Terminal on a Mac). To connect to the Debian machine via Putty, you will need the IP of the machine from Step 1.19, and the username and password you created from Step 1.10.

Along with this guide, there is also associated documents available. These are essentially guides I use myself and do not comply with ADR-14.

- [Install Samba, Portainer and MQTT on Ubuntu or Debian](https://github.com/Kanga-Who/home-assistant/blob/master/Install%20Samba%2C%20Portainer%20and%20MQTT.md)
- [Backing up and Restoring your configuration](https://github.com/Kanga-Who/home-assistant/blob/master/Backup%20and%20restore%20your%20config.md)

I welcome feedback on this guide, please feel free to tag me or PM if you have suggestions on how to make improvements.

Thank you to [nickrout](https://community.home-assistant.io/u/nickrout/) for testing, feedback and edits to help improve this guide and to others who have contributed code and ideas, you support is appreciated.