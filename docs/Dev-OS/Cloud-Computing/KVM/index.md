virt-install is -cpu Penryn,kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on,+pcid,+ssse3,+sse4.2,+popcnt,+avx,+aes,+xsave,+xsaveopt,check \
--name Frytea-OSX \
--memory 8192 \
--vcpus sockets=1,cores=4,threads=2 \
--usb -device usb-kbd -device usb-mouse \
--device isa-applesmc,osk="ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc" \
--drive if=pflash,format=raw,readonly,file=/srv/kvm/OSX-KVM/OVMF_CODE.fd \
--drive if=pflash,format=raw,file=/srv/kvm/OSX-KVM/OVMF_VARS-1024x768.fd \
--smbios type=2 \
--device ich9-intel-hda -device hda-duplex \
--device ich9-ahci,id=sata \
--drive id=Clover,if=none,snapshot=on,format=qcow2,file=/srv/kvm/OSX-KVM/'Catalina/CloverNG.qcow2' \
--device ide-hd,bus=sata.2,drive=Clover \
--device ide-hd,bus=sata.3,drive=InstallMedia \
--drive id=InstallMedia,if=none,file=/srv/kvm/OSX-KVM/BaseSystem.img,format=raw \
--drive id=MacHDD,if=none,file=/srv/kvm/mac_hdd_ng.img,format=qcow2 \
--device ide-hd,bus=sata.4,drive=MacHDD \
--netdev tap,id=net0,ifname=tap0,script=no,downscript=no -device vmxnet3,netdev=net0,id=net0,mac=52:54:00:c9:18:27 \
--monitor stdio \
--vga vmware


## 相关文献

 - [GitHub/kholia/OSX-KVM](https://github.com/kholia/OSX-KVM)
 - [Nokia.La/OSX-KVM](https://nokia.la/projects/osx-kvm)
 - [CentOS 7/Repoforge (RPMforge) x86_64/dmg2img-1.6.2-1.el7.rf.x86_64.rpm](https://centos.pkgs.org/7/repoforge-x86_64/dmg2img-1.6.2-1.el7.rf.x86_64.rpm.html)
 - [在 KVM 虚拟机中运行 macOS 系统](https://www.jianshu.com/p/e95c458d78bd)
 - [Windows远程桌面连接Mac OS X](https://www.cnblogs.com/lolDragon/articles/6243728.html)