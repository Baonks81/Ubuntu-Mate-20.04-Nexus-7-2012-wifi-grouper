Vừa làm xong Ubuntu Mate cho Nexus 7 2012 mainline kernel 5.9-rc2-next-grate và mainline kernel 5.14-rc3-next-grate theo hướng dẫn của Worldblender trên xda-developers



https://forum.xda-developers.com/iconia-a500/linux-acer-iconia-tab-a500-2020-edition-t4136023



https://drive.google.com/drive/folders/1pMqaS5GaM6N9TAKlNGQZWCG8UTiRn4pK



https://github.com/grate-driver/linux/issues/8



Các bước chuẩn bị tạo .img cơ bản như sau:



Extract asus-grouper.img để lấy boot.img, initramfs, vmlinux, extend, kernel modules, firmware
lib module and firmware https://www.mediafire.com/file/key9s2nxhjy6jak/
boot.img-asus-grouper https://www.mediafire.com/file/honfdu89skw1wm5/
Download ubuntu-mate-20.04.1-armhf+acer-iconia-picasso.img và push vào usb 8GB trở lên bằng GNU Win32 Disk Imager hoặc các soft trên Linux, có 2 partition là pmOS_boot và pmOS_root
Xóa và chép boot.img, initramfs, vmlinux, extend vào pmOS_boot partition
Chép kernel modules, firmware vào /lib trên pmOS_root
Sửa /etc/fstab vì kernel 5.14-rc3 tìm LABEL pmOS_boot và pmOS_root để khởi động trong Ubuntu MATE
Sửa hosts bằng cách thêm dòng 127.0.1.1 ubuntu
Chép sysctl.conf, cpufreq.start, temp_throttle, clear_ram vào /opt để chỉnh lại virtual memory nếu cần.
Dùng dd, losetup, partprobe, gparted, truncate để tạo và cắt vùng unallocated trong file .img cho vừa kích thước userdata trên Nexus7 8GB(có thể dùng với 16GB và 32GB)
Để chroot được thì phải có host chạy Linux cho armv7 hoặc armhf



Cách kiểm tra Nexus 7 2012 là mã cũ PM269 hay E1565. Tham khao:

https://wiki.postmarketos.org/wiki/Google_Nexus_7_2012_(asus-grouper)



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM



Do I have grouper or tilapia?





TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia



Which hardware revision of grouper do I have?





TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Đặt máy về bootloader, để flash boot.img qua fastboot hoặc dùng method của postmarketOS. Kết nối Nexus 7 vào máy tính qua cáp usb chuẩn 1.0 → 2.0



$ sudo adb start-server



$ sudo adb reboot bootloader



$ sudo fastboot flash boot <boot_filename>.img



Vào TWRP for grouper 3.3.1-0 trở lên https://dl.twrp.me/grouper/

Dùng adb shell trên PC/laptop hoặc Advance/Terminal trong twrp để umount mmcblk0p9 (làm 2 lần cho chắc ăn) [với tilapia (bản 3G) là mmcblk0p10]



1. TWRP(Advance → Terminal): $ df

2. TWRP(Advance → Terminal): $ umount /dev/block/mmcblk0p__  <- fill partition number

3. TWRP(Advance → Terminal): $ umount /dev/block/mmcblk0p__  <- fill partition number



On PC/Laptop terminal:



$ adb push <rootfs_filename>.img /dev/block/mmcblk0p__  <- fill partition number



grouper has likely data on /dev/block/mmcblk0p9 but make sure!
tilapia has likely data on /dev/block/mmcblk0p10 but make sure!



Hoặc tải .img ở đây:



Bản chính thức ubuntu mate 20.04.1 f2fs rootfs

https://drive.google.com/drive/folders/173rpkppVvYtpVDfseaZUxUEqk4B5PSLy?usp=sharing



Bản chính thức ubuntu mate 20.04.1 etx4 rootfs

https://drive.google.com/drive/folders/1F8I5vrOtpAx3VSINj00TaCMOOPFWgZAS?usp=sharing



Thêm PPA



https://launchpad.net/~grate-driver/+archive/ubuntu/ppa



Để cài opentegra của grate-driver tăng tốc đồ hoạ 2D đủ dùng cho ubuntu-mate, chưa có tăng tốc đồ hoạ 3D. Chỉnh sửa opentegra.conf thành 00-opentegra.conf để driver chạy trước fbdev driver trong /usr/share/X11/xorg.conf.d



***Replace default compositor to Compton



$ sudo apt install compton



Menu → Preferences → Windows → Window Manager



Remove Enable software compositor in Windows Preferences



Add Menu → Preferences → Startup Applications



Command:

compton --backend glx --vsync opengl-swc --glx-no-stencil --unredir-if-possible --glx-no-rebind-pixmap --glx-swap-method 3 --paint-on-overlay -b



Thông số kernel, virtual memory và cpufreq ở đây



https://tinhte.vn/thread/cai-postmarketos-len-nexus-7-2012-wifi-3g-dung-luong-8gb-16gb-32gb.3179205/



