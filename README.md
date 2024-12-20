# Laporan FP Jarkom 2024

| Nama              | NRP                                                  | Tugas |
|----------------------|--------------------------------------------------------------| ----- |
| Daffa Rajendra Priyatama | 5027231009 | NAT, GRE Tunnel | 
| Naufal Syafi' Hakim        | 5027231022 | Routing, VLSM Tree |
| RM. Novian Malcolm Bayuputra | 5027231035 | Topologi, DHCP |
| Dimas Andhika Diputra              | 5027231074 | Subnetting, Config Static IP |


Laporan final project ini menjelaskan implementasi dari jaringan komputer yang dirancang menggunakan metode subnetting, routing, konfigurasi NAT, GRE Tunnel, dan lainnya untuk merepresentasikan komunikasi data yang optimal dalam lingkungan jaringan kompleks. Setiap bagian di topologi memiliki peran spesifik, seperti distribusi IP, pengelolaan koneksi antar-segmen jaringan, dan pengamanan data melalui NAT.

## Topologi
### Buatlah topologi jaringan yang mencakup gedung utama dengan lima lantai dan cabang yang terhubung melalui VPN di Cisco Packet Tracer. Gunakan perangkat jaringan yang sesuai (misalnya router, switch, dan perangkat lain) untuk membanguntopologi tersebut. (20 poin)

Topologi ini menggambarkan hubungan antar perangkat jaringan (router, switch, dan komputer) yang dihubungkan melalui berbagai interface.

