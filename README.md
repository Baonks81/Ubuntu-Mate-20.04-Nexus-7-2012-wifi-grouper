# Ubuntu-Mate-20.04-Nexus-7-2012-wifi-grouper

Ubuntu Mate for Nexus 7 2012 wifi grouper kernel 5.9 following Worldblender's guide on xda-developers



https://forum.xda-developers.com/iconia-a500/linux-acer-iconia-tab-a500-2020-edition-t4136023



https://github.com/grate-driver/linux/issues/8



Basic preparation to create .img from ubuntu mate for rpi 32bit:



1.Extract asus-grouper.img to getting boot.img, initramfs, vmlinux, extend, kernel modules, firmware

http://www.mediafire.com/file/yonz1jh5f05lsrz/lib.zip/file

http://www.mediafire.com/file/g6nvcxij1am0zr0/boot.zip/file

2.Download ubuntu-mate-20.04-beta1_for-acer-iconia-picasso.img and push it into usb 8GB or more space. Using GNU Win32 Disk Imager or Linux dd command, it had 2 partition: pmOS_boot and pmOS_root

3.Delete and copy boot.img, initramfs, vmlinux, extend into pmOS_boot partition

4.Copy kernel modules, firmware into /lib on pmOS_root partition
5.Edit /etc/fstab because of kernel 5.9 auto searching LABEL booting
6.Insert /etc/hosts this line 127.0.1.1 ubuntu
7.Copy sysctl.conf into /opt for virtual memory. Copy resolv.conf from host into /etc

8.Using dd, losetup, partprobe, gparted, truncate creating and cutting unallocated .img file using for Nexus7 8GB(16GB and 32GB)

9.Using chroot host have to run Linux armv7hf



Back to bootloader, flash boot.img in fastboot or postmarketOS method

http://www.mediafire.com/file/g6nvcxij1am0zr0/boot.zip/file



TWRP for grouper 3.3.1-0 and up https://dl.twrp.me/grouper/

umount mmcblk0p9 in adb shell trên PC/laptop or Twrp Advance/Terminal (2 times)



adb push <filename>.img to mmcblk0p9 and enjoy it. Or download it here:

https://drive.google.com/file/d/1Lx338vvTjIqB-s3RACZPGsDf7-a7qayZ/view?usp=sharing



Add grate-driver PPA



https://launchpad.net/~grate-driver/+archive/ubuntu/ppa



install opentegra / grate-driver to accelerate 2d gpu on ubuntu-mate, not performance for 3d accelerate gpu



The kernerl virtual memory and cpufreq



https://tinhte.vn/thread/cai-postmarketos-len-nexus-7-2012-wifi-3g-dung-luong-8gb-16gb-32gb.3179205/



https://tinhte.vn/thread/multirom-cai-ubuntu-14-04-6-lts-len-nexus-7-2012-grouper-dualboot-android.3134764/
