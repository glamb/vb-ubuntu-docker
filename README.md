## Introduction

I am going to show, how I got our webapp running in a VM that was using docker

## Dependancies

1. Make sure you have VirtualBox install on your machine.
  - https://www.virtualbox.org/
- Download the Ubuntu Desktop amd64 image
  - http://releases.ubuntu.com/14.04/ubuntu-14.04.3-desktop-amd64.iso

## Setting up the Ubuntu VM

I recommend setting up a fresh VM for our webapp. I have had issues using an existing VM and it was a pay to figure out what I might have changed that would have caused my issues.

1. Open up VirtualBox
- Click on the new button, to create a new vm
- Select Type: Linux and Version: Ubuntu (64-bit)
- Give the VM a name and click Continue
- Give you VM some ram and click Continue. I gave it 2g, since i had it to spare
- Select Create a virtual hard disk now and click Create
- On the Hard Disk File Type panel select VDI and click Continue
- On the storage panel I selected Fix size and pressed Continue
  - selecting fixed size is slower to setup, but once it's setup it's faster to read
- If you selected fixed size you will get prompted for how large of a hard driver you want.
  - VB recommends 8Gb, but I can tell you that is not enough.
  - I went with 15Gb and everything worked perfectly.
- You should now have a vm instance ready to install Ubuntu

#### Installing Ubuntu
1. Double click your newly created Ubuntu VM
  - You will be asked for the optical drive
- Using the file browser, locate your Ubuntu iso Download
- Once the iso loads up, follow the Ubuntu installation instructions
- You will be prompted to restart the machine and Ubuntu will now be installed.

#### Post installation
If you only see a quarter of your desktop you will need to following these steps
1. In you Host Virtualbox menu open the Devices menu
- Select `Insert Guest Additions CD Image...`
- In the dialog box in the vm select install.
- Enter your ubuntu password into the following PW prompt
- Once the Guest Additions have installed, restart your vm

Once your vm loads back up, you should now be able to scale the window and have the display properly match.

## Installing Docker and Docker-Compose
#### Docker
The following instructions can be found on the web here http://docs.docker.com/linux/step_one/

1. On your Ubuntu Vm open up terminal
- Make sure you Ubuntu is updated
```
$ sudo apt-get update
```
- Make sure you have `wget`
```
$ which wget
```
- Install the latest Docker package
```
$ wget -qO- https://get.docker.com/ | sh
```
- Once the script is complete, add yourself to the docker group
```
$ sudo usermod -aG docker <username>
```
- Now reboot your vm
```
$ sudo reboot
```
- When you vm comes back up open the terminal again
- Test and verify your docker installation with the following
```
$ docker run hello-world
```

#### Docker-Compose
1. Switch to root in order to install docker compose
```
$ sudo -i
```
- Copy down the latest docker-compose script
```
curl -L https://github.com/docker/compose/releases/download/1.5.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
```
  - You can find out the latest by going to the github release page. https://github.com/docker/compose/releases
- Change the files permissions, so we can use it as our user
```
$ chmod +x /usr/local/bin/docker-compose
```
- Check if the command works and you get the docker-compose help
```
$ docker-compose
```

## Installing Git
1. Using apt-get install Git
```
$ sudo apt-get install git
```
- Generate an ssh key to use in github. Since this is a fresh install of ubuntu you should not have a .ssh directory.
```
$ mkdir ~/.ssh
$ cd ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
- You will be asked about the name of the file and if you want to add a phrase. I just kit enter for all the questions
- Once the the key is created copy it and put it in your github settings page, https://github.com/settings/ssh
```
$ cat id_rsa.pub
```
- You should now be able to clone the webapp into you working directory
- If you are doing feature branching you can checkout your branch now

## Setting up Docker for our webapp
1. Since we use quay.io, we will need to login before we build
```
$ docker login quay.io
  Username: <ENTER YOUR USERNAME>
  Password: <ENTER QUAY.IO PASSWORD>
  Email: <ENTER YOUR EMAIL>
```
- Now have docker-compose build our app
```
$ docker-compose build
```
- After the build is complete you can start the service
```
$ docker-compose up -d
```

After some downloading your service should be up.
