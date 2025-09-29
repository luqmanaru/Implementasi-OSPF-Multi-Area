# Implementasi-OSPF-Multi-Area

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Contributors](https://img.shields.io/badge/contributors-1-green.svg)
![Forks](https://img.shields.io/badge/forks-0-lightgrey.svg)
![Stars](https://img.shields.io/badge/stars-0-yellow.svg)

Proyek ini mengimplementasikan routing protocol OSPF (Open Shortest Path First) multi-area pada jaringan dengan menggunakan router Cisco. OSPF digunakan untuk mencapai konektivitas end-to-end antar jaringan dengan pembagian area untuk efisiensi routing.

## 🌐 Topologi Jaringan

```
+------+     +------+     +------+     +------+     +------+
|      |     |      |     |      |     |      |     |      |
|  R0  |-----|  R1  |-----|  R2  |-----|  R3  |-----|  R4  |
|      |     |      |     |      |     |      |     |      |
+------+     +------+     +------+     +------+     +------+
  |            |            |            |            |
  |            |            |            |            |
+------+     +------+     +------+     +------+     +------+
| LAN1 |     | LAN3 |     |      |     |      |     | LAN4 |
+------+     +------+     +------+     +------+     +------+
                            |            |
                         +------+     +------+
                         |      |     |      |
                         | LAN2 |     |      |
                         |      |     |      |
                         +------+     +------+
```
<img width="827" height="354" alt="image" src="https://github.com/user-attachments/assets/d6748ef7-35ea-409b-aa58-fe5e5da19389" />

### Tabel Konfigurasi Router

| Router | Interface | IP Address | Subnet Mask | OSPF Area |
|--------|-----------|------------|-------------|-----------|
| R0 | GigabitEthernet0/0 | 10.10.10.2 | 255.255.255.252 | 0 |
| R0 | GigabitEthernet0/1 | 192.168.1.1 | 255.255.255.0 | 0 |
| R0 | GigabitEthernet0/2 | 192.168.2.1 | 255.255.255.0 | 0 |
| R1 | GigabitEthernet0/0 | 10.10.10.1 | 255.255.255.252 | 0 |
| R1 | GigabitEthernet0/1 | 20.20.20.2 | 255.255.255.252 | 0 |
| R1 | GigabitEthernet0/2 | 172.20.1.1 | 255.255.255.0 | 0 |
| R2 | GigabitEthernet0/0 | 20.20.20.1 | 255.255.255.252 | 0 |
| R2 | GigabitEthernet0/1 | 30.30.30.1 | 255.255.255.252 | 0 & 10 |
| R3 | GigabitEthernet0/0 | 30.30.30.2 | 255.255.255.252 | 0 |
| R3 | GigabitEthernet0/1 | 40.40.40.1 | 255.255.255.252 | 10 |
| R4 | GigabitEthernet0/0 | 40.40.40.2 | 255.255.255.252 | 10 |
| R4 | GigabitEthernet0/1 | 192.168.3.1 | 255.255.255.0 | 10 |

### Tabel Jaringan LAN

| LAN | Network | Subnet Mask | Gateway |
|-----|---------|-------------|---------|
| LAN1 | 192.168.1.0 | 255.255.255.0 | 192.168.1.1 |
| LAN2 | 192.168.2.0 | 255.255.255.0 | 192.168.2.1 |
| LAN3 | 172.20.1.0 | 255.255.255.0 | 172.20.1.1 |
| LAN4 | 192.168.3.0 | 255.255.255.0 | 192.168.3.1 |

## 📝 Penjelasan Konfigurasi

### OSPF Multi-Area

Proyek ini mengimplementasikan OSPF dengan dua area:
- **Area 0**: Backbone area yang menghubungkan R0, R1, R2, dan R3
- **Area 10**: Area yang menghubungkan R2, R3, dan R4

Router R2 dan R3 berfungsi sebagai ABR (Area Border Router) yang menghubungkan antara area 0 dan area 10.

### Konfigurasi Router

Setiap router dikonfigurasi dengan:
- IP address pada setiap interface
- OSPF process ID 1
- Network statement yang sesuai dengan area masing-masing
- Interface yang diaktifkan dengan perintah `no shutdown`

### Cara Kerja OSPF

1. Router membentuk adjacency dengan router tetangga di area yang sama
2. Setiap router mengirimkan LSU (Link State Update) ke semua router dalam area yang sama
3. Setiap router membangun LSDB (Link State Database) yang identik untuk area yang sama
4. Setiap router menjalankan algoritma SPF untuk menghitung jalur terpendek ke setiap network
5. ABR (Area Border Router) merangkum informasi routing dari satu area ke area lain

---
**luqmanaru**
