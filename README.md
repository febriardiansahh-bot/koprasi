# 🏦 KoperasiApp — Panduan Instalasi & Penggunaan
# Aplikasi Koperasi

Tugas proyek akhir mata kuliah Pemrograman Web.

## Cara Menjalankan
1. Pastikan Composer dan PHP sudah terinstall.
2. Clone repositori ini.
3. Jalankan `composer install`.
4. Copy `.env.example` menjadi `.env`.
5. Jalankan `php artisan serve`.

> **Aplikasi Manajemen Koperasi Simpan Pinjam**  
> Versi 1.0 · Laravel 13 · PHP 8.x · MySQL 8.x

---

## Daftar Isi

1. [Persyaratan Sistem](#1-persyaratan-sistem)
2. [Instalasi](#2-instalasi)
3. [Konfigurasi Database](#3-konfigurasi-database)
4. [Menjalankan Aplikasi](#4-menjalankan-aplikasi)
5. [Akun Default](#5-akun-default)
6. [Panduan Penggunaan](#6-panduan-penggunaan)
    - [Login](#61-login)
    - [Dashboard](#62-dashboard)
    - [Manajemen Anggota](#63-manajemen-anggota)
    - [Simpanan](#64-simpanan)
    - [Pinjaman](#65-pinjaman)
    - [Deposito](#66-deposito)
    - [Laporan](#67-laporan-admin-only)
7. [Hak Akses Per Role](#7-hak-akses-per-role)
8. [Troubleshooting](#8-troubleshooting)
9. [Struktur Folder](#9-struktur-folder)

---

## 1. Persyaratan Sistem

Pastikan komputer sudah terinstall semua software berikut sebelum memulai:

| Software            | Versi Minimum | Cara Cek             |
| ------------------- | ------------- | -------------------- |
| **PHP**             | 8.1+          | `php --version`      |
| **Composer**        | 2.x           | `composer --version` |
| **MySQL / MariaDB** | 8.0+          | `mysql --version`    |
| **Git**             | 2.x           | `git --version`      |

> 💡 **Rekomendasi:** Gunakan **Laragon** (Windows) karena sudah menyertakan PHP, MySQL, dan Composer sekaligus.  
> Download: https://laragon.org/download/

---

## 2. Instalasi

### Langkah 1 — Clone atau Ekstrak Project

**Jika menggunakan Git:**

```bash
git clone https://github.com/username/koperasi-app.git
cd koperasi-app
```

**Jika menggunakan ZIP:**

1. Ekstrak file `.zip` ke folder yang diinginkan (contoh: `D:\laragon\www\koperasi-app`)
2. Buka terminal / Command Prompt di folder tersebut

---

### Langkah 2 — Install Dependencies

```bash
composer install
```

> ⏳ Proses ini membutuhkan koneksi internet dan dapat memakan waktu 2–5 menit.

---

### Langkah 3 — Buat File `.env`

Copy file konfigurasi dari template:

```bash
# Windows (Command Prompt)
copy .env.example .env

# Windows (PowerShell)
Copy-Item .env.example .env

# Linux / Mac
cp .env.example .env
```

---

### Langkah 4 — Generate Application Key

```bash
php artisan key:generate
```

Output yang diharapkan:

```
INFO  Application key set successfully.
```

---

## 3. Konfigurasi Database

### Langkah 1 — Buat Database MySQL

Buka **phpMyAdmin** (http://localhost/phpmyadmin) atau jalankan perintah berikut:

```sql
CREATE DATABASE koperasi_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

Atau via terminal:

```bash
mysql -u root -p -e "CREATE DATABASE koperasi_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;"
```

---

### Langkah 2 — Edit File `.env`

Buka file `.env` menggunakan teks editor, cari bagian `DB_*` dan sesuaikan:

```env
APP_NAME="KoperasiApp"
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=koperasi_db
DB_USERNAME=root
DB_PASSWORD=          # isi password MySQL kamu (kosong jika tidak ada)
```

> ⚠️ **Perhatikan:**
>
> - Jika menggunakan **Laragon** → password biasanya `root`
> - Jika menggunakan **XAMPP** → password biasanya **kosong**
> - Jika menggunakan **WAMP** → sesuaikan dengan konfigurasi instalasi

---

### Langkah 3 — Jalankan Migrasi & Seeder

```bash
php artisan migrate --seed
```

Perintah ini akan:

- ✅ Membuat semua tabel di database
- ✅ Mengisi data awal: 3 akun pengguna + 10 anggota contoh

Output yang diharapkan:

```
INFO  Running migrations.
  ...semua tabel dibuat...

INFO  Seeding database.
  ✅ Users seeded: admin/admin123 · kasir/kasir123 · distributor/dist123
  ✅ Anggota seeded: 10 anggota dengan data simpanan
```

> ⚠️ **Jika ingin reset database dari awal** (hapus semua data):
>
> ```bash
> php artisan migrate:fresh --seed
> ```

---

## 4. Menjalankan Aplikasi

```bash
php artisan serve
```

Buka browser dan akses: **http://127.0.0.1:8000**

> 💡 **Tips:** Biarkan terminal tetap terbuka selama menggunakan aplikasi.  
> Untuk menghentikan server, tekan `Ctrl + C` di terminal.

---

## 5. Akun Default

Setelah seeder berhasil dijalankan, gunakan akun berikut untuk login:

| Role               | Username      | Password   | Akses                                            |
| ------------------ | ------------- | ---------- | ------------------------------------------------ |
| 🔴 **Admin**       | `admin`       | `admin123` | Full access: semua modul + laporan               |
| 🟡 **Kasir**       | `kasir`       | `kasir123` | Transaksi: anggota, simpanan, pinjaman, deposito |
| 🔵 **Distributor** | `distributor` | `dist123`  | Akses terbatas                                   |

> 🔒 **Keamanan:** Segera ganti password setelah pertama kali login!

---

## 6. Panduan Penggunaan

### 6.1 Login

1. Buka browser → akses `http://127.0.0.1:8000`
2. Masukkan **Username** dan **Password**
3. Centang "Ingat Saya" jika ingin sesi tersimpan
4. Klik **Masuk ke Sistem**

---

### 6.2 Dashboard

Setelah login, Anda akan melihat halaman Dashboard dengan informasi:

| Kartu Statistik       | Keterangan                                  |
| --------------------- | ------------------------------------------- |
| 👥 **Total Anggota**  | Jumlah anggota dengan status aktif          |
| 💰 **Total Simpanan** | Total saldo bersih simpanan (setor - tarik) |
| 🏦 **Pinjaman Aktif** | Jumlah pinjaman yang sedang berjalan        |
| ⚠️ **Menunggak**      | Jumlah pinjaman yang melewati jatuh tempo   |

**Fitur Dashboard:**

- 📊 **Grafik Aktivitas Simpanan** — Tren setoran & penarikan 6 bulan terakhir
- ⏰ **Angsuran Jatuh Tempo** — Daftar angsuran yang akan jatuh tempo dalam 7 hari
- 📋 **Transaksi Terakhir** — 5 transaksi simpanan terbaru
- 💳 **Pinjaman Terbaru** — 5 pinjaman terbaru

---

### 6.3 Manajemen Anggota

#### Melihat Daftar Anggota

- Klik menu **Anggota** di sidebar
- Gunakan kotak pencarian untuk mencari berdasarkan nama, nomor anggota, atau NIK
- Filter berdasarkan status: **Aktif** / **Tidak Aktif**

#### Mendaftarkan Anggota Baru

1. Klik tombol **Tambah Anggota**
2. Isi formulir:
    - **Nama Lengkap** — sesuai KTP
    - **NIK** — 16 digit nomor KTP (harus unik)
    - **No. WhatsApp** — opsional
    - **Tanggal Bergabung** — otomatis terisi hari ini
    - **Alamat** — opsional
    - **Setoran Awal** — minimal Rp 20.000 (dicatat sebagai Simpanan Pokok)
3. Klik **Daftarkan Anggota**
4. Nomor anggota otomatis: `A-2026-0001`

#### Melihat Detail Anggota

- Klik tombol **Detail** → tampil informasi lengkap:
    - Data pribadi
    - Saldo simpanan real-time
    - Riwayat 10 transaksi simpanan terakhir
    - Riwayat pinjaman

#### Edit Data Anggota

1. Klik **Detail** → klik **Edit Data**
2. Ubah data yang diperlukan → klik **Simpan Perubahan**

#### Nonaktifkan Anggota

- Klik **Nonaktif** di baris anggota
- ⚠️ Anggota yang memiliki pinjaman aktif **tidak dapat** dinonaktifkan

---

### 6.4 Simpanan

#### Mencatat Setoran

1. Klik menu **Simpanan** → tombol **Catat Setoran**
2. Pastikan tab **⬆ Setoran** aktif
3. Isi formulir → klik **💾 Simpan Setoran**

#### Mencatat Penarikan

1. Klik tombol **Penarikan** (merah) atau tab **⬇ Penarikan**
2. Isi formulir
3. ⚠️ Saldo setelah penarikan **tidak boleh kurang dari Rp 20.000**
4. Klik **💸 Proses Penarikan**

**Jenis Simpanan:**

| Jenis        | Keterangan                              |
| ------------ | --------------------------------------- |
| **Pokok**    | Setoran awal saat mendaftar             |
| **Wajib**    | Iuran bulanan rutin                     |
| **Sukarela** | Tabungan bebas, bisa ditarik kapan saja |

---

### 6.5 Pinjaman

#### Alur Pinjaman

```
Pengajuan → Menunggu → [Admin: Cairkan] → Aktif → Bayar Angsuran → Lunas
```

#### Mengajukan Pinjaman Baru

1. Klik menu **Pinjaman** → tombol **Ajukan Pinjaman**
2. Isi formulir (anggota, jumlah, bunga, tenor)
3. Kalkulator otomatis menampilkan angsuran per bulan
4. Klik **📤 Ajukan Pinjaman** → status: **⏳ Menunggu**

#### Mencairkan Pinjaman _(Admin only)_

1. Buka **Detail** pinjaman berstatus Menunggu
2. Klik **💸 Cairkan Sekarang**
3. Sistem otomatis:
    - Mengubah status → **Aktif**
    - Dana masuk ke simpanan sukarela anggota
    - Membuat jadwal angsuran otomatis

#### Membayar Angsuran

1. Buka **Detail** pinjaman → lihat tabel Jadwal Angsuran
2. Klik **✓ Bayar** pada angsuran yang ingin dibayar
3. Denda keterlambatan dihitung otomatis: **0.5% × nominal × hari terlambat**

**Kode Warna Status Angsuran:**

| Warna     | Status      | Keterangan                |
| --------- | ----------- | ------------------------- |
| 🟢 Hijau  | Lunas       | Sudah dibayar             |
| 🔴 Merah  | Menunggak   | Melewati jatuh tempo      |
| 🟡 Kuning | Jatuh Tempo | Akan jatuh tempo ≤ 7 hari |
| ⬜ Putih  | Belum Lunas | Belum waktunya            |

---

### 6.6 Deposito

#### Membuka Deposito Baru

1. Klik menu **Deposito** → tombol **Buka Deposito**
2. Isi formulir (anggota, nominal min. Rp 1 juta, bunga, jangka waktu)
3. Proyeksi otomatis menampilkan bunga dan total cair
4. Klik **💎 Buka Deposito**
5. Dana didebet otomatis dari simpanan sukarela anggota

> ⚠️ Saldo harus mencukupi: **nominal deposito + Rp 20.000**

#### Mencairkan Deposito

1. Pada tabel deposito → klik tombol **Cairkan**
2. Dana (pokok + bunga) otomatis masuk ke simpanan sukarela anggota

---

### 6.7 Laporan _(Admin Only)_

| Laporan     | Menu                      | Keterangan                              |
| ----------- | ------------------------- | --------------------------------------- |
| **Bulanan** | Laporan → Laporan Bulanan | Rekap transaksi per bulan + export PDF  |
| **Tahunan** | Laporan → Laporan Tahunan | Rekap per bulan dalam satu tahun        |
| **SHU**     | Laporan → Laporan SHU     | Distribusi Sisa Hasil Usaha per anggota |

---

## 7. Hak Akses Per Role

| Fitur                    | Admin | Kasir | Distributor |
| ------------------------ | :---: | :---: | :---------: |
| Dashboard                |  ✅   |  ✅   |     ✅      |
| Lihat Anggota            |  ✅   |  ✅   |     ✅      |
| Tambah / Edit Anggota    |  ✅   |  ✅   |     ❌      |
| Transaksi Simpanan       |  ✅   |  ✅   |     ❌      |
| Ajukan Pinjaman          |  ✅   |  ✅   |     ❌      |
| **Cairkan Pinjaman**     |  ✅   |  ❌   |     ❌      |
| Bayar Angsuran           |  ✅   |  ✅   |     ❌      |
| Kelola Deposito          |  ✅   |  ✅   |     ❌      |
| **Laporan & Export PDF** |  ✅   |  ❌   |     ❌      |

---

## 8. Troubleshooting

### ❌ `No application encryption key`

```bash
php artisan key:generate
```

---

### ❌ `SQLSTATE[HY000] [2002] Connection refused`

- Pastikan MySQL sudah berjalan (Laragon / XAMPP → Start MySQL)
- Cek `DB_PORT` di `.env` → biasanya `3306`
- Cek `DB_PASSWORD` di `.env`

---

### ❌ `Class not found` / `Target class does not exist`

```bash
composer dump-autoload
php artisan cache:clear
php artisan config:clear
```

---

### ❌ Halaman 404 saat akses route

```bash
php artisan route:clear
php artisan cache:clear
```

---

### ❌ `Access denied for user 'root'@'localhost'`

Edit `.env`:

```env
DB_PASSWORD=root    # sesuaikan dengan password MySQL kamu
```

---

### ❌ Tampilan CSS tidak muncul / halaman polos

Pastikan file `public/css/design-system.css` ada.

---

### ❌ Server tidak bisa diakses setelah menutup terminal

```bash
php artisan serve
```

---

### 🔄 Reset Aplikasi ke Kondisi Awal

> ⚠️ **PERINGATAN: Semua data akan terhapus!**

```bash
php artisan migrate:fresh --seed
```

---

## 9. Struktur Folder

```
koperasi-app/
├── app/
│   ├── Http/Controllers/     ← Logika aplikasi per modul
│   ├── Http/Middleware/      ← Guard akses per role
│   └── Models/               ← Model database
├── database/
│   ├── migrations/           ← Struktur tabel database
│   └── seeders/              ← Data awal (users, anggota)
├── public/
│   └── css/design-system.css ← File CSS utama (JANGAN HAPUS)
├── resources/views/          ← Tampilan halaman (Blade)
├── routes/web.php            ← Semua routing URL
├── .env                      ← Konfigurasi environment ⚠️
└── composer.json             ← Dependensi PHP
```

---

## ⚡ Quick Start (Ringkasan 6 Langkah)

```bash
# 1. Install dependensi
composer install

# 2. Buat .env dari template
copy .env.example .env     # Windows
# cp .env.example .env     # Linux/Mac

# 3. Generate app key
php artisan key:generate

# 4. Edit .env → sesuaikan DB_DATABASE, DB_USERNAME, DB_PASSWORD

# 5. Buat tabel dan isi data awal
php artisan migrate --seed

# 6. Jalankan server
php artisan serve
```

**Buka browser:** http://127.0.0.1:8000  
**Login:** `admin` / `admin123`

---

_KoperasiApp v1.0 — Dikembangkan dengan Laravel 13 · PHP 8.x · MySQL 8.x_
