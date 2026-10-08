---
title: "Week 5: Foundations of Business Intelligence - Databases and Information Management"
tags:
  - databases
  - rdbms
  - sql
  - normalization
  - er-diagram
  - nosql
  - blockchain
  - big-data
  - data-warehouse
  - olap
  - data-mining
  - obsidian-notes
course: KOM1333A - Sistem Informasi
textbook: "Management Information Systems (Laudon & Laudon, 16th Ed), Chapter 6"
date: 2026-10-08
type: study-note
---

# Week 5: Foundations of Business Intelligence: Databases and Information Management

> [!abstract] Ringkasan Eksekutif
> Catatan ini mengkaji manajemen data dan infrastruktur intelijen bisnis berbasis **Chapter 6 Laudon & Laudon**:
> 1. **Masalah Lingkungan File Tradisional:** Redundansi, inkonsistensi, dan dependensi program-data.
> 2. **Sistem Manajemen Basis Data (DBMS) & Relasional (RDBMS):** Struktur tabel (baris/tupel, kolom/atribut, primary key, foreign key), dan 3 operasi dasar: **SELECT, PROJECT, dan JOIN**.
> 3. **Perancangan Basis Data:** Prinsip **Normalisasi (1NF, 2NF, 3NF)** untuk mencegah anomali serta penyusunan **Entity-Relationship Diagram (ERD)**.
> 4. **NoSQL & Blockchain:** Basis data non-relasional, basis data cloud, dan teknologi buku besar terdistribusi (*distributed ledger*).
> 5. **Tantangan Big Data:** Karakteristik **3V (*Volume, Velocity, Variety*)**.
> 6. **Infrastruktur Business Intelligence (BI):** *Data Warehouse*, *Data Mart*, *Hadoop (HDFS & MapReduce)*, dan *In-Memory Computing*.
> 7. **Alat Analitis BI:** **OLAP (*Online Analytical Processing*)**, *Data Mining*, *Text Mining*, *Web Mining*, arsitektur web-ke-database, serta Tata Kelola Data (*Data Governance*).

---

