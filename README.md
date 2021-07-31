# Ubuntu-Mate-20.04-Nexus-7-2012-wifi-grouper

Vừa làm xong Ubuntu Mate cho Nexus 7 2012 kernel 5.9 theo hướng dẫn của Worldblender trên xda-developers


https://forum.xda-developers.com/iconia-a500/linux-acer-iconia-tab-a500-2020-edition-t4136023



https://drive.google.com/drive/folders/1pMqaS5GaM6N9TAKlNGQZWCG8UTiRn4pK



https://github.com/grate-driver/linux/issues/8



Các bước chuẩn bị tạo .img cơ bản như sau:



Extract asus-grouper.img để lấy boot.img, initramfs, vmlinux, extend, kernel modules, firmware
 lib module and firmware https://www.mediafire.com/file/key9s2nxhjy6jak/
 boot.img-asus-grouper https://www.mediafire.com/file/honfdu89skw1wm5/
Download ubuntu-mate-20.04-beta1_for-acer-iconia-picasso.img và push vào usb 8GB trở lên bằng GNU Win32 Disk Imager hoặc các soft trên Linux, có 2 partition là pmOS_boot và pmOS_root
Xóa và chép boot.img, initramfs, vmlinux, extend vào pmOS_boot partition
Chép kernel modules, firmware vào /lib trên pmOS_root
Sửa /etc/fstab vì kernel 5.9 tự tìm LABEL để khởi động
Sửa hosts bằng cách thêm dòng 127.0.1.1 ubuntu
Chép sysctl.conf vào /opt để chỉnh lại virtual memory nếu cần. Chép resolv.conf từ host vào /etc
Dùng dd, losetup, partprobe, gparted, truncate để tạo và cắt vùng unallocated trong file .img cho vừa kích thước userdata trên Nexus7 8GB(có thể dùng với 16GB và 32GB)
Để chroot được thì phải có host chạy Linux cho armv7hf



Cách kiểm tra Nexus 7 2012 là mã cũ PM269 hay E1565



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM



Do I have grouper or tilapia?





TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia



Which hardware revision of grouper do I have?





TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Đặt máy về bootloader, để flash boot.img qua fastboot hoặc dùng method của postmarketOS

http://www.mediafire.com/file/g6nvcxij1am0zr0/boot.zip/file



Vào TWRP for grouper 3.3.1-0 trở lên https://dl.twrp.me/grouper/

Dùng adb shell trên PC/laptop hoặc Advance/Terminal trong twrp để umount mmcblk0p9 (làm 2 lần cho chắc ăn)



adb push <filename>.img đã xong theo các bước chuẩn bị ở trên vào mmcblk0p9 và thưởng thức. Hoặc tải .img ở đây:



Bản chính thức ubuntu mate 20.04.1 sẽ update lại link sau

https://drive.google.com/drive/folders/1F8I5vrOtpAx3VSINj00TaCMOOPFWgZAS?usp=sharing



Thêm PPA



https://launchpad.net/~grate-driver/+archive/ubuntu/ppa



Để cài opentegra của grate-driver tăng tốc đồ hoạ 2D đủ dùng cho ubuntu-mate, chưa có tăng tốc đồ hoạ 3D



Thông số kernel, virtual memory và cpufreq ở đây



https://tinhte.vn/thread/cai-postmarketos-len-nexus-7-2012-wifi-3g-dung-luong-8gb-16gb-32gb.3179205/



https://tinhte.vn/thread/multirom-cai-ubuntu-14-04-6-lts-len-nexus-7-2012-grouper-dualboot-android.3134764/



Control CPU frequency matching with thermal, using shell scripts



https://github.com/Sepero/temp-throttle/tree/4e6fa06ea036129c4a815fc5d4494556578624e1



Low_temp = max_temp -3



$ sh temp_throttle.sh 55



Create startup temperature for cpu throttle at login

$ visudo



ALL ALL=(root) NOPASSWD: /path/to/temp_throttle.sh



$ sudo chown root /path/to/temp_throttle.sh



$ sudo chmod 755 /path/to/themp_throttle.sh



Settings → Sessions and applications startup



Command: sudo /path/to/temp_throttle.sh 55



[MEDIA=youtube]EnBm63VjTqQ[/MEDIA]
