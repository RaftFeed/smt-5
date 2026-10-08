---
title: "Week 7: Securing Information Systems"
tags:
  - security
  - cybersecurity
  - malware
  - ransomware
  - ddos
  - general-controls
  - risk-assessment
  - disaster-recovery
  - encryption
  - pki
  - obsidian-notes
course: KOM1333A - Sistem Informasi
textbook: "Management Information Systems (Laudon & Laudon, 16th Ed), Chapter 8"
date: 2026-10-08
type: study-note
---

# Week 7: Securing Information Systems

> [!abstract] Ringkasan Eksekutif
> Catatan ini mengkaji prinsip-prinsip komprehensif keamanan sistem informasi berbasis **Chapter 8 Laudon & Laudon**:
> 1. **Vektor Kerentanan & Ancaman Keamanan:** Arsitektur kerentanan multi-tier, kerentanan nirkabel (*war driving*), malware (virus, worm, trojan, **ransomware**), dan kejahatan siber (**spoofing, sniffing, DoS/DDoS, phishing**).
> 2. **Ancaman Internal & Kerentanan Perangkat Lunak:** Rekayasa sosial (*social engineering*), *insiders*, bug, dan celah hari-nol (**Zero-Day Flaws**).
> 3. **Nilai Bisnis Keamanan & Regulasi Kepatuhan:** Regulasi SOX, HIPAA, ISO 27001, UU PDP di Indonesia, serta forensik komputer (*Computer Forensics & Chain of Custody*).
> 4. **Kerangka Kerja Kontrol Sistem Informasi:** Kontrol Umum (*General Controls*) vs Kontrol Aplikasi (*Application Controls*), Penilaian Risiko (**Expected Annual Loss - EAL**), serta perencanaan pemulihan bencana (**DRP & BCP**).
> 5. **Teknologi Proteksi Keamanan:** Otentikasi multi-faktor (**MFA**), *Firewall*, IDS/IPS, *Unified Threat Management* (UTM), enkripsi simetris vs asimetris (**Public Key Infrastructure - PKI**), SSL/TLS, sertifikat digital (CA), sistem toleran-kesalahan (*fault-tolerant*), dan keamanan cloud/mobile.

---

