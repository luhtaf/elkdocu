# Pendahuluan

## Import Ovf

Hal pertama yang dilakukan adalah melakkukan import ubuntu pada mesin virtualisasi baik Virtualbox, VMware ataupun virtualisasi lainnya. Jaringan yang digunakan adalah NAT atau Bridge, pastikan mesin vm memiliki [IP](#ip)

Untuk Virtualbox, ovf dapat diimport melalui menu `File/Import Appliance`, seperti pada gambar berikut
![Gambar Virtualbox](https://drive.bssn.go.id/apps/files_sharing/publicpreview/Pc73kGpLY3C7WEq?fileId=2172348&file=/vbox.png&x=1536&y=864&a=true)

Sedangkan, untuk VMware, ovf dapat diimport melalui menu `File/Open`, seperti pada gambar berikut
![Gambar VM Ware](https://drive.bssn.go.id/apps/files_sharing/publicpreview/Pc73kGpLY3C7WEq?fileId=2172351&file=/vmware.png&x=1536&y=864&a=true)

## User Root

pastikan ganti user anda menjadi user `root` dengan mengetikkan perintah dibawah untuk memudahkan proses instalasi, karena semua tahap instalasi akan kita gunakan user root sebagai admin.

    sudo su

## IP
Kemudian pastikan os anda sudah memiliki ip lokal dengan mengetikkan `ip addr`, kurang lebih outputnya adalah:

    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
        valid_lft forever preferred_lft forever
    2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 00:0c:29:ce:43:4a brd ff:ff:ff:ff:ff:ff
        inet 192.168.38.3/24 brd 192.168.38.255 scope global dynamic ens33
        valid_lft 1650sec preferred_lft 1650sec
        inet6 fe80::20c:29ff:fece:434a/64 scope link
        valid_lft forever preferred_lft forever
IP yang digunakan pada contoh diatas adalah `192.168.38.3` pada interfacce `ens33`, yang berbeda masing masing DHCP mesin VM yang digunakan. 

Kemudian untuk memastikan mesin tersambung ke internet lakukan ping ke dns google

    ping 8.8.8.8


## SSH
Untuk memudahankan, dapat dilakukan remote via ssh langsung ke ip target mesin menggunakan aplikasi seperti putty atau mobaxterm dan sejenisnya.
![Gambar Tools SSH](https://drive.bssn.go.id/apps/files_sharing/publicpreview/Pc73kGpLY3C7WEq?fileId=2172309&file=/mobaxterm.png&x=1536&y=864&a=true)
