---
title: "LGW2E Chapter 5: Peer-to-Peer Protocols and Data Link Layer"
tags:
  - computer-networks
  - data-link-layer
  - p2p-protocols
  - arq
  - flow-control
  - tcp
  - hdlc
  - ppp
  - queueing-theory
  - obsidian-notes
course: Computer Networks / Komunikasi Data
textbook: "Communication Networks: Fundamental Concepts and Key Architectures (Leon-Garcia & Widjaja, 2nd Edition)"
date: 2026-09-21
type: study-note
---

# Chapter 5: Peer-to-Peer Protocols and Data Link Layer

> [!abstract] Ringkasan Eksekutif
> Catatan komprehensif ini merangkum seluruh materi presentasi **Chapter 5 (Leon-Garcia & Widjaja)** yang mencakup 148 slide ke dalam dua pilar utama:
> 1. **Part I: Peer-to-Peer Protocols** — Menelaah model layanan, mekanisme keandalan transfer data melalui **ARQ (Stop-and-Wait, Go-Back-N, Selective Repeat)** beserta analisis efisiensi matematisnya, kendali aliran (*flow control*), rekonstruksi waktu (*timing recovery*), serta implementasi end-to-end pada **TCP**.
> 2. **Part II: Data Link Controls** — Membahas teknik pembingkaian (*framing: bit-stuffing & byte-stuffing*), dua protokol data link fundamental (**PPP** dan **HDLC**), serta teknik efisiensi penggunaan link bersama melalui **Statistical Multiplexing** dan teori antrean ($M/M/1$ dan $M/M/1/K$).

---