## Daftar Isi (Table of Contents)
- [[#1. Kerentanan Sistem dan Ancaman Keamanan Siber]]
  - [[#1.1 Mengapa Sistem Informasi Begitu Rentan? (Arsitektur Vektor Serangan Berlapis)]]
  - [[#1.2 Kerentanan Jaringan Nirkabel (Wireless Vulnerabilities)]]
  - [[#1.3 Klasifikasi Perangkat Lunak Berbahaya (Malware)]]
  - [[#1.4 Peretas (Hackers) dan Kejahatan Komputer Kontemporer]]
  - [[#1.5 Ancaman Internal Organisasi (Insiders dan Rekayasa Sosial)]]
  - [[#1.6 Kerentanan Perangkat Lunak: Bug dan Zero-Day Vulnerabilities]]
- [[#2. Nilai Bisnis Keamanan dan Kontrol]]
  - [[#2.1 Dampak Finansial dan Reputasi dari Pelanggaran Keamanan]]
  - [[#2.2 Kepatuhan Regulasi Legal (SOX, HIPAA, dan UU PDP Indonesia)]]
  - [[#2.3 Bukti Elektronik dan Forensik Komputer (Computer Forensics)]]
- [[#3. Membangun Kerangka Kerja Keamanan dan Kontrol]]
  - [[#3.1 Kontrol Sistem Informasi: General Controls vs Application Controls]]
  - [[#3.2 Penilaian Risiko (Risk Assessment) dan Perhitungan Expected Annual Loss (EAL)]]
  - [[#3.3 Kebijakan Keamanan dan Kebijakan Penggunaan yang Dapat Diterima (AUP)]]
  - [[#3.4 Perencanaan Pemulihan Bencana (DRP) dan Kelangsungan Bisnis (BCP)]]
  - [[#3.5 Peran Audit Sistem Informasi (MIS Audit & Audit Trail)]]
- [[#4. Teknologi dan Alat Pelindung Sumber Daya Informasi]]
  - [[#4.1 Manajemen Identitas dan Otentikasi Pengguna (MFA dan Biometrik)]]
  - [[#4.2 Firewall, Intrusion Detection Systems (IDS/IPS), dan UTM]]
  - [[#4.3 Mengamankan Jaringan Nirkabel (WPA2 dan WPA3)]]
  - [[#4.4 Kriptografi dan Public Key Infrastructure (PKI)]]
  - [[#4.5 Menjamin Ketersediaan Sistem: Fault-Tolerant vs High-Availability Computing]]
  - [[#4.6 Keamanan Komputasi Awan dan Platform Perangkat Seluler (MDM)]]
- [[#5. Ringkasan & Latihan Ujian (Cheatsheet)]]

---

## 1. Kerentanan Sistem dan Ancaman Keamanan Siber

### 1.1 Mengapa Sistem Informasi Begitu Rentan? (Arsitektur Vektor Serangan Berlapis)

![[mis16e-fig-8-1-security-challenges-vulnerabilities.png]]
*Gambar 7.1: Arsitektur Kerentanan Sistem Multi-Tier dan Titik Ancaman Siber*

Ketika data dalam jumlah besar dipindahkan dan disimpan dalam bentuk elektronik, data tersebut terbuka terhadap beragam ancaman pada setiap lapisan arsitektur komputasi:

```
┌────────────────────────────────────────────────────────────────────────┐
│               ARSITEKTUR KERENTANAN SISTEM MULTI-TIER                  │
├─────────────────┬───────────────────┬──────────────────┬───────────────┤
│ LAPISAN KLIEN   │ LAPISAN SALURAN   │ LAPISAN SERVER   │ LAPISAN BASIS │
│  (Client Tier)  │   KOMUNIKASI      │  (Corporate Svr) │ DATA / SISTEM │
├─────────────────┼───────────────────┼──────────────────┼───────────────┤
│ • Malware/Virus │ • Penyadapan data │ • Peretasan OS   │ • Pencurian   │
│ • Spyware       │   (Sniffing)      │ • Serangan DDoS  │   Database    │
│ • Akses tanpa   │ • Spoofing IP     │ • Modifikasi     │ • Korupsi file│
│   izin fisik    │ • Pembajakan DNS  │   perangkat lunak│ • Kegagalan   │
│ • Kehilangan    │ • Penyadapan kabel│ • Vandalisme web │   hardware    │
│   ponsel/laptop │   atau gelombang  │                  │ • Kesalahan   │
│                 │   radio nirkabel  │                  │   operator    │
└─────────────────┴───────────────────┴──────────────────┴───────────────┘
```

### 1.2 Kerentanan Jaringan Nirkabel (Wireless Vulnerabilities)

![[mis16e-fig-8-2-wifi-security-challenges.png]]
*Gambar 7.2: Kerentanan Keamanan Jaringan Nirkabel Wi-Fi dan Penetrasi War Driving*

- Gelombang radio nirkabel menyebar ke segala arah tanpa dibatasi dinding fisik kantor.
- **War Driving:** Praktik penyerang mengendarai mobil di sekitar area gedung perkantoran untuk mendeteksi sinyal Wi-Fi yang tidak terproteksi sandi.
- **Rogue Access Points:** Titik akses nirkabel liar yang dipasang karyawan tanpa izin divisi TI, membuka pintu belakang (*backdoor*) bagi penyusup ke jaringan internal korporat.
- **Kelemahan WEP (*Wired Equivalent Privacy*):** Standar enkripsi nirkabel lama yang sangat cacat kuncinya dan dapat dipecahkan dalam hitungan menit menggunakan software pembobol sandi gratis.

### 1.3 Klasifikasi Perangkat Lunak Berbahaya (Malware)
Malware (*Malicious Software*) adalah program kode jahat yang dirancang untuk menginfeksi, merusak, atau mengeksploitasi sistem komputer:

| Jenis Malware | Mekanisme Infeksi & Cara Kerja | Karakteristik Kunci & Bahaya |
| :--- | :--- | :--- |
| **Virus Komputer** | Menempelkan dirinya pada file program atau dokumen lain; aktif saat program dijalankan manusia | Membutuhkan intervensi pengguna untuk menyebar (misal: membuka file `.exe` atau lampiran email) |
| **Worm (Cacing)** | Program independen yang menyebar otomatis melalui jaringan komputer tanpa bantuan tindakan manusia | Sangat merusak bandwidth jaringan; menggandakan diri secara eksponensial di jutaan komputer |
| **Trojan Horse** | Perangkat lunak berbahaya yang menyamar sebagai aplikasi bermanfaat (misal game atau antivirus bajakan) | Membuka pintu belakang (*backdoor*) rahasia bagi hacker untuk mengendalikan PC korban |
| **Ransomware** | Mengenkripsi seluruh file data pengguna dan mengunci layar komputer, menuntut uang tebusan kripto | Sangat melumpuhkan operasional rumah sakit dan institusi publik (*contoh: WannaCry, Sodinokibi*) |
| **Spyware** | Beroperasi secara sembunyi-sembunyi merekam aktivitas penjelajahan web pengguna | Mengirimkan data profil pengguna ke periklanan luar; memperlambat performa sistem komputer |
| **Keylogger** | Merekam setiap ketukan tombol keyboard yang diketik pengguna secara tersembunyi | Mencuri kata sandi perbankan, nomor kartu kredit, dan rahasia pesan pribadi |

### 1.4 Peretas (Hackers) dan Kejahatan Komputer Kontemporer
> [!info] Terminologi Peretas
> - **Hacker:** Individu yang memiliki keahlian mendalam dalam sistem komputasi (secara historis bertujuan eksplorasi intelektual).
> - **Cracker (Black-hat Hacker):** Peretas yang berniat jahat menerobos sistem komputer secara ilegal untuk mencuri data, memeras, atau melakukan sabotase (*cybervandalism*).

#### Vektor Serangan Kejahatan Siber Utama:
1. **Spoofing & Sniffing:**
   - *Spoofing:* Menyamarkan identitas penyerang (misal memalsukan alamat IP pengirim atau menggunakan alamat web palsu) untuk mengelabui target agar mempercayai sumber pesan.
   - *Sniffing (Packet Sniffer):* Program penguping lalu lintas jaringan yang menyadap paket data yang lewat tanpa enkripsi untuk menangkap sandi dan file rahasia.
2. **Denial-of-Service (DoS) dan Distributed DoS (DDoS):**
   - *Serangan DoS:* Membanjiri server jaringan target dengan ribuan permintaan layanan palsu hingga server kehabisan memori dan mogok (*crash*), menolak akses pengguna sah.
   - *Serangan DDoS:* Menggunakan ribuan hingga jutaan komputer PC dan perangkat IoT yang telah terinfeksi malware sebelumnya (**Zombie / Botnet**) untuk menyerang server target secara serempak dari seluruh dunia.
3. **Pencurian Identitas (Identity Theft) & Phishing:**
   - *Phishing:* Pembuatan email atau situs web palsu yang sangat mirip institusi resmi (seperti bank atau PayPal) untuk menjebak korban menyerahkan kredensial login.
   - *Spear Phishing:* Serangan phishing yang dipersonalisasi secara spesifik terhadap karyawan kunci di perusahaan tertentu.
   - *Evil Twins:* Jaringan Wi-Fi palsu yang dipasang di tempat umum (kafe/bandara) dengan nama meniru Wi-Fi resmi guna menyadap transaksi pengguna yang terkoneksi.
   - *Pharming:* Membajak server DNS lokal sehingga pengguna diarahkan ke situs tiruan palsu meskipun mereka mengetik alamat URL yang benar di browser.
4. **Perang Siber (Cyberwarfare) & Terorisme Siber:**
   - Serangan digital yang didanai negara (*state-sponsored cyberattacks*) untuk memata-matai atau melumpuhkan infrastruktur kritis negara lain (jaringan listrik, sistem perbankan nasional, pemurnian uranium Stuxnet).

### 1.5 Ancaman Internal Organisasi (Insiders dan Rekayasa Sosial)
- Banyak orang berasumsi bahaya terbesar berasal dari hacker luar, padahal **ancaman terbesar kerap kali berasal dari dalam organisasi (*Insiders - Karyawan Perusahaan*)**.
- Karyawan memiliki hak akses legal ke sistem internal, mengenal letak data sensitif, dan sering kali ceroboh terhadap prosedur keamanan fisik.
- **Social Engineering (Rekayasa Sosial):** Teknik manipulasi psikologis penyerang yang berpura-pura menjadi anggota tim IT helpdesk atau auditor resmi guna membujuk karyawan menyerahkan kata sandi akses sistem rahasia mereka.

### 1.6 Kerentanan Perangkat Lunak: Bug dan Zero-Day Vulnerabilities
- Kesalahan kode logika dalam perangkat lunak komersial (*bugs*) menciptakan celah pintu masuk bagi peretas.
- **Buffer Overflow:** Kondisi di mana program menerima input data lebih besar dari kapasitas memori buffer yang dialokasikan, menyebabkan data tumpah dan mengeksekusi kode perintah berbahaya penyerang.
- **Zero-Day Vulnerability:** Celah kerentanan perangkat lunak yang sama sekali belum diketahui oleh pembuat perangkat lunak dan belum tersedia tambalan pembaruannya (*patch*).
- **Patch Management:** Praktik mengunduh dan menguji tambalan pembaruan keamanan secara berkala dari vendor sistem operasi dan aplikasi.

---

## 2. Nilai Bisnis Keamanan dan Kontrol

### 2.1 Dampak Finansial dan Reputasi dari Pelanggaran Keamanan
Pelanggaran keamanan informasi bukan sekadar insiden teknis divisi TI, melainkan krisis eksistensial bagi korporasi:
- **Kerugian Finansial Langsung:** Biaya pemulihan sistem, pembayaran tebusan, dan denda sanksi regulasi.
- **Kerugian Reputasi:** Kehilangan kepercayaan pelanggan dan mitra bisnis secara permanen.
- **Penurunan Nilai Pasar:** Harga saham perusahaan publik anjlok drastis pasca pengumuman insiden peretasan data rahasia.

### 2.2 Kepatuhan Regulasi Legal (SOX, HIPAA, dan UU PDP Indonesia)
Hukum menuntut perusahaan melindungi integritas data bisnis secara ketat:
- **HIPAA (*Health Insurance Portability and Accountability Act*):** Menuntut penyedia layanan medis di AS melindungi kerahasiaan dan privasi rekam medis pasien.
- **Sarbanes-Oxley Act (SOX - Pasal 404):** Mewajibkan eksekutif perusahaan publik menjamin akurasi dan integritas sistem informasi internal yang menyusun laporan keuangan korporat.
- **ISO/IEC 27001:** Standar internasional untuk Sistem Manajemen Keamanan Informasi (SMKI).
- **UU Perlindungan Data Pribadi (UU PDP No. 27/2022 di Indonesia):** Mengharuskan pengendali data di Indonesia membangun infrastruktur perlindungan teknis tersertifikasi dan melaporkan kegagalan perlindungan data (kebocoran) maksimal dalam kurun waktu **3 x 24 jam**.

### 2.3 Bukti Elektronik dan Forensik Komputer (Computer Forensics)
- **Bukti Elektronik (*Electronic Evidence*):** Data komputer yang tersimpan pada media disk, file log server, dan email yang dapat diajukan dalam proses peradilan pengadilan.
  - *Ambient Data:* Informasi tersembunyi yang tertinggal di hard disk (seperti potongan file yang telah dihapus di sektor yang belum tertimpa data baru).
- **Computer Forensics:** Disiplin ilmiah yang mencakup pengumpulan, preservasi, otentikasi, dan analisis bukti digital secara metodologis sehingga sah diterima di pengadilan.
- **Chain of Custody (Rantai Bukti Penjagaan):** Dokumentasi tertulis ketat mengenai siapa yang memegang, menyalin, dan memproses barang bukti digital dari lokasi kejadian hingga ruang sidang.

---

## 3. Membangun Kerangka Kerja Keamanan dan Kontrol

### 3.1 Kontrol Sistem Informasi: General Controls vs Application Controls
Kontrol sistem informasi diklasifikasikan ke dalam dua pilar utama:

```
┌────────────────────────────────────────────────────────────────────────┐
│                   KONTROL SISTEM INFORMASI PERUSAHAAN                  │
├───────────────────────────────────┬────────────────────────────────────┤
│ KONTROL UMUM (General Controls)   │ KONTROL APLIKASI (App Controls)    │
│ [Berlaku di Seluruh Lingkungan TI]│ [Spesifik pada Perangkat Lunak]    │
├───────────────────────────────────┼────────────────────────────────────┤
│ 1. Kontrol Perangkat Lunak (OS)   │ 1. Kontrol Masukan (Input Controls) │
│ 2. Kontrol Perangkat Keras Fisik  │    • Validasi format & batas nilai  │
│ 3. Kontrol Operasi Komputer       │ 2. Kontrol Pemrosesan (Processing) │
│ 4. Kontrol Keamanan Data Sentral  │    • Uji konsistensi & perhitungan │
│ 5. Kontrol Implementasi Sistem    │ 3. Kontrol Keluaran (Output)       │
│ 6. Kontrol Administratif formal   │    • Verifikasi kebenaran laporan   │
└───────────────────────────────────┴────────────────────────────────────┘
```

1. **General Controls (Kontrol Umum):** Mengatur perancangan, keamanan, dan pemeliharaan seluruh program komputer dan fasilitas ruang server di seluruh organisasi.
2. **Application Controls (Kontrol Aplikasi):** Kontrol spesifik yang disematkan ke dalam kode aplikasi terkomputerisasi tertentu (seperti aplikasi penggajian):
   - *Input Controls:* Memastikan data yang diketik pengguna akurat dan valid (misal field usia tidak boleh berupa huruf atau negatif).
   - *Processing Controls:* Memastikan pemrosesan data bebas dari eror matematis selama eksekusi program.
   - *Output Controls:* Memastikan bahwa hasil keluaran pemrosesan data hanya sampai ke tangan pengguna yang berhak.

### 3.2 Penilaian Risiko (Risk Assessment) dan Perhitungan Expected Annual Loss (EAL)
> [!info] Rumus Expected Annual Loss (EAL)
> Penilaian risiko mengevaluasi potensi ancaman dan menghitung estimasi kerugian tahunan yang diharapkan:
> $$	ext{Expected Annual Loss (EAL)} = 	ext{Probabilitas Risiko (\%) } 	imes 	ext{Estimasi Kerugian Per Kejadian (\$)}$$

![[mis16e-fig-8-4-auditor-listing-control-weaknesses.png]]
*Gambar 7.3: Lembar Evaluasi Auditor Mengenai Kelemahan Kontrol dan Estimasi Risiko*

#### Contoh Kasus Analisis Risiko Mercer Paints:
| Tipe Ancaman | Probabilitas Kejadian / Thn | Estimasi Kerugian / Kasus | Expected Annual Loss (EAL) |
| :--- | :---: | :---: | :---: |
| **Korupsi Data Akuntansi** | 5% (0.05) | Rp 500.000.000 | Rp 25.000.000 |
| **Pemadaman Listrik Data Center** | 100% (1.00) | Rp 20.000.000 | Rp 20.000.000 |
| **Serangan Ransomware Server** | 10% (0.10) | Rp 1.000.000.000 | Rp 100.000.000 |

- Manajemen menggunakan angka EAL untuk menentukan apakah biaya pembelian alat keamanan bernilai sepadan (tidak masuk akal menghabiskan dana proteksi Rp 50 juta untuk ancaman yang memiliki potensi kerugian tahunan Rp 20 juta).

### 3.3 Kebijakan Keamanan dan Kebijakan Penggunaan yang Dapat Diterima (AUP)
- **Kebijakan Keamanan (*Security Policy*):** Pernyataan formal yang menetapkan aset informasi apa yang harus dilindungi, siapa yang berhak mengakses, dan prosedur tanggap darurat insiden.
- **Acceptable Use Policy (AUP):** Dokumen aturan yang mendefinisikan batas penggunaan wajar fasilitas komputer dan jaringan internet kantor oleh karyawan (larangan mengunduh materi terlarang, batas streaming video non-pekerjaan).
- **Prinsip Hak Akses Terkecil (*Principle of Least Privilege*):** Karyawan hanya diberikan hak otorisasi minimum yang dibutuhkan untuk menuntaskan tugas pekerjaannya.

### 3.4 Perencanaan Pemulihan Bencana (DRP) dan Kelangsungan Bisnis (BCP)
- **Disaster Recovery Planning (DRP):** Rencana teknis untuk memulihkan layanan komputasi, telekomunikasi, dan basis data perusahaan setelah terjadi bencana fisik (kebakaran, gempa bumi, banjir) atau serangan siber fatal.
  - *Hot Site:* Fasilitas pusat komputer cadangan komersial yang telah dilengkapi server, jaringan, dan replikasi data real-time; siap beroperasi penuh dalam hitungan detik/menit pasca bencana.
  - *Cold Site:* Bangunan kosong cadangan berpendingin ruangan dengan sambungan listrik yang siap dipasangi komputer pengganti dalam hitungan hari.
- **Business Continuity Planning (BCP):** Rencana strategis menyeluruh tentang bagaimana seluruh proses operasional bisnis perusahaan (bukan hanya divisi TI) dapat tetap berjalan selama dan setelah krisis darurat.

### 3.5 Peran Audit Sistem Informasi (MIS Audit & Audit Trail)
- **MIS Audit (Audit Sistem Informasi):** Pemeriksaan berkala yang independen atas kualitas lingkungan keamanan dan efektivitas kontrol umum serta aplikasi organisasi.
- **Audit Trail:** Log catatan rekaman transaksi komputer yang mendokumentasikan setiap aksi penambahan, penghapusan, atau modifikasi data secara mendetail (siapa pengguna yang login, kapan waktu eksekusi, dari terminal IP mana).

---

## 4. Teknologi dan Alat Pelindung Sumber Daya Informasi

### 4.1 Manajemen Identitas dan Otentikasi Pengguna (MFA dan Biometrik)
Otentikasi membuktikan kebenaran identitas pengguna melalui tiga faktor dasar:
1. *Something you know:* Kata sandi (*password*) atau PIN rahasia.
2. *Something you have:* Smart card, token perangkat keras, atau aplikasi authenticator di ponsel.
3. *Something you are:* Ciri fisik biometrik (sidik jari, pemindaian retina mata, pengenalan wajah).
- **Multi-Factor Authentication (MFA / 2FA):** Menggabungkan setidaknya dua faktor berbeda di atas (misal mengetik password DITAMBAH kode OTP dari token ponsel) untuk mengamankan login akun.

### 4.2 Firewall, Intrusion Detection Systems (IDS/IPS), dan UTM
1. **Firewall:** Gerbang keamanan yang menyaring lalu lintas data antara jaringan internal perusahaan dan internet luar:

![[mis16e-fig-8-3-corporate-firewalls.png]]
*Gambar 7.4: Arsitektur Firewall Korporat Berlapis Melindungi Jaringan Internal*
   - *Packet Filtering:* Memeriksa header paket IP (alamat asal, tujuan, nomor port) dan menolak paket mencurigakan.
   - *Stateful Inspection:* Memantau koneksi aktif untuk memastikan paket data merupakan bagian dari percakapan yang sah.
   - *Network Address Translation (NAT):* Menyembunyikan alamat IP internal perangkat LAN dari internet publik.
2. **Intrusion Detection Systems (IDS) & IPS:**
   - *IDS:* Memonitor lalu lintas jaringan secara real-time dan memberikan peringatan bahaya kepada administrator jika terdeteksi pola serangan atau anomali lalu lintas.
   - *IPS (Prevention):* Tidak hanya memberi tahu, tetapi langsung memblokir paket berbahaya dan memutus koneksi peretas secara otomatis.
3. **Unified Threat Management (UTM):** Peranti keamanan jaringan serba-bisa yang menyatukan firewall, VPN, filter spam email, antivirus jaringan, dan IDS/IPS ke dalam satu perangkat fisik terpadu.

### 4.3 Mengamankan Jaringan Nirkabel (WPA2 dan WPA3)
- Menghindari protokol usang WEP.
- Menerapkan **WPA2** atau generasi terbaru **WPA3 (*Wi-Fi Protected Access 3*)** yang menggunakan algoritma enkripsi **AES (*Advanced Encryption Standard*) 128-bit/192-bit** yang kuat.

### 4.4 Kriptografi dan Public Key Infrastructure (PKI)

![[mis16e-fig-8-5-public-key-encryption-pki.png]]
*Gambar 7.5: Mekanisme Enkripsi Kunci Publik (Asimetris) dengan Sepasang Kunci*

```
┌────────────────────────────────────────────────────────────────────────┐
│                   DUA PARADIGMA UTAMA ENKRIPSI KRIPTOGRAFI             │
├───────────────────────────────────┬────────────────────────────────────┤
│ ENKRIPSI KUNCI SIMETRIS           │ ENKRIPSI KUNCI ASIMETRIS / PUBLIK  │
│ (Symmetric Key Encryption)        │ (Public Key / Asymmetric)          │
├───────────────────────────────────┼────────────────────────────────────┤
│ • Pengirim dan penerima memakai   │ • Menggunakan sepasang kunci:      │
│   SATU KUNCI RAHASIA YANG SAMA    │   Kunci Publik & Kunci Privat      │
│ • Sangat cepat dan hemat daya     │ • Kunci publik disebar bebas;      │
│ • Masalah: Bagaimana membagikan   │   hanya Kunci privat yang dapat    │
│   kunci rahasia tanpa disadap?    │   mendekripsi pesan                │
│ • Contoh: AES, DES, Triple-DES    │ • Contoh: RSA, Elliptic Curve (ECC)│
└───────────────────────────────────┴────────────────────────────────────┘
```

#### Public Key Infrastructure (PKI), SSL/TLS, dan Otoritas Sertifikat (CA):
- **SSL (*Secure Sockets Layer*) & TLS (*Transport Layer Security*):** Protokol enkripsi yang mengamankan lalu lintas data antara browser web pengguna dan server web (ditandai dengan protokol `https://` dan ikon gembok pada browser).
- **Sertifikat Digital (*Digital Certificates*):** Berkas data identitas digital yang dikeluarkan oleh lembaga pihak ketiga tepercaya

![[mis16e-fig-8-6-digital-certificates-ca.png]]
*Gambar 7.6: Arsitektur Otoritas Sertifikasi (Certificate Authority - CA) Menerbitkan Sertifikat Digital* (**Certificate Authority - CA**, seperti DigiCert atau Let's Encrypt) untuk memverifikasi keaslian identitas server web dan mencegah serangan *Man-in-the-Middle*.

```
Pengirim (Alice)                   Saluran Publik                 Penerima (Bob)
┌──────────────┐                                                 ┌──────────────┐
│ Pesan Asli:  │                                                 │ Pesan Asli:  │
│ "Transfer    │                                                 │ "Transfer    │
│  Rp 10 Juta" │                                                 │  Rp 10 Juta" │
└──────┬───────┘                                                 └──────▲───────┘
       │ Dienkripsi memakai                                             │ Didekripsi memakai
       ▼ Kunci Publik Bob                                               │ Kunci Privat Bob
┌──────────────┐                Lalu Lintas Internet             ┌──────┴───────┐
│ Teks Teracak │ ───────────► [ 7a#x9!kLm0@wZ ] ────────────►   │ Teks Teracak │
│  (Ciphertext)│              (Aman dari Penyadapan)             │  (Ciphertext)│
└──────────────┘                                                 └──────────────┘
```

### 4.5 Menjamin Ketersediaan Sistem: Fault-Tolerant vs High-Availability Computing
- **Fault-Tolerant Computer Systems:** Sistem komputer yang memiliki komponen perangkat keras (CPU, RAM, power supply) dan perangkat lunak cadangan yang berjalan paralel secara terus-menerus. Jika salah satu komponen rusak, komponen cadangan langsung mengambil alih tanpa ada jeda downtime satu milidetik pun (wajib bagi bursa saham dan kendali penerbangan).
- **High-Availability Computing:** Sistem yang dirancang untuk pulih secara cepat (*failover*) dalam beberapa detik atau menit jika terjadi gangguan sistem.

### 4.6 Keamanan Komputasi Awan dan Platform Perangkat Seluler (MDM)
- **Model Tanggung Jawab Bersama Cloud (*Shared Responsibility Model*):** Penyedia cloud (AWS/Azure) bertanggung jawab atas keamanan infrastruktur fisik data center; pelanggan bertanggung jawab atas enkripsi data, manajemen identitas akun pengguna, dan konfigurasi firewall aplikasi mereka sendiri.
- **Mobile Device Management (MDM):** Perangkat lunak korporat yang memungkinkan departemen TI mengontrol, mengenkripsi, dan menghapus data bisnis dari jarak jauh (*remote wipe*) pada ponsel pintar karyawan yang hilang atau dicuri.

---

## 5. Ringkasan & Latihan Ujian (Cheatsheet)

> [!tip] Rangkuman Cepat untuk Ujian (Exam Quick Sheet)
> 1. **Malware:**
>    - Virus: Menempel pada file lain, butuh tindakan manusia.
>    - Worm: Program independen, menyebar otomatis lewat jaringan.
>    - Trojan: Menyamar sebagai aplikasi berguna, membuka backdoor.
>    - Ransomware: Mengenkripsi file dan meminta tebusan uang.
> 2. **DDoS Attack:** Membanjiri server dengan jutaan permintaan palsu menggunakan jaringan komputer zombie (*botnet*).
> 3. **Kontrol Umum vs Kontrol Aplikasi:** Kontrol Umum mengatur seluruh lingkungan TI korporat (hardware, OS, prosedur); Kontrol Aplikasi mengatur validasi masukan, pemrosesan, dan keluaran pada aplikasi software spesifik.
> 4. **Expected Annual Loss (EAL):** $	ext{EAL} = 	ext{Probabilitas Risiko} 	imes 	ext{Estimasi Kerugian}$.
> 5. **DRP vs BCP:** DRP fokus pada pemulihan infrastruktur komputasi dan database (Hot Site vs Cold Site); BCP fokus pada kelangsungan operasi seluruh bisnis perusahaan.
> 6. **Enkripsi Kunci Publik (PKI):** Menggunakan kunci publik untuk mengenkripsi dan kunci privat untuk mendekripsi pesan; keaslian diverifikasi oleh Certificate Authority (CA).
> 7. **Fault-Tolerant vs High-Availability:** Fault-Tolerant memiliki redundansi 100% tanpa jeda downtime; High-Availability pulih dalam hitungan detik/menit.