https://tinhte.vn/thread/multirom-cai-ubuntu-14-04-6-lts-len-nexus-7-2012-grouper-dualboot-android.3134764/



***Virtual memory, kernel parameters and CPU frequencies Ondemand governor



$ sudo nano /etc/sysctl.conf



# content of this file will override /etc/sysctl.d/*



vm.swappiness=15

vm.vfs_cache_pressure=80

vm.dirty_background_ratio=3

vm.dirty_ratio=30

vm.dirty_writeback_centisecs=3000

vm.dirty_expire_centisecs=3000

vm.lowmem_reserve_ratio=32 32 0

vm.min_free_kbytes=3072

vm.user_reserve_kbytes=6144

vm.admin_reserve_kbytes=3072

vm.panic_on_oom=0

vm.overcommit_memory=1

vm.overcommit_ratio=50

vm.drop_caches=3

vm.laptop_mode=0

vm.mmap_min_addr=4096

vm.oom_kill_allocating_task=0

vm.extfrag_threshold=500

vm.oom_dump_tasks=1

vm.page-cluster=0

vm.stat_interval=10

vm.compact_unevictable_allowed=0

vm.highmem_is_dirtyable=0



kernel.panic=10

kernel.panic_on_oops=1

kernel.tainted = 0

kernel.ctrl-alt-del = 1

kernel.threads-max = 15503

kernel.random.write_wakeup_threshold = 128

kernel.usermodehelper.bset=4294967295 4294967295

kernel.usermodehelper.inheritable=4294967295 4294967295

kernel.printk = 15 4 1 7

kernel.kptr_restrict = 2

kernel.randomize_va_space = 2

kernel.perf_event_paranoid = 3

kernel.keys.root_maxkeys=200

kernel.keys.root_maxbytes=20000

kernel.perf_event_paranoid=1

kernel.perf_cpu_time_max_percent=3

kernel.shmmax=268435456

kernel.shmall=2097152

kernel.msgmni=2048

kernel.msgmax=64000

kernel.sem=500 512000 64 2048

kernel.auto_msgmni=1

kernel.sched_child_runs_first=1

kernel.hung_task_timeout_secs=0



fs.file-max = 99200

fs.epoll.max_user_watches = 138922



net.core.somaxconn = 128

net.core.wmem_max = 131072

net.core.rmem_max = 131072

net.core.wmem_default = 112640

net.core.rmem_default = 112640

net.core.warnings = 1



net.ipv4.tcp_ecn=1

net.ipv4.tcp_fastopen=3

net.ipv4.tcp_timestamps=0

net.ipv4.tcp_tw_reuse=1

net.ipv4.tcp_wmem=1536 21845 131072

net.ipv4.tcp_rmem=1536 21845 131072



$ sudo sysctl -p



$ sudo nano /opt/cpufreq.start



# Set the governor to ondemand for all processors

for cpu in /sys/devices/system/cpu/cpufreq/policy*; do

echo ondemand > ${cpu}/scaling_governor

done



# Reduce the boost ignore_nice_load to 0

echo 0 > /sys/devices/system/cpu/cpufreq/ondemand/ignore_nice_load



# Reduce the boost io_is_busy to 0

echo 0 > /sys/devices/system/cpu/cpufreq/ondemand/io_is_busy



# Reduce the boost powersave_bias to 0 <-- tăng giảm xung của cpu/gpu

echo 0 > /sys/devices/system/cpu/cpufreq/ondemand/powersave_bias



# Reduce the boost sampling_down_factor to 2

echo 2 > /sys/devices/system/cpu/cpufreq/ondemand/sampling_down_factor



# Reduce the boost sampling_rate to 20000

echo 20000 > /sys/devices/system/cpu/cpufreq/ondemand/sampling_rate



# Reduce the boost threshold to 75%

echo 75 > /sys/devices/system/cpu/cpufreq/ondemand/up_threshold



$ sudo sh /opt/cpufreq.start



***Control CPU frequency matching with thermal, using shell scripts



https://github.com/Sepero/temp-throttle/tree/4e6fa06ea036129c4a815fc5d4494556578624e1



Low_temp = max_temp - 2



$ sh temp_throttle 58



Create startup temperature for cpu throttle at login

$ visudo



ALL ALL=(ALL) NOPASSWD: /path/to/temp_throttle



$ sudo chown root:root /path/to/temp_throttle



$ sudo chmod 755 /path/to/themp_throttle



Settings → Sessions and Applications Startup



Command: sudo /path/to/temp_throttle 58



***Fix sound ALC5642 cho tegra-rt5640***



https://help.ubuntu.com/community/SoundTroubleshooting



https://forum.ubuntuusers.de/topic/medion-akoya-e2228t/2/



$ sudo lsmod | grep "^snd" | cut -d " " -f 1



snd_soc_tegra30_i2s

snd_soc_tegra30_ahub

snd_soc_tegra_pcm

