---
title: "Week 3: Isu Etika, Privasi, dan Sosial dalam Sistem Informasi"
tags:
  - ethics
  - privacy
  - gdpr
  - uu-ite
  - uu-pdp
  - nora
  - intellectual-property
  - cybersecurity-law
  - obsidian-notes
course: KOM1333A - Sistem Informasi
textbook: "Management Information Systems (Laudon & Laudon, 16th Ed), Chapter 4; Slide Kuliah Dosen Bab 3"
date: 2026-10-08
type: study-note
---

# Week 3: Isu Etika, Privasi, dan Sosial dalam Sistem Informasi

> [!abstract] Ringkasan Eksekutif
> Catatan ini mengintegrasikan materi **Chapter 4 Laudon & Laudon** serta materi kuliah kontekstual **Slide Bab 3 Dosen (KOM1333A)** ke dalam kajian mendalam mengenai dilema moral dalam masyarakat informasi digital:
> 1. **Model Dinamika Etika, Sosial, dan Politik:** Analogi riak di atas kolam (*Ripples in the Pond*).
> 2. **Lima Dimensi Moral Era Informasi:** Hak Informasi, Hak Kepemilikan, Akuntabilitas, Kualitas Sistem, dan Kualitas Hidup.
> 3. **Tren Teknologi Pemicu Isu Etika:** Kemajuan analitik data, *Profiling*, dan **NORA (*Nonobvious Relationship Awareness*)**.
> 4. **Prinsip dan Metode Pengambilan Keputusan Etis:** 5 Langkah Analisis Etika dan 6 Prinsip Kandidat Etika.
> 5. **Privasi & Tantangan Internet:** Cookies, Web Beacons, Spyware, GDPR (*Opt-in vs Opt-out*).
> 6. **Studi Kasus Kontekstual & Hukum Indonesia:** Kasus kebocoran data BPJS (Bjorka), 300 juta data Dukcapil, aplikasi MuslimPro, model bisnis Google, regulasi **UU ITE (UU 11/2008 & UU 19/2016)**, serta **UU Perlindungan Data Pribadi (UU PDP No. 27/2022)**.

---

