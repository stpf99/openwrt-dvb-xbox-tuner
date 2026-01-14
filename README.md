openwrt-ramips-mt7620-asus_rt-ac51u-squashfs-sysupgrade.bin 14.02.2025

----------------------------------------------------------------------
root@OpenWrt:~# cat /etc/rc.local 

#/etc/init.d/minisatip restart

echo "1-1.2" > /sys/bus/usb/drivers/usb/unbind

sleep 2

echo "1-1.2" > /sys/bus/usb/drivers/usb/bind

#modprobe dvb-usb-dib0700

#modprobe dvb-usb-dw2102

#wait 5


exit 0

root@OpenWrt:~# 
-------------------------------------------------------------------------------
root@OpenWrt:~#  strings /lib/modules/6.6.77/dvb-usb-dib0700.ko | grep depends




depends=usbcore,dib0090,dib7000m,dib0070,dvb-usb,rc-core,dib3000mc

__UNIQUE_ID_depends240

__UNIQUE_ID_depends240

root@OpenWrt:~# 
