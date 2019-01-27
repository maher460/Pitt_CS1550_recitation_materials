# Instructions for setting up QEMU and linux

In the follwoing steps, we are going to assume that we are using the Pitt account mhk36. Obviously, replace this with your Pitt username. Also, we are going to use our private directory inside afs for these steps.

## Step 1: Setup the Kernel Source

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