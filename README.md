# QA Technical Test – 
Studi Kasus 1: Booking Validation

Repositori ini berisi test case manual dan script automation API untuk memvalidasi fitur booking lapangan berdasarkan studi kasus teknikal QA.

## Struktur Repository

- Studi Kasus 1/Test-Scenarios/Test Case Studi Kasus 1.md → Test case manual
- Studi Kasus 1/Postman/AYO Technical Test.postman_collection.json → Postman collection untuk automation test
- README.md → Penjelasan cara menjalankan pengujian

## Tujuan Pengujian
- Validasi bahwa harga booking sesuai dengan slot waktu pada tabel harga
- Cegah booking ganda (double booking) di waktu dan venue yang sama
- Tolak booking jika waktu tidak sesuai dengan jadwal yang tersedia

## Cara Menjalankan Automation Test

### 1. Jalankan Manual via Postman

- Buka Postman, import file `AYO Technical Test.postman_collection.json`
- Pilih request bernama `TC-HARGA-AUTO-CHECK`
- Ubah bagian body sesuai slot booking yang ingin diuji, contoh:

    {
      "venue_id": 15,
      "date": "2022-12-10",
      "start_time": "09:00:00",
      "end_time": "11:00:00"
    }

- Klik Send
- Hasil validasi otomatis akan muncul di tab Tests

### 2. Jalankan Otomatis via Terminal (Newman)

Jika menggunakan Newman, jalankan perintah berikut:

    newman run postman/AYO Technical Test.postman_collection.json



## Validasi Harga Otomatis

Script automation akan secara otomatis mengecek harga berdasarkan kombinasi waktu `start_time` dan `end_time`.

| Slot Booking     | Harga Diharapkan |
|------------------|------------------|
| 07:00–09:00       | 800000           |
| 09:00–11:00       | 1000000          |
| 11:00–13:00       | 1200000          |

Jika waktu booking tidak ditemukan di jadwal → test akan gagal.  
Jika harga tidak sesuai → test juga akan gagal.

## Test Case Manual

Test case manual tersedia di file berikut:

- Studi Kasus 1/Test-Scenarios/Test Case Studi Kasus 1.md
  
Test case mencakup:

- Validasi harga sesuai slot
- Validasi slot tidak valid
- Validasi double booking oleh user sama maupun berbeda
- Validasi kombinasi waktu booking

Studi Kasus 2 – Analisis Pengujian Website & Aplikasi AYO

1. Website AYO (https://ayo.co.id)
**Fokus: Stabilitas Transaksi**

Di versi web, pengguna cenderung melakukan booking dan riset harga. Bug terkait transaksi dan kompatibilitas browser bisa sangat berdampak.

### Strategi Pengujian:

#### **Prioritas 1 – Stok Lapangan **
- **Mekanisme**: Concurrency Testing  
- **Simulasi**: Dua user booking slot yang sama di waktu bersamaan  
- **Tujuan**: Hanya satu yang berhasil booking, yang lain mendapat error yang jelas  
- **Alasan**: Ini adalah bug paling fatal dalam bisnis sewa lapangan

#### **Prioritas 2 – Payment Gateway**
- **Mekanisme**: Integration Testing  
- **Skenario**: Simulasi kegagalan saat pembayaran (internet mati, respon lambat)  
- **Validasi**: Status booking jadi 'Pending' atau 'Failed'? Uang terpotong tapi booking gagal?

#### **Prioritas 3 – Responsiveness & Cross-Browser**
- **Mekanisme**: UI & Layout Testing  
- **Fokus**: Tampilan kalender/jadwal booking di Chrome vs Safari, Desktop vs Tablet  
- **Tujuan**: Hindari bug UI seperti elemen jam yang tidak terbaca atau tertutup

---

## 2. Aplikasi AYO (Android/iOS)  
🎯 **Fokus: User Experience & Fitur Eksklusif**

### Strategi Pengujian:

#### **Prioritas 1 – Fitur Eksklusif App (Reschedule & Cancel)**
- **Mekanisme**: Regression Testing  
- **Skenario**: Booking di-reschedule, pastikan slot lama kembali tersedia  
- **Tujuan**: Hindari kasus ghost booking akibat stok yang tidak perbarui

#### **Prioritas 2 – Notifikasi Real-time & Akses Lokasi**
- **Mekanisme**: Interrupt Testing  
- **Skenario**: Ada telepon/sms saat sedang memilih lapangan → aplikasi harus tetap stabil  
- **Fitur**: ‘Main Bareng’ → notifikasi push saat ada sparring baru harus realtime dan tidak delay

#### **Prioritas 3 – Device Fragmentation**
- **Mekanisme**: Compatibility Testing di real device
- **Fokus**: HP low-end (RAM kecil) atau koneksi lambat  
- **Tujuan**: Pastikan aplikasi tidak crash atau lemot saat load venue banyak

---

## 3. Rekomendasi Metode Testing

Saya merekomendasikan pendekatan **Hybrid Testing**:

### Core Function (Wajib Automation – Regression / E2E):
- Alur: **Login → Cari → Booking → Bayar**
- Metode: UI automation (Selenium/Web) + API test
- Alasan: Ini adalah alur “core bisnis” → wajib dites setiap kali ada update

### Per Menu (Manual/Exploratory):
- Contoh: Edit profil, galeri foto, artikel, dsb.
- Alasan: Risiko rendah, cukup diuji manual berdasarkan prioritas

---

