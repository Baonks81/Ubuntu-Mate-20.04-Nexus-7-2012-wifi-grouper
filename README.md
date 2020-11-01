# Ubuntu-Mate-20.04-Nexus-7-2012-wifi-grouper

Vừa làm xong Ubuntu Mate cho Nexus 7 2012 kernel 5.9 theo hướng dẫn của Worldblender trên xda-developers



https://forum.xda-developers.com/iconia-a500/linux-acer-iconia-tab-a500-2020-edition-t4136023



https://github.com/grate-driver/linux/issues/8



Các bước chuẩn bị tạo .img cơ bản như sau:



Extract asus-grouper.img để lấy boot.img, initramfs, vmlinux, extend, kernel modules, firmware
http://www.mediafire.com/file/yonz1jh5f05lsrz/lib.zip/file
http://www.mediafire.com/file/g6nvcxij1am0zr0/boot.zip/file
Download ubuntu-mate-20.04-beta1_for-acer-iconia-picasso.img và push vào usb 8GB trở lên bằng GNU Win32 Disk Imager hoặc các soft trên Linux, có 2 partition là pmOS_boot và pmOS_root
Xóa và chép boot.img, initramfs, vmlinux, extend vào pmOS_boot partition
Chép kernel modules, firmware vào /lib trên pmOS_root
Sửa /etc/fstab vì kernel 5.9 tự tìm LABEL để khởi động
Sửa hosts bằng cách thêm dòng 127.0.1.1 ubuntu
Chép sysctl.conf vào /opt để chỉnh lại virtual memory nếu cần. Chép resolv.conf từ host vào /etc
Dùng dd, losetup, partprobe, gparted, truncate để tạo và cắt vùng unallocated trong file .img cho vừa kích thước userdata trên Nexus7 8GB(có thể dùng với 16GB và 32GB)
Để chroot được thì phải có host chạy Linux cho armv7hf



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
