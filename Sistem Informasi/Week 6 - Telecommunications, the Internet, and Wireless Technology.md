---
title: "Week 6: Telecommunications, the Internet, and Wireless Technology"
tags:
  - telecommunications
  - networking
  - packet-switching
  - tcp-ip
  - ipv4-ipv6
  - dns
  - voip-vpn
  - wireless
  - rfid
  - wsn
  - 5g
  - obsidian-notes
course: KOM1333A - Sistem Informasi
textbook: "Management Information Systems (Laudon & Laudon, 16th Ed), Chapter 7"
date: 2026-10-08
type: study-note
---

# Week 6: Telecommunications, the Internet, and Wireless Technology

> [!abstract] Ringkasan Eksekutif
> Catatan ini mengupas tuntas arsitektur telekomunikasi, internet global, dan teknologi nirkabel berbasis **Chapter 7 Laudon & Laudon**:
> 1. **Tren Telekomunikasi Bisnis:** Konvergensi jaringan data dan suara serta adopsi komunikasi nirkabel pita lebar (*broadband*).
> 2. **Tiga Teknologi Kunci Jaringan Digital:** *Client/Server Computing*, **Packet Switching**, dan protokol **TCP/IP** (4 Lapisan Model Referensi).
> 3. **Klasifikasi Jaringan & Media Transmisi:** Sinyal analog vs digital, jaringan LAN/CAN/MAN/WAN, kabel tembaga (*twisted pair*), serat optik (*fiber-optic*), dan konsep *Bandwidth*.
> 4. **Internet Global & Pengalamatan:** Struktur ISP, perbandingan **IPv4 vs IPv6**, hirarki **Domain Name System (DNS)**, serta layanan **VoIP** dan **VPN (*Tunneling*)**.
> 5. **Evolusi Web:** Web 2.0 (interaktif dan sosial) menuju Web 3.0 (*Semantic Web* dan AI-driven search).
> 6. **Revolusi Nirkabel (*The Wireless Revolution*):** Jaringan seluler (3G, 4G LTE, 5G), standar nirkabel (**Bluetooth IEEE 802.15**, **Wi-Fi IEEE 802.11**, **WiMAX IEEE 802.16**), sistem pelacakan rantai pasok **RFID**, serta **Wireless Sensor Networks (WSN)**.

---

