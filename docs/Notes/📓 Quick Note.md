# 📓 Quick Note

- [ ] 写三天日记

virsh setmem Frytea-Win10 8388608

virt-install -enable-kvm -m 8192 -cpu Penryn,kvm=on,vendor=GenuineIntel,+invtsc,vmware-cpuid-freq=on,+pcid,+ssse3,+sse4.2,+popcnt,+avx,+aes,+xsave,+xsaveopt,check \
          -machine q35 \
          -vcpus sockets=1,cores=3,threads=2 \
          -usb -device usb-kbd -device usb-mouse \
          -device isa-applesmc,osk="ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc" \
          -drive if=pflash,format=raw,readonly,file=/OVMF_CODE.fd \
          -drive if=pflash,format=raw,file=$OVMF/OVMF_VARS-1024x768.fd \
          -smbios type=2 \
          -device ich9-intel-hda -device hda-duplex \
          -device ich9-ahci,id=sata \
          -drive id=Clover,if=none,snapshot=on,format=qcow2,file=./'Catalina/CloverNG.qcow2' \
          -device ide-hd,bus=sata.2,drive=Clover \
          -device ide-hd,bus=sata.3,drive=InstallMedia \
          -drive id=InstallMedia,if=none,file=BaseSystem.img,format=raw \
          -drive id=MacHDD,if=none,file=/srv/kvm/mac_hdd_ng.img,format=qcow2 \
          -device ide-hd,bus=sata.4,drive=MacHDD \
          -netdev tap,id=net0,ifname=tap0,script=no,downscript=no -device vmxnet3,netdev=net0,id=net0,mac=52:54:00:c9:18:27 \
          -monitor stdio \
          -vga vmware


https://u.nu/72vn / 中国共产党章程（全文）
https://u.nu/okjc / 党的十九大报告（全文）
https://u.nu/-8z4 / 中国共产党纪律处分条例

docker run -d --restart=always --name=serverstatus -v /var/docker/ServerStation/config.json:/ServerStatus/server/config.json -p 10080:80 -p 35601:35601 cppla/serverstatus

wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/cppla/ServerStatus/master/clients/client-linux.py' && nohup python client-linux.py SERVER=192.168.122.1 USER=frytea-Storage  >/dev/null 2>&1 &

wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/cppla/ServerStatus/master/clients/client-linux.py' && nohup python client-linux.py SERVER=192.168.122.1 USER=frytea_storage PASSWORD=tianlun666 >/dev/null 2>&1 &

wget --no-check-certificate -qO client-linux.py 'https://raw.githubusercontent.com/cppla/ServerStatus/master/clients/client-linux.py'

nohup python client-linux.py &

vim /lib/systemd/system/rc-local.service

docker run -d --restart=always --name=serverstatus -v

~/config.json:/ServerStatus/server/config.json -p 10080:80 -p 10081:35601 cppla/serverstatus docker run -d --restart=always --name=serverstatus -v ~/config.json:/ServerStatus/server/config.json -p 10080:80 -p 10081:35601 cppla/serverstatus

| name | cpu | ram | disk | ip | VNC |
:--: | :--: | :--:| :--: | :--: | :--: 
| Frytea-Win10 | 8 |12 | 100 | 192.168.122.2 | 5912 |
| ubuntu_nextcloud | 4 | 4 | 400 | 192.168.122.3 | 5913 |
| ubuntu_gitlab | 4 | 6 | 100 | 192.168.122.4 | 5914 |
| ubuntu_gitlab_runner | 6 | 2 | 40 | 192.168.122.5 | 5915 |
| total | 24 | 32 | 1024 |
| Residue | 2 | 8 | 360 |

{
    "registry-mirrors": [
        "https://docker.mirrors.ustc.edu.cn",
        "https://t1rwcd5u.mirror.aliyuncs.com",
        "https://registry.docker-cn.com"
    ]
}

qemu-img create -f qcow2 ubuntu_onlyoffice.qcow2 50G

virt-install \
--virt-type=kvm \
--name=nextcloud_onlyoffice \
--hvm \
--vcpus=4 \
--memory=4096 \
--disk path=/srv/kvm/ubuntu_onlyoffice.qcow2,size=50,format=qcow2 \
--network network=default \
--graphics vnc,password=tianlun666,listen=::,port=5916 \
--autostart \
--force

qemu-img create -f qcow2 ubuntu_onlyoffice.qcow2 50G
virt-install \
--virt-type=kvm \
--name=ubuntu_onlyoffice \
--hvm \
--vcpus=4 \
--memory=6144 \
--cdrom=/srv/kvm/iso/ubuntu-18.04.3-live-server-amd64.iso \
--disk path=/srv/kvm/ubuntu_onlyoffice.qcow2,size=50,format=qcow2 \
--network network=default \
--graphics vnc,password=tianlun666,listen=::,port=5916 \
--autostart \
--force

qemu-img create -f qcow2 ubuntu_gitlab_runner.qcow2 40G
virt-install \
--virt-type=kvm \
--name=ubuntu_gitlab_runner \
--hvm \
--vcpus=6 \
--memory=2048 \
--cdrom=/srv/kvm/iso/ubuntu-18.04.3-live-server-amd64.iso \
--disk path=/srv/kvm/ubuntu_gitlab_runner.qcow2,size=40,format=qcow2 \
--network network=default \
--graphics vnc,password=tianlun666,listen=::,port=5915 \
--autostart \
--force


qemu-img create -f qcow2 ubuntu_gitlab.qcow2 100G

virt-install \
--virt-type=kvm \
--name=ubuntu_gitlab \
--hvm \
--vcpus=4 \
--memory=6144 \
--cdrom=/srv/kvm/iso/ubuntu-18.04.3-live-server-amd64.iso \
--disk path=/srv/kvm/ubuntu_gitlab.qcow2,size=100,format=qcow2 \
--network network=default \
--graphics vnc,password=tianlun666,listen=::,port=5914 \
--autostart \
--force

virt-install \
--virt-type=kvm \
--name=centos_storage \
--hvm \
--vcpus=4 \
--memory=4096 \
--cdrom=/srv/kvm/iso/CentOS-7-x86_64-DVD-1908.iso \
--disk path=/srv/kvm/centos_storage.qcow2,size=400,format=qcow2 \
--network network=default \
--graphics vnc,password=tianlun666,listen=::,port=5913 \
--autostart \
--force

qemu-img create -f qcow2 Win10.qcow2 100G

virt-install \
--name Frytea-Win10 \
--memory 8192 \
--vcpus sockets=1,cores=3,threads=2 \
--cdrom=/srv/kvm/iso/Win10_1909_Chinese_Simplified_x64.iso \
--os-type=windows \
--os-variant=auto \
--disk /srv/kvm/Win10.qcow2,bus=virtio,size=100 \
--disk /srv/kvm/iso/virtio-win-0.1.173_amd64.vfd,device=floppy \
--network network=default,model=virtio \
--graphics vnc,password=tianlun666,listen=::,port=5912 \
--hvm \
--autostart \
--virt-type kvm

virt-install \
--virt-type=kvm \
--name=ubuntu_nextcloud \
--hvm \
--vcpus=4 \
--memory=4096 \
--cdrom=/srv/kvm/iso/ubuntu-18.04.3-live-server-amd64.iso \
--disk path=/srv/kvm/ubuntu_nextcloud.qcow2,size=400,format=qcow2 \
--network network=default \
--graphics vnc,password=tianlun666,listen=::,port=5913 \
--autostart \
--force