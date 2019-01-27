# Instructions for setting up QEMU and linux

In the follwoing steps, we are going to assume that we are using the Pitt account mhk36. Obviously, replace this with your Pitt username. Also, we are going to use our private directory inside afs for these steps.

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

Note that you will be running QEMU on your own machine in order to test your kernel. QEMU is a virtual machine. So, if your kernel runs on QEMU on your machine, then it should run on QEMU on any machine. Only follow the instructions that is associated with your machine in this section.

### Windows

- downlaod QEMU from this [link](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/qemu.zip)
- unzip qemu.zip using your favorite unzipper
- open qemu folder and you will see many files 
- read the README-en for more info
- a file "qemu-win.bat" starts QEMU; double click boots Linux on your desktop
- choose either linux(original) or linux(devel) using keyboard arrow keys and click enter to boot that particular kernel
- login as root user: username = "root" and password = "root"
- you have successfully booted linux using QEMU!

### Mac OS X (10.5 or later)

- make sure you have homebrew installed, if not then check this [link](https://brew.sh/)
- open Terminal (in Applications folder or hit Command + Space and search for Terminal)
- type the following command in Terminal
```
brew install qemu
```
- download qemu starter script and disk image from this [link](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/qemu-test.zip)
- unzip qemu-test.zip
- open the qemu-test directory using Terminal
```
cd qemu-test
```
- Run the following command to boot Linux using QEMU:
```
./start.sh
```
- choose either linux(original) or linux(devel) using keyboard arrow keys and click enter to boot that particular kernel
- login as root user: username = "root" and password = "root"
- you have successfully booted linux using QEMU!

### Debian/Ubuntu

- open Terminal
- type the following command in Terminal
```
apt-get install qemu
```
- download qemu starter script and disk image from this [link](https://github.com/maher460/Pitt_CS1550_recitation_materials/blob/master/project1/qemu-test.zip)
- unzip qemu-test.zip
- open the qemu-test directory using Terminal
```
cd qemu-test
```
- Run the following command to boot Linux using QEMU:
```
./start.sh
```
- choose either linux(original) or linux(devel) using keyboard arrow keys and click enter to boot that particular kernel
- login as root user: username = "root" and password = "root"
- you have successfully booted linux using QEMU!

<br>

## Step 2: Copy your own kernel files to QEMU

From QEMU, you will need to download two files from the new kernel that you just built.
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
- use scp command to download the kernel to a home directory (type in your Pitt username and password when instructed)
```
scp mhk36@thoth.cs.pitt.edu:/afs/pitt.edu/home/m/h/mhk36/private/linux-2.6.23.1/arch/i386/boot/bzImage .
scp mhk36@thoth.cs.pitt.edu:/afs/pitt.edu/home/m/h/mhk36/private/linux-2.6.23.1/System.map .
```

<br>

## Step 3: Install your own kernel in QEMU

- run QEMU (same as Step 1)
- choose linux (original)
- login as root user: username = "root" and password = "root"
- make sure you are in your home directory (pwd should show /root)
- copy the bzImage to boot directory; respond 'y' to the prompt to overwrite
```
cp bzImage /boot/bzImage-devel
```
- copy the System.map to boot directory; respond 'y' to the prompt to overwrite
```
cp System.map /boot/System.map-devel
```
Please note that we are replacing the -devel
files, the others are the original unmodified kernel so that if your kernel fails to boot for
some reason, you will always have a clean version to boot QEMU.

<br>

## Step 4: Update the bootloader and then boot into your own kernel

You need to update the bootloader when the kernel changes. To do this (do it every time
you install a new kernel if you like) as root type. 

- run QEMU (same as Step 1)
- choose linux (original)
- login as root user: username = "root" and password = "root"
- make sure you are in your home directory (pwd should show /root)
- run the following command:
```
lilo
```
lilo stands for LInux Loader, and is responsible for the menu that allows you to choose which
version of the kernel to boot into.

- Reboot QEMU by running the following command:
```
reboot
```
As root, you simply can use the reboot command to cause the system to restart. When LILO
starts (the red menu) make sure to use the arrow keys to select the linux(devel) option and
hit enter.

- Done! You are now inside your our modified kernel!

<br>

## Step 5: 

Now, whenever you modify your kernel you need to rebuild your kernel and copy it again to inside the QEMU. 

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


