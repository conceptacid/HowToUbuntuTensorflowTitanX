# Setting up Ubuntu machine for deep learning using Tensorflow on NVIDIA GPU (TitanX)
How to set up Ubuntu 14.04 for deep learning using Tensorflow v.0.12 on TitanX GPU

My instruction is a compilation from the following sources:

https://www.linkedin.com/pulse/building-personal-deep-learning-rig-gtx-1080-ubuntu-1604-ning
https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/
http://askubuntu.com/questions/272796/connecting-to-archive-ubuntu-com-takes-too-long
http://xcat-docs.readthedocs.io/en/stable/advanced/gpu/nvidia/verify_cuda_install.html


**Hardware**
- CPU: Intel Core-i7
- GPU: NVIDIA Titan X Pascal

## Install Ubuntu 14.04 server
See the comments at the bottom of [the page](https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) if you are deciding between Ubuntu 16 and Ubuntu 14.

1. Download Ubuntu 14.04 Server ISO, make a bootable USB stick using e.g. [Rufus] (https://rufus.akeo.ie/)

2. Follow the instructions to install Ubuntu, but DONT install any graphica user interface yet!

3. On the first reboot press 'e' to edit GRUB if you need to add something. See [this page](https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) for details. 

4. Log in to the command line

## Updates, desktop environment, extra packages and grub update.
5. Pretty much everything from here on will need to be done as root so add sudo to the beginning of the commands or just sudo -s to get a root shell.

**Do updates.**
```
sudo apt-get update
sudo apt-get dist-upgrade
```
## Install your desktop environment

6. The easiest way to take care of adding a desktop GUI is to run the "tasksel" command. If you run it without any arguments it will give you a very nice menu with many options for different desktop setups and other good stuff. It's a handy tool.

You can run tasksel as follows to add the default Ubuntu desktop without looking at the menu.
```
sudo tasksel
```

Just fot the record I decided to go with xubuntu

## Add extra programs

7. You now have your base desktop install so you might want to add a few extras at this point. I usually add the following.
```
sudo apt-get install build-essential emacs dkms synaptic ssh
```

## Grub update

8. If you are using any kernel options on startup you should probably take care of that now before the next reboot. Use your editor of choice and do something like the following.
```
edit the file /etc/default/grub 
the server install will have an empty option line like this,

GRUB_CMDLINE_LINUX_DEFAULT=""

For the motherboard I'm using I would change this to,

GRUB_CMDLINE_LINUX_DEFAULT="net.ifnames=0"

Note that a "normal" desktop install would also have "quiet splash" 
in this line to hide all of the boot messages during startup 
... sometimes I like to see them!
```
See [this page](https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) for details. 

9. After editing that file update grub with (surprise)
```
update-grub
```

## Install the NVIDIA display driver

10. I've been using the well maintained ["graphics-drivers" ppa] (https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa) for adding the NVIDIA display drivers. These have been up-to-date and well packaged. Using this will give you a convenient update path for new drivers. So far I haven't had any trouble with new drivers rebuilding against kernel source using dkms.
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-367
```
