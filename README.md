# 📖 Laporan Resmi Jaringan Komputer Modul 3 E16 2021 

**Anggota Kelompok:**
 - Ishaq Adheltyo (05111940000167)

**📝 Note untuk Asisten dan Penguji: Untuk Hasil Testing dan Screenshot Ada Pada Akhir Setelah Pengerjaan (Ada Sub Bagian Sendiri)**

## 📡 Topologi
![image](https://user-images.githubusercontent.com/49280352/141613485-c6661330-906b-47a7-bb37-1f8a31ebd022.png)

## 🔧 Config

#### 🔧 Edit Network Configuration Foosha

```
auto eth0
iface eth0 inet dhcp

auto eth1
iface eth1 inet static
	address 10.37.1.1
	netmask 255.255.255.0

auto eth2
iface eth2 inet static
	address 10.37.2.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 10.37.3.1
	netmask 255.255.255.0
```

#### 🔧 Edit Network Configuration EniesLobby

```
auto eth0
iface eth0 inet static
	address 10.37.2.2
	netmask 255.255.255.0
	gateway 10.37.2.1
```


#### 🔧 Edit Network Configuration Water7

```
auto eth0
iface eth0 inet static
    address 10.37.2.3
    netmask 255.255.255.0
    gateway 10.37.2.1
```

#### 🔧 Edit Network Configuration Jipangu

```
auto eth0
iface eth0 inet static
	address 10.37.2.4
	netmask 255.255.255.0
	gateway 10.37.2.1
```

#### 🔧 Edit Network Configuration Loguetown

```
auto eth0
iface eth0 inet dhcp
```

#### 🔧 Edit Network Configuration Alabasta

```
auto eth0
iface eth0 inet dhcp
```

#### 🔧 Edit Network Configuration TottoLand

```
auto eth0
iface eth0 inet dhcp
```

#### 🔧 Edit Network Configuration Skypie

```
auto eth0
iface eth0 inet dhcp #ip dari dhcp
hwaddress ether da:10:25:8d:c0:7f
```

Keterangan:
- Untuk **Foosha** mendapatkan IP static karena menjadi **DHCP Relay**
- Untuk **Jipangu** mendapatkan IP static karena menjadi **DHCP Server**
- Untuk **Water7** mendapatkan IP static karena menjadi **Proxy Server**
- Untuk **EniesLobby** mendapatkan IP static karena menjadi **DNS Server**
- Untuk **Loguetown, TottoLand, Alabasta** mendapatkan ip dari dhcp
- Untuk **Skypie** mendapatkan ip static 10.37.3.69 karena ketentuan soal nomor 7

#### 🔧 Configurasi tambahan pada Foosha

- Tambahkan perintah dibawah pada .bashrc
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s [Prefix IP].0.0/16
```
Keterangan:
- **iptables** merupakan suatu tools dalam sistem operasi Linux yang berfungsi sebagai filter terhadap lalu lintas data.
- **Nat** adalah metode penafsiran alamat jaringan yang digunakan untuk menghubungkan lebih dari satu komputer ke jaringan internet dengan satu IP.
- **Masquerade** berfungsi untuk menyamarkan paket, misal mengganti alamat pengirim dengan alamat router.

- Buka /etc/resolv.conf

```
cat /etc/resolv.conf
```
Hasil dari file diatas, salin pada node **Jipangu, EniesLobby, Water7** agar node dapat tersambung dengan internet.

## 🏷️ Prepare Tools: Sebelum lanjut ke soal shift pastikan meginstall semua tools dibawah

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Foosha

```
apt-get update
apt-get install nano
apt-get install isc-dhcp-relay -y
```

#### 🖥️ Node Jipangu

```
apt-get update
apt-get install nano
apt-get install isc-dhcp-server -y
```

#### 🖥️ Node EniesLobby

```
apt-get update
apt-get install nano
apt-get install bind9 -y
```

#### 🖥️ Node Water7

```
apt-get update
apt-get install nano
apt-get install squid -y
apt-get install apache2-utils -y
```

#### 🖥️ Node Skypie

- Install tools yang dibutuhkan pada seluruh soal

```
apt-get update
apt-get install nano
apt-get install apache2 -y
apt-get install php -y
apt-get install libapache2-mod-php7.0
```

- Download isi website yang sudah disediakan pada soal dan simpan di root agar tidak hilang (**cukup satu kali dijalankan**)

```
apt-get install wget -y
apt-get install unzip -y
wget https://github.com/FeinardSlim/Praktikum-Modul-2-Jarkom/archive/refs/heads/main.zip
unzip main.zip
cd Praktikum-Modul-2-Jarkom-main/
unzip super.franky.zip
```

#### 🖥️ Node Loguetown, Alabasta, TottoLand

```
apt-get update
apt-get install nano
apt-get install lynx -y
```

## 🏷️ Soal 1: Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai DNS Server, Jipangu sebagai DHCP Server, Water7 sebagai Proxy Server

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Jipangu

- Edit Config dhcp server dengan menaruh interfaces 'eth0' karena interface eth0 adalah interface yang dipakai untuk dhcp request (lihat topologi)

```
nano /etc/default/isc-dhcp-server
```

```
#Defaults for isc-dhcp-server initscript
#sourced by /etc/init.d/isc-dhcp-server
#installed at /etc/default/isc-dhcp-server by the maintainer scripts

#This is a POSIX shell fragment

#Path to dhcpd's config file (default: /etc/dhcp/dhcpd.conf).
#DHCPD_CONF=/etc/dhcp/dhcpd.conf

#Path to dhcpd's PID file (default: /var/run/dhcpd.pid).
#DHCPD_PID=/var/run/dhcpd.pid

#Additional options to start dhcpd with.
#Don't use options -cf or -pf here; use DHCPD_CONF/ DHCPD_PID instead
#OPTIONS=""

#On what interfaces should the DHCP server (dhcpd) serve DHCP requests?
#Separate multiple interfaces with spaces, e.g. "eth0 eth1".
INTERFACES="eth0"
```

## 🏷️ Soal 2: dan Foosha sebagai DHCP Relay

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Foosha

- Edit SERVERS berisi ip node dimana DHCP Relay mengetahui siapa DHCP Server (Jipangu), Isi INTERFACES dengan 'eth1, eth2, eth3' untuk kemana saja DHCP relay bisa mendistribusikan IP dari DHCP Server (lihat topologi)

```
nano /etc/default/isc-dhcp-relay 
```

```
# Defaults for isc-dhcp-relay initscript
# sourced by /etc/init.d/isc-dhcp-relay
# installed at /etc/default/isc-dhcp-relay by the maintainer scripts

#
# This is a POSIX shell fragment
#

# What servers should the DHCP relay forward requests to?
SERVERS="10.37.2.4"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth1 eth2 eth3"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```

```
service isc-dhcp-relay start
```

## 🏷️ Soal 3, 4, 5, 6: Semua client yang ada HARUS menggunakan konfigurasi IP dari DHCP Server. Client yang melalui Switch1 mendapatkan range IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50 . Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut. Lama waktu DHCP server meminjamkan alamat IP kepada Client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit. Dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Jipangu

- Edit dhcpd.conf dan tambahkan beberapa config subnet untuk membantu DHCP Server mengenali topologi, dan isikan beberapa aturan di subnet 1 10.37.1.0 dan 10.37.2.0 sesuai permintaan soal nomor 3, 4, dan 6

```
# tambahkan config dibawah ini pada dhcpd.conf
subnet 'NID' netmask 'Netmask' {
    range 'IP_Awal' 'IP_Akhir';
    option routers 'iP_Gateway';
    option broadcast-address 'IP_Broadcast';
    option domain-name-servers 'DNS_yang_diinginkan';
    default-lease-time 'Waktu';
    max-lease-time 'Waktu';
}
```

```
nano /etc/dhcp/dhcpd.conf
```

```
#
# Sample configuration file for ISC dhcpd for Debian
#
# Attention: If /etc/ltsp/dhcpd.conf exists, that will be used as
# configuration file instead of this file.
#
#

# The ddns-updates-style parameter controls whether or not the server will
# attempt to do a DNS update when a lease is confirmed. We default to the
# behavior of the version 2 packages ('none', since DHCP v2 didn't
# have support for DDNS.)
ddns-update-style none;

# option definitions common to all supported networks...
option domain-name "example.org";
option domain-name-servers ns1.example.org, ns2.example.org;

default-lease-time 600;
max-lease-time 7200;

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
#authoritative;

# Use this to send dhcp log messages to a different log file (you also
# have to hack syslog.conf to complete the redirection).
log-facility local7;

# No service will be given on this subnet, but declaring it helps the
# DHCP server to understand the network topology.

#subnet 10.152.187.0 netmask 255.255.255.0 {
#}

# This is a very basic subnet declaration.

#subnet 10.254.239.0 netmask 255.255.255.224 {
#  range 10.254.239.10 10.254.239.20;
#  option routers rtr-239-0-1.example.org, rtr-239-0-2.example.org;
#}

# This declaration allows BOOTP clients to get dynamic addresses,
# which we don't really recommend.

#subnet 10.254.239.32 netmask 255.255.255.224 {
#  range dynamic-bootp 10.254.239.40 10.254.239.60;
#  option broadcast-address 10.254.239.31;
#  option routers rtr-239-32-1.example.org;
#}

# A slightly different configuration for an internal subnet.
#subnet 10.5.5.0 netmask 255.255.255.224 {
#  range 10.5.5.26 10.5.5.30;
#  option domain-name-servers ns1.internal.example.org;
#  option domain-name "internal.example.org";
#  option subnet-mask 255.255.255.224;
#  option routers 10.5.5.1;
#  option broadcast-address 10.5.5.31;
#  default-lease-time 600;
#  max-lease-time 7200;
#}

# Hosts which require special configuration options can be listed in
# host statements.   If no address is specified, the address will be
# allocated dynamically (if possible), but the host-specific information
# will still come from the host declaration.

#host passacaglia {
#  hardware ethernet 0:0:c0:5d:bd:95;
#  filename "vmunix.passacaglia";
#  server-name "toccata.fugue.com";
#}

# Fixed IP addresses can also be specified for hosts.   These addresses
# should not also be listed as being available for dynamic assignment.
# Hosts for which fixed IP addresses have been specified can boot using
# BOOTP or DHCP.   Hosts for which no fixed address is specified can only
# be booted with DHCP, unless there is an address range on the subnet
# to which a BOOTP client is connected which has the dynamic-bootp flag
# set.
#host fantasia {
#  hardware ethernet 08:00:07:26:c0:a5;
#  fixed-address fantasia.fugue.com;
#}

# You can declare a class of clients and then do address allocation
# based on that.   The example below shows a case where all clients
# in a certain class get addresses on the 10.17.224/24 subnet, and all
# other clients get addresses on the 10.0.29/24 subnet.

#class "foo" {
#  match if substring (option vendor-class-identifier, 0, 4) = "SUNW";
#}

#shared-network 224-29 {
#  subnet 10.17.224.0 netmask 255.255.255.0 {
#    option routers rtr-224.example.org;
#  }
#  subnet 10.0.29.0 netmask 255.255.255.0 {
#    option routers rtr-29.example.org;
#  }
#  pool {
#    allow members of "foo";
#    range 10.17.224.10 10.17.224.250;
#  }
#  pool {
#    deny members of "foo";
#    range 10.0.29.10 10.0.29.230;
#  }
#}

subnet 10.37.1.0 netmask 255.255.255.0 {
    range 10.37.1.20 10.37.1.99;
    range 10.37.1.150 10.37.1.169;
    option routers 10.37.1.1;
    option broadcast-address 10.37.1.255;
    option domain-name-servers 10.37.2.2;
    default-lease-time 360;
    max-lease-time 7200;
}

subnet 10.37.3.0 netmask 255.255.255.0 {
    range 10.37.3.30 10.37.3.50;
    option routers 10.37.3.1;
    option broadcast-address 10.37.3.255;
    option domain-name-servers 10.37.2.2;
    default-lease-time 720;
    max-lease-time 7200;
}

subnet 10.37.2.0 netmask 255.255.255.0 {
        option routers 10.37.2.1;
}
```

```
service isc-dhcp-server stop
service isc-dhcp-server start
```

Keterangan:
- **subnet 'NID'** adalah IP Interface tujuan dengan byte terakhirnya di 0 kan (Untuk NID yang proper akan dibahas pada modul berikutnya)
- **netmask 'Netmask'** adalah nilai netmask pada subnet, bisa lihat di edit network config GNS3
- **range 'IP_Awal' 'IP_Akhir'** Rentang alamat IP yang akan didistribusikan dan dipakai secara dinamis
- **option routers 'Gateway'** adalah IP gateway dari router menuju client sesuai konfigurasi subnet
- **option broadcast-address 'IP_Broadcast'** adalah IP broadcast pada subnet
- **option domain-name-servers 'DNS_yang_diinginkan'** adalah DNS yang ingin kita berikan pada client
- **Lease time** adalah Waktu yang dialokasikan ketika sebuah IP dipinjamkan kepada komputer client. 
- **default-lease-time 'Waktu'** adalah Lama waktu DHCP server meminjamkan alamat IP kepada client, dalam satuan detik
- **max-lease-time 'Waktu'** adalah Waktu maksimal yang di alokasikan untuk peminjaman IP oleh DHCP server ke client dalam satuan detik

#### 🖥️ Node EniesLobby

- Edit named.conf.options untuk menambahkan forwarders agar client bisa mengakses internet

```
nano /etc/bind/named.conf.options
```

```
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
                192.168.122.1;
        };

        //=====================================================================$
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //=====================================================================$
        //dnssec-validation auto;
        allow-query { any; };

        auth-nxdomain no;    # conform to RFC1035
        listen-on-v6 { any; };
};
```

```
service bind9 restart
```

## 🏷️ Soal 7: Luffy dan Zoro berencana menjadikan Skypie sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Jipangu

- Cek Hardware Address dengan command ```ip a``` kemudian lihat interface yang berhubungan dengan router pada kasus ini adalah 'eth0' (lihat topologi), kemudian lihat pada bagian ```link/ether``` didapatkan nilai ```da:10:25:8d:c0:7f```
- Edit dhcpd.conf dengan menambahkan config baru dibawah

```
nano /etc/dhcp/dhcpd.conf
```

```
host Skypie {
    hardware ethernet da:10:25:8d:c0:7f;
    fixed-address 10.37.3.69;
}
```

```
service isc-dhcp-server restart
```

Keterangan:
- **hardware ethernet** adalah berisi hardware address node yang sudah didapat dengan cara diatas
- **fixed-address** berisi ip fixed yang mau diisi

#### 🖥️ Node Jipangu

- Edit Config Interface **(Jika diawal belum diganti)**

```
nano /etc/network/interfaces
```

```
auto eth0
iface eth0 inet dhcp #ip dari dhcp
hwaddress ether da:10:25:8d:c0:7f #hardware address
```

## 🏷️ Soal 8: Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000. Pada Loguetown, proxy harus bisa diakses dengan nama jualbelikapal.yyy.com dengan port yang digunakan adalah 5000

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Water7

- Edit squid.conf dengan menambah beberapa aturan pada awal file

```
nano /etc/squid/squid.conf
```

```
http_port 5000
visible_hostname jualbelikapal.e16.com
http_access allow all
```

```
service squid restart
```

Keterangan:
- **http_port** adalah port untuk akses http
- **visible_hostname** adalah nama hostname yang bisa dilihat oleh user
- **http_access allow all** adalah config untuk membolehkan seluruh client mengakses http

#### 🖥️ Node EniesLobby

- Edit named.conf.local dengan menambahkan zone untuk jualbelikapal.e16.com

```
nano /etc/bind/named.conf.local
```

```
zone "jualbelikapal.e16.com" {
        type master;
        file "/etc/bind/kaizoku/jualbelikapal.e16.com";
};
```

```
mkdir kaizoku
nano /etc/bind/kaizoku/jualbelikapal.e16.com
```

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     jualbelikapal.e16.com. root.jualbelikapal.e16.com. (
                        2021110901      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      jualbelikapal.e16.com.
@       IN      A       10.37.2.3
```

```
service bind9 restart
```

## 🏷️ Soal 9: Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu luffybelikapalyyy dengan password luffy_yyy dan zorobelikapalyyy dengan password zoro_yyy

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Water7

- Buat akun untuk luffybelikapale16 dan zorobelikapale16 yang disimpan di ```/etc/squid/passwd```

```
touch /etc/squid/passwd

htpasswd -m /etc/squid/passwd luffybelikapale16
luffy_e16

htpasswd -m /etc/squid/passwd zorobelikapale16
zoro_e16
```

- Edit config squid dengan menambahkan config auth baru

```
nano /etc/squid/squid.conf
```

```
http_port 5000
visible_hostname jualbelikapal.e16.com

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

```
service squid restart
```

Keterangan:
- **auth_param** digunakan untuk mengatur auth pada squid
- **auth_param basic program** digunakan untuk mendelarasikan file auth eksternal
- **auth_param basic children** adalah jumlah maksimal autentikator muncul
- **auth_param basic realm Proxy** Teks yang akan muncul pada pop-up autentikasi
- **auth_param basic credentialsttl** adalah masa aktif suatu autentikasi
- **auth_param basic casesensitive** digunakan untuk mengatur apakah username bersifat case sensitive atau tidak
- **acl** digunakan untuk mendefinisikan pengaturan akses tertentu
- **acl USERS proxy_auth REQUIRED** digunakan untuk memberitahu bahwa dibutuhkan auth
- **http_access allow USERS** untuk memasukan aturan USERS pada http_access dimana yang diperbolehkan membuka http hanya melewati auth

## 🏷️ Soal 10: Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi hanya dapat diakses setiap hari Senin-Kamis pukul 07.00-11.00 dan setiap hari Selasa-Jum’at pukul 17.00-03.00 keesokan harinya (sampai Sabtu pukul 03.00)

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Water7

- Buat file bernama acl.conf yang berisi acl untuk waktu yang dilarang akses

```
nano /etc/squid/acl.conf
```

```
acl AVAILABLE_WORKING_1 time S 00:00-23:59
acl AVAILABLE_WORKING_2 time MT 00:00-06:59
acl AVAILABLE_WORKING_3 time M 11:01-23:59
acl AVAILABLE_WORKING_4 time TWH 11:01-16:59
acl AVAILABLE_WORKING_5 time WH 03:01-06:59
acl AVAILABLE_WORKING_6 time F 03:01-16:59
acl AVAILABLE_WORKING_7 time A 03:01-23:59
```

- Edit config squid dengan include acl.conf serta deny http_access pada waktu yang dilarang di acl.conf

```
nano /etc/squid/squid.conf
```

```
include /etc/squid/acl.conf

http_port 5000
visible_hostname jualbelikapal.e16.com

http_access deny AVAILABLE_WORKING_1
http_access deny AVAILABLE_WORKING_2
http_access deny AVAILABLE_WORKING_3
http_access deny AVAILABLE_WORKING_4
http_access deny AVAILABLE_WORKING_5
http_access deny AVAILABLE_WORKING_6
http_access deny AVAILABLE_WORKING_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS
```

```
service squid restart
```

**NOTE: cara untuk nomor 10 bisa 2, deny waktu yang dilarang atau membuat allow pada waktu yang diperbolehkan**

## 🏷️ Soal 11: Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses google.com, akan diredirect menuju super.franky.yyy.com dengan website yang sama pada soal shift modul 2. Web server super.franky.yyy.com berada pada node Skypie

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node EniesLobby

- Edit named.conf.local dengan menambahkan zone untuk super.franky.e16.com

```
nano /etc/bind/named.conf.local
```

```
zone "jualbelikapal.e16.com" {
        type master;
        file "/etc/bind/kaizoku/jualbelikapal.e16.com";
};

zone "super.franky.e16.com" {
    type master;
    file "/etc/bind/kaizoku/super.franky.e16.com";
    allow-transfer { 10.37.3.69; }; // Masukan IP Skypie tanpa tanda petik
};
```

- Buat bind data untuk super.franky.e16.com pada folder kaizoku arahkan ke Skypie karena webserver ada disana

```
nano /etc/bind/kaizoku/super.franky.e16.com
```

```
;
; BIND data file for local loopback interface
;
$TTL    604800
@       IN      SOA     super.franky.e16.com. root.super.franky.e16.com. (
                        2021110901      ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL
;
@       IN      NS      super.franky.e16.com.
@       IN      A       10.37.3.69
www     IN      CNAME   super.franky.e16.com.
```

```
service bind9 restart
```

#### 🖥️ Node Skypie

- Buat config untuk super.franky.e16.com pada ```/etc/apache2/sites-available/```

```
nano /etc/apache2/sites-available/super.franky.e16.com.conf
```

```
<VirtualHost *:80>
    ServerName super.franky.e16.com
    ServerAlias www.super.franky.e16.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/super.franky.e16.com

    <Directory /var/www/super.franky.e16.com>
        Options +Indexes
        AllowOverride All
    </Directory>

    <Directory /var/www/super.franky.e16.com/public>
        Options +Indexes
    </Directory>

    Alias "/js" "/var/www/super.franky.e16.com/public/js"

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined

    ErrorDocument 404 /error/404.html
</VirtualHost>
```

- Enable config super.franky.e16.com dengan ```a2ensite``` dan copy isi super.franky.e16.com dari zip github modul 2 **(Link Repo dan cara download ada pada langkah install tools diatas)**

```
cd /etc/apache2/sites-available
a2ensite super.franky.e16.com.conf
service apache2 restart
cd
cd Praktikum-Modul-2-Jarkom-main
cp -r super.franky /var/www/super.franky.e16.com
```

#### 🖥️ Node Water7

- Edit resolv.conf dengan menambah IP EniesLobby

```
nano /etc/resolv.conf
```

```
nameserver 10.37.2.2
nameserver 192.168.122.1
```

- Edit squid.conf dengan menambahkan config untuk redirect dari google.com ke super.franky.e16.com

```
nano /etc/squid/squid.conf
```

```
include /etc/squid/acl.conf

http_port 5000
visible_hostname jualbelikapal.e16.com

http_access deny AVAILABLE_WORKING_1
http_access deny AVAILABLE_WORKING_2
http_access deny AVAILABLE_WORKING_3
http_access deny AVAILABLE_WORKING_4
http_access deny AVAILABLE_WORKING_5
http_access deny AVAILABLE_WORKING_6
http_access deny AVAILABLE_WORKING_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

acl BLACKLIST dstdomain google.com
deny_info http://super.franky.e16.com/ BLACKLIST
http_reply_access deny BLACKLIST
```

```
service squid restart
```

Keterangan:
- **acl BLACKLIST dstdomain google.com** adalah acl yang berisi config untuk blacklist destination domain kearah google.com
- **deny_info http://super.franky.e16.com/ BLACKLIST** adalah untuk return error jika request tidak memenuhi pada http_access
- **http_reply_access deny BLACKLIST** untuk blockir response yang didapat dari google.com

## 🏷️ Soal 12 dan 13: Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di super.franky.yyy.com. Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan gambar, ia mendapatkan gambar dan melihatnya dengan kecepatan 10 kbps. Sedangkan, Zoro yang sangat bersemangat untuk mencari harta karun, sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya.

### ✍️ Langkah-Langkah Pengerjaan:

#### 🖥️ Node Water7

- Buat file acl-bandwidth.conf untuk menaruh aturan config pembatasan bandwidth pada squid

```
nano /etc/squid/acl-bandwidth.conf
```

```
acl download url_regex -i \.jpg$ \.png$

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd

acl luffy proxy_auth luffybelikapale16
acl zoro proxy_auth zorobelikapale16

delay_pools 2
delay_class 1 1
delay_parameters 1 1250/1250
delay_access 1 deny zoro
delay_access 1 allow download
delay_access 1 deny all

delay_class 2 1
delay_parameters 2 -1/-1
delay_access 2 allow zoro
delay_access 2 deny luffy
delay_access 2 deny all
```

Keterangan: 
- **acl download url_regex -i \.jpg$ \.png$** untuk deklarasi acl download yang berisi aturan memiliki url yang mengandung kata .jpg dan .png
- **auth_param basic program** untuk deklarasi file auth diluar
- **acl luffy proxy_auth luffybelikapale16** untuk deklarasi acl luffy berisi aturan auth username luffybelikapale16
- **acl zoro proxy_auth zorobelikapale16** untuk deklarasi acl zoro berisi aturan auth username zorobelikapale16
- **delay_pools** digunakan untuk menentukan berapa bagian/pool yang akan dibuat
- **delay_class** digunakan untuk menentukan tipe/class pembagian bandwith dari setiap pool
- **delay_access** digunakan untuk membuat aturan mengakses pool yang telah dibuat (ada allow atau deny)
- **delay_parameter** digunakan untuk mengatur parameter dari pool yang telah dibuat
- **delay_parameters 1 1250/1250** karena pada soal diminta maksimal 10 kbps maka, 1250 bytes -> 10000 bit (10kbps)

- Edit squid.conf dengan include file acl-bandwidth.conf yang telah dibuat tadi

```
nano /etc/squid/squid.conf
```

```
include /etc/squid/acl.conf
include /etc/squid/acl-bandwidth.conf

http_port 5000
visible_hostname jualbelikapal.e16.com

http_access deny AVAILABLE_WORKING_1
http_access deny AVAILABLE_WORKING_2
http_access deny AVAILABLE_WORKING_3
http_access deny AVAILABLE_WORKING_4
http_access deny AVAILABLE_WORKING_5
http_access deny AVAILABLE_WORKING_6
http_access deny AVAILABLE_WORKING_7

auth_param basic program /usr/lib/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Proxy
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive on
acl USERS proxy_auth REQUIRED
http_access allow USERS

acl BLACKLIST dstdomain google.com
deny_info http://super.franky.e16.com/ BLACKLIST
http_reply_access deny BLACKLIST
```

```
service squid restart 
```

## 👨‍💻 Testing dan Screenshot

### 🏷️ Soal 1

#### 🖥️ Node EniesLobby

- Memiliki Bind9 sebagai DNS Server
![image](https://user-images.githubusercontent.com/49280352/141642528-9f4e399f-ac7c-4dcf-9e78-46786ec83ace.png)

#### 🖥️ Node Jipangu

- Memiliki isc-dhcp-server sebagai DHCP Server
![image](https://user-images.githubusercontent.com/49280352/141642561-58a756c7-20e2-4b9b-b91a-f95a22820968.png)

#### 🖥️ Node Water7

- Memiliki squid sebagai Proxy Server
![image](https://user-images.githubusercontent.com/49280352/141642615-d49c0760-7251-4c50-a611-d4e33dd1daf4.png)

### 🏷️ Soal 2

#### 🖥️ Node Foosha

- Memiliki isc-dhcp-relay sebagai DHCP Relay
![image](https://user-images.githubusercontent.com/49280352/141642695-08414648-04f9-4e49-9887-e89d850e29f3.png)

### 🏷️ Soal 3

#### 🖥️ Node Loguetown

- Memiliki IP rentang 10.37.20 - 10.37.99 dan 10.37.150 - 10.37.169
![image](https://user-images.githubusercontent.com/49280352/141642857-f9a48a95-fb85-453e-ba58-fe01005f5a22.png)

#### 🖥️ Node Alabasta

- Memiliki IP rentang 10.37.20 - 10.37.99 dan 10.37.150 - 10.37.169
![image](https://user-images.githubusercontent.com/49280352/141642877-14884af9-bd2f-4665-918c-07cbb5d660d4.png)

### 🏷️ Soal 4

#### 🖥️ Node TottoLand

- Memiliki IP rentang 10.37.20 - 10.37.99 dan 10.37.30 - 10.37.50
![image](https://user-images.githubusercontent.com/49280352/141642913-240d5171-9524-40aa-8a5e-b5fb9e4fdf16.png)


#### 🖥️ Node Foosha

- Memiliki isc-dhcp-relay sebagai DHCP Relay
![image](https://user-images.githubusercontent.com/49280352/141642695-08414648-04f9-4e49-9887-e89d850e29f3.png)

### 🏷️ Soal 5 dan 6

#### 🖥️ Node Loguetown

- Bisa Akses Internet, punya dns dari EniesLobby, dan Lease Time 360 detik
![image](https://user-images.githubusercontent.com/49280352/141643015-63a1f1e1-434a-4107-9992-3f7728fccb33.png)

#### 🖥️ Node Alabasta

- Bisa Akses Internet, punya dns dari EniesLobby, dan Lease Time 360 detik
![image](https://user-images.githubusercontent.com/49280352/141643037-be8b82aa-d819-4f4e-9bd4-44a0df5ea3c5.png)

#### 🖥️ Node TottoLand

- Bisa Akses Internet, punya dns dari EniesLobby, dan Lease Time 720 detik
![image](https://user-images.githubusercontent.com/49280352/141643055-6de04426-88c5-49c9-a697-3496ec91093e.png)

#### 🖥️ Node Skypie

- Bisa Akses Internet dan punya dns dari EniesLobby
![image](https://user-images.githubusercontent.com/49280352/141643083-7e88a9a0-e554-4656-929f-b50ccd582e03.png)

### 🏷️ Soal 7

#### 🖥️ Node Skypie

- Mempunyai IP Static 10.37.3.69 pada Node Skypie seperti yang ditentukan pada soal
![image](https://user-images.githubusercontent.com/49280352/141643083-7e88a9a0-e554-4656-929f-b50ccd582e03.png)

### 🏷️ Soal 8

#### 🖥️ Node Loguetown / Alabasta / TottoLand

- Lakukan ```export http_proxy=http://jualbelikapal.e16.com:5000```
![image](https://user-images.githubusercontent.com/49280352/141644000-a37fbfb9-2f22-409f-a47e-2520f8a4c1a7.png)

### 🏷️ Soal 9

#### 🖥️ Node Water7

- Memiliki auth luffybelikapale16 dan zorobelikapale16
![image](https://user-images.githubusercontent.com/49280352/141644317-4b8279af-f56e-4ed1-85d4-bbbe648f61ec.png)

#### 🖥️ Node Loguetown / Alabasta / TottoLand

- Memerlukan Auth berisi Username dan Password untuk mengakses HTTP
![image](https://user-images.githubusercontent.com/49280352/141647091-ef272b51-90ad-45a0-9fa9-544cc21bb5b3.png)

### 🏷️ Soal 10

#### 🖥️ Node Loguetown / Alabasta / TottoLand

- Untuk waktu akses diperbolehkan **9 November Jam 18.00**
![image](https://user-images.githubusercontent.com/49280352/141647145-92aa9eb0-d08e-4058-93ec-69a05fe963bb.png)
![image](https://user-images.githubusercontent.com/49280352/141647091-ef272b51-90ad-45a0-9fa9-544cc21bb5b3.png)

- Untuk waktu akses dilarang **9 November Jam 13.00**
![image](https://user-images.githubusercontent.com/49280352/141647178-be79abfd-2b92-4bb8-8f63-7ec48e446357.png)
![image](https://user-images.githubusercontent.com/49280352/141647223-27b7762f-6fb0-4cee-827e-556f930fa7a6.png)

### 🏷️ Soal 11

#### 🖥️ Node Loguetown / Alabasta / TottoLand

- Jika mengunjungi situs google.com akan dilakukan redirect ke super.franky.e16.com 
 ![11](https://user-images.githubusercontent.com/49280352/141647722-77b272d8-649f-47f4-8be5-1d6d2ae20f6e.gif)

### 🏷️ Soal 12

#### 🖥️ Node Loguetown / Alabasta / TottoLand

- Bandwidth download luffy dibatasi hingga 10Kbps untuk file .jpg atau .png
![12-picture](https://user-images.githubusercontent.com/49280352/141647903-0f8b063f-dff5-45cc-ae7e-b4fc50487656.gif)

- Bandwidth download luffy tidak dibatasi kecepatannya untuk file selain .jpg atau .png
![12-nopicture](https://user-images.githubusercontent.com/49280352/141647932-2aa3359f-9e56-4098-84c9-033cceeee64b.gif)

### 🏷️ Soal 13

#### 🖥️ Node Loguetown / Alabasta / TottoLand

- Bandwidth download zoro tidak dibatasi kecepatannya untuk file apapun (contoh: .jpg atau .png)
![13-picture](https://user-images.githubusercontent.com/49280352/141648019-1f39ff37-1a04-4647-a6ff-a7b58421bea8.gif)

- Bandwidth download zoro tidak dibatasi kecepatannya untuk file apapun (contoh: selain .jpg atau .png)
![13-nopicture](https://user-images.githubusercontent.com/49280352/141648045-41b241b9-07f8-4f61-8b65-d67735546de0.gif)