## Daftar Isi (Table of Contents)
- [[#1. Telekomunikasi dan Jaringan dalam Dunia Bisnis Modern]]
  - [[#1.1 Tren Utama Jaringan: Konvergensi dan Mobilitas Pita Lebar]]
  - [[#1.2 Komponen Dasar Jaringan Komputer]]
  - [[#1.3 Jaringan Perusahaan Skala Besar dan Software-Defined Networking (SDN)]]
- [[#2. Tiga Teknologi Kunci Jaringan Digital]]
  - [[#2.1 Komputasi Klien/Server (Client/Server Computing)]]
  - [[#2.2 Pertukaran Paket (Packet Switching)]]
  - [[#2.3 Protokol TCP/IP dan Empat Lapisan Model Referensi]]
- [[#3. Jaringan Komunikasi dan Media Transmisi]]
  - [[#3.1 Sinyal Analog vs Sinyal Digital dan Fungsi Modem]]
  - [[#3.2 Tipe Jaringan Berdasarkan Jangkauan Geografis: LAN, CAN, MAN, dan WAN]]
  - [[#3.3 Media Transmisi Fisik: Twisted Pair, Coaxial, Fiber-Optic, dan Nirkabel]]
  - [[#3.4 Kecepatan Transmisi dan Bandwidth Jaringan]]
- [[#4. Internet Global: Arsitektur, Layanan, dan Tata Kelola]]
  - [[#4.1 Struktur Hierarki Penyedia Layanan Internet (ISP Tier 1, 2, 3)]]
  - [[#4.2 Pengalamatan Internet: IPv4 vs IPv6]]
  - [[#4.3 Hierarki Domain Name System (DNS)]]
  - [[#4.4 Layanan Internet Utama: Web, VoIP, dan Virtual Private Network (VPN)]]
  - [[#4.5 Evolusi Web: Dari Web 2.0 ke Web 3.0 (Semantic Web)]]
  - [[#4.6 Mesin Pencari, SEO, dan Pemasaran Mesin Pencari (SEM)]]
- [[#5. Revolusi Nirkabel (The Wireless Revolution)]]
  - [[#5.1 Generasi Sistem Seluler: Dari 3G, 4G LTE, hingga 5G]]
  - [[#5.2 Jaringan Komputer Nirkabel: Bluetooth, Wi-Fi, dan WiMAX]]
  - [[#5.3 Radio Frequency Identification (RFID) dan Pelacakan Rantai Pasok]]
  - [[#5.4 Jaringan Sensor Nirkabel (Wireless Sensor Networks - WSN)]]
- [[#6. Ringkasan & Latihan Ujian (Cheatsheet)]]

---

## 1. Telekomunikasi dan Jaringan dalam Dunia Bisnis Modern

### 1.1 Tren Utama Jaringan: Konvergensi dan Mobilitas Pita Lebar
Dalam dekade terakhir, lanskap telekomunikasi korporat telah mengalami revolusi mendasar:
- **Konvergensi Jaringan (*Network Convergence*):** Di masa lalu, perusahaan memelihara dua jaringan terpisah: jaringan telepon kabel suara dan jaringan komputer data. Saat ini, kedua jaringan telah melebur menjadi satu jaringan digital tunggal yang berbasis pada protokol dan standar internet.
- **Ledakan Nirkabel Pita Lebar (*Broadband Wireless Explosion*):** Akses data internet berkecepatan tinggi didominasi oleh perangkat nirkabel bergerak (ponsel pintar, tablet, dan sensor IoT).

### 1.2 Komponen Dasar Jaringan Komputer
Sebuah jaringan komputer sederhana terdiri dari komponen-komponen perangkat keras dan lunak berikut:

![[mis16e-fig-7-1-components-simple-network.png]]
*Gambar 6.1: Komponen-Komponen Dasar Jaringan Komputer Klien/Server*

```
┌──────────────┐                                                 ┌──────────────┐
│  KOMPUTER    │                                                 │  KOMPUTER    │
│   KLIEN      │◄──────────┐                         ┌──────────►│   SERVER     │
│ (Desktop PC) │           │                         │           │ (File/App)   │
└──────────────┘           ▼                         ▼           └──────────────┘
                    ┌─────────────┐           ┌─────────────┐
                    │ SWITCH / HUB│◄─────────►│   ROUTER    │◄───► [ JARINGAN  ]
                    │  (Koneksi   │           │ (Penghubung │      [ INTERNET  ]
                    │   Lokal)    │           │  ke Luar)   │      [  GLOBAL   ]
                    └─────────────┘           └─────────────┘
```

1. **Komputer Klien dan Server:** Klien meminta data; server memproses dan melayani permintaan.
2. **Network Interface Card (NIC):** Kartu antarmuka perangkat keras yang terpasang pada komputer untuk menghubungkannya ke media transmisi kabel atau antena nirkabel.
3. **Media Koneksi (*Connection Medium*):** Kabel fisik (tembaga, serat optik) atau gelombang radio udara.
4. **Network Operating System (NOS):** Perangkat lunak sistem yang mengarahkan dan mengelola komunikasi pada jaringan serta mengalokasikan sumber daya (misal: Windows Server, Linux Red Hat).
5. **Hubs, Switches, dan Routers:**
   - *Hub:* Perangkat sederhana yang meneruskan paket data ke seluruh port yang terhubung tanpa pemfilteran (*broadcast*).
   - *Switch:* Perangkat cerdas yang memfilter dan meneruskan paket data hanya ke simpul tujuan spesifik berdasarkan alamat MAC perangkat di dalam jaringan lokal (LAN).
   - *Router:* Perangkat komunikasi jaringan yang menghubungkan dua atau lebih jaringan berbeda (misalnya menghubungkan LAN perusahaan ke internet publik) dan merutekan paket data melalui rute terbaik.

### 1.3 Jaringan Perusahaan Skala Besar dan Software-Defined Networking (SDN)
- Korporasi multinasional modern mengoperasikan infrastruktur jaringan kompleks yang terdiri dari ratusan LAN lokal, jaringan telekomunikasi nirkabel seluler, koneksi intranet antarcabang, dan sistem konferensi video.
- **Software-Defined Networking (SDN):** Pendekatan arsitektur jaringan baru di mana fungsi kendali (*control plane*) dipisahkan dari perangkat keras fisik router/switch dan dipusatkan pada satu program perangkat lunak pengendali berbasis software. Hal ini memungkinkan administrator mengatur alur lalu lintas data jaringan secara fleksibel dan otomatis dari jarak jauh.

---

## 2. Tiga Teknologi Kunci Jaringan Digital

Jaringan digital modern bersandar pada tiga pilar teknologi fundamental:

```
                          ┌───────────────────────────┐
                          │    TIGA TEKNOLOGI KUNCI   │
                          │      JARINGAN DIGITAL     │
                          └─────────────┬─────────────┘
               ┌────────────────────────┼────────────────────────┐
               ▼                        ▼                        ▼
      1. CLIENT/SERVER          2. PACKET SWITCHING         3. TCP/IP
        COMPUTING
   Komputasi terdistribusi     Pemecahan pesan digital    Standar bahasa &
   antara klien desktop &      menjadi paket mandiri      protokol komunikasi
   server tersentralisasi      yang dirutekan dinamis     universal 4 lapis
```

### 2.1 Komputasi Klien/Server (Client/Server Computing)
- Model komputasi terdistribusi di mana sebagian besar daya pemrosesan berada pada komputer klien kecil (PC atau perangkat seluler), yang terhubung dengan komputer server yang menyediakan layanan file, komputasi, dan database.
- Menggantikan model komputasi tersentralisasi mainframe lama.

### 2.2 Pertukaran Paket (Packet Switching)

![[mis16e-fig-7-3-packet-switching-data.png]]
*Gambar 6.2: Mekanisme Packet Switching dan Transmisi Paket Mandiri*

> [!info] Packet Switching vs Circuit Switching
> - **Circuit Switching (Jaringan Telepon Kuno):** Membangun satu sirkuit fisik khusus yang terus terbuka tanpa putus antara pengirim dan penerima selama panggilan berlangsung. Jika tidak ada suara yang lewat, kapasitas sirkuit terbuang percuma.
> - **Packet Switching (Internet Modern):** Pesan digital dipotong-potong menjadi paket-paket kecil independen. Setiap paket diberi label alamat asal, alamat tujuan, nomor urut, dan informasi koreksi kesalahan, lalu dilewatkan melalui berbagai saluran komunikasi alternatif yang sedang kosong sebelum dirangkai kembali secara utuh di tempat tujuan.

```
Pesan Asli: [ D A T A - U S E R ]
                 │
                 ▼ Dipotong menjadi paket-paket kecil
 Paket 1: [Hdr | D A | Chk] ───► Lewat Jalur Rute A ───┐
 Paket 2: [Hdr | T A | Chk] ───► Lewat Jalur Rute B ───┼──► Dirangkai kembali:
 Paket 3: [Hdr | - U | Chk] ───► Lewat Jalur Rute C ───┤   [ D A T A - U S E R ]
 Paket 4: [Hdr | S E | Chk] ───► Lewat Jalur Rute A ───┘
```
- **Keunggulan:** Utilisasi saluran transmisi menjadi sangat efisien; jika salah satu kabel terputus di tengah jalan, paket dapat memutar mencari jalur alternatif lain secara otomatis.

### 2.3 Protokol TCP/IP dan Empat Lapisan Model Referensi

![[mis16e-fig-7-4-tcp-ip-reference-model.png]]
*Gambar 6.3: Empat Lapisan Model Referensi Protokol TCP/IP*

Protokol adalah serangkaian aturan formal yang mengatur transmisi informasi antara dua titik dalam jaringan. **TCP/IP (*Transmission Control Protocol / Internet Protocol*)** adalah bahasa komunikasi universal yang diadopsi seluruh perangkat di dunia untuk berkomunikasi di internet.

```
┌────────────────────────────────────────────────────────────────────────┐
│               EMPAT LAPISAN MODEL PROTOKOL TCP/IP                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. APPLICATION LAYER (Lapisan Aplikasi)                                │
│    Menyediakan antarmuka bagi aplikasi pengguna untuk mengakses layanan │
│    jaringan (HTTP, HTTPS, FTP, SMTP, DNS, SSH, Telnet).                │
├────────────────────────────────────────────────────────────────────────┤
│ 2. TRANSPORT LAYER (Lapisan Transport)                                 │
│    Bertanggung jawab atas pengiriman data ujung-ke-ujung (end-to-end). │
│    • TCP: Berorientasi koneksi, menjamin urutan paket dan keandalan data│
│    • UDP: Tanpa koneksi, cepat, toleran terhadap kehilangan paket (VoIP)│
├────────────────────────────────────────────────────────────────────────┤
│ 3. INTERNET LAYER (Lapisan Internet)                                   │
│    Menangani pengalamatan logis dan perutean paket melintasi jaringan   │
│    berbeda menggunakan protokol IP (IPv4 & IPv6).                      │
├────────────────────────────────────────────────────────────────────────┤
│ 4. NETWORK INTERFACE LAYER (Lapisan Antarmuka Jaringan)                │
│    Menangani pengiriman sinyal fisik pada media transmisi (Ethernet,   │
│    Wi-Fi, serat optik, kabel tembaga).                                 │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Jaringan Komunikasi dan Media Transmisi

### 3.1 Sinyal Analog vs Sinyal Digital dan Fungsi Modem
- **Sinyal Analog:** Gelombang kontinu (*continuous waveform*) yang bergerak melalui media transmisi dan digunakan untuk komunikasi suara tradisional.
- **Sinyal Digital:** Gelombang diskret berbentuk pulsa biner (dua status: 0 dan 1, ada tegangan atau tidak ada tegangan) yang diproses oleh komputer.
- **Modem (*Modulator-Demodulator*):** Perangkat keras

![[mis16e-fig-7-5-functions-of-modem.png]]
*Gambar 6.4: Fungsi Modem Mengubah Sinyal Digital ke Analog dan Sebaliknya* yang mengubah sinyal digital komputer menjadi sinyal analog agar dapat merambat melalui saluran kabel telepon/kabel koaksial (*modulasi*), dan mengubah kembali sinyal analog menjadi sinyal digital di ujung penerima (*demodulasi*).

### 3.2 Tipe Jaringan Berdasarkan Jangkauan Geografis: LAN, CAN, MAN, dan WAN

| Tipe Jaringan | Kepanjangan | Skala Geografis | Contoh Penggunaan |
| :--- | :--- | :--- | :--- |
| **LAN** | *Local Area Network* | Hingga 500 meter | Menghubungkan komputer di satu ruangan kantor atau gedung sekolah |
| **CAN** | *Campus Area Network* | Hingga 1 kilometer | Menghubungkan antargedung di dalam satu kompleks kampus universitas |
| **MAN** | *Metropolitan Area Network* | Lingkup satu kota | Jaringan televisi kabel kota atau jaringan perbankan lokal metropolitan |
| **WAN** | *Wide Area Network* | Lintas negara, benua, global | Jaringan telekomunikasi transnasional; **Internet** adalah WAN terbesar di dunia |

### 3.3 Media Transmisi Fisik: Twisted Pair, Coaxial, Fiber-Optic, dan Nirkabel
1. **Twisted Pair Cable (Kabel Pasangan Terpilin):**
   - Terdiri dari untaian kawat tembaga berpasangan yang dipilin untuk mengurangi interferensi elektromagnetik (*contoh: Cat 5e, Cat 6*).
   - Biaya murah, kecepatan 10 Mbps hingga 10 Gbps, namun jarak transmisi terbatas (maksimal 100 meter sebelum butuh repeater).
2. **Coaxial Cable (Kabel Koaksial):**
   - Kawat tembaga berisolasi tebal, memiliki pelindung interferensi lebih baik dibanding twisted pair; biasa digunakan untuk TV kabel dan internet kabel berkecepatan tinggi.
3. **Fiber-Optic Cable (Kabel Serat Optik):**
   - Untaian serat kaca bening yang mentransmisikan data dalam bentuk pulsa cahaya laser.
   - Kecepatan luar biasa (ratusan Gbps), kapasitas data masif, tahan terhadap interferensi listrik, sangat tipis dan ringan; ideal untuk tulang punggung (*backbone*) telekomunikasi lintas negara dan dasar laut.
4. **Media Transmisi Nirkabel (*Wireless*):**
   - Gelombang mikro (*Microwave*), gelombang radio seluler, dan satelit komunikasi di orbit bumi untuk lokasi geografis yang sulit dijangkau kabel fisik.

### 3.4 Kecepatan Transmisi dan Bandwidth Jaringan
- Kecepatan transmisi diukur dalam **bits per second (bps)**:
  - 1 Kbps = 1.000 bps; 1 Mbps = 1.000.000 bps; 1 Gbps = 1.000.000.000 bps.
- **Hertz (Hz):** Satuan frekuensi gelombang per detik.
- **Bandwidth:** Rentang frekuensi (*range of frequencies*) yang dapat ditampung oleh saluran telekomunikasi tertentu. Semakin besar rentang frekuensi suatu kanal, semakin tinggi kapasitas pita data yang dapat dilewatkan.

---

## 4. Internet Global: Arsitektur, Layanan, dan Tata Kelola

### 4.1 Struktur Hierarki Penyedia Layanan Internet (ISP Tier 1, 2, 3)

![[mis16e-fig-7-7-internet-network-architecture.png]]
*Gambar 6.5: Arsitektur Jaringan Internet Global dan Tulang Punggung ISP*

Internet global tidak dimiliki oleh satu entitas tunggal, melainkan dioperasikan melalui hierarki komersial penyedia jasa internet (*Internet Service Providers - ISPs*):
- **Tier 1 ISPs (Backbone Providers):** Operator raksasa telekomunikasi global (seperti AT&T, Verizon, Lumen) yang memiliki jaringan kabel serat optik bawah laut lintas benua. Mereka saling terhubung di titik pertukaran bebas biaya (*Peering Points*).
- **Tier 2 ISPs (Regional Providers):** Operator tingkat regional atau nasional yang membeli akses dari operator Tier 1 dan menjualnya kembali ke operator lokal.
- **Tier 3 ISPs (Local ISPs):** Penyedia internet ritel lokal yang menjual koneksi langsung ke konsumen perumahan dan perkantoran (*contoh: Indihome, Biznet, FirstMedia*).
- **Internet Exchange Points (IXPs):** Titik simpul fisik pertukaran lalu lintas data internet antaroperator secara langsung untuk memangkas latensi.

### 4.2 Pengalamatan Internet: IPv4 vs IPv6
Setiap perangkat yang terhubung ke internet wajib memiliki alamat unik yang disebut **IP Address (*Internet Protocol Address*)**:
- **IPv4 (32-bit):**
  - Terdiri dari angka 32-bit biner yang direpresentasikan dalam format 4 angka desimal terpisah titik (misal: `192.168.1.1` atau `207.46.250.119`).
  - Menyediakan sekitar 4,3 miliar alamat unik. Seiring ledakan smartphone dan perangkat IoT, alamat IPv4 dunia telah habis teralokasi.
- **IPv6 (128-bit):**
  - Standar pengalamatan baru menggunakan bilangan 128-bit yang ditulis dalam format heksadesimal terpisah tanda titik dua (misal: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`).
  - Mampu menghasilkan lebih dari $3.4 	imes 10^{38}$ alamat unik (cukup untuk memberikan alamat IP pada setiap butir pasir di bumi).

### 4.3 Hierarki Domain Name System (DNS)
Karena manusia sulit mengingat rangkaian angka IP address numerik, internet menggunakan **Domain Name System (DNS)**

![[mis16e-fig-7-6-domain-name-system-dns.png]]
*Gambar 6.6: Struktur Hierarki Domain Name System (Root, TLD, Second-Level, Host)* untuk menerjemahkan nama domain ramah manusia menjadi alamat IP numerik mesin:

```
                            ┌────────────────────────┐
                            │    ROOT DOMAIN (".")   │
                            └───────────┬────────────┘
                                        │
             ┌──────────────────────────┴──────────────────────────┐
             ▼                                                     ▼
    TOP-LEVEL DOMAINS (TLD)                               TOP-LEVEL DOMAINS (TLD)
  [ .com, .edu, .gov, .org ]                                [ .id, .sg, .uk, .jp ]
             │                                                     │
             ▼                                                     ▼
   SECOND-LEVEL DOMAINS                                  SECOND-LEVEL DOMAINS
      [ google.com ]                                         [ ac.id ]
             │                                                     │
             ▼                                                     ▼
     THIRD-LEVEL / HOST                                    THIRD-LEVEL / HOST
   [ maps.google.com ]                                     [ ipb.ac.id ]
```

1. **Root Domain:** Diwakili oleh titik `.` di puncak hierarki server DNS dunia.
2. **Top-Level Domain (TLD):** Domain tingkat teratas (gTLD seperti `.com`, `.org`, `.edu` dan ccTLD seperti `.id` untuk Indonesia).
3. **Second-Level Domain:** Nama unik yang didaftarkan oleh organisasi atau individu (misal `google` atau `kemdikbud`).
4. **Third-Level / Subdomain:** Menentukan server spesifik di dalam organisasi (misal `mail` dalam `mail.google.com` atau `kuliah` dalam `kuliah.ipb.ac.id`).

### 4.4 Layanan Internet Utama: Web, VoIP, dan Virtual Private Network (VPN)
1. **World Wide Web (WWW):** Sistem dokumen hiperteks yang diakses melalui protokol HTTP/HTTPS.
2. **Voice over IP (VoIP):**
   - Teknologi yang mentransmisikan informasi suara manusia

![[mis16e-fig-7-8-how-voip-works.png]]
*Gambar 6.7: Arsitektur Pemrosesan Panggilan Suara Voice over IP (VoIP)* dalam bentuk paket data digital melalui internet (*packet-switched network*) alih-alih menggunakan saluran sirkuit telepon konvensional (PSTN).
   - Menghilangkan biaya pulsa sambungan langsung jarak jauh (SLI) dan memungkinkan fleksibilitas komunikasi korporat (contoh: panggilan Skype, WhatsApp Voice, Zoom).
3. **Virtual Private Network (VPN):**
   - Jaringan privat aman dan terenkripsi

![[mis16e-fig-7-9-virtual-private-network-vpn.png]]
*Gambar 6.8: Mekanisme Tunneling pada Virtual Private Network (VPN)* yang dibangun di atas infrastruktur publik internet.
   - Menggunakan mekanisme **Tunneling (Penerowongan):** Paket data privat dibungkus (*enkapsulasi*) di dalam paket IP publik dan dienkripsi sehingga tidak dapat dibaca oleh penyadap jaringan di internet publik.

```
┌──────────────┐         ┌───────────────────────────────┐         ┌──────────────┐
│  KARYAWAN DI │         │       INTERNET PUBLIK         │         │ JARINGAN LAN │
│  RUMAH (W條H)│────────►│   ┌───────────────────────┐   ├────────►│  KANTOR PUSAT│
│              │◄────────┤   │ TEROWONGAN ENKRIPSI   │   │◄────────┤  PERUSAHAAN  │
└──────────────┘         │   │ (VPN TUNNEL TERLINDUNG│   │         └──────────────┘
                         │   └───────────────────────┘   │
                         └───────────────────────────────┘
```

### 4.5 Evolusi Web: Dari Web 2.0 ke Web 3.0 (Semantic Web)
- **Web 1.0:** Web statis di mana pengguna hanya membaca halaman teks dan gambar (*read-only*).
- **Web 2.0:** Web partisipatif dan kolaboratif generasi kedua yang memungkinkan pengguna membuat, berbagi, dan berinteraksi secara dinamis (*read-write*):
  - *Fitur Kunci:* Blog, RSS (*Rich Site Summary*), Wikis, Jejaring Sosial (Instagram, LinkedIn), dan Mashups aplikasi web.
- **Web 3.0 (Semantic Web):**
  - Generasi masa depan di mana semua data dan informasi di web terstruktur secara semantik sehingga mesin komputer cerdas dapat memahami arti makna konten (*meaning of data*), bukan sekadar mencocokkan kata kunci teks.
  - Mengintegrasikan kecerdasan buatan, pemrosesan bahasa alami (NLP), dan Web of Things.

### 4.6 Mesin Pencari, SEO, dan Pemasaran Mesin Pencari (SEM)
- **Mesin Pencari (*Search Engines*):** Algoritma perangkat lunak yang merayapi (*crawling*) web untuk mengindeks halaman dan menyajikan hasil relevan seketika (Google Hummingbird, PageRank).
- **Search Engine Optimization (SEO):** Proses menyempurnakan struktur dan konten situs web agar mendapatkan peringkat setinggi mungkin dalam hasil pencarian organik mesin pencari tanpa membayar iklan.
- **Search Engine Marketing (SEM):** Membeli penempatan tautan bersponsor berbayar (*sponsored links*) pada halaman hasil pencarian (misal Google Ads berbasis pay-per-click).

---

## 5. Revolusi Nirkabel (The Wireless Revolution)

### 5.1 Generasi Sistem Seluler: Dari 3G, 4G LTE, hingga 5G
- **3G Networks:** Kecepatan 144 Kbps hingga 2 Mbps; dirancang untuk penjelajahan web dasar dan email seluler.
- **4G LTE (*Long-Term Evolution*):** Kecepatan 10 Mbps hingga 100 Mbps; memungkinkan streaming video definisi tinggi (HD), game online interaktif, dan konferensi video lancar.
- **5G Networks:**
  - Kecepatan transmisi puncak mencapai gigabit per detik (Gbps).
  - **Latensi Ultra-Rendah (*Ultra-low latency* < 1 milidetik):** Esensial untuk navigasi mobil otonom tanpa pengemudi (*autonomous vehicles*), operasi bedah jarak jauh real-time, dan otomasi pabrik robotik.
  - Mendukung konektivitas miliaran sensor perangkat IoT dalam kepadatan geografis tinggi (*Massive IoT*).

### 5.2 Jaringan Komputer Nirkabel: Bluetooth, Wi-Fi, dan WiMAX

![[mis16e-fig-7-12-bluetooth-pan-network.png]]
*Gambar 6.9: Jaringan Nirkabel Bluetooth Personal Area Network (PAN)*

![[mis16e-fig-7-13-wifi-wireless-lan-80211.png]]
*Gambar 6.10: Arsitektur Jaringan Wi-Fi 802.11 Wireless Local Area Network (WLAN)*

```
┌────────────────────────────────────────────────────────────────────────┐
│               STANDAR JARINGAN KOMPUTER NIRKABEL                       │
├─────────────────────────┬──────────────────────────┬───────────────────┤
│ BLUETOOTH (IEEE 802.15) │ WI-FI (IEEE 802.11)      │ WiMAX (IEEE 802.16│
├─────────────────────────┼──────────────────────────┼───────────────────┤
│ • Wireless Personal     │ • Wireless Local Area    │ • Worldwide       │
│   Area Network (WPAN)   │   Network (WLAN)         │   Interoperability│
│ • Jangkauan: ~10 meter  │ • Jangkauan: ~30-50 meter│   for Microwave   │
│ • Daya sangat rendah    │ • Kecepatan: Hingga Gbps │   Access          │
│ • Digunakan untuk periferal│ • Hotspots di kafe,     │ • Jangkauan luas: │
│   headphone, smartwatch,│   kampus, dan kantor     │   Hingga 50 km    │
│   dan transfer file dekat│ • Menggunakan Access Point│ • Akses broadband │
│                         │   (AP) terhubung ke kabel│   nirkabel kota   │
└─────────────────────────┴──────────────────────────┴───────────────────┘
```

### 5.3 Radio Frequency Identification (RFID) dan Pelacakan Rantai Pasok

![[mis16e-fig-7-14-how-rfid-works.png]]
*Gambar 6.11: Mekanisme Kerja RFID Tag dan Reader dalam Rantai Pasokan*

> [!info] Definisi RFID
> **Radio Frequency Identification (RFID)** adalah teknologi yang menggunakan gelombang radio mikro untuk mentransmisikan data identitas unik yang tersimpan pada tag mikrochip kecil ke alat pembaca (*reader*) secara nirkabel tanpa memerlukan kontak fisik langsung atau garis pandang (*line of sight*).

#### Komponen Sistem RFID:
1. **Tag RFID:** Mikrochip yang terhubung ke antena kecil.
   - *Tag Pasif:* Tidak memiliki baterai internal; memperoleh energi daya dari gelombang radio yang dipancarkan oleh alat pembaca (*reader*). Murah dan jangkauan pendek (~beberapa meter).
   - *Tag Aktif:* Memiliki baterai internal kecil sendiri; mampu memancarkan sinyal radio lebih kuat dengan jangkauan ratusan meter, namun berbiaya lebih mahal.
2. **RFID Reader (Pembaca):** Pemancar radio yang memancarkan sinyal untuk mengaktifkan tag dan membaca data identitas muatannya.
- **Aplikasi Terkenal: Mandat Rantai Pasok Walmart**
  - Walmart mewajibkan seluruh pemasok utamanya menyematkan tag RFID pada setiap kotak palet barang.
  - Ketika truk kontainer masuk ke dermaga gudang, pemindai pintu otomatis membaca ribuan kotak sekaligus tanpa perlu karyawan membuka segel kardus untuk memindai barcode satu per satu.

### 5.4 Jaringan Sensor Nirkabel (Wireless Sensor Networks - WSN)

![[mis16e-fig-7-16-wireless-sensor-network-wsn.png]]
*Gambar 6.12: Arsitektur Jaringan Sensor Nirkabel (WSN) Topologi Mesh*

- **Definisi:** Jaringan yang terdiri dari ratusan hingga ribuan perangkat sensor nirkabel kecil mandiri yang disebut **Nodes (*Motes*)** yang disebar di lingkungan fisik.
- **Karakteristik:**
  - Setiap mote dilengkapi sensor fisik (mengukur suhu, kelembaban, tekanan, getaran, atau kadar kimia), prosesor pemroses data mikro, pemancar radio, dan baterai kecil.
  - Menggunakan topologi **Mesh Networking**: Mote saling meneruskan paket data dari satu node ke node lain secara estafet (*hop-by-hop*) hingga mencapai simpul komputer pusat (*gateway*).
- **Aplikasi Lapangan:**
  - *Pertanian Cerdas (Smart Agriculture):* Mengukur kelembaban tanah dan mendeteksi hama perkebunan secara real-time.
  - *Pemantauan Lingkungan:* Mendeteksi dini kebakaran hutan lindung dan radiasi nuklir.
  - *Infrastruktur Sipil:* Memantau retakan mikro pada jembatan gantung jalan tol dan bendungan air.

---

## 6. Ringkasan & Latihan Ujian (Cheatsheet)

> [!tip] Rangkuman Cepat untuk Ujian (Exam Quick Sheet)
> 1. **Packet Switching:** Memecah pesan menjadi paket-paket kecil independen, dirutekan melalui rute bebas yang berbeda, dan dirangkai ulang di tujuan. Jauh lebih efisien dibanding circuit switching.
> 2. **Empat Lapisan TCP/IP:** Application -> Transport (TCP/UDP) -> Internet (IP) -> Network Interface (Ethernet/Wi-Fi).
> 3. **DNS:** Menerjemahkan nama domain ramah manusia (`ipb.ac.id`) menjadi alamat IP numerik mesin (`119.252.161.x`).
> 4. **IPv4 vs IPv6:** IPv4 berukuran 32-bit (4,3 miliar alamat, sudah habis); IPv6 berukuran 128-bit ($3.4 	imes 10^{38}$ alamat heksadesimal).
> 5. **VoIP vs VPN:**
>    - VoIP: Pengiriman suara via paket IP internet menggantikan saluran telepon PSTN.
>    - VPN: Terowongan privat terenkripsi (*tunneling*) di atas jaringan publik internet.
> 6. **Standar Nirkabel:**
>    - Bluetooth (802.15): Jarak dekat 10m, daya rendah, WPAN.
>    - Wi-Fi (802.11): Jarak sedang 30-50m, kecepatan tinggi, WLAN.
>    - WiMAX (802.16): Jangkauan luas hingga 50km, broadband nirkabel.
> 7. **RFID vs Barcode:** RFID tidak membutuhkan garis pandang (*line of sight*) dan dapat membaca ribuan item secara simultan melalui gelombang radio.
> 8. **WSN (Wireless Sensor Networks):** Ribuan node mote sensor nirkabel berdaya baterai yang membentuk jaringan mesh untuk memantau fenomena fisik lingkungan.
