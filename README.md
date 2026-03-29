openwrt-ramips-mt7620-asus_rt-ac51u-squashfs-sysupgrade.bin 14.02.2025

----------------------------------------------------------------------
root@OpenWrt:~# cat /etc/rc.local 
#!/bin/sh

# Auto-wykryj i przeładuj wszystkie urządzenia DVB USB

for dev in /sys/bus/usb/devices/*/; do
    product=$(cat "$dev/product" 2>/dev/null)
    idVendor=$(cat "$dev/idVendor" 2>/dev/null)
    idProduct=$(cat "$dev/idProduct" 2>/dev/null)
    
    # Sprawdź czy to urządzenie DVB
    if [ -d "$dev/dvb" ] || ls "$dev"*/dvb 2>/dev/null | grep -q dvb; then
        busnum=$(cat "$dev/busnum" 2>/dev/null)
        devpath=$(cat "$dev/devpath" 2>/dev/null)
        usbid="$busnum-$devpath"
        echo "Znaleziono DVB: $product ($usbid)"
        echo "$usbid" >> /tmp/dvb_devices
    fi
done

# Jeśli nie znalazło przez /dvb, szukaj po znanych nazwach
if [ ! -s /tmp/dvb_devices ]; then
    for dev in /sys/bus/usb/devices/*/; do
        product=$(cat "$dev/product" 2>/dev/null)
        case "$product" in
            *Cinergy*|*Xbox*One*|*Tuner*|*DVB*|*dib0700*|*dw2102*)
                busnum=$(cat "$dev/busnum" 2>/dev/null)
                devpath=$(cat "$dev/devpath" 2>/dev/null)
                usbid="$busnum-$devpath"
                echo "Znaleziono DVB (name): $product ($usbid)"
                echo "$usbid" >> /tmp/dvb_devices
            ;;
        esac
    done
fi

# Unbind wszystkich
while read usbid; do
    echo "Unbind: $usbid"
    echo "$usbid" > /sys/bus/usb/drivers/usb/unbind
done < /tmp/dvb_devices

sleep 3

# Bind wszystkich
while read usbid; do
    echo "Bind: $usbid"
    echo "$usbid" > /sys/bus/usb/drivers/usb/bind
    sleep 2
done < /tmp/dvb_devices

rm -f /tmp/dvb_devices

/etc/init.d/minisatip restart

exit 0

root@OpenWrt:~# 
-------------------------------------------------------------------------------
root@OpenWrt:~#  strings /lib/modules/6.6.77/dvb-usb-dib0700.ko | grep depends




depends=usbcore,dib0090,dib7000m,dib0070,dvb-usb,rc-core,dib3000mc

__UNIQUE_ID_depends240

__UNIQUE_ID_depends240

root@OpenWrt:~# 