## Daftar Isi (Table of Contents)
- [[#1. Model Berpikir Isu Etika, Sosial, dan Politik]]
  - [[#1.1 Analogi Riak di Atas Kolam (Ripples in the Pond Model)]]
  - [[#1.2 Lima Dimensi Moral dalam Era Informasi]]
- [[#2. Tren Teknologi Utama Pemicu Isu Etika]]
  - [[#2.1 Kekuatan Komputasi dan Penurunan Biaya Penyimpanan]]
  - [[#2.2 Analitik Data Tingkat Lanjut: Profiling & NORA]]
  - [[#2.3 Jaringan Global dan Perangkat Seluler]]
- [[#3. Konsep Dasar Etika dan Kerangka Analisis]]
  - [[#3.1 Konsep Inti: Tanggung Jawab, Akuntabilitas, Liabilitas, dan Due Process]]
  - [[#3.2 Kerangka 5 Langkah Analisis Etika (Five-Step Ethical Analysis)]]
  - [[#3.3 Enam Prinsip Kandidat Etika (Candidate Ethical Principles)]]
- [[#4. Hak Informasi: Privasi dan Kebebasan di Era Digital]]
  - [[#4.1 Definisi Privasi dan Fair Information Practices (FIP)]]
  - [[#4.2 Regulasi Privasi Global: European Union GDPR (Opt-In vs Opt-Out)]]
  - [[#4.3 Vektor Pelanggaran Privasi di Internet: Cookies, Web Beacons, dan Spyware]]
- [[#5. Studi Kasus Nyata dan Regulasi Siber di Indonesia]]
  - [[#5.1 Studi Kasus 1: Mengapa Google Gratis? (Model Bisnis Berbasis Data)]]
  - [[#5.2 Studi Kasus 2: Pelacakan Lokasi Seluler dan Aplikasi MuslimPro]]
  - [[#5.3 Studi Kasus 3: Kebocoran Data Skala Masif di Indonesia (Bjorka BPJS & Dukcapil)]]
  - [[#5.4 Kerangka Hukum Positif Indonesia: UU ITE dan UU Perlindungan Data Pribadi (UU PDP)]]
- [[#6. Hak Kepemilikan: Kekayaan Intelektual di Era Digital]]
  - [[#6.1 Rahasia Dagang (Trade Secrets)]]
  - [[#6.2 Hak Cipta (Copyrights)]]
  - [[#6.3 Paten (Patents)]]
  - [[#6.4 Tantangan Era Internet terhadap HAKI]]
- [[#7. Kualitas Sistem, Liabilitas, dan Kualitas Hidup]]
  - [[#7.1 Masalah Liabilitas dan Kualitas Perangkat Lunak]]
  - [[#7.2 Dampak Sosial: Jam Kerja, Otomasi Lapangan Kerja, dan Dampak Kesehatan (RSI, CVS)]]
- [[#8. Ringkasan & Latihan Ujian (Cheatsheet)]]

---

## 1. Model Berpikir Isu Etika, Sosial, dan Politik

### 1.1 Analogi Riak di Atas Kolam (Ripples in the Pond Model)
Pengenalan teknologi informasi baru diibaratkan seperti melempar batu ke tengah kolam yang tenang:

![[mis16e-fig-4-1-ethical-social-political-model.jpg]]
*Gambar 3.1: Model Hubungan Dinamika Isu Etika, Sosial, dan Politik dalam Masyarakat Informasi*

```
                            ┌────────────────────────┐
                            │    INSTITUSI POLITIK   │
                            │  (Hukum Baru, UU PDP,  │
                            │    Regulasi Negara)    │
                            └───────────┬────────────┘
                                        │
                            ┌───────────┴────────────┐
                            │      MASYARAKAT        │
                            │  (Norma Sosial Baru,   │
                            │    Ekspektasi Publik)  │
                            └───────────┬────────────┘
                                        │
                            ┌───────────┴────────────┐
                            │       INDIVIDU         │
                            │ (Dilema Etika Personal,│
                            │   Keputusan Moral)     │
                            └───────────┬────────────┘
                                        │
                                        ▼
                            ┌────────────────────────┐
                            │      BATU TEKNOLOGI    │
                            │   INFORMASI DILUNCURKAN│
                            └────────────────────────┘
```

1. **Guncangan Pertama (Individu / Etika):** Individu menghadapi dilema baru yang belum memiliki pedoman moral yang mapan (misal: apakah boleh memantau chat karyawan?).
2. **Guncangan Kedua (Masyarakat / Sosial):** Norma-norma sosial lama terguncang; masyarakat menuntut kejelasan batasan moral bersama.
3. **Guncangan Ketiga (Politik / Regulasi):** Lembaga legislatif merespons dengan menyusun regulasi hukum formal untuk mengembalikan stabilitas tatanan masyarakat.

### 1.2 Lima Dimensi Moral dalam Era Informasi
Isu etika, sosial, dan politik dalam masyarakat informasi diorganisasikan ke dalam lima dimensi moral:

![[si-bab3-p07-5-dimensi-moral.png]]
*Gambar 3.2: 5 Dimensi Moral dalam Masyarakat Informasi (Slide Dosen KOM1333A)*
1. **Hak dan Kewajiban Informasi (*Information Rights and Obligations*):** Hak apa yang dimiliki individu atas data pribadi mereka? Kewajiban apa yang diemban oleh organisasi pengumpul data untuk melindunginya?
2. **Hak dan Kewajiban Kepemilikan (*Property Rights and Obligations*):** Bagaimana hak kekayaan intelektual (HAKI) dilindungi ketika informasi digital dapat disalin dan disebarkan secara instan tanpa biaya tambahan?
3. **Akuntabilitas dan Kendali (*Accountability and Control*):** Siapa yang bertanggung jawab secara hukum dan moral ketika terjadi kerugian fatal akibat kesalahan sistem atau perangkat lunak?
4. **Kualitas Sistem (*System Quality*):** Standar kualitas data dan keandalan sistem seperti apa yang dapat ditoleransi publik untuk menjaga keselamatan masyarakat?
5. **Kualitas Hidup (*Quality of Life*):** Nilai-nilai budaya dan kemanusiaan apa yang harus dipertahankan dari serbuan teknologi (keseimbangan kerja-keluarga, privasi, lapangan kerja)?

---

## 2. Tren Teknologi Utama Pemicu Isu Etika

Perkembangan teknologi memicu dilema etika karena mengubah kapasitas manusia dalam bertindak secara radikal:

| Tren Teknologi | Dampak Bisnis / Kemampuan | Dilema Etika yang Muncul |
| :--- | :--- | :--- |
| **Penggandaan Daya Komputasi (Moore's Law)** | Semakin banyak organisasi mengandalkan komputasi otomatis untuk operasional inti | Ketergantungan berlebihan pada sistem rentan; kerentanan terhadap malafungsi sistemik |
| **Penurunan Drastis Biaya Penyimpanan Data** | Perusahaan mampu menyimpan data transaksi personal secara permanen seumur hidup | Pelanggaran privasi massal; tidak ada lagi data yang benar-benar terlupakan |
| **Kemajuan Analitik Data (Big Data & AI)** | Analisis profil perilaku konsumen (*profiling*) dan pemodelan prediktif | Pengawasan terselubung; diskriminasi harga; manipulasi psikologis |
| **Kemajuan Jaringan & Internet** | Data dapat dipindahkan dan diakses dari mana saja tanpa batasan geografis | Data pribadi mudah dibobol atau ditransfer lintas yurisdiksi hukum tanpa izin |
| **Pertumbuhan Perangkat Seluler** | Smartphone melacak lokasi fisik pengguna secara presisi 24 jam sehari | Pengawasan lokasi (*location tracking*) tanpa sepengetahuan pemilik perangkat |

### 2.2 Analitik Data Tingkat Lanjut: Profiling & NORA
- **Profiling:** Penggabungan data dari berbagai sumber berbeda (riwayat belanja kartu kredit, riwayat browsing web, data demografi) untuk menyusun berkas informasi terperinci (*dossier*) mengenai profil perilaku seorang individu.
- **NORA (*Nonobvious Relationship Awareness*):**
  - Teknologi analitik intelijen canggih

![[mis16e-fig-4-2-nora-diagram.jpg]]
*Gambar 3.3: Diagram Arsitektur Nonobvious Relationship Awareness (NORA)* yang mampu menambang data dari sumber publik dan rahasia (catatan kriminal, panggilan telepon, catatan perhotelan, transaksi keuangan) guna menemukan koneksi tersembunyi antarmanusia.
  - *Contoh Penggunaan:* Mendeteksi agen teroris, pencucian uang, atau pelamar kerja di kasino yang memiliki hubungan tersembunyi dengan sindikat kriminal perjudian.

```
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ REKAM PERBANKAN │       │ REKAM KRIMINAL  │       │ DATA RESERVASI  │
└────────┬────────┘       └────────┬────────┘       └────────┬────────┘
         │                         │                         │
         └─────────────────┐       │       ┌─────────────────┘
                           ▼       ▼       ▼
                   ┌───────────────────────────────┐
                   │          MESIN NORA           │
                   │ (Analisis Hubungan Tersembunyi│
                   │       secara Real-Time)       │
                   └───────────────┬───────────────┘
                                   │
                                   ▼
                   ┌───────────────────────────────┐
                   │ PERINGATAN KORELASI TINGGI:   │
                   │ "Kandidat X bertempat tinggal │
                   │  bersama buronan Y"           │
                   └───────────────────────────────┘
```

---

## 3. Konsep Dasar Etika dan Kerangka Analisis

### 3.1 Konsep Inti: Tanggung Jawab, Akuntabilitas, Liabilitas, dan Due Process
Pemikiran etis didasarkan pada empat konsep fundamental:
1. **Responsibility (Tanggung Jawab Pribadi):** Penerimaan atas potensi biaya, tugas, dan kewajiban atas keputusan yang dibuat secara sadar oleh individu.
2. **Accountability (Akuntabilitas):** Fitur dari institusi sosial yang menentukan siapa yang melakukan tindakan dan siapa yang dapat dimintai pertanggungjawaban di hadapan publik.
3. **Liability (Liabilitas / Tanggung Jawab Hukum):** Fitur sistem hukum yang memungkinkan individu menuntut ganti rugi atas kerugian yang ditimbulkan oleh pihak lain.
4. **Due Process (Proses Hukum yang Adil):** Proses peradilan berbasis hukum di mana undang-undang diketahui dan dipahami oleh publik, serta terdapat kemampuan untuk mengajukan banding kepada otoritas yang lebih tinggi.

### 3.2 Kerangka 5 Langkah Analisis Etika (Five-Step Ethical Analysis)
Ketika menghadapi situasi dilema moral di tempat kerja atau proyek perangkat lunak, terapkan langkah terstruktur berikut:
1. **Identifikasi dan jelaskan fakta dengan jelas:** Cari tahu siapa yang melakukan apa, kepada siapa, kapan, di mana, dan bagaimana faktanya tanpa prasangka awal.
2. **Definisikan konflik atau dilema dan identifikasi nilai-nilai luhur yang terlibat:** Identifikasi dua nilai atau hak yang saling bertentangan (misalnya: hak perusahaan melindungi aset vs. hak privasi karyawan).
3. **Identifikasi para pemangku kepentingan (*stakeholders*):** Siapa saja pihak yang terdampak langsung maupun tidak langsung oleh keputusan tersebut?
4. **Identifikasi opsi-opsi tindakan yang wajar yang dapat diambil:** Cari alternatif jalan keluar yang tidak hanya hitam-putih; apakah ada opsi kompromi yang etis?
5. **Identifikasi potensi konsekuensi dari setiap opsi tindakan:** Nilai konsekuensi positif dan negatif dari setiap opsi terhadap semua pemangku kepentingan.

### 3.3 Enam Prinsip Kandidat Etika (Candidate Ethical Principles)
Prinsip-prinsip klasik yang dapat digunakan sebagai kompas pemandu moral:
1. **The Golden Rule (Aturan Emas):** Perlakukan orang lain sebagaimana Anda sendiri ingin diperlakukan oleh orang lain.
2. **Immanuel Kant's Categorical Imperative (Imperatif Kategoris):** Jika suatu tindakan tidak pantas dilakukan oleh semua orang di dunia, maka tindakan tersebut tidak pantas dilakukan oleh siapa pun.
3. **Descartes' Rule of Change / Slippery Slope (Lereng Licin):** Jika suatu tindakan tidak dapat dilakukan secara berulang-ulang tanpa merusak sistem, maka tindakan tersebut tidak boleh dilakukan sama sekali walau hanya sekali.
4. **The Utilitarian Principle (Prinsip Utilitarian):** Pilihlah tindakan yang menghasilkan kebaikan terbesar atau nilai manfaat tertinggi bagi jumlah orang terbanyak.
5. **Risk Aversion Principle (Prinsip Penghindaran Risiko):** Pilihlah tindakan yang menghasilkan potensi kerugian, bahaya, atau biaya terminimal bagi pihak yang rentan.
6. **Ethical "No Free Lunch" Rule (Aturan Tiada Makan Siang Gratis):** Asumsikan bahwa semua benda berwujud maupun tidak berwujud adalah milik seseorang kecuali dinyatakan sebaliknya secara eksplisit. Jika Anda memanfaatkan karya orang lain, Anda harus memberikan kompensasi atau atribusi.

---

## 4. Hak Informasi: Privasi dan Kebebasan di Era Digital

### 4.1 Definisi Privasi dan Fair Information Practices (FIP)
> [!info] Definisi Privasi
> **Privasi (*Privacy*)** adalah klaim individu untuk dibiarkan sendiri, bebas dari pengawasan atau campur tangan pihak lain atau organisasi luar, termasuk negara.

Prinsip **FTC Fair Information Practices (FIP)** yang menjadi acuan regulasi internasional:
1. **Notice / Awareness (Pemberitahuan):** Situs web wajib memberitahu pengguna secara jelas mengenai data apa yang dikumpulkan sebelum data diambil.
2. **Choice / Consent (Pilihan & Persetujuan):** Pengguna harus diberikan pilihan bagaimana data mereka akan digunakan (terutama untuk tujuan pihak ketiga).
3. **Access / Participation (Akses & Partisipasi):** Pengguna berhak melihat data tentang diri mereka dan memperbaiki data yang keliru.
4. **Integrity / Security (Keamanan):** Pengumpul data wajib menjamin akurasi dan pengamanan data dari pencurian atau peretasan.
5. **Enforcement (Penegakan Hukum):** Harus ada mekanisme sanksi hukum yang mengikat jika penyelenggara melanggar prinsip-prinsip di atas.

### 4.2 Regulasi Privasi Global: European Union GDPR (Opt-In vs Opt-Out)
- **GDPR (*General Data Protection Regulation*):** Regulasi perlindungan data komprehensif Uni Eropa (berlaku sejak Mei 2018) dengan sanksi denda fantastis hingga 4% dari total omzet global tahunan perusahaan pelanggar.
- **Model Persetujuan:**
  - **Model Opt-In (Eropa / GDPR):** Bisnis dilarang mengumpulkan data pribadi apa pun KECUALI konsumen secara eksplisit mencentang persetujuan (*explicit consent*).
  - **Model Opt-Out (AS Tradisional):** Bisnis bebas mengumpulkan data KECUALI konsumen secara proaktif meminta pengumpulan dihentikan.

### 4.3 Vektor Pelanggaran Privasi di Internet: Cookies, Web Beacons, dan Spyware

![[mis16e-fig-4-3-cookies-identify-web-visitors.jpg]]
*Gambar 3.4: Mekanisme Bagaimana Cookies Mengidentifikasi dan Melacak Pengunjung Web*

- **Cookies:** File teks kecil yang disematkan server situs ke hard disk komputer pengguna untuk mengingat preferensi kunjungan.
  - *Third-party cookies (Tracking cookies):* Ditanam oleh jaringan periklanan luar (misal Google/Meta) untuk merekam jejak penjelajahan pengguna di ribuan situs web yang berbeda (*cross-site tracking*).
- **Web Beacons (Web Bugs):** File grafik berukuran 1x1 piksel yang tak terlihat, disematkan pada email atau halaman web untuk melacak apakah pesan telah dibuka dan dibaca.
- **Spyware & Keyloggers:** Perangkat lunak mencurigakan yang diam-diam merekam ketukan papan ketik atau mengirimkan riwayat penjelajahan pengguna ke server pihak luar tanpa persetujuan.

---

## 5. Studi Kasus Nyata dan Regulasi Siber di Indonesia

### 5.1 Studi Kasus 1: Mengapa Google Gratis? (Model Bisnis Berbasis Data)
Berdasarkan materi pembahasan slide dosen:

![[si-bab3-p08-kasus-google-gratis.png]]
*Gambar 3.5: Diskusi Kasus Mengapa Google Gratis (Slide Dosen KOM1333A)*
- **Paradoks Layanan Cuma-Cuma:** Mengapa mesin pencari Google, YouTube, Gmail, dan Maps dapat dinikmati pengguna secara gratis?
- **Fakta Finansial:** Lebih dari 80% pendapatan Alphabet Inc. bersumber dari **Advertising Revenue (Google Ads & AdSense)**.
- **Biaya yang Tak Kasat Mata:** Pengguna tidak membayar dengan uang tunai, melainkan **membayar dengan data pribadi dan privasi mereka**.
- **Perangkap Terms of Service (ToS):**
  - Mayoritas mutlak pengguna tidak pernah membaca ToS sebelum mengklik tombol *"I Agree"*.
  - ToS memberikan hak legal bagi korporasi untuk menganalisis isi email, riwayat pencarian, pergerakan lokasi GPS, dan riwayat video demi menargetkan iklan berbayar secara presisi.

### 5.2 Studi Kasus 2: Pelacakan Lokasi Seluler dan Aplikasi MuslimPro

![[si-bab3-p05-kasus-location-tracking.png]]
*Gambar 3.6: Kasus Mobile Location Tracking Systems (Slide Dosen KOM1333A)*

![[si-bab3-p06-kasus-muslimpro.png]]
*Gambar 3.7: Kasus Penjualan Data Lokasi Pengguna Aplikasi MuslimPro*

- **Latar Belakang Kasus:** Pada tahun 2020 terungkap investigasi jurnalisme bahwa data lokasi presisi dari jutaan pengguna aplikasi pengingat salat **MuslimPro** telah dibeli oleh pialang data (*data broker: X-Mode / Babel Street*) dan diteruskan ke pihak militer Amerika Serikat (*US Special Operations Command*).
- **Dilema Etika:** Pengguna memasang aplikasi religius dengan keyakinan sakral, namun metadata koordinat GPS mereka dimonetisasi dan digunakan untuk tujuan pengawasan intelijen militer global tanpa pemahaman sadar dari pengguna.

### 5.3 Studi Kasus 3: Kebocoran Data Skala Masif di Indonesia (Bjorka BPJS & Dukcapil)
Berdasarkan arsip slide dosen:

![[si-bab3-p03-kasus-bjorka-bpjs.png]]
*Gambar 3.8: Kasus Kebocoran Data BPJS yang Diunggah Peretas Bjorka (Slide Dosen KOM1333A)*

![[si-bab3-p04-kasus-dukcapil-hoaks.png]]
*Gambar 3.9: Kasus Kebocoran 300 Juta Data Kependudukan Dukcapil & Situs Hoaks*

1. **Kasus Peretasan Data BPJS Kesehatan oleh Bjorka:**
   - 279 juta data kependudukan peserta BPJS Kesehatan (termasuk NIK, nomor telepon, alamat, riwayat keluarga, dan data gaji) bocor dan diperjualbelikan di forum peretas *RaidForums / Breached.vc*.
2. **Kasus 300 Juta Data Dukcapil Kemendagri:**
   - Kebocoran masif data induk kependudukan yang mengekspos identitas jutaan warga negara Indonesia, memicu gelombang penipuan perbankan dan pinjaman online ilegal.
3. **Penyebaran Hoaks di Ruang Siber Indonesia:**
   - Data Kementerian Kominfo mencatat lebih dari 800.000 situs dan kanal penyebar hoaks/disinformasi di Indonesia, menuntut pengawasan ketat antara menjaga ketertiban umum vs. menjaga kebebasan berekspresi.

### 5.4 Kerangka Hukum Positif Indonesia: UU ITE dan UU Perlindungan Data Pribadi (UU PDP)
Untuk melindungi masyarakat dan menegakkan kepastian hukum, pemerintah Indonesia mengesahkan dua payung hukum utama:

#### 1. UU ITE (UU No. 11 Tahun 2008 jo. UU No. 19 Tahun 2016)
- Mengatur keabsahan tanda tangan elektronik, transaksi niaga daring, serta pertanggungjawaban hukum penyelenggara sistem elektronik.
- Mengatur delik pidana siber:
  - *Pasal 27:* Larangan mendistribusikan konten asusila, perjudian, pencemaran nama baik, dan pemerasan secara elektronik.
  - *Pasal 30:* Larangan akses ilegal (*illegal access / hacking*) terhadap sistem komputer milik orang lain.
  - *Pasal 32:* Larangan mengubah, merusak, atau memindahkan informasi/dokumen elektronik orang lain.

#### 2. UU Perlindungan Data Pribadi / UU PDP (UU No. 27 Tahun 2022)
Tonggak sejarah hukum perlindungan data pribadi di Indonesia (mengadopsi standar Uni Eropa GDPR):
- **Klasifikasi Data Pribadi:**
  - *Data Pribadi Spesifik:* Data kesehatan, data biometrik, data genetika, riwayat kejahatan, data anak, data keuangan pribadi.
  - *Data Pribadi Umum:* Nama lengkap, jenis kelamin, kewarganegaraan, agama, status perkawinan, data kombinasi identitas.
- **Hak Subjek Data:** Hak atas kejelasan identitas pemroses, hak melengkapi/memperbaiki data, hak mengakhiri pemrosesan/menghapus data (*right to erasure*), dan hak menuntut ganti rugi atas kebocoran.
- **Kewajiban Pengendali Data:** Wajib menjaga keamanan data secara enkripsi, wajib memberitahukan kegagalan pelindungan data (kebocoran) secara tertulis kepada subjek data dan lembaga berwenang maksimal **3 x 24 jam**.
- **Sanksi:** Denda administratif hingga 2% dari total pendapatan tahunan, serta pidana penjara hingga 6 tahun bagi pihak yang sengaja memperjualbelikan data pribadi tanpa hak.

---

## 6. Hak Kepemilikan: Kekayaan Intelektual di Era Digital

Kekayaan Intelektual (*Intellectual Property - IP*) dilindungi melalui tiga instrumen legal:

```
┌────────────────────────────────────────────────────────────────────────┐
│              TIGA INSTRUMEN PERLINDUNGAN KEKAYAAN INTELEKTUAL          │
├─────────────────────────┬──────────────────────────┬───────────────────┤
│ RAHASIA DAGANG          │ HAK CIPTA (COPYRIGHT)    │ PATEN (PATENT)    │
│ (Trade Secret)          │                          │                   │
├─────────────────────────┼──────────────────────────┼───────────────────┤
│ • Formula, desain, atau │ • Melindungi ekspresi    │ • Melindungi      │
│   kompilasi data bisnis │   orisinal karya (kode   │   penemuan metode     │
│   yang dirahasiakan     │   sumber program, teks,  │   mesin, proses, atau │
│ • Tidak ada kadaluarsa  │   seni, audio)           │   alat baru           │
│ • Lemah jika dibongkar  │ • Masa berlaku: Seumur   │ • Masa berlaku:   │
│   lewat rekayasa balik  │   hidup pencipta + 70 th │   20 tahun sejak izin │
│   (reverse engineering) │ • Tidak melindungi ide   │ • Monopoli kuat   │
│   secara legal          │   dasar, hanya ekspresi  │   secara hukum        │
└─────────────────────────┴──────────────────────────┴───────────────────┘
```

### 6.4 Tantangan Era Internet terhadap HAKI
- Perangkat lunak, musik, film, dan buku kini berwujud bit digital yang dapat disalin jutaan kali tanpa biaya marginal (*zero marginal cost*) dan ditransmisikan secara instan ke seluruh dunia.
- Perlawanan industri media melahirkan **Digital Millennium Copyright Act (DMCA)** dan mekanisme proteksi **Digital Rights Management (DRM)**.

---

## 7. Kualitas Sistem, Liabilitas, dan Kualitas Hidup

### 7.1 Masalah Liabilitas dan Kualitas Perangkat Lunak
- Siapa yang bertanggung jawab jika perangkat lunak navigasi mobil otonom salah membaca rambu lalu lintas dan menabrak pejalan kaki? Apakah pemrogram, produsen mobil, atau pemilik kendaraan?
- **Kemustahilan Zero-Defect Software:** Menguji seluruh kemungkinan kombinasi logika kode dalam perangkat lunak enterprise yang terdiri dari jutaan baris kode adalah kemustahilan ekonomis. Kegagalan sistem selalu menyisakan risiko tak terduga.

### 7.2 Dampak Sosial: Jam Kerja, Otomasi Lapangan Kerja, dan Dampak Kesehatan
1. **Pengikisan Batas Kerja dan Keluarga (*Boundary Erosion*):** Email dan ponsel pintar membuat karyawan harus selalu siaga (*always on*), memicu stres kerja kronis.
2. **Otomasi dan Lapangan Kerja (*Job Displacement*):** Perdebatan apakah AI dan robotika akan menciptakan profesi baru lebih cepat daripada memusnahkan pekerjaan kerah putih dan kerah biru.
3. **Kesenjangan Digital (*Digital Divide*):** Kesenjangan akses teknologi dan keahlian komputasi antar kelompok masyarakat kaya vs miskin, serta perkotaan vs pedesaan.
4. **Dampak Kesehatan Fisik dan Mental:**
   - *RSI (Repetitive Stress Injury):* Cedera otot/saraf akibat gerakan repetitif mengetik atau menggunakan mouse (*contoh: Carpal Tunnel Syndrome*).
   - *CVS (Computer Vision Syndrome):* Kelelahan otot mata, pandangan kabur, dan sakit kepala akibat menatap layar monitor komputer secara terus-menerus.
   - *Technostress:* Tekanan psikologis atau kecemasan akibat ketidakmampuan beradaptasi dengan tuntutan teknologi informasi yang berubah sangat cepat.

---

## 8. Ringkasan & Latihan Ujian (Cheatsheet)

> [!tip] Rangkuman Cepat untuk Ujian (Exam Quick Sheet)
> 1. **5 Dimensi Moral Era Informasi:** Hak Informasi, Hak Kepemilikan, Akuntabilitas & Kontrol, Kualitas Sistem, Kualitas Hidup.
> 2. **NORA & Profiling:** Profiling menggabungkan data transaksi individu; NORA menemukan hubungan tersembunyi antar data dari berbagai repositori terpisah.
> 3. **Prinsip Kandidat Etika:**
>    - Golden Rule (Perlakukan orang lain sebagaimana ingin diperlakukan).
>    - Kant's Imperative (Jika tak pantas bagi semua, tak pantas bagi siapa pun).
>    - Descartes' Slippery Slope (Jika tak bisa berulang, jangan lakukan sama sekali).
>    - Utilitarian (Pilih yang mendatangkan manfaat terbesar).
>    - Risk Aversion (Pilih yang meminimalkan bahaya/biaya terendah).
>    - No Free Lunch (Asumsikan semua karya berwujud/tak berwujud adalah milik seseorang).
> 4. **GDPR vs AS (Privasi):** Eropa menganut sistem **Opt-In** (persetujuan eksplisit sebelum data diambil), sedangkan AS condong pada **Opt-Out**.
> 5. **Hukum Siber Indonesia:**
>    - UU ITE (UU 11/2008 & UU 19/2016): Transaksi elektronik & delik pidana siber (akses ilegal, manipulasi data).
>    - UU PDP (UU 27/2022): Mengatur hak subjek data, kewajiban pelaporan kebocoran 3x24 jam, klasifikasi data pribadi umum & spesifik.
> 6. **Tiga Perlindungan HAKI:** Rahasia Dagang (formula rahasia), Hak Cipta (ekspresi tulisan/kode), Paten (penemuan mesin/proses 20 tahun).
> 7. **Penyakit Digital:** RSI (cedera saraf tangan/pergelangan), CVS (kelelahan retina/mata), Technostress (tekanan mental).
