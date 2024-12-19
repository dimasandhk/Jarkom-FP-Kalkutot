# Laporan FP Jarkom 2024

| Nama              | NRP                                                  | Tugas |
|----------------------|--------------------------------------------------------------| ----- |
| Daffa Rajendra Priyatama | 5027231009 | NAT, GRE Tunnel | 
| Naufal Syafi' Hakim        | 5027231022 | Routing, DHCP |
| RM. Novian Malcolm Bayuputra | 5027231035 | Topologi, DHCP |
| Dimas Andhika Diputra              | 5027231074 | Subnetting, Config Static IP |

## Topologi
![Screenshot 2024-12-19 224949](https://github.com/user-attachments/assets/fce1df3c-fba3-4331-8321-314c383d3dd6)

## Subnetting
https://docs.google.com/spreadsheets/d/1erulimzxzkYmx6gwraKSa-8v8KGgdl_1LfiLL3vuD1k/edit?gid=282142602#gid=282142602
![Screenshot 2024-12-19 221755](https://github.com/user-attachments/assets/3e45754e-43a4-41af-b898-815d31cec1bc)

## Configuration

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
```
Router(config)# ip dhcp pool pc
Router(dhcp-config)# network 10.75.0.0 255.255.255.128
Router(dhcp-config)# default-router 10.75.0.1
Router(dhcp-config)# dns-server 10.75.0.1
Router(config)# ip dhcp excluded-address 10.75.0.0 10.75.0.10
```

## NAT
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
