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

3. On the first reboot press 'e' to edit GRUB if need something. See [this page](https://www.pugetsystems.com/labs/hpc/Install-Ubuntu-16-04-or-14-04-and-CUDA-8-and-7-5-for-NVIDIA-Pascal-GPU-825/) for details. 

4. Log in to the command line

5. Updates, desktop environment, extra packages and grub update Pretty much everything from here on will need to be done as root so add sudo to the beginning of the commands or just sudo -s to get a root shell.

**Do updates.**
```
apt-get update
apt-get dist-upgrade
```
