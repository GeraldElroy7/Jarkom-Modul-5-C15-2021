# Jarkom-Modul-5-C15-2021

## Anggota Kelompok

1. Gerald Elroy Van Ryan - 05111940000187
2. Riza Dwi Andhika - 05111940000149
3. Gian Ega Wijaya - 05111940000214

## A. Topologi

Pertama, buat terlebih dulu topologi yang akan dipakai pada soal ini seperti berikut :

![image](https://user-images.githubusercontent.com/64303057/145675706-23165d1a-600f-4294-9e3b-6d80566c39aa.png)

dengan beberapa keterangan tambahan :

- Doriki sebagai DNS Server

Yang perlu ditambahkan :

i. Install bind9

```
apt-get update
apt-get install bind9 -y
```

ii. Edit konfigurasi pada file `/etc/bind/named.conf.options` dengan *uncomment* forwarders dan ubah IP-nya menjadi `192.168.122.1`. Tambahkan juga `allow-query{any;};`. seperti berikut :

![image](https://user-images.githubusercontent.com/64303057/145675874-3f2f944d-98ab-45ec-8afc-e907f5acecaa.png)

Jangan lupa untuk restart service dns dengan `service bind9 restart`

- Jipangu sebagai DHCP Server

Yang perlu ditambahkan :

i. Install DHCP

```
apt-get update
apt-get install isc-dhcp-server -y
```

ii. Edit konfigurasi pada file `/etc/default/isc-dhcp-server` & tambahkan **eth 0** pada `INTERFACES`. Hasilnya sebagai berikut :

![image](https://user-images.githubusercontent.com/64303057/145675952-7ea26af1-4577-4e0b-8bd8-bed906f8e799.png)

Restart service DHCP dengan command `service isc-dhcp-server restart`.

- Maingate dan Jorge adalah Web Server
-	Jumlah Host pada Blueno adalah 100 host
-	Jumlah Host pada Cipher adalah 700 host
-	Jumlah Host pada Elena adalah 300 host
-	Jumlah Host pada Fukurou adalah 200 host

## B. Subnetting

Pada soal ini, kami akan menggunakan metode subnetting VLSM. Pembagiannya sebagai berikut :

![topologi jarkom praktikum 5](https://user-images.githubusercontent.com/64303057/145676040-98e7eb6b-3a33-47ee-8c3c-e6fad871ea77.png)

![image](https://user-images.githubusercontent.com/64303057/145676066-0975dc6f-17ba-43d8-88de-e32c2254bacd.png)

Lakukan perhitungan dengan tree unduk mendapatkan NID pada tiap subnet :

![diagram jarkom praktikum 5](https://user-images.githubusercontent.com/64303057/145676091-806845ad-d95a-433a-be26-8940e6bc839f.png)

![image](https://user-images.githubusercontent.com/64303057/145676113-afb9dfe4-43d7-470b-af00-1e5b22ac1494.png)

Karena semua host diberi IP oleh DHCP Server, maka static subnetting hanya dilakukan di Router (ex : FOOSHA), DNS Server, DHCP Server, dan Web Server.

Pada masing-masing network configuration adalah sebagai berikut :

**FOOSHA**
```
auto eth0
iface eth0 inet static
    address 192.168.122.25
    netmask 255.255.255.0
    gateway 192.168.122.1

auto eth1
iface eth1 inet static
	address 192.191.0.2
	netmask 255.255.255.252

auto eth2
iface eth2 inet static
	address 192.191.0.5
	netmask 255.255.255.252
```

**WATER7**

```
auto eth0
iface eth0 inet static
	address 192.191.0.1
	netmask 255.255.255.252

auto eth1
iface eth1 inet static
	address 192.191.0.129
	netmask 255.255.255.128

auto eth2
iface eth2 inet static
	address 192.191.4.1
	netmask 255.255.252.0

auto eth3
iface eth3 inet static
	address 192.191.0.9
	netmask 255.255.255.248
```

**GUANHAO**

```
auto eth0
iface eth0 inet static
	address 192.191.0.6
	netmask 255.255.255.252
	gateway 192.168.0.5

auto eth1
iface eth1 inet static
	address 192.191.2.1
	netmask 255.255.254.0

auto eth2
iface eth2 inet static
	address 192.191.1.1
	netmask 255.255.255.0

auto eth3
iface eth3 inet static
	address 192.191.0.17
	netmask 255.255.255.248
```

**JIPANGU**

**DORIKI**

**JORGE**

**MAINGATE**