## Daftar Isi (Table of Contents)
- [[#1. Pengorganisasian Data Tradisional dan Masalahnya]]
  - [[#1.1 Hirarki Data Komputer: Bit hingga Database]]
  - [[#1.2 Keterbatasan dan Masalah Pemrosesan File Tradisional]]
- [[#2. Pendekatan Database: Database Management Systems (DBMS)]]
  - [[#2.1 Definisi DBMS dan Pemisahan Logis vs Fisik]]
  - [[#2.2 Relational DBMS (RDBMS): Tabel, Tupel, dan Kunci]]
  - [[#2.3 Tiga Operasi Dasar RDBMS: SELECT, PROJECT, dan JOIN]]
  - [[#2.4 Kapabilitas DBMS: DDL, Kamus Data (Data Dictionary), dan SQL]]
- [[#3. Perancangan Basis Data (Database Design)]]
  - [[#3.1 Normalisasi Basis Data (Normalization: Mencegah Anomali Data)]]
  - [[#3.2 Diagram Hubungan Entitas (Entity-Relationship Diagram - ERD)]]
- [[#4. Database Non-Relasional (NoSQL), Cloud Database, dan Blockchain]]
  - [[#4.1 Keterbatasan RDBMS dan Kebangkitan Basis Data NoSQL]]
  - [[#4.2 Basis Data Komputasi Awan (Cloud Databases)]]
  - [[#4.3 Blockchain: Buku Besar Terdistribusi dan Kekal]]
- [[#5. Tantangan Big Data dan Infrastruktur Business Intelligence]]
  - [[#5.1 Karakteristik Big Data: Volume, Velocity, dan Variety (3V)]]
  - [[#5.2 Gudang Data (Data Warehouse) dan Data Mart]]
  - [[#5.3 Ekosistem Apache Hadoop (HDFS dan MapReduce)]]
  - [[#5.4 In-Memory Computing dan Platform Analitik]]
- [[#6. Alat Analitis: Menemukan Pola, Tren, dan Hubungan Tersembunyi]]
  - [[#6.1 Online Analytical Processing (OLAP) dan Kubus Data]]
  - [[#6.2 Data Mining: Asosiasi, Sekuens, Klasifikasi, dan Kluster]]
  - [[#6.3 Text Mining, Analisis Sentimen, dan Web Mining]]
  - [[#6.4 Arsitektur Integrasi Database dengan Web]]
- [[#7. Tata Kelola Data dan Menjamin Kualitas Data]]
  - [[#7.1 Kebijakan Informasi dan Data Governance]]
  - [[#7.2 Audit Kualitas Data dan Pembersihan Data (Data Cleansing)]]
- [[#8. Ringkasan & Latihan Ujian (Cheatsheet)]]

---

## 1. Pengorganisasian Data Tradisional dan Masalahnya

### 1.1 Hirarki Data Komputer: Bit hingga Database
Sistem komputer mengorganisasikan data ke dalam hierarki piramida yang bertingkat dari unit sirkuit terkecil hingga repositori enterprise:

![[mis16e-fig-6-1-data-hierarchy.jpg]]
*Gambar 5.1: Hirarki Data Komputer: Dari Bit, Byte, Field, Record, File, hingga Database*

```
                            ┌────────────────────────┐
                            │        DATABASE        │  Kumpulan file/tabel
                            └───────────┬────────────┘  yang saling berelasi
                                        │
                            ┌───────────┴────────────┐
                            │      FILE / TABEL      │  Kumpulan record yang
                            └───────────┬────────────┘  berkaitan (misal data mahasiswa)
                                        │
                            ┌───────────┴────────────┐
                            │     RECORD / BARIS     │  Kumpulan field yang
                            └───────────┬────────────┘  menggambarkan satu entitas
                                        │
                            ┌───────────┴────────────┐
                            │      FIELD / KOLOM     │  Kumpulan karakter yang
                            └───────────┬────────────┘  menggambarkan atribut tunggal
                                        │
                            ┌───────────┴────────────┐
                            │      BYTE (Karakter)   │  Kumpulan 8 bit (misal huruf 'A')
                            └───────────┬────────────┘
                                        │
                            ┌───────────┴────────────┐
                            │     BIT (0 atau 1)     │  Unit data terkecil komputer
                            └────────────────────────┘
```

- **Entitas (*Entity*):** Orang, tempat, atau peristiwa di mana informasi disimpan (contoh: Mahasiswa, Transaksi Penjualan).
- **Atribut (*Attribute*):** Karakteristik atau sifat yang menggambarkan entitas tertentu (contoh: NIM, Nama, IPK).

### 1.2 Keterbatasan dan Masalah Pemrosesan File Tradisional

![[mis16e-fig-6-2-traditional-file-processing.jpg]]
*Gambar 5.2: Masalah Lingkungan Pemrosesan File Tradisional (Duplikasi Data & Ketergantungan)*

Sebelum adanya DBMS modern, setiap departemen membangun program aplikasinya sendiri yang memiliki file data mandiri (*file-oriented processing*). Pendekatan kuno ini memicu persoalan kritis:
1. **Redundansi dan Inkonsistensi Data (*Data Redundancy & Inconsistency*):**
   - *Redundansi:* Data yang sama (misal alamat pelanggan) diduplikasi di banyak file di divisi pemasaran, akuntansi, dan pengiriman.
   - *Inkonsistensi:* Ketika pelanggan pindah rumah, alamat diubah di departemen pemasaran tetapi tidak diubah di bagian penagihan akuntansi.
2. **Ketergantungan Program-Data (*Program-Data Dependence*):**
   - Hubungan kaku antara kode program software dan struktur file fisik di disk. Jika panjang field kode pos diubah dari 5 menjadi 9 digit, ratusan program lama yang mengakses file tersebut harus ditulis ulang secara manual.
3. **Kurangnya Fleksibilitas (*Lack of Flexibility*):**
   - Sistem pemrosesan file tradisional mampu mencetak laporan rutin terjadwal, tetapi gagal menjawab kueri spontan (*ad hoc queries*) dari manajer (misal: *"Tampilkan daftar pelanggan yang belum membeli selama 6 bulan terakhir"* membutuhkan penulisan kode program berminggu-minggu).
4. **Keamanan Data yang Buruk (*Poor Security*):**
   - Karena manajemen data terdistribusi acak di berbagai file lokal, sulit mengontrol otorisasi akses dan enkripsi secara terpusat.
5. **Kurangnya Ketersediaan dan Berbagi Data (*Lack of Data Sharing*):**
   - Data terisolasi dalam silo-silo informasi independen sehingga mustahil digabungkan untuk analisis korporat holistik.

---

## 2. Pendekatan Database: Database Management Systems (DBMS)

### 2.1 Definisi DBMS dan Pemisahan Logis vs Fisik
> [!info] Definisi DBMS
> **Database Management System (DBMS)** adalah perangkat lunak khusus yang mengontrol pembuatan, pemeliharaan, keamanan, dan penggunaan basis data bersama. DBMS bertindak sebagai perantara cerdas antara program aplikasi pengguna dan file data fisik di media penyimpanan.

![[mis16e-fig-6-3-hr-database-multiple-views.jpg]]
*Gambar 5.3: Basis Data SDM dengan Pemisahan Logical Views dan Tampilan Fisik Tunggal*

- **Pemisahan Tampilan Logis vs Tampilan Fisik:**
  - *Tampilan Fisik (Physical View):* Menunjukkan bagaimana data sebenarnya disusun dan disimpan secara fisik pada disk magnetik atau media SSD.
  - *Tampilan Logis (Logical View):* Menyajikan data sebagaimana data tersebut dipahami dan dibutuhkan oleh pengguna bisnis atau programmer aplikasi.
  - *Manfaat Pemisahan:* Programmer aplikasi tidak perlu memikirkan letak sektor hard disk; mereka cukup memanggil kueri logis seperti `SELECT Nama FROM Mahasiswa`.

```
┌─────────────────────┐       ┌─────────────────────┐
│  APLIKASI PENGGAJIAN│       │  APLIKASI HRD / SDM │
└──────────┬──────────┘       └──────────┬──────────┘
           │ (Tampilan Logis 1)          │ (Tampilan Logis 2)
           ▼                             ▼
┌───────────────────────────────────────────────────┐
│                    SISTEM DBMS                    │
│      (Oracle, Microsoft SQL Server, MySQL)        │
└──────────────────────────┬────────────────────────┘
                           │ (Tampilan Fisik Terpadu)
                           ▼
┌───────────────────────────────────────────────────┐
│              BASIS DATA FISIK TERSENTRAL          │
│       [Data Pegawai, Gaji, Asuransi, Jam Kerja]   │
└───────────────────────────────────────────────────┘
```

### 2.2 Relational DBMS (RDBMS): Tabel, Tupel, dan Kunci
Tipe DBMS paling dominan dalam bisnis modern adalah **Relational DBMS (RDBMS)**:

![[mis16e-fig-6-4-relational-database-tables.jpg]]
*Gambar 5.4: Struktur Tabel Database Relasional (Baris/Tupel, Kolom/Atribut, Primary & Foreign Key)*
- Mengorganisasikan data ke dalam tabel dua dimensi yang disebut **Relasi (*Relations*)**.
- **Baris (*Rows / Tuples*):** Mewakili record data aktual tentang entitas individu.
- **Kolom (*Columns / Fields / Attributes*):** Mewakili atribut spesifik dari entitas.
- **Kunci Utama (*Primary Key*):** Field atau atribut yang secara unik mengidentifikasi setiap baris dalam tabel sehingga tidak ada record yang ambigu (contoh: `NIM` atau `Nomor_KTP`).
- **Kunci Asing (*Foreign Key*):** Atribut dalam suatu tabel yang merujuk pada Primary Key tabel lain untuk membentuk relasi logis antartabel.

### 2.3 Tiga Operasi Dasar RDBMS: SELECT, PROJECT, dan JOIN
RDBMS memanipulasi data melalui tiga operasi aljabar relasional fundamental:

![[mis16e-fig-6-5-three-operations-relational-dbms.jpg]]
*Gambar 5.5: Tiga Operasi Aljabar Relasional: SELECT, PROJECT, dan JOIN*

```
Tabel SUMBER_A (Supplier)                    Tabel SUMBER_B (Part)
┌────────────┬─────────────┬────────┐        ┌─────────┬──────────────┬────────────┐
│ SupplierID │ Nama        │ Kota   │        │ PartNum │ NamaPart     │ SupplierID │
├────────────┼─────────────┼────────┤        ├─────────┼──────────────┼────────────┤
│ 101        │ PT Maju     │ Bogor  │        │ P-01    │ Baut Baja    │ 101        │
│ 102        │ CV Lancar   │ Jakarta│        │ P-02    │ Mur Hexagonal│ 102        │
└────────────┴─────────────┴────────┘        └─────────┴──────────────┴────────────┘
```

1. **SELECT:** Memilih dan membuat subset dari **baris-baris (*rows*)** yang memenuhi kriteria logis tertentu (misal: Pilih baris di mana `Kota = 'Bogor'`).
2. **PROJECT:** Memilih dan membuat subset dari **kolom-kolom (*columns*)** tertentu dari tabel, mengabaikan kolom lainnya (misal: Hanya ambil kolom `NamaPart` dan `SupplierID`).
3. **JOIN:** Menggabungkan dua atau lebih tabel relasional menjadi satu tabel baru berdasarkan atribut kunci yang cocok (misal: Hubungkan `Tabel Supplier` dan `Tabel Part` berdasarkan kesamaan `SupplierID`).

### 2.4 Kapabilitas DBMS: DDL, Kamus Data (Data Dictionary), dan SQL
- **Data Definition Language (DDL):** Perintah formal untuk menetapkan struktur konten database, membuat tabel baru, dan mendefinisikan tipe data field (contoh: `CREATE TABLE`, `ALTER TABLE`).
- **Data Dictionary (Kamus Data):** Berkas repositori terotomasi

![[mis16e-fig-6-6-access-data-dictionary.jpg]]
*Gambar 5.6: Fitur Kamus Data (Data Dictionary) untuk Mengelola Metadata Atribut* yang menyimpan informasi metadata mengenai elemen data (definisi field, tipe numerik/teks, batas panjang karakter, hak akses pengguna).
- **Data Manipulation Language (DML):** Bahasa khusus untuk memanipulasi data di dalam database (menambah, mengubah, mengambil data).
  - Standar industri paling universal adalah **SQL (*Structured Query Language*)**:
    ```sql
    SELECT PartNum, NamaPart, Harga
    FROM Part
    WHERE Harga > 50000
    ORDER BY Harga DESC;
    ```

---

## 3. Perancangan Basis Data (Database Design)

Untuk merancang basis data yang tangguh, tim analis harus melalui perancangan **Model Konseptual (Logis)** sebelum menerapkan **Model Fisik**.

### 3.1 Normalisasi Basis Data (Normalization: Mencegah Anomali Data)
> [!info] Definisi Normalisasi
> **Normalisasi** adalah proses sistematis menyederhanakan kelompok data yang kompleks untuk meminimalkan elemen data redundan, menghilangkan relasi banyak-ke-banyak yang canggung, dan mencegah anomali manipulasi data.

#### Tiga Anomali yang Dihilangkan oleh Normalisasi:
1. **Update Anomaly:** Mengubah alamat pelanggan mengharuskan perubahan di ribuan baris pesanan; jika terlewat satu baris, data menjadi rusak/inkonsisten.
2. **Insertion Anomaly:** Tidak bisa memasukkan informasi vendor baru ke database sebelum vendor tersebut memasok pesanan barang tertentu.
3. **Deletion Anomaly:** Menghapus pesanan barang tertentu secara tidak sengaja menghapus seluruh rekam jejak identitas pelanggan dari sistem.

![[mis16e-fig-6-9-unnormalized-relation-order.jpg]]
*Gambar 5.7: Tabel Relasi Pesanan Sebelum Dinormalisasi (Mengandung Grup Berulang)*

![[mis16e-fig-6-10-normalized-tables-order.jpg]]
*Gambar 5.8: Tabel Hasil Normalisasi Relasional yang Efisien dan Bebas Anomali*

#### Tahapan Bentuk Normal:
- **First Normal Form (1NF):** Tidak ada grup elemen berulang (*repeating groups*); setiap sel tabel hanya bernilai tunggal (*atomic*).
- **Second Normal Form (2NF):** Sudah 1NF dan seluruh atribut bukan kunci bergantung sepenuhnya pada seluruh kunci utama (*no partial functional dependency*).
- **Third Normal Form (3NF):** Sudah 2NF dan tidak ada dependensi transitif antaratribut non-kunci (*no transitive dependency*).

### 3.2 Diagram Hubungan Entitas (Entity-Relationship Diagram - ERD)
Diagram skematis yang menggambarkan hubungan logis antarentitas dalam sistem basis data:

![[mis16e-fig-6-11-entity-relationship-diagram.jpg]]
*Gambar 5.9: Entity-Relationship Diagram (ERD) untuk Sistem Manajemen Pesanan*

```
┌──────────────────┐               ┌──────────────────┐
│     CUSTOMER     │ 1           N │      ORDER       │
│  (Pelanggan)     ├───────────────┤   (Pesanan)      │
│ PK: Customer_ID  │  Melakukan    │ PK: Order_Num    │
│     Nama, Alamat │               │ FK: Customer_ID  │
└──────────────────┘               └────────┬─────────┘
                                            │ M
                                            │ Berisi
                                            │ N
                                   ┌────────┴─────────┐
                                   │       LINE_ITEM  │
                                   │ PK: Order_Num,   │
                                   │     Part_Num     │
                                   └────────┬─────────┘
                                            │ N
                                            │ Merujuk
                                            │ 1
                                   ┌────────┴─────────┐
                                   │       PART       │
                                   │  (Suku Cadang)   │
                                   │ PK: Part_Num     │
                                   │     Nama, Harga  │
                                   └──────────────────┘
```

#### Tipe Kardinalitas Hubungan:
- **One-to-One (1:1):** Satu entitas berhubungan dengan tepat satu entitas lain (misal: Dosen dengan Ruang Kerja Kepala Lab).
- **One-to-Many (1:N):** Satu entitas berhubungan dengan banyak entitas lain (misal: Satu Pelanggan dapat membuat Banyak Pesanan).
- **Many-to-Many (M:N):** Banyak entitas berhubungan dengan banyak entitas lain (misal: Banyak Mahasiswa mengambil Banyak Mata Kuliah; diselesaikan dengan membuat *tabel penghubung / junction table*).

---

## 4. Database Non-Relasional (NoSQL), Cloud Database, dan Blockchain

### 4.1 Keterbatasan RDBMS dan Kebangkitan Basis Data NoSQL
Meskipun RDBMS sangat baik untuk data terstruktur transaksi keuangan (*ACID compliant*), RDBMS kesulitan menangani:
- Kumpulan data berskala ratusan terabyte hingga petabyte.
- Data web tak terstruktur (dokumen teks bebas, grafik jejaring sosial, posting media sosial, log klik video).
- **NoSQL (*Not Only SQL*):** Database non-relasional yang menggunakan model data lebih fleksibel tanpa skema tabel kaku, sangat mudah diskalakan secara horizontal (*scale-out*) melintasi ribuan server kluster murah.
  - *Document Store:* Menyimpan dokumen format JSON/BSON (contoh: **MongoDB**, CouchDB).
  - *Key-Value Store:* Menyimpan pasangan kunci-nilai berkecepatan kueri tinggi (contoh: **Redis**, **Amazon DynamoDB**).
  - *Wide-Column Store:* Dioptimasi untuk pencarian kueri kolom masif (contoh: **Apache Cassandra**).
  - *Graph Database:* Dioptimasi untuk menelusuri hubungan jejaring sosial (contoh: **Neo4j**).

### 4.2 Basis Data Komputasi Awan (Cloud Databases)
- Database yang berjalan di infrastruktur cloud penyedia pihak ketiga, menawarkan replikasi data global, cadangan otomatis, dan elastisitas kapasitas.
- Contoh: Amazon Relational Database Service (Amazon RDS), Amazon Aurora, Microsoft Azure SQL Database, dan Google Cloud Spanner.

### 4.3 Blockchain: Buku Besar Terdistribusi dan Kekal

![[mis16e-fig-6-12-how-blockchain-works.jpg]]
*Gambar 5.10: Mekanisme Rantai Blok Kriptografi dalam Blockchain*

> [!info] Definisi Blockchain
> **Blockchain** adalah teknologi buku besar terdistribusi (*distributed ledger*) terdesentralisasi yang mencatat transaksi secara permanen, aman secara kriptografis, dan tahan terhadap manipulasi perubahan data (*immutable*).

```
┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│   BLOK 101   │      │   BLOK 102   │      │   BLOK 103   │
├──────────────┤      ├──────────────┤      ├──────────────┤
│ Data Transaksi│      │ Data Transaksi│      │ Data Transaksi│
│ Hash: 7a9f...├─────►│ Prev: 7a9f...│      │ Prev: 3b1c...│
│ Nonce: 48192 │      │ Hash: 3b1c...├─────►│ Hash: e82d...│
└──────────────┘      └──────────────┘      └──────────────┘
```

- **Mekanisme Konsensus:** Tidak ada server database pusat; seluruh simpul (*nodes*) jaringan memverifikasi keabsahan transaksi melalui konsensus algoritma (misal: *Proof of Work* atau *Proof of Stake*).
- **Aplikasi Enterprise:** Pelacakan rantai pasok makanan (melacak asal daging sapi dari peternakan ke supermarket), verifikasi kepemilikan aset properti, dan kontrak pintar (*smart contracts*).

---

## 5. Tantangan Big Data dan Infrastruktur Business Intelligence

![[mis16e-fig-6-13-contemporary-bi-infrastructure.jpg]]
*Gambar 5.11: Infrastruktur Business Intelligence (BI) Kontemporer (Hadoop, Warehouse, Analytic)*

### 5.1 Karakteristik Big Data: Volume, Velocity, dan Variety (3V)
Data korporat modern tumbuh menjadi **Big Data**:
- **Volume:** Jumlah data yang sangat masif (petabyte hingga exabyte data).
- **Velocity:** Kecepatan lalu lintas data yang masuk secara real-time (jutaan stream feed per detik dari sensor IoT, GPS, dan feed media sosial).
- **Variety:** Keberagaman format data (terstruktur angka, semi-terstruktur JSON/XML, dan tak terstruktur video/audio/teks).
*(Sering ditambahkan Veracity / keakuratan dan Value / nilai bisnis)*.

### 5.2 Gudang Data (Data Warehouse) dan Data Mart
Untuk menganalisis data bisnis historis tanpa membebani sistem transaksi operasional (TPS), korporasi membangun:

```
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ TRANSAKSI (TPS) │       │ SITUS E-COMMERCE│       │ RIWAYAT KREDIT  │
└────────┬────────┘       └────────┬────────┘       └────────┬────────┘
         │                         │                         │
         └─────────────────┐       │       ┌─────────────────┘
                           ▼       ▼       ▼
                   ┌───────────────────────────────┐
                   │    PROSES EKSTRAKSI & ETL     │
                   │ (Extract, Transform, Cleanse) │
                   └───────────────┬───────────────┘
                                   │
                                   ▼
                   ┌───────────────────────────────┐
                   │        DATA WAREHOUSE         │
                   │ (Penyimpanan Historis Sentral)│
                   └───────┬───────────────┬───────┘
                           │               │
                 ┌─────────┘               └─────────┐
                 ▼                                   ▼
       ┌──────────────────┐                ┌──────────────────┐
       │    DATA MART     │                │    DATA MART     │
       │ (Divisi Penjualan│                │ (Divisi Keuangan │
       │   & Pemasaran)   │                │   & Akuntansi)   │
       └──────────────────┘                └──────────────────┘
```

- **Data Warehouse:** Basis data terpusat yang menyimpan data historis dan kumulatif dari seluruh departemen perusahaan yang telah distandarisasi, dibersihkan, dan dikonsolidasi semata-mata untuk tujuan analisis manajemen dan pelaporan. Bersifat hanya-baca (*read-only*).
- **Data Mart:** Subset terkecil dari data warehouse yang berfokus pada lini bisnis atau departemen tertentu yang terdesentralisasi (misal: data mart khusus untuk departemen pemasaran ritel).

### 5.3 Ekosistem Apache Hadoop (HDFS dan MapReduce)
- Framework perangkat lunak open-source yang dikelola Apache Software Foundation untuk menangani pemrosesan Big Data yang terdistribusi di ribuan server komoditas murah.
- **Dua Komponen Inti Hadoop:**
  1. **HDFS (*Hadoop Distributed File System*):** Memecah file data besar menjadi blok-blok kecil dan mendistribusikannya ke berbagai node server dalam kluster, lengkap dengan replikasi cadangan otomatis.
  2. **MapReduce:** Mesin perangkat lunak yang memecah instruksi kueri komputasi kompleks menjadi pekerjaan-pekerjaan kecil paralel yang dieksekusi langsung di masing-masing node data (*Map*), lalu menggabungkan hasilnya kembali (*Reduce*).

### 5.4 In-Memory Computing dan Platform Analitik
- **In-Memory Computing:** Mengeliminasi hambatan kecepatan akses disk lambat dengan memuat seluruh basis data secara langsung ke dalam memori utama komputer (RAM) (contoh: **SAP HANA**). Mampu mempercepat kalkulasi kueri analitik hingga 10.000 kali lebih cepat.
- **Platform Analitik (*Analytic Platforms*):** Perangkat keras dan perangkat lunak terintegrasi khusus (*pre-configured hardware-software appliances*) yang dirancang khusus untuk pemrosesan kueri analitik raksasa (contoh: IBM PureData System / Netezza, Oracle Exadata).

---

## 6. Alat Analitis: Menemukan Pola, Tren, dan Hubungan Tersembunyi

Setelah basis data terintegrasi dalam infrastruktur BI, para analis bisnis menggunakan berbagai alat analisis:

### 6.1 Online Analytical Processing (OLAP) dan Kubus Data

![[mis16e-fig-6-14-multidimensional-data-model-cube.jpg]]
*Gambar 5.12: Model Data Multidimensi OLAP Cube (Irisan Produk, Wilayah, Waktu)*

> [!info] Definisi OLAP
> **OLAP** memungkinkan pengguna menganalisis data multidimensi secara cepat dan interaktif dari berbagai perspektif.

- **Model Kubus Multidimensi (*The Multidimensional Cube*):**
  - Menggabungkan beberapa dimensi data (misal: Produk, Wilayah Geografis, dan Waktu).
  - *Operasi OLAP:*
    - **Slicing & Dicing:** Mengambil irisan data spesifik (misal: Penjualan produk laptop di Pulau Jawa pada Kuartal II).
    - **Drill-Down:** Menelusuri dari angka agregat ringkasan tinggi ke rincian transaksi detail terkecil (dari penjualan per tahun -> per bulan -> per hari).
    - **Roll-Up:** Menggabungkan rincian data ke level agregat yang lebih tinggi.

```
                  ┌──────────────────────┐
                 /                      /│
                /                      / │
               /                      /  │
              ┌──────────────────────┐   │  DIMENSI PRODUK
              │                      │   │  (Komputer, TV, HP)
              │   KUBUS DATA OLAP    │   │
  DIMENSI     │                      │   │
  WILAYAH     │  Menganalisis matriks│  /
  (Jabar,     │   Penjualan secara   │ /
   Jateng)    │     Multidimensi     │/
              └──────────────────────┘
                   DIMENSI WAKTU
                   (Q1, Q2, Q3, Q4)
```

### 6.2 Data Mining: Asosiasi, Sekuens, Klasifikasi, dan Kluster
> [!note] Data Mining vs OLAP
> Jika OLAP digerakkan oleh kueri pengguna (*user-driven*), **Data Mining** digerakkan oleh penemuan pola tersembunyi secara otomatis oleh algoritma matematika (*discovery-driven*).

#### Lima Tipe Pola yang Ditemukan oleh Data Mining:
1. **Asosiasi (*Associations / Market Basket Analysis*):** Peristiwa yang berkorelasi dalam satu kejadian transaksi tunggal (contoh: Jika konsumen membeli keripik kentang, ada probabilitas 65% mereka juga membeli minuman soda bersoda).
2. **Sekuens (*Sequences*):** Peristiwa yang terhubung dalam kurun waktu berurutan (contoh: Konsumen yang membeli rumah baru dalam 2 bulan ke depan memiliki probabilitas 70% membeli kulkas baru).
3. **Klasifikasi (*Classification*):** Menetapkan objek data ke dalam kelas-kelas kategori yang telah ditentukan berdasarkan pola atribut (contoh: Mengklasifikasikan nasabah kartu kredit ke dalam profil *"Risiko Gagal Bayar Tinggi"* vs *"Risiko Rendah"*).
4. **Pengelompokan (*Clustering*):** Mengelompokkan data yang memiliki kemiripan karakteristik ketika kelas atau kategori belum ditentukan sebelumnya (contoh: Menemukan segmen pelanggan baru yang tak terduga berdasarkan demografi dan jam belanja).
5. **Prakiraan (*Forecasting*):** Memprediksi nilai masa depan menggunakan deret waktu historis (contoh: Memprediksi volume penjualan mantel musim dingin tahun depan).

### 6.3 Text Mining, Analisis Sentimen, dan Web Mining
- **Text Mining:** Ekstraksi pengetahuan dari data teks tidak terstruktur (email, file PDF, transkrip call center).
  - *Sentiment Analysis (Opinion Mining):* Mendeteksi muatan opini positif, netral, atau negatif dari ulasan pelanggan di media sosial terhadap produk baru.
- **Web Mining:**
  - *Web Content Mining:* Mengekstrak teks dan gambar dari konten halaman web.
  - *Web Structure Mining:* Menganalisis topologi tautan (*hyperlinks*) antar halaman web (algoritma PageRank Google).
  - *Web Usage Mining:* Menganalisis log data klik (*clickstream data*) pengguna untuk mengoptimalkan navigasi situs e-commerce.

### 6.4 Arsitektur Integrasi Database dengan Web
Bagaimana pengguna internet mengakses data transaksi perusahaan dari smartphone atau peramban web:

![[mis16e-fig-6-15-linking-databases-to-web.jpg]]
*Gambar 5.13: Arsitektur Menghubungkan Basis Data Korporat ke Jaringan Web Publik*

```
┌───────────┐         ┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│  CLIENT   ├────────►│  WEB SERVER  ├────────►│  APPLICATION │├────────►│     DBMS     │
│ (Browser  │◄────────┤(Menyajikan   │◄────────┤    SERVER    ││◄────────┤(Oracle/MySQL)│
│ Pengguna) │         │ Halaman HTML)│         │(Skrip PHP,   ││         └──────┬───────┘
└───────────┘         └──────────────┘         │ Python, Node)││                │
                                               └──────────────┘                 ▼
                                                                        ┌──────────────┐
                                                                        │  BASIS DATA  │
                                                                        │  PERUSAHAAN  │
                                                                        └──────────────┘
```

---

## 7. Tata Kelola Data dan Menjamin Kualitas Data

### 7.1 Kebijakan Informasi dan Data Governance
- **Kebijakan Informasi (*Information Policy*):** Aturan formal organisasi yang mengatur bagaimana data dibagikan, disebarkan, distandarisasi, dan siapa saja yang memiliki wewenang mengakses data tersebut.
- **Administrasi Data (*Data Administration*):** Penetapan kebijakan, perencanaan data, dan pengembangan kamus data korporat.
- **Administrasi Basis Data (*Database Administration - DBA*):** Kelompok teknis yang bertanggung jawab atas desain fisik database, alokasi memori disk, pemantauan performa kueri, dan pemulihan cadangan data (*backup/restore*).
- **Tata Kelola Data (*Data Governance*):** Kebijakan menyeluruh yang memastikan data organisasi akurat, konsisten, aman, dan patuh terhadap regulasi hukum eksternal.

### 7.2 Audit Kualitas Data dan Pembersihan Data (Data Cleansing)
- **Audit Kualitas Data (*Data Quality Audit*):** Survei terstruktur atas keakuratan dan kelengkapan data dalam sistem informasi (menguji sampel data transaksi).
- **Pembersihan Data (*Data Cleansing / Scrubbing*):** Aktivitas mendeteksi dan mengoreksi data yang salah, tidak lengkap, diformat secara keliru, atau terduplikasi sebelum dimasukkan ke dalam data warehouse.

---

## 8. Ringkasan & Latihan Ujian (Cheatsheet)

> [!tip] Rangkuman Cepat untuk Ujian (Exam Quick Sheet)
> 1. **Hirarki Data:** Bit -> Byte -> Field -> Record -> File -> Database.
> 2. **Masalah File Tradisional:** Redundansi data, inkonsistensi data, dependensi program-data, ketiadaan fleksibilitas, keamanan lemah, kurangnya sharing data.
> 3. **Tiga Operasi RDBMS:**
>    - SELECT: Memilih baris (*tuples*) berdasarkan kriteria.
>    - PROJECT: Memilih kolom (*fields*) tertentu.
>    - JOIN: Menggabungkan baris dari dua tabel berdasarkan kunci bersama.
> 4. **Normalisasi:** Proses dekomposisi tabel bertahap (1NF, 2NF, 3NF) guna membasmi redundansi dan anomali (*update, insertion, deletion*).
> 5. **NoSQL:** Basis data non-relasional tanpa skema kaku, dioptimasi untuk komputasi skala masif horizontal dan data tak terstruktur (MongoDB, Redis, Cassandra).
> 6. **3V Big Data:** Volume (skala besar), Velocity (kecepatan masuk tinggi), Variety (keberagaman format terstruktur/tak terstruktur).
> 7. **Hadoop:** Framework open-source untuk komputasi terdistribusi data besar; HDFS untuk penyimpanan terdistribusi dan MapReduce untuk komputasi paralel.
> 8. **Data Warehouse vs Data Mart:** Data Warehouse adalah repositori historis sentral korporat; Data Mart adalah subset terdesentralisasi khusus divisi fungsional tertentu.
> 9. **Data Mining:** Penemuan pola tersembunyi (*associations, sequences, classifications, clusters, forecasting*). OLAP menyajikan analisis kubus multidimensi (*slice, dice, drill-down*).