![Screenshot 2024-12-19 224949](https://github.com/user-attachments/assets/fce1df3c-fba3-4331-8321-314c383d3dd6)

## Subnetting
### Lakukan subnetting pada jaringan dan tentukan Subnet Mask, Network ID, Broadcast Address, serta Range IP untuk setiap subnet. Buatlah tabel alokasi subnet untuk setiap departemen berdasarkan jumlah perangkat yang dibutuhkan di masing- masing subnet. Gunakan alamat IP Private sesuai dengan standar yang ada (misalnya 10.0.0.0/8, 172.16.0.0/12, atau 192.168.0.0/16). (15 poin)

Subnetting adalah metode untuk membagi jaringan menjadi beberapa sub-jaringan yang lebih kecil, membantu mengoptimalkan alokasi alamat IP dan meningkatkan efisiensi routing. Spreadsheet di atas menjelaskan pembagian IP berdasarkan VLSM (Variable Length Subnet Mask), memungkinkan setiap subnet memiliki ukuran yang disesuaikan dengan kebutuhan.

https://docs.google.com/spreadsheets/d/1erulimzxzkYmx6gwraKSa-8v8KGgdl_1LfiLL3vuD1k/edit?gid=282142602#gid=282142602
![Screenshot 2024-12-19 221755](https://github.com/user-attachments/assets/3e45754e-43a4-41af-b898-815d31cec1bc)

## Configuration
### Konfigurasi Static IP pada setiap perangkat jaringan di semua departemen, kecuali lantai 2, sesuai dengan pembagian subnetting yang telah dilakukan. Pastikan alamat IP yang diberikan sesuai dengan alokasi subnet yang sudah dibuat. (10 poin)
### Konfigurasi Static Routing agar perangkat di setiap departemen dapat berkomunikasi lintas subnet. Jangan menggunakan default route untuk menghubungkan subnet - subnet yang ada. Pastikan setiap router pada masing-masing lantai terhubung dengan benar dan dapat melakukan routing antar subnet. (15 poin)

Berikut adalah konfigurasi static IP dab static Routing dari tiap node pada topologi
### Router 0
```
interface FastEthernet0/0
ip address dhcp

interface FastEthernet0/1
ip address 10.75.2.21 255.255.255.252

interface FastEthernet1/0
ip address 10.75.2.25 255.255.255.252

ip classless
ip route 10.75.0.128 255.255.255.128 10.75.2.22
ip route 10.75.2.0 255.255.255.252 10.75.2.22
ip route 10.75.0.0 255.255.255.128 10.75.2.22
ip route 10.75.2.4 255.255.255.252 10.75.2.22
ip route 10.75.1.64 255.255.255.192 10.75.2.22
ip route 10.75.2.8 255.255.255.252 10.75.2.22
ip route 10.75.1.128 255.255.255.192 10.75.2.22
ip route 10.75.2.12 255.255.255.252 10.75.2.22
ip route 10.75.1.0 255.255.255.192 10.75.2.22
ip route 10.75.1.192 255.255.255.192 10.75.2.26
```

### Lantai 1
```
ip name-server 8.8.8.8

spanning-tree mode pvst

interface Tunnel0
ip address 10.75.2.17 255.255.255.252
mtu 1476
tunnel source FastEthernet0/1
tunnel destination 10.75.2.26

interface FastEthernet0/0
ip address 10.75.2.1 255.255.255.252
ip nat inside

interface FastEthernet0/1
ip address 10.75.2.22 255.255.255.252
ip nat outside

interface FastEthernet1/0
ip address 10.75.0.129 255.255.255.128
ip nat inside

router rip

ip nat inside source list 1 interface FastEthernet0/1 overload
ip classless
ip route 10.75.0.0 255.255.255.128 10.75.2.2
ip route 10.75.2.4 255.255.255.252 10.75.2.2
ip route 10.75.1.64 255.255.255.192 10.75.2.2
ip route 10.75.2.8 255.255.255.252 10.75.2.2
ip route 10.75.1.128 255.255.255.192 10.75.2.2
ip route 10.75.2.12 255.255.255.252 10.75.2.2
ip route 10.75.1.0 255.255.255.192 10.75.2.2
ip route 8.8.8.0 255.255.255.0 10.75.2.21
ip route 10.75.2.24 255.255.255.252 10.75.2.21
ip route 10.75.1.192 255.255.255.192 10.75.2.18

access-list 1 permit 10.75.0.0 0.0.255.255
access-list 1 permit any
```

### Lantai 2
```
ip dhcp excluded-address 10.75.0.1 10.75.0.10

ip dhcp pool pc
network 10.75.0.0 255.255.255.128
default-router 10.75.0.1
dns-server 10.75.0.1

ip name-server 8.8.8.8

interface FastEthernet0/0
ip address 10.75.2.2 255.255.255.252

interface FastEthernet0/1
ip address 10.75.2.5 255.255.255.252

interface FastEthernet1/0
ip address 10.75.0.1 255.255.255.128

ip classless
ip route 10.75.0.128 255.255.255.128 10.75.2.1
ip route 10.75.1.64 255.255.255.192 10.75.2.6
ip route 10.75.2.8 255.255.255.252 10.75.2.6
ip route 10.75.1.128 255.255.255.192 10.75.2.6
ip route 10.75.2.12 255.255.255.252 10.75.2.6
ip route 10.75.1.0 255.255.255.192 10.75.2.6
ip route 8.8.8.0 255.255.255.0 10.75.2.1
ip route 10.75.2.20 255.255.255.252 10.75.2.1
ip route 10.75.2.16 255.255.255.252 10.75.2.1
ip route 10.75.1.192 255.255.255.192 10.75.2.1
```

### Lantai 3
```
ip name-server 8.8.8.8

interface FastEthernet0/0
ip address 10.75.2.6 255.255.255.252

interface FastEthernet0/1
ip address 10.75.2.9 255.255.255.252

interface FastEthernet1/0
ip address 10.75.1.65 255.255.255.192

ip classless
ip route 10.75.0.128 255.255.255.128 10.75.2.5
ip route 10.75.2.0 255.255.255.252 10.75.2.5
ip route 10.75.0.0 255.255.255.128 10.75.2.5
ip route 10.75.1.128 255.255.255.192 10.75.2.10
ip route 10.75.2.12 255.255.255.252 10.75.2.10
ip route 10.75.1.0 255.255.255.192 10.75.2.10
ip route 8.8.8.0 255.255.255.0 10.75.2.5
ip route 10.75.2.20 255.255.255.252 10.75.2.5
ip route 10.75.2.16 255.255.255.252 10.75.2.5
ip route 10.75.1.192 255.255.255.192 10.75.2.5
```

### Lantai 4
```
ip name-server 8.8.8.8

interface FastEthernet0/0
ip address 10.75.2.10 255.255.255.252

interface FastEthernet0/1
ip address 10.75.2.13 255.255.255.252

interface FastEthernet1/0
ip address 10.75.1.129 255.255.255.192

ip classless
ip route 10.75.0.128 255.255.255.128 10.75.2.9
ip route 10.75.2.0 255.255.255.252 10.75.2.9
ip route 10.75.0.0 255.255.255.128 10.75.2.9
ip route 10.75.2.4 255.255.255.252 10.75.2.9
ip route 10.75.1.64 255.255.255.192 10.75.2.9
ip route 10.75.1.0 255.255.255.192 10.75.2.14
ip route 8.8.8.0 255.255.255.0 10.75.2.9
ip route 10.75.2.20 255.255.255.252 10.75.2.9
ip route 10.75.2.16 255.255.255.252 10.75.2.9
ip route 10.75.1.192 255.255.255.192 10.75.2.9
```

### Lantai 5
```
ip name-server 8.8.8.8

interface FastEthernet0/0
ip address 10.75.2.14 255.255.255.252

interface FastEthernet0/1
ip address 10.75.1.1 255.255.255.192

ip classless
ip route 10.75.0.128 255.255.255.128 10.75.2.13
ip route 10.75.2.0 255.255.255.252 10.75.2.13
ip route 10.75.0.0 255.255.255.128 10.75.2.13
ip route 10.75.2.4 255.255.255.252 10.75.2.13
ip route 10.75.1.64 255.255.255.192 10.75.2.13
ip route 10.75.2.8 255.255.255.252 10.75.2.13
ip route 10.75.1.128 255.255.255.192 10.75.2.13
ip route 10.75.2.20 255.255.255.252 10.75.2.13
ip route 8.8.8.0 255.255.255.0 10.75.2.13
ip route 10.75.2.16 255.255.255.252 10.75.2.13
ip route 10.75.1.192 255.255.255.192 10.75.2.13
```

### Gedung Lain
```
ip name-server 8.8.8.8

interface Tunnel0
ip address 10.75.2.18 255.255.255.252
mtu 1476
tunnel source FastEthernet0/0
tunnel destination 10.75.2.22

interface FastEthernet0/0
ip address 10.75.2.26 255.255.255.252

interface FastEthernet0/1
ip address 10.75.1.193 255.255.255.192

ip classless
ip route 8.8.8.0 255.255.255.0 10.75.2.25
ip route 10.75.2.0 255.255.255.252 10.75.2.17
ip route 10.75.0.0 255.255.255.128 10.75.2.17
ip route 10.75.2.4 255.255.255.252 10.75.2.17
ip route 10.75.1.64 255.255.255.192 10.75.2.17
ip route 10.75.2.8 255.255.255.252 10.75.2.17
ip route 10.75.1.128 255.255.255.192 10.75.2.17
ip route 10.75.2.12 255.255.255.252 10.75.2.17
ip route 10.75.1.0 255.255.255.192 10.75.2.17
ip route 10.75.0.128 255.255.255.128 10.75.2.17
ip route 10.75.2.20 255.255.255.252 10.75.2.25
```

## Konfigurasi DHCP
### Departemen R&D, Pemasaran, dan Penjualan akan menggunakan DHCP untuk mengalokasikan alamat IP secara dinamis kepada perangkat mereka. Konfigurasikan
DHCP server pada router dan pastikan perangkat di tiga departemen tersebut mendapatkan IP otomatis sesuai dengan rentang yang telah ditentukan. (15 poin)

```
Router(config)# ip dhcp pool pc
Router(dhcp-config)# network 10.75.0.0 255.255.255.128
Router(dhcp-config)# default-router 10.75.0.1
Router(dhcp-config)# dns-server 10.75.0.1
Router(config)# ip dhcp excluded-address 10.75.0.0 10.75.0.10
```

## NAT
### Perusahaan membutuhkan akses ke internet untuk semua perangkat yang terhubung. Konfigurasikan NAT Overload (PAT) pada router utama untuk memungkinkan perangkat dalam jaringan lokal mengakses internet, misalnya dengan melakukan ping ke 8.8.8.8. (15 poin)

Set IP Inside(fa0/0, fa1/0) and Outside(fa0/1)
```
Router(config)# interface <interface>
Router(dhcp-config)# ip nat <outside/inside>
```
Set IP Overload
```
ip nat inside source list 1 interface FastEthernet0/1 overload
```

## GRE Tunnel
### Hubungkan Gedung Utama ke Cabang menggunakan GRE Tunnel. Konfigurasikan Generic Routing Encapsulation (GRE Tunnel) antara router di gedung utama dan router di cabang untuk membangun koneksi virtual yang aman. Pastikan kedua router dapat saling berkomunikasi melalui GRE Tunnel dan verifikasi konektivitasnya dengan melakukan ping antar router atau perangkat di masing-masing jaringan. (10 poin) 

### Lantai 1
```
Router(config)# interface Tunnel0
Router(config-if)# ip address 10.75.2.17 255.255.255.252
Router(config-if)# tunnel source FastEthernet0/1
Router(config-if)# tunnel destination 10.75.2.26
```
### Gedung Lain
```
Router(config)# interface Tunnel0
Router(config-if)# ip address 10.75.2.18 255.255.255.252
Router(config-if)# tunnel source FastEthernet0/0
Router(config-if)# tunnel destination 10.75.2.22
```

## Untuk video dari demo praktikum kami dapat dilihat pada tautan berikut ini 
https://drive.google.com/file/d/17de7FnlFtpl0BRO-nYZ2fpyYoF5tOzTj/view?usp=sharing
