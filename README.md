# QA Technical Test – Studi Kasus 1: Booking Validation

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

