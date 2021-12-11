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

Karena semua host diberi IP oleh DHCP Server, maka static subnetting hanya dilakukan di Router (contoh : FOOSHA), DNS Server, DHCP Server, dan Web Server.

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




## Soal 4. Akses dari subnet Blueno dan Cipher hanya diperbolehkan pada pukul 07.00 - 15.00 pada hari Senin sampai Kamis.

Langkah 1: pada `Doriki`

- untuk `Chipper`

```
iptables -A INPUT -s 192.191.0.0/22 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.191.0.0/22 -j REJECT
```

- untuk `Blueno`

```
iptables -A INPUT -s 192.191.4.0/25 -m time --timestart 07:00 --timestop 15:00 --weekdays Mon,Tue,Wed,Thu -j ACCEPT
iptables -A INPUT -s 192.191.4.0/25 -j REJECT
```

Langkah 2: Testing pada `Chipper`<br>

- berhasil<br>
  ![image](https://user-images.githubusercontent.com/71221969/145382963-dd030fad-aa30-4e4e-9e81-9c6954e52dee.png)
- gagal<br>
  ![image](https://user-images.githubusercontent.com/71221969/145386599-cc6492b5-5e94-4d06-9c12-96c9769bcac1.png)
  <br>Langkah 3: Testing pada `Blueno`<br>
- berhasil<br>
  ![image](https://user-images.githubusercontent.com/71221969/145497155-3e8f3a8d-9ab5-49b0-8945-5fab29275209.png)
- gagal<br>
  ![image](https://user-images.githubusercontent.com/71221969/145497201-99b042a7-e8d1-4702-a3e6-10209f08a28c.png)

### Kendala:

- Selalu ada typo ketika mengerjakan project

<br>

## Soal 5. Akses dari subnet Elena dan Fukuro hanya diperbolehkan pada pukul 15.01 hingga pukul 06.59 setiap harinya.

Langkah 1: pada `Doriki`

- untuk `Elena`

```
iptables -A INPUT -s 192.191.16.0/23 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```

- untuk `Fukurou`

```
iptables -A INPUT -s 192.191.18.0/24 -m time --timestart 07:00 --timestop 15:00 -j REJECT
```

Langkah 2: Testing pada `Elena`<br>

- berhasil<br>
  ![image](https://user-images.githubusercontent.com/71221969/145495422-e27865c3-2f37-4062-b010-5cfc8202f827.png)
- gagal<br>
  ![image](https://user-images.githubusercontent.com/71221969/145495225-c4c5f5a1-3d26-479b-93fb-57a7d2f89dfe.png)
  <br>
  Langkah 3: Testing pada `Fukurou`<br>
- berhasil<br>
  ![image](https://user-images.githubusercontent.com/71221969/145495587-04969f1d-e8c5-4776-8894-eed7a2d08efb.png)
  <br>- gagal
  ![image](https://user-images.githubusercontent.com/71221969/145497035-3d13aa84-8723-47b3-8da0-8accf181e8c3.png)

### Kendala:

- Selalu ada typo ketika mengerjakan project

<br>

## Soal 6. Karena kita memiliki 2 Web Server, Luffy ingin Guanhao disetting sehingga setiap request dari client yang mengakses DNS Server akan didistribusikan secara bergantian pada Jorge dan Maingate

- pada `Doriki`:
  1. membuat domain (DNS) yang mengarah ke IP random (dalam hal ini `192.191.21.1`) pada file `/etc/bind/named.conf`
     ```
     zone "jarkomc15.com" {
         type master;
         file "/etc/bind/jarkom/jarkomc15.com";
     };
     ```
  2. Buat folder jarkom dengan command:
     ```
     mkdir /etc/bind/jarkom
     ```
  3. Lalu copy `db.local` ke file `/etc/bind/jarkom/jarkomc15.com`
     ```
     cp /etc/bind/db.local /etc/bind/jarkom/jarkomc15.com
     ```
  4. Lalu edit file `/etc/bind/jarkom/jarkomc15.com`
     ![image](https://user-images.githubusercontent.com/68406409/145676368-292e45d7-6a74-4b7c-b0f7-e25fbbec73d5.png)
- pada `Guanhao`

```
iptables -A PREROUTING -t nat -p tcp -d 192.191.21.1 --dport 80 -m statistic --mode nth --every 2 --packet 0 -j DNAT --to-destination 192.191.19.2:80
iptables -A PREROUTING -t nat -p tcp -d 192.191.21.1 --dport 80 -j DNAT --to-destination 192.191.19.3:80
iptables -t nat -A POSTROUTING -p tcp -d 192.191.19.2 --dport 80 -j SNAT --to-source 192.191.21.1:80
iptables -t nat -A POSTROUTING -p tcp -d 192.191.19.3 --dport 80 -j SNAT --to-source 192.191.21.1:80
```

- Testing
  - Pada `Guanhao`, `Jorge`, `Maingate`, `Elena` dan `Fukurou` install `apt-get install netcat`
  - Pada Jorge ketikkan perintah: `nc -l -p 80`
  - Pada Maingate ketikkan perintah: `nc -l -p 80`
  - Pada client Elena dan fukurou ketikkan perintah: `nc 192.191.21.1 80`
  - Ketikkan sembarang pada client Elena dan fukurou, nanti akan muncul bergantian
- Pada `Elena`<br>
  ![image](https://user-images.githubusercontent.com/68406409/145676273-0f4a3fa8-c26b-4fdb-9f7e-f42afc0369b1.png)
- Pada `Fukurou`<br>
  ![image](https://user-images.githubusercontent.com/71221969/145501554-da1dcb3a-542a-4b34-a2a7-bffacee30b8d.png)
- Pada `Jorge`<br>
  ![image](https://user-images.githubusercontent.com/71221969/145501591-c2f8cfc7-9ee6-4fa7-82c0-30d356e4d2b5.png)
- Pada `Maingate`<br>
  ![image](https://user-images.githubusercontent.com/68406409/145676322-6f652405-ffb9-40ac-ab0a-0e8102e54ef0.png)

### Kendala:

- Selalu ada typo ketika mengerjakan project
- Sempat kesulitan dalam testing nomor 6
