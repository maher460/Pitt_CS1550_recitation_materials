# Pitt CS1550 Project 1 (Spring 2019)

Kindly, find the project description [here](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/project1.pdf). However, please go through the following instructions on how to setup and boot your linux kernel using QEMU.

<br>

# Instructions for setting up QEMU and linux

In the following steps, we are going to assume that we are using the Pitt account mhk36. Obviously, replace this with your own Pitt username. Also, we are going to use our private directory inside afs for these steps.

<br>

## Step 0: Setup the Kernel Source

- ssh to thoth.cs.pitt.edu
```
ssh mhk36@thoth.cs.pitt.edu
```
- cd into private directory
```
cd private
```
- copy the linux-2.6.23.1.tar.bz file
```
cp /u/OSLab/original/linux-2.6.23.1.tar.bz2 .
```
- extract
```
tar xfj linux-2.6.23.1.tar.bz2
```
- change into linux-2.6.23.1/ directory
```
cd linux-2.6.23.1
```
- copy the .config file
```
cp /u/OSLab/original/.config .
```
- build
```
make ARCH=i386 bzImage
```

<br>

## Step 1: Download QEMU for your machine

Note that you will be running QEMU on your own machine in order to test your kernel. QEMU is a virtual machine monitor. So, if your kernel runs on QEMU on your machine, then it should run on QEMU on any machine. Only follow the instructions that is associated with your own machine in this section.

### Windows

- download QEMU from this [link](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/qemu_windows.zip)
- unzip qemu_windows.zip using your favorite unzipper
- open qemu_windows folder
- read the README-en for more info
- a file "qemu-win.bat" starts QEMU; double click boots Linux on your desktop
- choose linux(original) using keyboard arrow keys and click enter to boot that particular kernel
- login as root user: username = "root" and password = "root"
- you have successfully booted linux using QEMU!

### Mac OS X (10.5 or later)

- make sure you have homebrew installed, if not then check this [link](https://brew.sh/)
- open Terminal (in Applications folder or hit Command + Space and search for Terminal)
- type the following command in Terminal
```
brew install qemu
```
- download qemu starter script and disk image from this [link](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/qemu_mac_ubuntu.zip)
- unzip qemu_mac_ubuntu.zip
- open the qemu_mac_ubuntu directory using Terminal
```
cd qemu_mac_ubuntu
```
- Run the following command to boot Linux using QEMU:
```
./start.sh
```
- choose either linux(original) using keyboard arrow keys and click enter to boot that particular kernel
- login as root user: username = "root" and password = "root"
- you have successfully booted linux using QEMU!

### Debian/Ubuntu

- open Terminal
- type the following command in Terminal
```
apt-get install qemu
```
- download qemu starter script and disk image from this [link](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/qemu_mac_ubuntu.zip)
- unzip qemu_mac_ubuntu.zip
- open the qemu_mac_ubuntu directory using Terminal
```
cd qemu_mac_ubuntu
```
- Run the following command to boot Linux using QEMU:
```
./start.sh
```
- choose either linux(original) using keyboard arrow keys and click enter to boot that particular kernel
- login as root user: username = "root" and password = "root"
- you have successfully booted linux using QEMU!

<br>

## Step 2: Copy your own kernel files to QEMU

From inside the QEMU virtual machine, you will need to download two files from thoth. These files are the new kernel that you built in Step 0.
The kernel itself is a file named bzImage that lives in the directory linux2.6.23.1/arch/i386/boot/. There is also a supporting file called System.map in
the linux-2.6.23.1/ directory that tells the system how to find the system calls.

- run QEMU (same as Step 1)
- choose linux (original)
- login as root user: username = "root" and password = "root"
- make sure you are in your home directory (pwd should show /root)
```
cd ~
pwd
```
- use scp command to download the kernel to a home directory (type in your Pitt password when instructed and don't forget to replace mhk36 with your Pitt username)
```
scp mhk36@thoth.cs.pitt.edu:/afs/pitt.edu/home/m/h/mhk36/private/linux-2.6.23.1/arch/i386/boot/bzImage .
scp mhk36@thoth.cs.pitt.edu:/afs/pitt.edu/home/m/h/mhk36/private/linux-2.6.23.1/System.map .
```

<br>

## Step 3: Install your own kernel in QEMU

- After you successfully copied your kernel files into the QEMU virtual machine (Step 2 above), make sure you are in your home directory (pwd should show /root)
- copy the bzImage to boot directory; respond 'y' to the prompt to overwrite
```
cp bzImage /boot/bzImage-devel
```
- copy the System.map to boot directory; respond 'y' to the prompt to overwrite
```
cp System.map /boot/System.map-devel
```
Please note that we are replacing the -"devel" files, the others are the original unmodified kernel so that if your kernel fails to boot for
some reason, you will always have a clean version to boot QEMU using the linux (original) option on the boot menu of the QEMU virtual machine.

<br>

## Step 4: Update the bootloader and then boot into your own kernel

You need to update the bootloader when the kernel changes. To do this (do it every time you install a new kernel) as root type.

- run QEMU (same as Step 1)
- choose linux (original)
- login as root user: username = "root" and password = "root"
- make sure you are in your home directory (pwd should show /root)
- run the following command:
```
lilo
```
lilo stands for LInux LOader, and is responsible for the menu that allows you to choose which
version of the kernel to boot into.

- Reboot QEMU by running the following command:
```
reboot
```
As root, you simply can use the reboot command to cause the system to restart. When LILO
starts (the red menu) make sure to use the arrow keys to select the linux (devel) option and
hit enter.

- Done! You are now inside your modified kernel!

<br>

## Step 5: Rebuilding your modified kernel and running it

Now, whenever you modify your kernel you need to rebuild your kernel and copy it again to inside the QEMU virtual machine.

- ssh to thoth.cs.pitt.edu
```
ssh mhk36@thoth.cs.pitt.edu
```
- cd into private directory
```
cd private
```
- change into linux-2.6.23.1/ directory
```
cd linux-2.6.23.1
```
- rebuild
```
make ARCH=i386 bzImage
```
- Now, repeat Step 2 to Step 4

<br>

## Step 6 (optional): Using GitHub private repo to maintain your work

It's good to use a GitHub private repo to maintain your code base as it helps to easily sync all your code files and it provides a powerful versioning system to keep track of all your changes. If you would like to do that, then follow these steps (more detailed instructions are in my Recitation Week 2 [slides](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/week2/CS1550_week_2a_xv6_intro.pdf)):

- signup/login to GitHub.com
- create a new __private__ repository (double check that it is private and completely empty)
- set the git remote origin url of your private linux-2.6.23.1 directory in the AFS folder (accessible from thoth) to the newly created private GitHub repo
- git add, commit and push all the code from thoth/linux to your private GitHub repo
- git clone your private GitHub repo to your own local machine (Mac/Windows/Ubuntu/Whatever)

Now, just work on the code on your own local machine. Then, just git add, commit, and push all changes to your private GitHub repo. After that, git pull these changes on the remote thoth machine. Lastly, compile on the thoth machine.

### Hints and Tricks

- To cleanly close the QEMU virtual machine type:

```
poweroff
```