## Daftar Isi (Table of Contents)
- [[#1. Peer-to-Peer Protocols & Service Models]]
  - [[#1.1 Konsep Komunikasi Peer-to-Peer]]
  - [[#1.2 Model Layanan: Connection-Oriented vs Connectionless]]
  - [[#1.3 Karakteristik & Dimensi Layanan Lainnya]]
  - [[#1.4 End-to-End vs Hop-by-Hop: Trade-off Desain]]
- [[#2. ARQ Protocols & Reliable Data Transfer]]
  - [[#2.1 Komponen Dasar ARQ]]
  - [[#2.2 Urgensi Sequence Number]]
  - [[#2.3 Stop-and-Wait ARQ]]
  - [[#2.4 Analisis Efisiensi Matematis Stop-and-Wait]]
  - [[#2.5 Go-Back-N (GBN) ARQ]]
  - [[#2.6 Mengapa Batas Jendela GBN adalah W_s le 2^m - 1]]
  - [[#2.7 Analisis Efisiensi Matematis Go-Back-N]]
  - [[#2.8 Selective Repeat (SR) ARQ]]
  - [[#2.9 Aturan Ukuran Jendela Selective Repeat: W_s + W_r le 2^m]]
  - [[#2.10 Analisis Efisiensi Matematis Selective Repeat]]
  - [[#2.11 Perbandingan Komprehensif Protokol ARQ]]
- [[#3. Flow Control (Kontrol Aliran)]]
  - [[#3.1 Masalah Buffer Overflow]]
  - [[#3.2 Open-Loop vs Closed-Loop Flow Control]]
  - [[#3.3 Sliding Window Flow Control]]
  - [[#3.4 Coupling vs Decoupling Error & Flow Control]]
- [[#4. Timing Recovery & Playout Buffer]]
  - [[#4.1 Karakteristik Trafik Real-Time & Network Jitter]]
  - [[#4.2 Mekanisme Playout Buffer]]
  - [[#4.3 Clock Drift & Clock Recovery]]
  - [[#4.4 Real-Time Transport Protocol (RTP)]]
- [[#5. Layanan TCP Reliable Stream & Flow Control]]
  - [[#5.1 Karakteristik Layanan TCP]]
  - [[#5.2 TCP ARQ & Fast Retransmit]]
  - [[#5.3 TCP Connection Lifecycle (Handshake & Teardown)]]
  - [[#5.4 TCP Sliding Window Flow Control & Advertised Window]]
  - [[#5.5 TCP Retransmission Timeout (RTO) & Estimasi RTT]]
- [[#6. Framing pada Data Link Layer]]
  - [[#6.1 Urgensi Framing]]
  - [[#6.2 Character-Oriented Framing & Byte Stuffing]]
  - [[#6.3 Bit-Oriented Framing & Bit Stuffing (HDLC)]]
  - [[#6.4 Generic Framing Procedure (GFP)]]
- [[#7. Point-to-Point Protocol (PPP)]]
  - [[#7.1 Ruang Lingkup & Aplikasi PPP]]
  - [[#7.2 Struktur Frame PPP]]
  - [[#7.3 Byte Stuffing pada PPP]]
  - [[#7.4 Tiga Komponen Inti PPP (LCP, Auth, NCP)]]
  - [[#7.5 Siklus Hidup Koneksi PPP (Link Transition Phases)]]
- [[#8. High-Level Data Link Control (HDLC)]]
  - [[#8.1 Tipe Stasiun & Konfigurasi Link]]
  - [[#8.2 Mode Transfer Data (NRM, ABM, ARM)]]
  - [[#8.3 Format Frame & Field Kontrol HDLC]]
  - [[#8.4 Klasifikasi Frame: I-Frames, S-Frames, U-Frames]]
  - [[#8.5 Contoh Operasi HDLC: NRM Polling vs ABM Symmetrical]]
- [[#9. Link Sharing Menggunakan Statistical Multiplexing]]
  - [[#9.1 Konsep Dasar & Tradeoff Statistical Multiplexing]]
  - [[#9.2 Pemodelan Multiplexer & Karakteristik Trafik Internet]]
  - [[#9.3 Analisis Model Antrean M/M/1/K (Buffer Terbatas)]]
  - [[#9.4 Analisis Antrean M/M/1 & M/D/1 (Buffer Tak Terbatas) serta Efek Skala]]
  - [[#9.5 Header Overhead & Goodput]]
  - [[#9.6 Burst Multiplexing & Speech Interpolation (DSI)]]
  - [[#9.7 Packet Speech Multiplexing & Packet Voice Switching]]
- [[#10. Ringkasan Rumus & Cheatsheet Ujian]]

---

# PART I: PEER-TO-PEER PROTOCOLS

## 1. Peer-to-Peer Protocols & Service Models

### 1.1 Konsep Komunikasi Peer-to-Peer
Protokol peer-to-peer mengatur pertukaran data antara dua entitas logis yang berada pada lapisan (*layer*) yang sama di dua mesin/perangkat yang berbeda.

![[lgw2e-p2p-sdu-pdu.png]]

- **Service Data Unit (SDU):** Unit data informasi yang diserahkan oleh lapisan atas ($n+1$) kepada lapisan bawah ($n$) untuk ditransmisikan.
- **Protocol Data Unit (PDU):** Unit data terformat yang dipertukarkan antar-peer lapisan $n$ melalui kanal fisik/jaringan. PDU dibentuk dari SDU yang dibungkus dengan *Protocol Control Information* (PCI) berupa header dan trailer.
- **Relasi Enkapsulasi:**
  $$\text{PDU Layer } n = \text{Header Layer } n + \text{SDU Layer } n + \text{Trailer Layer } n$$

```
+-------------------------------------------------------------+
| Layer-(n+1) Peer Process           Layer-(n+1) Peer Process |
|          |                                    ^             |
|   SDU    | Passes SDU                  Passes |   SDU       |
|          v                                    |             |
+-------------------------------------------------------------+
| Layer-n Peer Process <=== PDU Transfer ===> Layer-n Peer    |
| (Adds Header & Trailer)                    (Strips H & T)   |
+-------------------------------------------------------------+
```

### 1.2 Model Layanan: Connection-Oriented vs Connectionless
Layanan yang disediakan layer-$n$ kepada layer-$(n+1)$ terbagi menjadi dua paradigma fundamental:

| Parameter | Connection-Oriented Transfer | Connectionless Transfer |
| :--- | :--- | :--- |
| **Fase Operasi** | 3 Fase: *Establishment*, *Data Transfer*, *Release* | 1 Fase: *Data Transfer* langsung |
| **Alokasi Sumber Daya** | Negosiasi parameter awal (Seq num, buffer) | Tidak ada alokasi sumber daya di muka |
| **Penyertaan Alamat** | Alamat lengkap hanya pada fase handshake awal; setelahnya menggunakan *Connection ID* / *Port* | Setiap paket harus membawa alamat asal & tujuan lengkap |
| **Overhead & Kecepatan** | Ada latensi koneksi di awal, efisien saat streaming panjang | Tanpa handshake, cepat untuk transaksi pendek |
| **Contoh Protokol** | **TCP**, **PPP**, **HDLC (ABM)**, X.25 | **UDP**, **IP**, Ethernet |

### 1.3 Karakteristik & Dimensi Layanan Lainnya
1. **Message Size & Structure:** Layanan dapat berupa *byte stream* kontinu (tanpa batasan pesan, misal TCP) atau *message stream* berbasis blok (misal email/voice mail).
   - **Segmentation & Reassembly:** Memecah pesan besar dari layer atas menjadi blok-blok kecil sesuai kapasitas layer bawah.
   - **Blocking & Unblocking:** Menggabungkan pesan-pesan kecil menjadi satu blok besar sebelum dikirim untuk menghemat overhead header.
2. **Reliability & Sequencing:**
   - *Reliability*: Menjamin seluruh data sampai di tujuan bebas error, tanpa kehilangan (*loss*), dan tanpa duplikasi.
   - *Sequencing*: Menjamin data diserahkan ke lapisan atas dalam urutan yang tepat sesuai urutan pengiriman.
3. **Pacing & Flow Control:** Mencegah pengirim membanjiri penerima melampaui kapasitas buffer yang tersedia (*backpressure mechanism*).
4. **Timing Control:** Mengendalikan keterlambatan (*delay*) dan variasi keterlambatan (*jitter*) untuk aplikasi multimedia real-time.
5. **Multiplexing:** Memungkinkan banyak aplikasi/pengguna layer-$(n+1)$ menggunakan bersama satu layanan layer-$n$ dengan menyematkan tag/identifier (misal Port Number pada UDP/TCP).
6. **Security (Kerahasiaan, Integritas, Autentikasi):** Menjamin pesan tidak dapat diintip (*privacy*), tidak diubah (*integrity*), dan identitas pengirim/penerima sah (*authentication*).

### 1.4 End-to-End vs Hop-by-Hop: Trade-off Desain

![[lgw2e-hop-by-hop-error-control.png]]
*Gambar: Error control dilakukan di setiap link lompatan (Hop-by-Hop).*

![[lgw2e-end-to-end-error-control.png]]
*Gambar: Error control dilakukan secara End-to-End di lapisan transport.*

```
HOP-BY-HOP APPROACH (Data Link Layer):
[Host A] <--- Error Control ---> [Router 1] <--- Error Control ---> [Router 2] <--- Error Control ---> [Host B]
- Setiap simpul harus memvalidasi CRC, mengelola buffer, dan melakukan retransmisi lokal.

END-TO-END APPROACH (Transport Layer):
[Host A] <======================== Error Control (TCP) ========================> [Host B]
    |                                                                              ^
    +---> [Router 1] -------------> [Router 2] -------------> [Router 3] ----------+
          (Hanya bertugas forwarding paket secepat mungkin, tanpa retransmisi)
```

> [!important] Prinsip End-to-End (Saltzer, Reed, & Clark, 1984)
> - **Hop-by-Hop:** Sangat efektif jika link fisik memiliki tingkat kesalahan sangat tinggi (misal: kanal nirkabel/radio, kabel tembaga tua). Retransmisi lokal mencegah paket rusak melintasi seluruh jaringan.
> - **End-to-End:** Menjadi pilihan utama pada jaringan modern (seperti Internet berbasis fiber optic). Beban pemrosesan dan penyimpanan buffer di router tengah dihilangkan sehingga throughput jaringan meningkat tajam. Lapisan transport di kedua host ujung menangani keandalan akhir.

---

## 2. ARQ Protocols & Reliable Data Transfer

### 2.1 Komponen Dasar ARQ
**Automatic Repeat reQuest (ARQ)** adalah mekanisme standar untuk mengubah kanal transmisi fisik yang tidak andal (*error-prone*) menjadi koneksi logis yang andal (*reliable*). ARQ bertumpu pada 4 pilar:
1. **Error Detection Code:** Menambahkan bit paritas redundan (CRC - Cyclic Redundancy Check) pada setiap frame.
2. **Acknowledgment (ACK / NAK):** Umpan balik dari penerima ke pengirim. ACK menandakan frame diterima sukses; NAK menandakan frame rusak.
3. **Retransmission Timer (Timeout):** Pengirim menyalakan timer setelah mengirim frame. Jika ACK tidak diterima hingga batas waktu tertentu, pengirim mengasumsikan frame atau ACK hilang, lalu melakukan retransmisi.
4. **Sequence Numbering:** Penomoran setiap frame dan ACK untuk membedakan frame baru dari frame duplikat.

---

### 2.2 Urgensi Sequence Number

![[lgw2e-stop-and-wait-states.png]]

Tanpa nomor urut (*sequence number*), protokol akan mengalami kegagalan logika (*fatal failure*):
- **Kasus 1 (Frame Hilang/Rusak):** Pengirim mengirim frame 0 $\to$ frame hilang $\to$ timeout $\to$ retransmisi frame 0 $\to$ penerima menerima. (Ini bekerja baik).
- **Kasus 2 (ACK Hilang/Delay):** Pengirim mengirim frame 0 $\to$ penerima menerima frame 0 dan membalas ACK $\to$ **ACK hilang di jaringan** $\to$ pengirim timeout $\to$ pengirim mengirim ulang frame 0.
  - *Jika tidak ada sequence number:* Penerima tidak tahu bahwa frame yang datang adalah duplikat dari frame sebelumnya! Penerima akan menyerahkan frame tersebut ke layer atas untuk kedua kalinya (**terjadi duplikasi data**).
- **Kasus 3 (Delayed Duplicate ACK):** ACK lama yang tertunda tiba setelah frame berikutnya dikirim, menyebabkan pengirim salah mengira bahwa frame baru telah diakui padahal frame baru tersebut hilang.

> [!tip] Solusi: Alternating Bit Protocol
> Untuk Stop-and-Wait, nomor urut **1-bit (modulo 2: nilai 0 dan 1)** sudah cukup membongkar ambiguitas antara frame baru dan frame duplikat!

---

### 2.3 Stop-and-Wait ARQ

![[lgw2e-stop-and-wait-fsm.png]]
*Finite State Machine (FSM) Pengirim dan Penerima pada Stop-and-Wait ARQ.*

![[lgw2e-stop-and-wait-timing.png]]
*Diagram Waktu Transmisi Stop-and-Wait ARQ.*

```
SENDER                                                   RECEIVER
  |                                                         |
  |--- Frame 0 -------------------------------------------->| (Diterima, simpan 0)
  |<-- ACK 0 -----------------------------------------------| (Kirim ACK 0)
  |                                                         |
  |--- Frame 1 ----------------------X                      | (Frame hilang di jalan)
  |    [Timeout Timer Expired!]                             |
  |--- Frame 1 (Retransmit) ------------------------------->| (Diterima, simpan 1)
  |<-- ACK 1 -----------------------------------------------| (Kirim ACK 1)
  |                                                         |
  |--- Frame 0 -------------------------------------------->| (Diterima, simpan 0)
  |<-- ACK 0 -----------------------X                       | (ACK 0 hilang!)
  |    [Timeout Timer Expired!]                             |
  |--- Frame 0 (Retransmit) ------------------------------->| (Duplikat! Buang data,
  |<-- ACK 0 -----------------------------------------------|  tetap kirim ACK 0)
```

**Mekanisme Kerja:**
1. Pengirim mengirim 1 frame dengan nomor $S_{last}$, menyalakan timer, dan **berhenti (wait)** menunggu ACK.
2. Penerima menunggu frame bernomor $R_{next}$. Jika frame cocok dan bebas error CRC, kirim ACK $R_{next}$, naikkan $R_{next} = (R_{next} + 1) \pmod 2$.
3. Jika frame tiba dengan nomor salah (duplikat), penerima membuang payload tetapi **wajib mengirim ulang ACK** untuk frame tersebut agar pengirim tidak macet.

---

### 2.4 Analisis Efisiensi Matematis Stop-and-Wait

#### A. Definisi Parameter Fisik:
- $n_f$ = Total bit dalam satu frame (termasuk header, payload, trailer).
- $n_o$ = Bit overhead dalam frame ($n_f - n_o$ = bit payload data bersih).
- $n_a$ = Bit dalam frame ACK.
- $R$ = Laju transmisi kanal (*bit rate*, bps).
- $d$ = Jarak fisik antar dua simpul (meter).
- $c$ = Kecepatan propagasi gelombang elektromagnetik pada media ($\approx 2 \times 10^8 \text{ m/s}$ pada tembaga/kaca).
- $t_{prop} = \frac{d}{c}$ = Waktu propagasi sinyal melintasi kabel.
- $t_f = \frac{n_f}{R}$ = Waktu transmisi frame.
- $t_a = \frac{n_a}{R}$ = Waktu transmisi ACK.
- $t_{proc}$ = Waktu pemrosesan di stasiun pengirim dan penerima.

#### B. Total Waktu Satu Siklus Transmisi Bebas Error ($t_0$):
$$t_0 = t_f + 2 t_{prop} + 2 t_{proc} + t_a$$

Jika $t_{proc}$ dan $t_a$ sangat kecil dibandingkan $t_f$ dan $t_{prop}$:
$$t_0 \approx t_f + 2 t_{prop}$$

#### C. Parameter Normalisasi Bandwidth-Delay Product ($a$):
Didefinisikan rasio waktu propagasi terhadap waktu transmisi:
$$a = \frac{t_{prop}}{t_f} = \frac{d / c}{n_f / R} = \frac{R \cdot d}{c \cdot n_f}$$

#### D. Efisiensi pada Kanal Bebas Error ($\eta_0$):
$$\eta_0 = \frac{\text{Waktu transmisi data bersih}}{\text{Total waktu siklus}} = \frac{\frac{n_f - n_o}{R}}{t_0} = \frac{n_f - n_o}{R \cdot (t_f + 2 t_{prop} + 2 t_{proc} + t_a)}$$

Mengabaikan overhead kecil ($n_o \ll n_f, t_a \ll t_f, t_{proc} \approx 0$):
$$\eta_0 \approx \frac{t_f}{t_f + 2 t_{prop}} = \frac{1}{1 + 2 \frac{t_{prop}}{t_f}} = \frac{1}{1 + 2a}$$

> [!warning] Efek Bandwidth-Delay Product Tinggi
> - Jika $a \ll 1$ (kabel pendek, laju rendah, frame besar): $\eta_0 \approx 100\%$.
> - Jika $a \gg 1$ (koneksi satelit, link antarbenua 10 Gbps): nilai $2a$ sangat besar sehingga $\eta_0 \to 0$. Pipa transmisi sebagian besar waktu kosong karena pengirim menganggur menunggu ACK!

#### E. Efisiensi pada Kanal dengan Error Transmisi:
Misalkan $p$ adalah *Bit Error Rate* (BER). Probabilitas frame rusak ($P_f$):
$$P_f = 1 - (1 - p)^{n_f}$$

Jumlah transmisi rata-rata $E[N]$ hingga 1 frame berhasil diterima mengikuti distribusi geometrik:
$$E[N] = \sum_{i=1}^\infty i \cdot (1 - P_f) P_f^{i-1} = \frac{1}{1 - P_f}$$

Jika waktu timeout $t_{out} \approx t_0$, maka waktu rata-rata pengiriman satu frame sukses adalah:
$$E[t_{total}] = \frac{t_0}{1 - P_f}$$

Maka efisiensi nyata Stop-and-Wait dengan error ($\eta_{SW}$) adalah:
$$\eta_{SW} = \frac{n_f - n_o}{R \cdot E[t_{total}]} = (1 - P_f) \eta_0 = (1 - P_f) \cdot \frac{1 - \frac{n_o}{n_f}}{1 + \frac{n_a}{n_f} + \frac{2(t_{prop} + t_{proc})R}{n_f}}$$

---

### 2.5 Go-Back-N (GBN) ARQ
Untuk mengatasi kelemahan Stop-and-Wait di mana pengirim banyak menganggur, **Go-Back-N** menerapkan teknik **pipelining**. Pengirim diizinkan mentransmisikan sejumlah frame tanpa menunggu ACK, dibatasi oleh ukuran jendela transmisi (*Send Window*, $W_s$).

![[lgw2e-gobackn-sliding-window.png]]
*Konsep Sliding Window pada Pengirim dan Penerima Go-Back-N.*

![[lgw2e-gobackn-timeline.png]]
*Timeline Transmisi Pipelined Go-Back-N.*

![[lgw2e-gobackn-recovery-timeline.png]]
*Timeline Penanganan Error pada Go-Back-N.*

```
Transmitter Window (Ws = 4):
[ 0   1   2   3 ]  4   5   6   7   ...
  ^               ^
S_last          S_last + Ws - 1

Frame 0 terkirim, ACK 0 diterima -> Jendela bergeser ke kanan:
  0 [ 1   2   3   4 ]  5   6   7   ...
```

**Karakteristik Utama Go-Back-N:**
- **Jendela Penerima ($W_r = 1$):** Penerima hanya memiliki buffer sebesar 1 frame dan hanya menerima frame yang tiba persis sesuai urutan ($R_{next}$).
- **Out-of-Order Frames Discarded:** Setiap frame yang tiba tidak berurutan (meskipun bebas error CRC) **wajib dibuang**.
- **Cumulative Acknowledgment:** ACK bernilai $R_{next}$ menyatakan bahwa seluruh frame hingga nomor $R_{next} - 1$ telah diterima sukses.
- **Go-Back Penalty:** Jika frame $k$ hilang atau rusak, pengirim yang kehabisan batas window akan mengalami timeout untuk frame $k$, lalu dipaksa melompat mundur (**Go Back**) dan mengirim ulang frame $k$ beserta seluruh frame setelahnya ($k+1, k+2, \dots$) yang telah sempat dikirim sebelumnya.

---

### 2.6 Mengapa Batas Jendela GBN adalah $W_s \le 2^m - 1$?

Misalkan nomor urut menggunakan field $m$-bit, sehingga variasi nomor urut adalah $2^m$ (yaitu $0, 1, \dots, 2^m - 1$).
- **Jika kita salah menetapkan $W_s = 2^m$:**
  - Misal $m = 2 \implies 2^2 = 4$ nomor urut ($0, 1, 2, 3$). Set $W_s = 4$.
  - Pengirim mengirim frame 0, 1, 2, 3.
  - Penerima menerima keempat frame dengan sempurna dan menaikkan $R_{next}$ menjadi 0 (siklus nomor urut berikutnya). Penerima mengirim ACK untuk frame 3 ($R_{next} = 0$).
  - **Skenario Bencana:** Seluruh ACK hilang di jaringan.
  - Pengirim mengalami timeout pada frame 0, lalu mengirim ulang frame 0 dari siklus lama.
  - Penerima yang sedang menunggu frame 0 (dari siklus baru) melihat kedatangan frame 0!
  - **Fatal:** Penerima mengira ini adalah data baru siklus berikutnya, padahal ini adalah duplikat dari data lama!
- **Maka Syarat Mutlak Go-Back-N:**
  $$W_s \le 2^m - 1$$
  Untuk $m=3$ (nomor 0–7), ukuran window maksimal adalah $W_s = 7$.

---

### 2.7 Analisis Efisiensi Matematis Go-Back-N

#### A. Kondisi Bebas Error:
Agar pengirim dapat membanjiri kanal tanpa henti (*keep the pipe full*), kapasitas jendela harus cukup menampung transmisi selama satu *Round-Trip Time* (RTT):
$$W_s \ge 1 + 2a$$
- Jika $W_s \ge 1 + 2a$: Pengirim tidak pernah menunggu, efisiensi maksimal:
  $$\eta_0 = 1 - \frac{n_o}{n_f}$$
- Jika $W_s < 1 + 2a$: Pengirim terpaksa berhenti menunggu ACK setelah mengirim $W_s$ frame:
  $$\eta_0 = \frac{W_s \cdot \left(1 - \frac{n_o}{n_f}\right)}{1 + 2a}$$

#### B. Kondisi dengan Error:
Setiap kali 1 frame mengalami error, frame tersebut beserta $W_s - 1$ frame setelahnya terbuang sia-sia dan harus ditransmisikan ulang.
Waktu total yang dibutuhkan per frame sukses:
$$E[t_{total}] = t_f + (W_s - 1) t_f \cdot \frac{P_f}{1 - P_f} = t_f \cdot \frac{1 + (W_s - 1) P_f}{1 - P_f}$$

Maka efisiensi nyata Go-Back-N ($\eta_{GBN}$) ketika $W_s \ge 1 + 2a$ adalah:
$$\eta_{GBN} = \frac{1 - P_f}{1 + (W_s - 1) P_f} \cdot \left(1 - \frac{n_o}{n_f}\right)$$

> [!danger] Kerentanan Go-Back-N
> Jika nilai $W_s$ besar (pada link bandwidth-delay product tinggi) dan saluran mengalami sedikit kenaikan probabilitas error ($P_f$), efisiensi Go-Back-N langsung anjlok drastis karena hukuman retransmisi $W_s$ frame sekaligus!

---

### 2.8 Selective Repeat (SR) ARQ

![[lgw2e-selective-repeat-window.png]]
*Jendela Pengirim dan Jendela Penerima pada Selective Repeat ARQ.*

![[lgw2e-selective-repeat-recovery.png]]
*Timeline Pemulihan Error pada Selective Repeat: Hanya Frame yang Rusak yang Ditransmisikan Ulang.*

**Karakteristik Utama Selective Repeat:**
- **Jendela Penerima ($W_r > 1$):** Penerima memiliki buffer untuk menampung frame-frame yang tiba di luar urutan (*out-of-order*), asalkan berada di dalam rentang jendela penerima dan bebas dari error CRC.
- **Retransmisi Selektif:** Pengirim hanya mentransmisikan ulang frame spesifik yang rusak atau hilang (dipicu oleh timeout individual atau NAK/SREJ). Frame tetangganya yang telah berada di buffer penerima tidak perlu dikirim ulang!

```
SENDER                                                    RECEIVER (Buffer Wr > 1)
  |--- Frame 0 -------------------------------------------->| (Diterima, simpan 0)
  |--- Frame 1 ----------------------X                      | (Rusak/Hilang)
  |--- Frame 2 -------------------------------------------->| (Out-of-order! Disimpan di buffer)
  |--- Frame 3 -------------------------------------------->| (Out-of-order! Disimpan di buffer)
  |<-- SREJ 1 ----------------------------------------------| (Kirim Negative ACK minta Frame 1)
  |                                                         |
  |--- Frame 1 (Retransmit Saja) -------------------------->| (Frame 1 tiba!)
  |                                                         | [Reassembly buffer: 0, 1, 2, 3 urut!]
  |                                                         | (Serahkan 0,1,2,3 ke layer atas)
```

---

### 2.9 Aturan Ukuran Jendela Selective Repeat: $W_s + W_r \le 2^m$

![[lgw2e-selective-repeat-window-condition.png]]

Untuk mencegah tumpang tindih antara rentang nomor urut frame lama yang mungkin dikirim ulang dan rentang frame baru yang diharapkan oleh penerima ketika seluruh ACK hilang:
$$W_s + W_r \le 2^m$$

Pada praktiknya, jendela dibagi seimbang antara pengirim dan penerima:
$$W_s = W_r = 2^{m-1} = \frac{2^m}{2}$$

> [!example] Contoh Numerik
> Jika menggunakan nomor urut 3-bit ($m = 3 \implies 2^3 = 8$ nilai nomor urut: $0 \dots 7$):
> - Pada Go-Back-N: $W_s = 2^3 - 1 = 7, W_r = 1$.
> - Pada Selective Repeat: $W_s = 2^{3-1} = 4, W_r = 4$.

---

### 2.10 Analisis Efisiensi Matematis Selective Repeat

Karena hanya frame yang mengalami error yang dikirim ulang, tidak ada pemborosan transmisi frame yang valid. Jumlah rata-rata transmisi per frame semata-mata adalah:
$$E[N] = \frac{1}{1 - P_f}$$

Maka efisiensi nyata Selective Repeat ($\eta_{SR}$) ketika $W_s \ge 1 + 2a$ adalah:
$$\eta_{SR} = (1 - P_f) \cdot \eta_0 = (1 - P_f) \cdot \left(1 - \frac{n_o}{n_f}\right)$$

Perhatikan bahwa efisiensi Selective Repeat **sama sekali tidak terdegradasi oleh ukuran jendela $W_s$** saat terjadi error, menjadikannya protokol yang sangat superior pada kanal berkecepatan tinggi dengan latensi panjang.

---

### 2.11 Perbandingan Komprehensif Protokol ARQ

![[lgw2e-arq-efficiency-comparison.png]]
*Perbandingan Efisiensi ARQ: Stop-and-Wait vs Go-Back-N vs Selective Repeat.*

| Aspek Evaluasi | Stop-and-Wait ARQ | Go-Back-N ARQ | Selective Repeat ARQ |
| :--- | :--- | :--- | :--- |
| **Ukuran Window Pengirim ($W_s$)** | $W_s = 1$ | $W_s \le 2^m - 1$ | $W_s \le 2^{m-1}$ |
| **Ukuran Window Penerima ($W_r$)** | $W_r = 1$ | $W_r = 1$ | $W_r \le 2^{m-1}$ |
| **Kebutuhan Buffer Penerima** | 1 frame | 1 frame | $W_r$ frame (butuh reassembly & sorting) |
| **Penanganan Frame Out-of-Order** | Ditolak / Dibuang | Ditolak / Dibuang | Disimpan dalam buffer |
| **Beban Retransmisi saat Error** | 1 frame | $W_s$ frame (seluruh jendela pipa) | 1 frame (hanya frame rusak) |
| **Efisiensi Bebas Error ($\eta_0$)** | $\approx \frac{1}{1 + 2a}$ | $\approx 1$ (jika $W_s \ge 1+2a$) | $\approx 1$ (jika $W_s \ge 1+2a$) |
| **Efisiensi Kanal Ber-Error ($\eta$)** | $(1 - P_f) \eta_0$ | $\frac{1 - P_f}{1 + (W_s - 1)P_f} \eta_0$ | $(1 - P_f) \eta_0$ |
| **Kompleksitas Logika & Memori** | Sangat Rendah | Rendah (hanya 1 pointer di Rx) | Tinggi (butuh timer per-frame & buffer) |
| **Penerapan Nyata** | Xmodem, TFTP, USB 1.1 | HDLC, SDLC, Radio Link Lama | **TCP**, Wi-Fi (802.11 Block ACK) |

---

# 3. Flow Control (Kontrol Aliran)

### 3.1 Masalah Buffer Overflow
Jika laju transmisi data pengirim melampaui kemampuan penerima dalam mengosongkan antrean buffernya (akibat pemrosesan lambat oleh aplikasi atas), maka buffer penerima akan penuh (*overflow*) dan frame-frame yang baru tiba akan dibuang (*dropped*).

### 3.2 Open-Loop vs Closed-Loop Flow Control
1. **Open-Loop Flow Control:** Tidak ada sinyal balik real-time dari penerima. Pengirim membatasi transmisi berdasarkan kontrak laju (*rate policing*) atau reservasi bandwidth di awal.
2. **Closed-Loop Flow Control:** Menggunakan mekanisme umpan balik aktif (*feedback*):
   - **Stop-and-Wait / XON-XOFF:** Penerima mengirim karakter kontrol `XOFF` untuk memerintahkan pengirim berhenti, dan `XON` saat buffer sudah lega.
   - **Window-Based Flow Control:** Penerima secara dinamis mengumumkan kapasitas buffer kosong yang tersedia (*dynamic credit*).

### 3.3 Sliding Window Flow Control
Dalam kendali aliran berbasis jendela geser:
- Batas jendela pengirim $W_s$ diikat langsung dengan jumlah slot buffer bebas di penerima.
- Setiap kali data diambil oleh aplikasi, penerima mengirimkan update jendela baru ke pengirim (*window update*).

### 3.4 Coupling vs Decoupling Error & Flow Control
- **Coupled (Tergabung):** Pada protokol lama seperti HDLC, nomor ACK $N(R)$ dan frame $RNR$ digunakan sekaligus untuk mengakui penerimaan frame (error control) dan membatasi kuota pengiriman (flow control). Hal ini kaku karena masalah buffer dapat menahan ACK.
- **Decoupled (Terpisah):** Pada protokol modern seperti **TCP**, mekanisme error control (Sequence/ACK number) dan flow control (field *Advertised Window* $W_A$) dipisahkan secara independen di dalam header.

---

# 4. Timing Recovery & Playout Buffer

### 4.1 Karakteristik Trafik Real-Time & Network Jitter
Pada aplikasi multimedia interaktif (seperti Voice over IP dan Video Conferencing):
- Sinyal suara/video disampel secara periodik di pengirim pada interval konstan $T$ (misal tiap 20 ms).
- Ketika paket melintasi antrean router dan link switched di jaringan IP, setiap paket mengalami delay acak yang bervariasi. Fenomena variasi waktu kedatangan paket ini disebut **Network Jitter**.

```
PENGIRIM (T konstan):
[Paket 1] ---- 20ms ---- [Paket 2] ---- 20ms ---- [Paket 3] ---- 20ms ---- [Paket 4]
    |                        |                        |                        |
====|========================|========================|========================|==== JARINGAN IP (Delay Acak)
    v                        v                        v                        v
PENERIMA (Kedatangan Tidak Teratur / Jitter):
[Paket 1] ------- 35ms ------- [Paket 2] - 5ms - [Paket 3] ---------- 50ms ---------- [Paket 4]
(Jika langsung diputar: suara patah-patah, cepat lalu berhenti!)
```

---

### 4.2 Mekanisme Playout Buffer

![[lgw2e-playout-buffer.png]]

Untuk merekonstruksi sinyal suara/video agar kembali periodik dan mulus:
1. Penerima menyediakan **Playout Buffer**.
2. Paket pertama yang tiba sengaja ditahan selama durasi tertentu yang disebut **Playout Delay ($T_{playout}$)** sebelum diputar ke speaker.
3. Selama penundaan tersebut, paket-paket berikutnya yang sempat tertunda di jaringan memiliki peluang untuk tiba dan masuk ke antrean buffer sebelum giliran waktu pemutarannya tiba.

> [!tip] Trade-off Playout Delay
> - **$T_{playout}$ Terlalu Kecil:** Buffer sering mengalami *underflow* (kehabisan paket). Paket yang datang terlambat terpaksa dibuang (*late packet loss*), menghasilkan suara terputus-putus (*glitches*).
> - **$T_{playout}$ Terlalu Besar:** Seluruh paket aman di buffer, tetapi jeda bicara antar-pengguna menjadi terlalu panjang (*conversational latency* mengganggu komunikasi dua arah). Nilai target one-way delay VoIP umumnya dijaga $< 150 \text{ ms}$.

---

### 4.3 Clock Drift & Clock Recovery
- Generator detak (*clock oscillator*) pada transmitter dan receiver tidak pernah identik sempurna ($\Delta f \neq 0$).
- Jika jam pengirim berjalan sedikit lebih cepat dari penerima $\implies$ buffer penerima lambat laun akan meluap (*overflow*).
- Jika jam pengirim lebih lambat $\implies$ buffer penerima perlahan terkuras habis (*underflow*).
- **Solusi:** Penerima menerapkan algoritma penyesuaian clock (*timing recovery/resynchronization*) atau melakukan penyisipan/pemotongan sampel mikro pada saat periode hening (*silence period*).

---

### 4.4 Real-Time Transport Protocol (RTP)
Didefinisikan dalam RFC 3550, RTP berjalan di atas UDP dan menyediakan layanan pendukung playout buffer melalui tiga field utama pada headernya:
1. **Sequence Number (16 bit):** Mendeteksi kehilangan paket dan memulihkan urutan paket yang tertukar di rute internet.
2. **Timestamp (32 bit):** Mencatat waktu instan saat sampel pertama di dalam paket diambil oleh codec audio/video, memungkinkan penerima merekonstruksi timing pemutaran secara absolut.
3. **Payload Type (7 bit):** Mengidentifikasi format pengkodean audio/video (misal: G.711 PCMU, G.729, H.264).

---

# 5. Layanan TCP Reliable Stream & Flow Control

### 5.1 Karakteristik Layanan TCP
Transmission Control Protocol (TCP) mengimplementasikan seluruh teori peer-to-peer di atas secara komprehensif pada lapisan Transport:
- **Connection-Oriented & Full Duplex:** Koneksi dua arah simultan antara sepasang socket IP & Port `(SrcIP, SrcPort, DstIP, DstPort)`.
- **Byte-Stream Abstraction:** Data dipandang sebagai aliran byte kontinu tanpa tanda batas pesan. Setiap byte individual diberi nomor urut unik 32-bit.
- **Maximum Segment Size (MSS):** TCP memotong aliran byte menjadi segmen-segmen dengan batasan payload MSS (umumnya 1460 byte pada Ethernet MTU 1500 byte).

---

### 5.2 TCP ARQ & Fast Retransmit
- TCP menggunakan varian **Selective Repeat** yang dipadukan dengan **Cumulative Acknowledgment**.
- Field `Acknowledgment Number` menyatakan: nomor byte *berikutnya* yang ditunggu oleh penerima.
- **Fast Retransmit:** Jika pengirim menerima **3 duplicate ACK yang identik** untuk segmen yang sama, pengirim tidak perlu menunggu timer retransmisi habis, melainkan langsung mengirim ulang segmen yang hilang tersebut secara cepat.
- **Selective Acknowledgment (SACK - RFC 2018):** Opsi tambahan pada header TCP di mana penerima dapat melaporkan blok-blok byte non-kontinu yang telah berhasil diterima.

---

### 5.3 TCP Connection Lifecycle (Handshake & Teardown)

#### A. 3-Way Handshake (Pembentukan Koneksi):

![[lgw2e-tcp-three-way-handshake.png]]

```
CLIENT (Host A)                                             SERVER (Host B)
      |                                                            |  LISTEN
      |--- [SYN, Seq = x] ---------------------------------------->|  SYN_RCVD
      |    (Inisialisasi koneksi & tawarkan ISN Client = x)         |
      |                                                            |
      |<-- [SYN + ACK, Seq = y, Ack = x + 1] ----------------------|
      |    (Terima ISN Client, tawarkan ISN Server = y)             |
      |                                                            |
ESTAB |--- [ACK, Seq = x + 1, Ack = y + 1] ----------------------->|  ESTABLISHED
      |    (Koneksi terbentuk, payload data sudah boleh dibawa)     |
```

#### B. Pertukaran Data & Piggybacking:

![[lgw2e-tcp-data-exchange.png]]

- Paket data dapat sekaligus membawa nomor ACK untuk arah sebaliknya (*piggybacking*), menghemat transmisi paket terpisah.

#### C. Graceful Close (Pemutusan Koneksi 4-Way Handshake):

![[lgw2e-tcp-connection-termination.png]]

```
CLIENT (Host A)                                             SERVER (Host B)
      |--- [FIN, Seq = u] ---------------------------------------->| (Tutup arah Client -> Server)
      |<-- [ACK, Ack = u + 1] -------------------------------------|
      |                                                            |
      |    (Server menyelesaikan sisa transfer data...)            |
      |                                                            |
      |<-- [FIN, Seq = v] -----------------------------------------| (Tutup arah Server -> Client)
      |--- [ACK, Ack = v + 1] ------------------------------------>|
      | [TIME_WAIT State: 2 * MSL]                                 | CLOSED
      v
    CLOSED
```
> [!note] Mengapa Butuh State TIME_WAIT ($2 \times MSL$)?
> State `TIME_WAIT` (berlangsung selama $2 \times \text{Maximum Segment Lifetime} \approx 60 - 120 \text{ detik}$) memastikan bahwa ACK terakhir dari klien benar-benar sampai ke server. Jika ACK tersebut hilang, server akan mengirim ulang paket `FIN`, dan klien yang masih berada di `TIME_WAIT` dapat membalas ACK kembali alih-alih mengirim sinyal `RST`.

---

### 5.4 TCP Sliding Window Flow Control & Advertised Window

![[lgw2e-tcp-window-flow-control.png]]

TCP memisahkan kendali aliran dari kendali error melalui field **Window Size ($W_A$)**:
- Penerima selalu mengumumkan sisa ruang buffernya di setiap header segmen TCP:
  $$\text{Window Size } (W_A) = \text{RcvBuffer} - (\text{LastByteReceived} - \text{LastByteReadByApp})$$
- Pengirim dibatasi agar jumlah byte yang sedang berada dalam perjalanan (*in-flight*) tidak melampaui $W_A$:
  $$\text{LastByteSent} - \text{LastByteAcked} \le W_A$$
- **Zero-Window Probing:** Jika buffer penerima penuh, penerima mengumumkan $W_A = 0$. Pengirim berhenti mentransmisikan data, tetapi secara berkala mengirimkan segmen khusus berukuran 1-byte (*Zero-Window Probe*) untuk memaksa penerima membalas ACK berisi nilai $W_A$ terkini saat aplikasi telah mengosongkan buffer.

---

### 5.5 TCP Retransmission Timeout (RTO) & Estimasi RTT

Nilai latensi bolak-balik (*Round-Trip Time*, RTT) di internet bersifat dinamis dan bervariasi. TCP menggunakan algoritma adaptif **Jacobson (RFC 6298)** untuk menghitung batas waktu retransmisi ($RTO$):

1. **Ukur SampleRTT:** Waktu dari pengiriman segmen hingga ACK-nya tiba.
2. **Update Exponential Weighted Moving Average (EWMA) dari RTT:**
   $$\text{EstimatedRTT} = (1 - \alpha) \cdot \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT} \quad (\text{Direkomendasikan } \alpha = 0.125 = \frac{1}{8})$$
3. **Ukur Estimasi Variansi Deviasi RTT:**
   $$\text{DevRTT} = (1 - \beta) \cdot \text{DevRTT} + \beta \cdot |\text{SampleRTT} - \text{EstimatedRTT}| \quad (\text{Direkomendasikan } \beta = 0.25 = \frac{1}{4})$$
4. **Hitung Batas Waktu Retransmisi (RTO):**
   $$\text{RTO} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}$$

> [!important] Aturan Karn (Karn's Algorithm)
> 1. **No RTT Sampling on Retransmission:** Jangan pernah mengukur `SampleRTT` untuk segmen yang pernah mengalami retransmisi (ambiguitas: apakah ACK yang datang merespons pengiriman pertama atau pengiriman ulang?).
> 2. **Timer Backoff:** Setiap kali terjadi timeout, gandakan nilai RTO secara eksponensial ($RTO \leftarrow 2 \times RTO$) untuk meredakan kemacetan jaringan (*congestion backoff*).

---

# PART II: DATA LINK CONTROLS

# 6. Framing pada Data Link Layer

### 6.1 Urgensi Framing
Lapisan fisik (Layer 1) hanya memancarkan aliran bit kontinu murni tanpa tanda pemisah. Lapisan Data Link (Layer 2) bertugas mengelompokkan aliran bit tersebut ke dalam blok-blok diskrit yang disebut **Frame**.
Tantangan utama framing adalah **Transparansi Data (*Data Transparency*)**: memastikan bahwa jika bit/karakter pola pemisah muncul secara alami di dalam payload data pengguna, receiver tidak salah mengira pola tersebut sebagai penutup frame!

---

### 6.2 Character-Oriented Framing & Byte Stuffing

![[lgw2e-character-oriented-framing.png]]

- Digunakan pada protokol berbasis karakter teks (seperti BISYNC).
- Frame diawali karakter khusus `DLE STX` (*Data Link Escape - Start of Text*) dan diakhiri `DLE ETX` (*End of Text*).
- **Byte Stuffing / Character Stuffing:** Jika pola karakter kontrol `DLE` secara kebetulan muncul di dalam data biner pengguna, pengirim menyisipkan satu byte `DLE` tambahan di depannya (`DLE DLE`).
- Penerima yang melihat dua karakter `DLE` berurutan akan menghapus DLE pertama dan menginterpretasikan DLE kedua sebagai data payload biasa.

---

### 6.3 Bit-Oriented Framing & Bit Stuffing (HDLC)

![[lgw2e-hdlc-bit-stuffing.png]]

Protokol modern berbasis bit seperti **HDLC** menggunakan pola flag delimeter unik 8-bit:
$$\text{Flag Pattern} = \mathbf{01111110} \quad (\text{0x7E, yaitu enam angka 1 berurutan diapit angka 0})$$

#### Aturan Bit Stuffing (Sisi Pengirim):
Setiap kali pengirim memindai payload data dan menemukan **LIMA angka 1 berurutan** (`11111`), pengirim **WAJIB SECARA OTOMATIS MENYISIPKAN SATU BIT '0'** tepat setelah bit '1' kelima tersebut, apapun nilai bit berikutnya!

```
Data Pengguna:      0 1 1 0 1 1 1 1 1 1 1 1 1 1 0 0
                            ^^^^^       ^^^^^
Setelah Stuffing:   0 1 1 0 1 1 1 1 1 0 1 1 1 1 1 0 1 1 0 0
                                      ^           ^
                                  Bit 0 disisipkan!
```

#### Aturan Destuffing (Sisi Penerima):
Penerima memantau aliran bit. Setiap kali menemukan lima bit '1' berurutan (`11111`):
1. Periksa bit ke-6:
   - **Jika bit ke-6 bernilai '0':** Buang bit '0' tersebut! (Itu adalah bit stuffing).
   - **Jika bit ke-6 bernilai '1':** Periksa bit ke-7:
     - Jika bit ke-7 bernilai '0' (`01111110`): Ini adalah **FLAG Batas Frame**!
     - Jika bit ke-7 bernilai '1' (`01111111`): Ini adalah sinyal **ABORT** (terjadi error transmisi).

---

### 6.4 Generic Framing Procedure (GFP)

![[lgw2e-gfp-frame-format.png]]

Didefinisikan dalam standar ITU-T G.7041, GFP digunakan untuk memetakan paket data variabel (seperti Ethernet dan IP) ke dalam jaringan sinkron transport optik berkecepatan tinggi (SONET/SDH):
- Tidak menggunakan bit stuffing yang membuat panjang frame tidak pasti.
- Menggunakan header dengan field penunjuk panjang payload: **Payload Length Indicator (PLI)**.
- Dilindungi oleh **cHEC (Core Header Error Control)**: kode CRC yang mampu mengoreksi kesalahan 1-bit pada header PLI secara langsung tanpa retransmisi.

---

# 7. Point-to-Point Protocol (PPP)

### 7.1 Ruang Lingkup & Aplikasi PPP
**PPP (RFC 1661)** adalah standar de-facto untuk komunikasi lapisan data link melalui koneksi langsung titik-ke-titik (*point-to-point lines*):
- Sambungan dial-up telepon analog / modem ke ISP.
- Koneksi router-ke-router melalui saluran sewa (*leased line*).
- **PPPoE (PPP over Ethernet - RFC 2516):** Menjalankan sesi PPP di atas jaringan akses Ethernet pada modem DSL dan jaringan fiber optic perumahan (FTTH) untuk keperluan autentikasi dan manajemen tagihan pengguna.

---

### 7.2 Struktur Frame PPP

![[lgw2e-ppp-frame-format.png]]

```
+----------+----------+----------+----------+----------------+----------+----------+
|   Flag   | Address  | Control  | Protocol |  Information   |   FCS    |   Flag   |
| (1 byte) | (1 byte) | (1 byte) |(1/2 byte)| (0 - 1500 byte)|(2/4 byte)| (1 byte) |
|   0x7E   |   0xFF   |   0x03   |          |  (IP / LCP)    |  CRC-16  |   0x7E   |
+----------+----------+----------+----------+----------------+----------+----------+
```

- **Flag (0x7E):** Penanda batas awal dan akhir frame (`01111110`).
- **Address (0xFF):** Alamat broadcast all-stations (karena link point-to-point tidak memerlukan addressing spesifik).
- **Control (0x03):** Menandakan Unnumbered Information (UI) frame.
- **Protocol (16-bit):** Menentukan jenis paket yang dibawa di dalam payload:
  - `0x0021`: Internet Protocol (IPv4)
  - `0x8021`: Internet Protocol Control Protocol (IPCP)
  - `0xC021`: Link Control Protocol (LCP)
  - `0xC023`: Password Authentication Protocol (PAP)
  - `0xC223`: Challenge Handshake Authentication Protocol (CHAP)
- **Information:** Payload data jaringan (default Maximum Receive Unit / MRU = 1500 byte).
- **Frame Check Sequence (FCS):** 16-bit (atau 32-bit) CRC untuk deteksi error.

---

### 7.3 Byte Stuffing pada PPP

![[lgw2e-ppp-byte-stuffing.png]]

PPP beroperasi secara berorientasi karakter/byte (*character-oriented*). Karakter kontrol escape yang digunakan adalah `0x7D` (`01111101`):
1. Jika byte flag `0x7E` muncul di dalam payload:
   $$\text{0x7E} \implies \text{disisipkan 0x7D diikuti (0x7E XOR 0x20) = } \mathbf{\text{0x7D 0x5E}}$$
2. Jika byte escape `0x7D` muncul di dalam payload:
   $$\text{0x7D} \implies \text{disisipkan 0x7D diikuti (0x7D XOR 0x20) = } \mathbf{\text{0x7D 0x5D}}$$
3. Byte-byte kontrol ASCII dengan nilai numerik $< 0x20$ dapat di-escape dengan cara yang sama jika dinegosiasikan pada fase inisialisasi.

---

### 7.4 Tiga Komponen Inti PPP
Arsitektur PPP terdiri atas 3 komponen fungsional yang bekerja bertahap:
1. **Link Control Protocol (LCP):** Menginisialisasi saluran fisik, menguji loopback (*Echo-Request/Reply*), menegosiasikan ukuran frame (MRU), memilih protokol autentikasi, dan mematikan saluran.
2. **Authentication Protocols:**
   - **PAP (Password Authentication Protocol):** Autentikasi 2 langkah (*two-way handshake*). Pengirim mengirimkan ID dan Password dalam bentuk teks terbuka (*plaintext*). **Tidak aman** terhadap penyadapan.
   - **CHAP (Challenge Handshake Authentication Protocol):** Autentikasi 3 langkah (*three-way handshake*) berbasis kriptografi. Server mengirim bilangan acak (*Challenge string*), klien meng-hash nilai challenge digabung shared secret password menggunakan algoritma MD5, lalu mengirim hasil hash kembali ke server. Password asli tidak pernah dikirim melintasi kabel jaringan.
3. **Network Control Protocols (NCP):** Serangkaian protokol pelengkap untuk mengonfigurasi parameter layer network. Contoh paling penting adalah **IPCP (RFC 1332)** yang bertugas menegosiasikan alamat IP klien, IP gateway, dan alamat server DNS secara dinamis.

---

### 7.5 Siklus Hidup Koneksi PPP (Link Transition Phases)

![[lgw2e-ppp-phases.png]]
*State Machine Fase Transisi PPP.*

![[lgw2e-ppp-connection-setup-example.png]]
*Contoh Prosedur Setup Koneksi PPP Dial-up ke ISP.*

```
+------------+       Physical Link Up
|    DEAD    | ----------------------------+
+------------+                             |
      ^                                    v
      | Close                 +-------------------------+
      +---------------------- | ESTABLISH (LCP Config)  |
      |                       +-------------------------+
      |                                    | Success
      | Failure                            v
      |                       +-------------------------+
      +<--------------------- | AUTHENTICATE (PAP/CHAP) |
      |                       +-------------------------+
      |                                    | Success
      | Close                              v
      |                       +-------------------------+
      +<--------------------- | NETWORK (NCP/IPCP)      |
      |                       +-------------------------+
      |                                    | Success
      | Close                              v
      |                       +-------------------------+
      | Terminate             | OPEN (Data Transfer IP) |
      +---------------------- +-------------------------+
```

---

# 8. High-Level Data Link Control (HDLC)

### 8.1 Tipe Stasiun & Konfigurasi Link
HDLC (ISO 3309 / 4335) mendefinisikan tiga tipe stasiun logis:
1. **Primary Station:** Bertanggung jawab mengendalikan operasi link. Mengeluarkan perintah (*Commands*) dan menerima tanggapan (*Responses*).
2. **Secondary Station:** Beroperasi di bawah kendali stasiun primary. Hanya mengeluarkan respons (*Responses*) jika diminta.
3. **Combined Station:** Menggabungkan fungsi primary dan secondary. Dapat mengeluarkan perintah maupun tanggapan secara mandiri.

**Konfigurasi Fisik Link:**
- **Unbalanced Configuration:** Terdiri dari 1 stasiun Primary dan 1 atau lebih stasiun Secondary (mendukung topologi *Point-to-Point* maupun *Multidrop* saluran bersama).
- **Balanced Configuration:** Terdiri dari 2 stasiun Combined pada link *Point-to-Point* murni.

---

### 8.2 Mode Transfer Data (NRM, ABM, ARM)
1. **Normal Response Mode (NRM):** Digunakan pada konfigurasi *unbalanced* (multidrop). Stasiun secondary hanya boleh mengirim data jika di-polling secara eksplisit oleh stasiun primary dengan menyalakan bit $P=1$.
2. **Asynchronous Balanced Mode (ABM):** Digunakan pada konfigurasi *balanced*. Kedua stasiun combined memiliki hak setara dan dapat mentransmisikan frame secara asinkron kapan saja tanpa menunggu instruksi pihak lawan. Mode ini paling luas diterapkan.
3. **Asynchronous Response Mode (ARM):** Konfigurasi unbalanced di mana secondary dapat memulai transmisi tanpa menunggu poll, tetapi primary tetap memegang kendali inisialisasi link.

---

### 8.3 Format Frame & Field Kontrol HDLC

![[lgw2e-hdlc-control-field.png]]

Struktur Frame HDLC:
```
+----------+----------+----------+----------------+----------+----------+
|   Flag   | Address  | Control  |  Information   |   FCS    |   Flag   |
| (8 bit)  | (8 bit)  | (8 bit)  |   (Payload)    | (16 bit) | (8 bit)  |
| 01111110 |  Station |  Tipe    |  (Data Layer 3)|  CRC-16  | 01111110 |
+----------+----------+----------+----------------+----------+----------+
```

Field paling krusial adalah **Control Field (8-bit)** yang membagi frame HDLC ke dalam 3 format:

```
1. Information Frame (I-Frame):
   Bit:  [ 0 |   N(S)   | P/F |   N(R)   ]
               (3 bit)          (3 bit)

2. Supervisory Frame (S-Frame):
   Bit:  [ 1   0 |  S S  | P/F |   N(R)   ]
                   (2 bit)       (3 bit)

3. Unnumbered Frame (U-Frame):
   Bit:  [ 1   1 |  M M  | P/F |  M M M  ]
                   (2 bit)       (3 bit)
```

---

### 8.4 Klasifikasi Frame: I-Frames, S-Frames, U-Frames

#### 1. Information Frames (I-Frames)
Membawa payload data pengguna dari lapisan jaringan.
- $N(S)$: Nomor urut frame transmisi (0–7).
- $N(R)$: Nomor urut penerimaan yang berfungsi sebagai ACK kumulatif (*piggybacking*).
- $P/F$ (*Poll/Final*):
  - $P=1$ (Poll) jika dikirim oleh primary untuk meminta respons stasiun lawan.
  - $F=1$ (Final) jika dikirim oleh secondary sebagai frame penutup tanggapan poll.

#### 2. Supervisory Frames (S-Frames)
Digunakan untuk kendali error (ARQ) dan kendali aliran (Flow Control) tanpa membawa data:
- **RR (Receive Ready - $SS=00$):** ACK positif. Mengakui semua frame hingga $N(R)-1$ dan menyatakan stasiun siap menerima frame nomor $N(R)$.
- **RNR (Receive Not Ready - $SS=01$):** ACK positif hingga $N(R)-1$, tetapi stasiun mengalami buffer penuh (*flow control stop transmission*).
- **REJ (Reject - $SS=10$):** Negative ACK (NAK) untuk **Go-Back-N ARQ**. Meminta pengirim mengulang transmisi mulai dari frame $N(R)$.
- **SREJ (Selective Reject - $SS=11$):** Negative ACK untuk **Selective Repeat ARQ**. Meminta pengirim mengulang transmisi *hanya* untuk frame spesifik bernomor $N(R)$.

#### 3. Unnumbered Frames (U-Frames)
Digunakan untuk manajemen link, negosiasi mode kerja, dan pemutusan sesi. 5 bit modifier ($M$) mendefinisikan hingga 32 fungsi:
- `SABM` (*Set Asynchronous Balanced Mode*): Perintah inisialisasi koneksi mode ABM.
- `SNRM` (*Set Normal Response Mode*): Perintah inisialisasi mode NRM.
- `DISC` (*Disconnect*): Perintah pemutusan sesi link.
- `UA` (*Unnumbered Acknowledgment*): Respons konfirmasi sukses untuk frame perintah U-frame.
- `DM` (*Disconnected Mode*): Respons stasiun dalam kondisi offline/tidak siap.
- `FRMR` (*Frame Reject*): Pelaporan frame tidak valid atau korup yang tidak dapat dipulihkan dengan retransmisi standar.

---

### 8.5 Contoh Operasi HDLC: NRM Polling vs ABM Symmetrical

#### A. Operasi NRM Polling (Primary A melayani Secondaries B dan C):

![[lgw2e-hdlc-nrm-example.png]]

```
PRIMARY A                                       SECONDARY B        SECONDARY C
    |--- [B, RR, 0, P] ----------------------------->|                 |
    |    (Poll B: apakah ada data untuk dikirim?)    |                 |
    |<-- [B, I, 0, 0] -------------------------------|                 |
    |<-- [B, I, 1, 0, F] ----------------------------|                 |
    |    (B mengirim data dan menutup dengan F=1)    |                 |
    |                                                                  |
    |--- [C, RR, 0, P] ----------------------------------------------->|
    |    (Poll C: apakah ada data?)                                    |
    |<-- [C, RR, 0, F] ------------------------------------------------|
    |    (C tidak punya data, hanya membalas RR dengan F=1)
```

#### B. Operasi ABM Peer-to-Peer (Stasiun Gabungan A dan B):

![[lgw2e-hdlc-abm-example.png]]

- Inisialisasi: Stasiun A mengirim `SABM`, Stasiun B membalas `UA`.
- Transfer data dua arah simultan memanfaatkan field $N(S)$ dan $N(R)$ pada I-frame untuk saling mengakui data (*piggybacking*).
- Pemutusan koneksi: Stasiun A mengirim `DISC`, Stasiun B membalas `UA`.

---

# 9. Link Sharing Menggunakan Statistical Multiplexing

### 9.1 Konsep Dasar & Tradeoff Statistical Multiplexing

![[lgw2e-statistical-multiplexing-buffer.png]]

- **Statistical Multiplexing:** Mengkonsentrasikan trafik yang bersifat meletup-letup (*bursty traffic*) dari sejumlah jalur masukan (*input lines*) ke dalam satu saluran transmisi bersama (*shared output line*). Hal ini menghasilkan efisiensi penggunaan link yang jauh lebih tinggi dan penghematan biaya (*greater efficiency and lower cost*).
- **Tradeoff Delay vs Efisiensi:**
  - **Jalur Terdedikasi (*Dedicated Lines*):** Pengguna tidak perlu mengantre atau menunggu pengguna lain (*no waiting for other users*), namun kapasitas saluran menjadi sangat tidak efisien dan banyak menganggur ketika trafik pengguna bersifat *bursty*.
  - **Jalur Bersama (*Shared Lines*):** Paket-paket dari banyak pengguna dikonsentrasikan ke satu saluran bersama. Paket disimpan sementara di dalam **buffer** (mengalami penundaan/delay antrean) ketika saluran transmisi sedang tidak tersedia atau sedang mentransmisikan paket lain.
- **Multiplexer Inherent pada Packet Switch:** Pada switch jaringan paket, frame/paket yang masuk diteruskan ke buffer sebelum dikirimkan keluar switch. Proses multiplexing secara inheren terjadi di dalam buffer antrean switch ini.

---

### 9.2 Pemodelan Multiplexer & Karakteristik Trafik Internet

#### A. Lima Elemen Pemodelan Multiplexer
Untuk menganalisis kinerja multiplexer secara matematis, terdapat lima pertanyaan dasar pemodelan:
1. **Arrivals (Pola Kedatangan):** Bagaimana pola distribusi waktu antar-kedatangan paket (*packet interarrival pattern*)? (Misal: proses kedatangan Poisson).
2. **Service Time (Waktu Layanan):** Berapa lama waktu yang dibutuhkan untuk mentransmisikan paket? (Tergantung pada panjang paket $L$ dan kapasitas saluran $R$).
3. **Service Discipline (Disiplin Layanan):** Bagaimana urutan transmisi paket yang mengantre? (Umumnya FIFO / FCFS - *First-Come, First-Served*).
4. **Buffer Discipline (Disiplin Buffer):** Jika buffer penuh, paket mana yang harus dibuang (*packet drop policy*)?
5. **Performance Measures (Ukuran Kinerja Sistem):**
   - Distribusi penundaan (*Delay Distribution*)
   - Probabilitas kehilangan paket (*Packet Loss Probability*)
   - Utilisasi saluran (*Line Utilization / Load* $\rho$)

#### B. Komponen Delay Sistem
Waktu penundaan total (*delay*) satu paket di dalam sistem terdiri dari dua komponen utama:
$$\text{Delay } (T) = \text{Waiting Time } (W) + \text{Service Time } (X)$$
- **Waiting Time ($W$):** Waktu dari saat paket tiba di antrean buffer hingga transmisi paket tersebut dimulai.
- **Service Time ($X$):** Waktu untuk mentransmisikan seluruh bit paket ke saluran fisik:
  $$X = \frac{L}{R}$$
  dengan $L$ adalah jumlah bit dalam paket dan $R$ adalah laju transmisi saluran (*transmission rate*, bps).
- **Fluktuasi Paket dalam Sistem ($N(t)$):** Jumlah paket di dalam sistem $N(t)$ berfluktuasi secara dinamis seiring waktu seiring datangnya ledakan paket dan selesainya transmisi.

#### C. Distribusi Panjang Paket & Waktu Layanan
Karena panjang paket $L$ bervariasi, distribusi panjang paket menentukan distribusi waktu layanan:
1. **Constant packet length:** Seluruh paket memiliki panjang yang seragam/sama ($L$ konstan).
2. **Exponential distribution:** Panjang paket dimodelkan mengikuti variabel acak kontinu eksponensial.
3. **Internet Measured Distributions:** Distribusi riil berdasarkan pengukuran trafik Internet empiris.

#### D. Data Empiris Distribusi Paket Internet (Survei CAIDA)
Berdasarkan pengukuran trafik aktual di Internet (sumber: *caida.org*):
- **Didominasi Trafik TCP:** Sekitar $85\%$ dari total paket adalah paket TCP.
- **~40% Paket Berukuran Minimum (40 byte):** Paket kecil yang merupakan TCP ACKs dan frame kontrol tanpa muatan data payload.
- **~15% Paket Berukuran Maksimum (1500 byte):** Frame data ukuran penuh sesuai standar Ethernet MTU 1500 byte.
- **~15% Paket Berukuran 552 & 576 byte:** Berasal dari implementasi TCP yang tidak menggunakan *Path MTU Discovery*.
- **Parameter Statistik:**
  - Panjang Rata-rata (*Mean*): **413 bytes**
  - Deviasi Standar (*Standard Deviation*): **509 bytes**

---

### 9.3 Analisis Model Antrean M/M/1/K (Buffer Terbatas)

![[lgw2e-mm1k-queue-model.png]]

#### A. Definisi & Parameter Model M/M/1/K
- **Kedatangan Poisson:** Paket tiba dengan laju kedatangan rata-rata $\lambda$ paket/detik.
- **Waktu Layanan Eksponensial:** Waktu transmisi paket berdistribusi eksponensial dengan laju layanan $\mu$ paket/detik, di mana rata-rata waktu layanan $E[X] = 1/\mu$.
- **Kapasitas Terbatas ($K$):** Sistem mengizinkan maksimal $K$ pelanggan/paket di dalam sistem secara bersamaan:
  - $1$ paket sedang dilayani / ditransmisikan di server.
  - Maksimal $K - 1$ paket dapat mengantre di dalam buffer.
- **Parameter Beban / Utilisasi (*Load*):**
  $$\rho = \frac{\lambda}{\mu} = \frac{\lambda L}{R}$$

#### B. Karakteristik Sistem Berdasarkan Nilai Beban ($\rho$)
- **Saat $\lambda \ll \mu$ ($\rho \approx 0$):** Paket tiba sangat jarang dan biasanya menemukan sistem dalam kondisi kosong. Penundaan (*delay*) sangat rendah dan peluang terjadinya kehilangan paket hampir nol.
- **Saat $\lambda \to \mu$ ($\rho \to 1$):** Paket mulai menumpuk (*bunching up*), delay rata-rata meningkat pesat, dan paket mulai sering terbuang (*losses occur more frequently*).
- **Saat $\lambda > \mu$ ($\rho > 1$):** Paket tiba lebih cepat daripada kapasitas pemrosesan link. Mayoritas paket yang baru tiba menemukan buffer penuh dan langsung dibuang (*blocked*). Paket-paket yang berhasil masuk harus menunggu sekitar $K - 1$ waktu layanan.

#### C. Distribusi Kedatangan Poisson & Waktu Eksponensial
- Laju kedatangan rata-rata adalah $\lambda$ paket per detik, dengan kedatangan memiliki probabilitas yang sama untuk terjadi pada sembarang titik waktu.
- Waktu antar-kedatangan berturutan berdistribusi eksponensial dengan rata-rata $1/\lambda$:
  $$P[X > t] = e^{-\lambda t}, \quad P[X \le t] = 1 - e^{-\lambda t}, \quad f(t) = \lambda e^{-\lambda t} \quad (t > 0)$$
- Jumlah kedatangan paket $k$ dalam interval waktu $t$ detik mengikuti distribusi Poisson:
  $$P[k \text{ kedatangan dalam } t \text{ detik}] = \frac{(\lambda t)^k e^{-\lambda t}}{k!}$$

#### D. Hasil Kinerja Model M/M/1/K (Appendix A)
1. **Probabilitas Paket Hilang / Overflow ($P_{\text{loss}}$ atau $P_K$):**
   $$P_{\text{loss}} = \frac{(1 - \rho)\rho^K}{1 - \rho^{K+1}}$$
2. **Rata-rata Jumlah Paket dalam Sistem ($E[N]$):**
   $$E[N] = \frac{\rho}{1 - \rho} - \frac{(K+1)\rho^{K+1}}{1 - \rho^{K+1}}$$
3. **Rata-rata Total Delay Paket ($E[T]$):**
   $$E[T] = \frac{E[N]}{\lambda (1 - P_K)}$$
   *(dengan $\lambda(1 - P_K)$ adalah laju kedatangan efektif dari paket yang berhasil masuk ke dalam buffer).*

#### E. Analisis Kasus Spesifik: M/M/1/10 ($K = 10$)
- Maksimal $10$ paket yang diizinkan berada dalam sistem (1 di server + 9 di antrean).
- **Delay Minimum:** $1$ waktu layanan ($1 \cdot E[X]$) saat sistem sepi.
- **Delay Maksimum:** $10$ waktu layanan ($10 \cdot E[X]$) saat antrean penuh.
- **Ambang Kritis Beban:** Pada beban sekitar $70\%$ ($\rho = 0.7$), baik kurva delay ternormalisasi ($E[T]/E[X]$) maupun kurva probabilitas kehilangan paket mulai melonjak secara tajam.
- **Tradeoff Menambah Kapasitas Buffer:** Menambah kapasitas buffer ($K$) memperkecil probabilitas paket dibuang, namun meningkatkan penundaan maksimum yang dialami paket saat terjadi penumpukan antrean.

---

### 9.4 Analisis Antrean M/M/1 & M/D/1 (Buffer Tak Terbatas) serta Efek Skala

#### A. Model Antrean M/M/1 ($K = \infty$)
Pada sistem antrean $M/M/1$, buffer dianggap tak terbatas:
- Tidak ada paket yang dibuang karena keterbatasan buffer ($P_b = 0$).
- **Syarat Stabilitas:** Beban harus $\rho = \lambda/\mu < 1$. Jika $\lambda \ge \mu$, antrean akan terus memanjang tanpa batas (*unstable*).
- **Rata-rata Waktu Total dalam Sistem ($E[T]$):**
  $$E[T]_M = \frac{1}{\mu}\left[\frac{1}{1 - \rho}\right] = \frac{1}{\mu - \lambda} = \frac{1}{\mu} + \frac{1}{\mu}\left[\frac{\rho}{1 - \rho}\right]$$
- **Rata-rata Waktu Menunggu dalam Antrean ($E[W]$):**
  $$E[W]_M = \frac{1}{\mu}\left[\frac{\rho}{1 - \rho}\right]$$

#### B. Perbandingan Rata-rata Delay: M/M/1 vs M/D/1
Pada model **M/D/1**, paket tiba secara Poisson namun memiliki panjang dan waktu layanan yang konstan (*deterministic service time*):
$$E[T]_D = \frac{1}{\mu}\left[1 + \frac{\rho}{2(1 - \rho)}\right] = \frac{1}{\mu}\left[\frac{2 - \rho}{2(1 - \rho)}\right]$$

![[lgw2e-mm1-queue-delay-curve.png]]
*Kurva Perbandingan Normalized Average Delay ($E[T]/E[X]$) antara M/M/1 dan M/D/1.*

- Delay rata-rata pada sistem M/D/1 selalu **lebih rendah** dibandingkan sistem M/M/1 untuk setiap tingkat beban $\rho$.
- Waktu antrean tunggu ($E[W]$) pada M/D/1 tepat **setengah** dari waktu tunggu M/M/1:
  $$E[W]_D = \frac{1}{2} E[W]_M$$
  karena tidak adanya variabilitas/deviasi pada panjang paket.

#### C. Efek Skala (Effect of Scale / Aggregation of Flows)
Penggabungan banyak aliran trafik ke dalam satu link berkapasitas besar memberikan peningkatan performa penundaan yang luar biasa:

| Parameter | Saluran Skala Kecil | Saluran Skala Besar (Agregasi 100x) |
| :--- | :--- | :--- |
| **Kapasitas Link ($C$)** | $100{,}000\text{ bps}$ ($100\text{ kbps}$) | $10{,}000{,}000\text{ bps}$ ($10\text{ Mbps}$) |
| **Panjang Rata-rata Paket ($L$)** | $10{,}000\text{ bits}$ | $10{,}000\text{ bits}$ |
| **Waktu Transmisi ($E[X]$)** | $\frac{10{,}000}{100{,}000} = 0.1\text{ detik}$ | $\frac{10{,}000}{10{,}000{,}000} = 0.001\text{ detik}$ |
| **Laju Kedatangan ($\lambda$)** | $7.5\text{ paket/detik}$ | $750\text{ paket/detik}$ (100 aliran digabung) |
| **Beban Link ($\rho = \lambda / \mu$)** | $\rho = \frac{7.5}{10} = 0.75$ | $\rho = \frac{750}{1000} = 0.75$ |
| **Rata-rata Total Delay ($E[T]$)** | $E[T] = \frac{0.1}{1 - 0.75} = \mathbf{0.4\text{ detik}}$ | $E[T] = \frac{0.001}{1 - 0.75} = \mathbf{0.004\text{ detik}}$ |

> [!important] Reduksi Delay Sebesar Faktor 100
> Pada tingkat utilisasi yang sama persis ($\rho = 75\%$), penggabungan 100 aliran ke saluran berkecepatan 100x menghasilkan **penurunan total delay sebesar faktor 100x** (dari 400 ms menjadi 4 ms).

---

### 9.5 Header Overhead & Goodput

Goodput mengukur laju bit data informasi bersih yang berhasil ditransmisikan di luar overhead header protokol.

#### A. Pemodelan & Formula Goodput
- Misal kapasitas transmisi link $R = 64\text{ kbps} = 64{,}000\text{ bps}$.
- Ukuran gabungan header IP + TCP tetap = $40\text{ bytes} = 320\text{ bits}$.
- Paket memiliki panjang total konstan $L \in \{200, 400, 800, 1200\}\text{ bytes}$.
- Laju layanan:
  $$\mu = \frac{64{,}000}{8L}\text{ paket/detik}$$
- Beban saluran:
  $$\rho = \frac{\lambda}{\mu} = \lambda \cdot \frac{8L}{64{,}000}$$
- **Goodput Aktual:**
  $$\text{Goodput} = \lambda \times 8(L - 40)\text{ bits/detik}$$
- **Maximum Goodput (saat saluran jenuh $\rho = 1$):**
  $$\text{Max Goodput} = \left(1 - \frac{40}{L}\right) \times 64{,}000\text{ bps}$$

#### B. Dampak Ukuran Paket terhadap Goodput & Delay
- Header overhead membatasi batas atas kapasitas goodput maksimum.
- Untuk $L = 200\text{ byte}$: Max Goodput hanya $(1 - 40/200) \times 64 = 51.2\text{ kbps}$ ($20\%$ bandwidth habis untuk header).
- Untuk $L = 1200\text{ byte}$: Max Goodput mencapai $(1 - 40/1200) \times 64 = 61.87\text{ kbps}$ (hanya $3.3\%$ terbuang).
- Tradeoff: Ukuran paket besar meningkatkan efisiensi goodput saluran, namun memperpanjang waktu transmisi per paket sehingga meningkatkan rata-rata delay antrean.

---

### 9.6 Burst Multiplexing & Speech Interpolation (DSI)

#### A. Karakteristik Suara & Prinsip Kerja
- Dalam percakapan telepon dua arah, manusia hanya berbicara aktif (*talkspurt*) kurang dari $40\%$ waktu panggilan ($\text{aktivitas } p < 0.4$). Sisanya adalah fase hening (*silence*).
- **Speech Interpolation (DSI):** Bekerja tanpa buffer (*no buffering*). Ledakan suara (*bursts*) dialokasikan secara langsung (*on-the-fly*) ke saluran fisik (*trunks*) yang sedang tidak terpakai.
- Memungkinkan sistem melayani **2 hingga 3 kali lebih banyak panggilan** dibandingkan jumlah trunk fisik yang tersedia.
- **Tradeoff Kinerja:** Utilisasi Trunk (*Trunk Utilization*) vs Kehilangan Potongan Suara (*Speech Loss*).
- **Fractional Speech Loss:** Fraksi dari durasi ucapan aktif yang terpotong/hilang karena seluruh trunk sedang penuh saat pembicara mulai berbicara.

#### B. Formula Speech Loss
Probabilitas kehilangan potongan ucapan saat $n$ pembicara aktif berebut $m$ trunk fisik:
$$\text{Speech Loss} = \sum_{k=m+1}^n \frac{k - m}{np} \binom{n}{k} p^k (1 - p)^{n-k}$$
di mana:
$$\binom{n}{k} = \frac{n!}{k!(n-k)!}$$

#### C. Efek Skala & Multiplexing Gain pada Suara
Kebutuhan jumlah trunk $m$ untuk menjaga performa standar $\text{Speech Loss} \le 1\%$:
$$\text{Multiplexing Gain} = \frac{\text{Jumlah Pembicara } (n)}{\text{Jumlah Saluran Trunk } (m)}$$

| Pembicara ($n$) | Trunk Dibutuhkan ($m$) | Utilisasi Trunk | Multiplexing Gain |
| :---: | :---: | :---: | :---: |
| **24** | 13 | 0.74 | 1.85 |
| **32** | 16 | 0.80 | 2.00 |
| **40** | 20 | 0.80 | 2.00 |
| **48** | 23 | 0.83 | 2.09 |

Semakin besar skala sistem (*larger flows*), utilisasi trunk semakin tinggi dan Multiplexing Gain semakin besar.

---

### 9.7 Packet Speech Multiplexing & Packet Voice Switching

![[lgw2e-packet-speech-multiplexing.png]]

#### A. Konsep Packet Speech Multiplexing
- Suara digital dikemas ke dalam paket-paket berukuran tetap (*fixed-length packets*).
- Tidak ada paket yang dibangkitkan saat pembicara hening (*silence*).
- Paket-paket dibangkitkan secara sinkron dan periodik saat pembicara aktif berbicara.
- Paket-paket disimpan di dalam buffer dan ditransmisikan melalui saluran bersama berkecepatan tinggi.
- **Tradeoff Kinerja:** Utilisasi saluran vs Penundaan/Jitter (*Delay/Jitter*) dan Kehilangan Paket (*Packet Loss*).

#### B. Isu & Mekanisme Penanganan Packet Voice

![[lgw2e-playout-buffer.png]]

1. **Packetization Delay:** Waktu yang dibutuhkan untuk mengumpulkan sampel audio suara hingga mengisi penuh satu paket payload.
2. **Jitter (Variasi Keterlambatan Paket):** Interval kedatangan paket di sisi penerima bervariasi secara acak akibat perbedaan delay antrean di switch jaringan.
3. **Mekanisme Playout Buffer (Playback Strategies):**
   - Penerima memasukkan penundaan fleksibel (*flexible delay*) sebelum memulai pemutaran paket pertama.
   - Penundaan fleksibel ini menyerap variasi jitter kedatangan paket sehingga menghasilkan total delay end-to-end yang konstan dan pemutaran audio yang mulus.
4. **Countermeasures Buffer Overflow & Underflow:**
   - *Buffer Overflow:* Paket datang terlalu cepat saat buffer penuh sehingga paket terpaksa dibuang.
   - *Buffer Underflow:* Paket terlambat tiba ketika giliran pemutarannya telah tiba, menyebabkan suara terputus-putus.
5. **Clock Recovery Algorithm:** Diperlukan algoritma pemulihan clock pada penerima agar frekuensi pemutaran audio sinkron sempurna dengan laju sampling pengirim.

---

# 10. Ringkasan Rumus & Cheatsheet Ujian

> [!summary] Kumpulan Formula Kunci Chapter 5

### 1. Parameter Kanal Fisik
- Waktu Transmisi Frame: $t_f = \frac{n_f}{R}$
- Waktu Propagasi: $t_{prop} = \frac{d}{c}$
- Normalized Delay-Bandwidth Product: $a = \frac{t_{prop}}{t_f} = \frac{R \cdot d}{c \cdot n_f}$
- Frame Error Probability dari BER $p$: $P_f = 1 - (1 - p)^{n_f}$

### 2. Efisiensi Protokol ARQ
| Protokol | Efisiensi Kanal Bebas Error ($\eta_0$) | Efisiensi Nyata Kanal Ber-Error ($\eta$) | Syarat Window |
| :--- | :--- | :--- | :--- |
| **Stop-and-Wait** | $\approx \frac{1}{1 + 2a}$ | $(1 - P_f) \cdot \eta_0$ | $W_s = 1, W_r = 1$ |
| **Go-Back-N** | $1$ (jika $W_s \ge 1+2a$) | $\frac{1 - P_f}{1 + (W_s - 1)P_f} \cdot \eta_0$ | $W_s \le 2^m - 1, W_r = 1$ |
| **Selective Repeat** | $1$ (jika $W_s \ge 1+2a$) | $(1 - P_f) \cdot \eta_0$ | $W_s + W_r \le 2^m \implies W_s \le 2^{m-1}$ |

### 3. Algoritma TCP RTO (Jacobson)
- $\text{EstimatedRTT} \leftarrow (1 - \alpha) \cdot \text{EstimatedRTT} + \alpha \cdot \text{SampleRTT} \quad (\alpha = 0.125)$
- $\text{DevRTT} \leftarrow (1 - \beta) \cdot \text{DevRTT} + \beta \cdot |\text{SampleRTT} - \text{EstimatedRTT}| \quad (\beta = 0.25)$
- $\text{RTO} = \text{EstimatedRTT} + 4 \cdot \text{DevRTT}$

### 4. Teori Antrean & Statistical Multiplexing
- Beban / Utilisasi Sistem: $\rho = \frac{\lambda}{\mu} = \frac{\lambda L}{R}$
- Komponen Delay Total: $T = W + X \implies E[T] = E[W] + E[X]$
- Distribusi Kedatangan Poisson: $P[k \text{ kedatangan dalam } t] = \frac{(\lambda t)^k e^{-\lambda t}}{k!}$
- Model Antrean $M/M/1/K$ (Buffer Terbatas):
  $$P_{\text{loss}} = \frac{(1 - \rho)\rho^K}{1 - \rho^{K+1}}, \quad E[N] = \frac{\rho}{1 - \rho} - \frac{(K+1)\rho^{K+1}}{1 - \rho^{K+1}}, \quad E[T] = \frac{E[N]}{\lambda(1 - P_K)}$$
- Model Antrean $M/M/1$ ($K = \infty$):
  $$E[T]_M = \frac{1}{\mu(1 - \rho)} = \frac{1}{\mu - \lambda}, \quad E[W]_M = \frac{1}{\mu}\left[\frac{\rho}{1 - \rho}\right]$$
- Model Antrean $M/D/1$ (Waktu Layanan Konstan):
  $$E[T]_D = \frac{1}{\mu}\left[1 + \frac{\rho}{2(1 - \rho)}\right] = \frac{1}{\mu}\left[\frac{2 - \rho}{2(1 - \rho)}\right], \quad E[W]_D = \frac{1}{2} E[W]_M$$
- Header Overhead & Goodput:
  $$\text{Goodput} = \lambda \cdot 8(L - 40)\text{ bps}, \quad \text{Max Goodput} = \left(1 - \frac{40}{L}\right) \times R$$
- Speech Loss vs Trunks (Speech Interpolation):
  $$\text{Speech Loss} = \sum_{k=m+1}^n \frac{k - m}{np} \binom{n}{k} p^k (1 - p)^{n-k}$$
- Multiplexing Gain:
  $$G = \frac{\text{Jumlah Pembicara } (n)}{\text{Jumlah Trunk } (m)}$$

---
*Catatan ini disusun secara menyeluruh untuk Obsidian dari dokumen presentasi Leon-Garcia & Widjaja Chapter 5 (148 slide).*
