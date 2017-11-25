
# 利用raspberry pi 來掃描beacon

---
# 需求
raspberry pi zero w x1

3.7v轉5v升壓板 x1

3.7v 1s錐聚電池 x1

---
# 使用腳位
pin7 and pin6 (gpio4 and gnd)

pin10 and pin14 (gpio15 and gnd)

pin2 and pin39 (5v and gnd)

---
# 電路接法
pin7先接120歐姆在接led正極
pin6接led負極

pin10先接120歐姆在接led正極
pin14接led負

pin2 接升壓板上usb v+
pin39接升壓板上usb v-

---
# raspberry pi安裝
由raspberry.org 下載img後安裝到sd card

因pi zero w沒有rj45 port

直接在sd card

boot中 touch ssh (打開ssh 服務)

boot中 nano wpa_supplicant.conf


參考 (可設定多個wifi ap)

‘’’ sh

country=CN
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
 
network={
ssid="WiFi-A"
psk="12345678"
key_mgmt=WPA-PSK
priority=1
}
 
network={
ssid="WiFi-B"
psk="12345678"
key_mgmt=WPA-PSK
priority=2
scan_ssid=1
}

‘’’

---

# 記錄 1s電池可用多久時間 log

crontab -e

‘’’sh
@reboot /bin/sh /root/log.sh

nano /root/log.sh
LOGFILE=/log/$(date +%Y-%m-%d).txt
log(){
    message="$@"
    echo $message
    echo $message >>$LOGFILE
}
log "open system $(date)"

---
crontab -e
*/10 * * * * /bin/sh /root/10minlog.sh

nano /root/10minlog.sh

LOGFILE=/log/10min/10-$(date +%Y-%m-%d).txt
log(){
    message="$@"
    echo $message
    echo $message >>$LOGFILE
}
log "10min log data - $(date)"

---

#
ibeacon_scan
#iBeacon Scan by Radius Networks

ledgos
#過濾beacon uuid用

runled
#放到crontab or /etc/rc.local


# 重點
需安裝藍芽及相關。。。

sudo apt-get install bluez bluez-hcidump -y

sudo apt-get install bc

—-
