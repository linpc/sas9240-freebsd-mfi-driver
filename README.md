sas9240-freebsd-mfi-driver
======================

About
----------------------

This patch is for LSI MegaRAID SAS 9240-4i / 9240-8i driver using.

Test only on FreeBSD 9.0-release amd64, IBM M3 X3550.

How to use this patch:
----------------------

1. Make sure the kernel source has been installed on your system.
2. Copy the kernel configuration:

	cd /usr/src/sys/amd64/conf
	cp GENERIC MYKERNEL

3. Comment out the following line:

	vi MYKERNEL

	--- device     cbb	    # cardbus (yenta) bridg
	+++ #device     cbb	    # cardbus (yenta) bridg

3. Add the following line to the file /usr/src/sys/conf/files , note that the added line should next to the line which has `dev/mfi/mfi_cam.c optional mfi`:

	    dev/mfi/mfi_cam.c optional mfi
	+++ dev/mfi/mfi_syspd.c optional mfi

4. Replace the mfi driver patch:

	mv /usr/src/sys/dev/mfi /root/mfibackup
	cd ~
	git clone git://github.com/linpc/sas9240-freebsd-mfi-driver.git .
	cp -r sas9240-freebsd-mfi-driver/9.0/mfi /usr/src/sys/dev/mfi

5. Rebuild the kernel:

	cd /usr/src
	make kernel KERNCONF=MYKERNEL

One step patch using the patch file:
----------------------

If you don't want to do manual patch, you can just use the .patch file in the repository.

Also, make sure you have kernel source installed:

	cd /usr/src
	patch -s -d /usr/src  < /root/sas9240-freebsd-mfi-driver/patch_9.0.diff
	make kernel KERNCONF=GENERIC

Supported Controllers
----------------------
* MegaRAID SAS 9240-4i
* MegaRAID SAS 9240-8i
* MegaRAID SAS 9260-4i
* MegaRAID SAS 9260-8i
* MegaRAID SAS 9260DE-8i
* MegaRAID SAS 9261-8i
* MegaRAID SAS 9280-4i4e
* MegaRAID SAS 9280-8e
* MegaRAID SAS 9280DE-8e
* MegaRAID SAS 9280-24i4e
* MegaRAID SAS 9280-16i4e
* MegaRAID SAS 9280-16i

Copyright:
----------------------

All copyright claimed by what signed in the source code. (The FreeBSD Project)
