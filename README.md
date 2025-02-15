openwrt-ramips-mt7620-asus_rt-ac51u-squashfs-sysupgrade.bin 14.02.2025

----------------------------------------------------------------------
root@OpenWrt:~# cat /etc/rc.local 



modprobe dvb-usb-dib0700
wait 5
/etc/init.d/minisatip restart

exit 0


-------------------------------------------------------------------------------
root@OpenWrt:~#  strings /lib/modules/6.6.77/dvb-usb-dib0700.ko | grep depends




depends=usbcore,dib0090,dib7000m,dib0070,dvb-usb,rc-core,dib3000mc

__UNIQUE_ID_depends240

__UNIQUE_ID_depends240

root@OpenWrt:~# 
