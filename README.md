# Setting up Ubuntu machine for deep learning using Tensorflow on NVIDIA GPU (TitanX)
How to set up Ubuntu 14.04 for deep learning using Tensorflow v.0.12 on TitanX GPU

My instruction is a compilation from the following sources, plus some additional information:

* https://www.linkedin.com/pulse/building-personal-deep-learning-rig-gtx-1080-ubuntu-1604-ning
* https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/
* http://xcat-docs.readthedocs.io/en/stable/advanced/gpu/nvidia/verify_cuda_install.html


**Hardware**
- CPU: Intel Core-i7
- GPU: NVIDIA Titan X Pascal

## Install Ubuntu 14.04 server
See the comments at the bottom of [the page](https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) if you are deciding between Ubuntu 16 and Ubuntu 14.

**Step 1**. Download the **HEADLESS** Ubuntu 14.04 Server ISO, make a bootable USB stick using e.g. [Rufus] (https://rufus.akeo.ie/)

**Step 2**. Follow the instructions to install Ubuntu, but DONT install any graphica user interface yet!

**Step 3**. On the first reboot press 'e' to edit GRUB if you need to add something. See [this page](https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) for details. 

**Step 4**. Log in to the command line

## Updates, desktop environment, extra packages and grub update.
**Step 5**. Pretty much everything from here on will need to be done as root so add sudo to the beginning of the commands or just sudo -s to get a root shell.

**Do updates.**
```
sudo apt-get update
sudo apt-get dist-upgrade
```
## Install your desktop environment

**Step 6**. The easiest way to take care of adding a desktop GUI is to run the "tasksel" command. If you run it without any arguments it will give you a very nice menu with many options for different desktop setups and other good stuff. It's a handy tool.

You can run tasksel as follows to add the default Ubuntu desktop without looking at the menu.
```
sudo tasksel
```

Just fot the record I decided to go with xubuntu

## Add extra programs

**Step 7**. You now have your base desktop install so you might want to add a few extras at this point. I usually add the following.
```
sudo apt-get install build-essential emacs dkms synaptic ssh
```

## Grub update

**Step 8**. If you are using any kernel options on startup you should probably take care of that now before the next reboot. Use your editor of choice and do something like the following.
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

**Step 9**. After editing that file update grub with (surprise)
```
update-grub
```

## Install the NVIDIA display driver

**Step 10**. I've been using the well maintained ["graphics-drivers" ppa] (https://launchpad.net/~graphics-drivers/+archive/ubuntu/ppa) for adding the NVIDIA display drivers. These have been up-to-date and well packaged. Using this will give you a convenient update path for new drivers. So far I haven't had any trouble with new drivers rebuilding against kernel source using dkms.
```
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-367
```

## CUDA install, setup and fixes
**Dependencies**

We are doing a manual CUDA toolkit install since we want both version 7.5 and 8.0rc (and since the packaged .deb and .rpm files are basically broken right now!)

I did a "dry-run" CUDA install from the deb files to pull a list of system packages that would get installed as dependencies (outside of the CUDA repo). You may or may not need these, but it is what the old (working) deb install from the CUDA repo would have pulled in. I put them in a file called cuda-deps... sorry for the long scroll line but I didn't want any line breaks in there in case you want to copy that to a file.

**Step 11**
```
cat cuda-deps 
ca-certificates-java default-jre default-jre-headless fonts-dejavu-extra freeglut3 freeglut3-dev java-common libatk-wrapper-java libatk-wrapper-java-jni  libdrm-dev libgl1-mesa-dev libglu1-mesa-dev libgnomevfs2-0 libgnomevfs2-common libice-dev libpthread-stubs0-dev libsctp1 libsm-dev libx11-dev libx11-doc libx11-xcb-dev libxau-dev libxcb-dri2-0-dev libxcb-dri3-dev libxcb-glx0-dev libxcb-present-dev libxcb-randr0-dev libxcb-render0-dev libxcb-shape0-dev libxcb-sync-dev libxcb-xfixes0-dev libxcb1-dev libxdamage-dev libxdmcp-dev libxext-dev libxfixes-dev libxi-dev libxmu-dev libxmu-headers libxshmfence-dev libxt-dev libxxf86vm-dev lksctp-tools mesa-common-dev  x11proto-core-dev x11proto-damage-dev  x11proto-dri2-dev x11proto-fixes-dev x11proto-gl-dev x11proto-input-dev x11proto-kb-dev x11proto-xext-dev x11proto-xf86vidmode-dev xorg-sgml-doctools xtrans-dev libgles2-mesa-dev
```
If you put those package names in a file called cuda-deps you can do the following to install them easily,
**Step 12**
```
cat cuda-deps | xargs sudo apt-get -y install
```
At this point I had a problem with IPv6: the packages were not downloaded as the progress stuck at 0%
In order to resolve it I switched off IPv6 according to the instructions on [this page](http://askubuntu.com/questions/272796/connecting-to-archive-ubuntu-com-takes-too-long)


## CUDA toolkit installs

**Step 13** Download the ".run" install files from NVIDIA (you will need to be registered as a developer to get the 8.0 version)

**You want to run these install scripts and NOT install the bundled display drivers!**

You can run the scripts and answer the prompts or you can do,
```
./cuda_7.5.18_linux.run --help
```
to see the script options. Then, if you trust me, you can do the following,
**Step 14**
```
chmod 755 cuda_*
./cuda_7.5.18_linux.run --silent --toolkit --samples --samplespath=/usr/local/cuda-7.5/samples --override
./cuda_8.0.27_linux.run --silent --toolkit --samples --samplespath=/usr/local/cuda-8.0/samples --override
```

That will give you both CUDA toolkit versions with the sample code directories where they belong.

There will be a symbolic link from /usr/local/cuda-8.0 to /usr/local/cuda. You can change this link to the 7.5 version like this,
**Step 15 (optional)**
```
sudo rm /usr/local/cuda
sudo ln -s /usr/local/cuda-7.5 /usr/local/cuda
```

## System CUDA environment

I like to have have base development system tools like CUDA on the default bin and lib path so I create the following files,
**Step 16**
```
/etc/profile.d/cuda.sh

export PATH=$PATH:/usr/local/cuda/bin
export CUDADIR=/usr/local/cuda
export GLPATH=/usr/lib
```

and for libs,

```
/etc/ld.so.conf.d/cuda.conf
/usr/local/cuda/lib64
```

Run **ldconfig** after adding that last file.

## Fix "broken" stuff
I don know if this step is necessary for Ubuntu 14.04. It was described [in this manual] (https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) for Ubuntu 16
**Step 17**
```
sed -i '/unsupported GNU version/ s/^/\/\//' /usr/local/cuda-7.5/include/host_config.h
sed -i '/unsupported GNU version/ s/^/\/\//' /usr/local/cuda-8.0/include/host_config.h
find /usr/local/cuda-7.5/samples -type f -exec sed -i 's/nvidia-3../nvidia-367/g' {} +
find /usr/local/cuda-8.0/samples -type f -exec sed -i 's/nvidia-3../nvidia-367/g' {} +
```

## Reboot
The NVIDIA driver and CUDA are now installed