snd_soc_tegra_machine

snd_soc_core

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ sudo nano /etc/modules



snd_soc_rl6231

snd_soc_rt5640

snd_soc_tegra30_i2s

snd_soc_tegra30_ahub

snd_soc_tegra_pcm

snd_soc_tegra_machine

snd_soc_tegra_wm8903

snd_soc_core

snd_soc_wm8903

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ sudo reboot



Checking soc soundcard loaded:



$ sudo cat /proc/asound/card*/id



ALC5642



$ sudo alsa force-reload



$ alsamixer -c 0



Enable các thông số thiết lập (phím M hoặc phím mũi tên lên/xuống): "Speaker R" "Speaker L" "DAC MIXR INF1" "DAC MIXL INF1" "SPOL MIX DAC R1" "SPOL MIX DAC L1" "Stereo DAC MIXR DAC R1" "Stereo DAC MIXL DAC L1"



***Auto rotate screen using gawk and iio-sensor-proxy***



https://github.com/donbowman/kde-auto-rotate



Switch code left-up to normal, switch code right-up to bottom-up. Following readme to install or download edit package: https://www.mediafire.com/file/pvfqfm2zee84xzz/



***Activate 3G modem module using simcard dùng sakis3g.tar.gz***



http://raspberry-at-home.com/installing-3g-modem/



$ sudo apt-get install bleachbit tlp tlp-rdw ubuntu-restricted-extras preload



Remove all rpi packages



$ sudo apt purge --auto-remove libraspberrypi-bin libraspberrypi0 linux-firmware-raspi2 linux-headers-raspi linux-image-raspi linux-raspi pi-bluetooth rpi-eeprom u-boot-rpi flash-kernel linux-headers-5.4.0-1015-raspi linux-image-5.4.0-1015-raspi linux-image-5.4.0-1022-raspi linux-modules-5.4.0-1015-raspi linux-modules-5.4.0-1022-raspi linux-raspi-headers-5.4.0-1022 fwupd fwupd-signed gnome-firmware



***Config firefox 92, install Ublock origin and h264ify addons



https://gitlab.com/postmarketOS/mobile-config-firefox/blob/master/src/mobile-config-prefs.js



In firefox URL, type about:config



general.useragent.override → Mozilla/5.0 (Android 12; Mobile; rv:91.0) Gecko/91.0 Firefox/91.0



layers.acceleration.disabled → false



layers.acceleration.forced.enabled → true



gfx.webrender.all → true



Install chromium without snap

https://askubuntu.com/questions/1204571/chromium-without-snap/1206153#1206153



$ sudo nano /etc/apt/sources.list



deb http://deb.debian.org/debian/ bullseye contrib main non-free

# deb http://deb.debian.org/debian-debug/ bullseye-debug contrib main non-free

# deb-src http://deb.debian.org/debian/ bullseye contrib main non-free



deb http://security.debian.org/debian-security bullseye-security main contrib non-free

# deb-src http://security.debian.org/debian-security bullseye-security main contrib non-free



# Backports

deb http://deb.debian.org/debian bullseye-backports main contrib non-free



$ sudo apt update && sudo apt install chromium



Accelerate chromium with EGL support

https://gitlab.com/postmarketOS/pmaports/-/issues/998



$ sudo nano /etc/chromium/local.conf



unset GDK_BACKEND



$ sudo nano /usr/share/applications/chromium.desktop



Exec=/usr/bin/chromium %U → Exec=/usr/bin/chromium --use-gl=egl %U



***Fix authenticatilon require on chromium***



$ sudo apt install seahorse



Cretate new key Password keyring → name “Default keyring”



***Control nfc using neard.deb package from Debian 10 Buster or sid repository



$ sudo apt install neard neard-tools



$ sudo systemctl start neard



$ sudo nfctool -d nfc0 -1 -p



***Backup full filesystem boot and rootfs



Connect Nexus 7 to PC/Laptop using micro-usb cable, enter TWRP recovery mode → Advance → Terminal



# df



On PC/Laptop



# adb start-server



Backup boot: # sudo adb pull /dev/block/mmcblk0p2 /path/to/boot-kernel-5.14-rc3-next-grate.img



Backup rootfs for grouper(wifi): # sudo adb pull /dev/block/mmcblk0p9 /path/to/rootfs.img



Backup rootfs for tilapia(3G): # sudo adb pull /dev/block/mmcblk0p10 /path/to/rootfs.img



Backup full: sudo adb pull /dev/block/mmcblk0 /path/to/full_backup_mmcblk0.img



Reference link: https://forum.xda-developers.com/t/linux-on-the-nexus-7-2012-wifi-rev-e1565-grouper-2021-edition.4323099/



[MEDIA=youtube]suDeAVPLKF8[/MEDIA]



[MEDIA=youtube]EnBm63VjTqQ[/MEDIA]



Accelerate GPU with grate-driver



[MEDIA=youtube]h0IrfQy8M0Q[/MEDIA]
