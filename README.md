
# Jarkom Modul 3 A08 - 2021

Anggota:
-   05111940000030 - Bunga Fairuz Wijdan
-   05111940000062 - Thomas Felix Brilliant
-   05111940000145 - Ikhlasul Amal Rivel

**Daftar Soal:**

* [Soal 1](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-1)
* [Soal 2](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-2)
* [Soal 3](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-3)
* [Soal 4](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-4)
* [Soal 5](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-5)
* [Soal 6](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-6)
* [Soal 7](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-7)
* [Soal 8](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-8)
* [Soal 9](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-9)
* [Soal 10](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-10)
* [Soal 11](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-11)
* [Soal 12](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-12)
* [Soal 13](https://github.com/ThomasFel/Jarkom-Modul-3-A08-2021#Soal-13)

## Narasi Pendahuluan

Luffy yang sudah menjadi Raja Bajak Laut ingin mengembangkan daerah kekuasaannya dengan membuat peta seperti berikut:

<img src="https://user-images.githubusercontent.com/37539546/141499741-55668306-c5dd-4510-99a0-626dc5b8fdb4.JPG" width=600>

## Soal 1

### Luffy bersama Zoro berencana membuat peta tersebut dengan kriteria EniesLobby sebagai `DNS Server`, Jipangu sebagai `DHCP Server`, Water7 sebagai `Proxy Server`.

### Jawaban:

Pertama, membuat topologi sesuai permintaan soal. Kemudian *setting network* masing-masing *node* dengan fitur `Edit network configuration` yang ada di menu `Configure`. Karena akan menggunakan DHCP, maka *setting network* akan sedikit berbeda dengan [Praktikum Modul 2](https://github.com/ThomasFel/Jarkom-Modul-2-A08-2021). *Setting* awal yang sudah ada dapat dihapus dan diganti dengan konfigurasi berikut:

- Foosha
  ```
  auto eth0
  iface eth0 inet dhcp

  auto eth1
  iface eth1 inet static
	  address 10.3.1.1
	  netmask 255.255.255.0

  auto eth2
  iface eth2 inet static
	  address 10.3.2.1
	  netmask 255.255.255.0

  auto eth3
    iface eth3 inet static
	  address 10.3.3.1
	  netmask 255.255.255.0
  ```
  
- Loguetown
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
  
- Alabasta
  ```
  auto eth0
  iface eth0 inet dhcp
  ```
  
- EniesLobby
  ```
  auto eth0
  iface eth0 inet static
	  address 10.3.2.2
	  netmask 255.255.255.0
	  gateway 10.3.2.1
  ```
  
- Water7
  ```
  auto eth0
  iface eth0 inet static
	  address 10.3.2.3
	  netmask 255.255.255.0
	  gateway 10.3.2.1
  ```

- Jipangu
  ```
  auto eth0
  iface eth0 inet static
	  address 10.3.2.4
	  netmask 255.255.255.0
	  gateway 10.3.2.1
  ```
  
- TottoLand
  ```
  auto eth0
  iface eth0 inet dhcp
  ```

- Skypiea
  ```
  auto eth0
  iface eth0 inet dhcp
  ```

### Foosha

*Restart* semua *node*. Lalu jalankan *command* berikut pada *router* `Foosha` untuk pengaturan lalu lintas komputer.
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE -s 10.3.0.0/16
```
(Note: *Prefix* IP yang digunakan sesuai *Prefix* IP Kelompok, dalam hal ini kelompok A8 adalah **10.3**).

Dan ketikkan *command* ini pada `Foosha` untuk melihat IP DNS:
```
cat /etc/resolv.conf
```
Akan muncul *nameserver* yang akan digunakan pada konfigurasi selanjutnya.

<img src="https://user-images.githubusercontent.com/37539546/139521833-df4ae23e-08eb-4927-bd0e-992ec548670f.JPG" width="350">

### Semua node (kecuali Foosha)

Agar *node*-*node* lainnya dapat mengakses internet, jalankan *command* berikut dan gunakan IP DNS dari `Foosha` tadi.
```
echo nameserver 192.168.122.1 > /etc/resolv.conf
```

*Restart* semua *node* kembali. Lalu, *testing* semua *node* apakah sudah terkoneksi dengan internet dengan `ping` ke [**google.com**](google.com). Sebagai contoh pada `Loguetown`:

<img src="https://user-images.githubusercontent.com/37539546/139522122-43ddb61d-0b3a-484f-a4a5-433109dd6529.JPG" width="600">

## Soal 2

### Dan Foosha sebagai `DHCP Relay`.

### Jawaban:

### Foosha

Melakukan instalasi **DHCP Relay** terlebih dahulu pada `Foosha` dengan *update package list*. *Command* yang dijalankan adalah sebagai berikut.
```
apt-get update
apt-get install isc-dhcp-relay -y
```

Buka *file* **/etc/default/isc-dhcp-relay** dan edit seperti konfigurasi berikut.

<img src="https://user-images.githubusercontent.com/37539546/141504252-90f1983b-2380-4739-93e1-42ccbb75d38c.JPG" width="600">

*Restart* DHCP Relay.
```
service isc-dhcp-relay restart
```

Untuk memastikan apakah DHCP Relay berjalan, dapat menggunakan *command* berikut.
```
service isc-dhcp-relay status
```

## Soal 3

### Luffy dan Zoro menyusun peta tersebut dengan hati-hati dan teliti.

Ada beberapa kriteria yang ingin dibuat oleh Luffy dan Zoro, yaitu:
1.  Semua *client* yang ada **HARUS** menggunakan konfigurasi IP dari **DHCP Server**.
2.  *Client* yang melalui **Switch1** mendapatkan *range* IP dari [prefix IP].1.20 - [prefix IP].1.99 dan [prefix IP].1.150 - [prefix IP].1.169.

### Jawaban:

## Soal 4

### (Lanjutan) 3. Client yang melalui Switch3 mendapatkan range IP dari [prefix IP].3.30 - [prefix IP].3.50.

### Jawaban:

## Soal 5

### (Lanjutan) 4. Client mendapatkan DNS dari EniesLobby dan client dapat terhubung dengan internet melalui DNS tersebut.

### Jawaban:

## Soal 6

### (Lanjutan) 5. Lama waktu DHCP server meminjamkan alamat IP kepada client yang melalui Switch1 selama 6 menit sedangkan pada client yang melalui Switch3 selama 12 menit, dengan waktu maksimal yang dialokasikan untuk peminjaman alamat IP selama 120 menit.

### Jawaban:

## Soal 7

### Luffy dan Zoro berencana menjadikan Skypiea sebagai server untuk jual beli kapal yang dimilikinya dengan alamat IP yang tetap dengan IP [prefix IP].3.69.

### Jawaban:

## Soal 8

### Loguetown digunakan sebagai client proxy agar transaksi jual beli dapat terjamin keamanannya, juga untuk mencegah kebocoran data transaksi. Pada Loguetown, proxy harus bisa diakses dengan nama [jualbelikapal.yyy.com](jualbelikapal.yyy.com) dengan port yang digunakan adalah 5000.

### Jawaban:

## Soal 9

### Agar transaksi jual beli lebih aman dan pengguna website ada dua orang, proxy dipasang autentikasi user proxy dengan enkripsi MD5 dengan dua username, yaitu `luffybelikapalyyy` dengan password `luffy_yyy` dan `zorobelikapalyyy` dengan password `zoro_yyy`.

### Jawaban:

## Soal 10

### Transaksi jual beli tidak dilakukan setiap hari, oleh karena itu akses internet dibatasi dan hanya dapat diakses setiap hari Senin - Kamis pukul 07.00 - 11.00 dan setiap hari Selasa - Jumat pukul 17.00 - 03.00, keesokan harinya (sampai Sabtu pukul 03.00).

### Jawaban:

## Soal 11

### Agar transaksi bisa lebih fokus berjalan, maka dilakukan redirect website agar mudah mengingat website transaksi jual beli kapal. Setiap mengakses [google.com](https://www.google.com/), akan di-redirect menuju [super.franky.yyy.com](super.franky.yyy.com) dengan website yang sama pada soal shift modul 2. Web server [super.franky.yyy.com](super.franky.yyy.com) berada pada node Skypiea.

### Jawaban:

## Soal 12

### Saatnya berlayar! Luffy dan Zoro akhirnya memutuskan untuk berlayar untuk mencari harta karun di [super.franky.yyy.com](super.franky.yyy.com). Tugas pencarian dibagi menjadi dua misi, Luffy bertugas untuk mendapatkan gambar (.png, .jpg), sedangkan Zoro mendapatkan sisanya. Karena Luffy orangnya sangat teliti untuk mencari harta karun, ketika ia berhasil mendapatkan harta karunnya, kemudian ia mengambil gambar dan melihatinya satu-satu dengan kecepatan 10 Kbps.

### Jawaban:

## Soal 13

### Sedangkan, Zoro sangat bersemangat untuk mencari harta karun sehingga kecepatan kapal Zoro tidak dibatasi ketika sudah mendapatkan harta yang diinginkannya.

### Jawaban:

## Notes
1. **yyy** pada URL adalah kode kelompok.
2. Untuk nomor 9, harus **htpasswd** yang melakukan enkripsi.
3. Bisa melakukan wget [https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip](https://raw.githubusercontent.com/FeinardSlim/Praktikum-Modul-2-Jarkom/main/super.franky.zip)  untuk mendapatkan file untuk [super.franky.yyy.com](super.franky.yyy.com).

## Kendala
