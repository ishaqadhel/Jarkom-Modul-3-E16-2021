# 📖 Laporan Resmi Jaringan Komputer Modul 3 E16 2021 

**Anggota Kelompok:**
 - Ishaq Adheltyo (05111940000167)

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
- iptables merupakan suatu tools dalam sistem operasi Linux yang berfungsi sebagai filter terhadap lalu lintas data.
- Nat adalah metode penafsiran alamat jaringan yang digunakan untuk menghubungkan lebih dari satu komputer ke jaringan internet dengan satu IP.
- Masquerade berfungsi untuk menyamarkan paket, misal mengganti alamat pengirim dengan alamat router.

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

