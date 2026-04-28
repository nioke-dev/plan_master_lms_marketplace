# Skenario Pengujian Detail: Validasi Antarmuka & Database

Dokumen ini digunakan sebagai panduan QA (Quality Assurance) untuk memastikan data yang diinput oleh Learning Coordinator masuk ke database dengan sempurna.

## 1. Pengujian: Pembuatan Usulan Baru (Create TNA)

### Langkah Pengujian:
1. Masuk ke halaman **"Buat Usulan Baru"**.
2. Isi form dengan data berikut:
   - Judul: "Pelatihan Advanced Laravel 11"
   - Kategori: "Technical"
   - Estimasi Biaya: "5.000.000"
   - Peserta: Pilih "Budi" dan "Ani".
3. Klik tombol **"Kirim Usulan"**.

### Verifikasi Antarmuka (UI):
- Pastikan muncul notifikasi sukses: *"Usulan berhasil dikirim!"*.
- Pastikan halaman otomatis pindah ke **"Daftar Usulan"**.
- Pastikan baris paling atas di tabel menampilkan "Pelatihan Advanced Laravel 11" dengan status **"Reviewing"**.

### Verifikasi Database (DB Check):
Lakukan query berikut: `SELECT * FROM tna_submissions ORDER BY id DESC LIMIT 1;`
**Ekspektasi Data pada Row Terakhir:**
- Kolom `name`: Wajib berisi "Pelatihan Advanced Laravel 11".
- Kolom `status`: Wajib berisi "reviewing".
- Kolom `budget_estimate`: Wajib berisi "5000000".
- Kolom `created_by`: Wajib berisi ID user dari LC yang sedang login.

**Cek Tabel Relasi (Participants):**
Query: `SELECT * FROM tna_submission_participants WHERE tna_submission_id = [ID_DI_ATAS];`
- Pastikan jumlah row yang muncul adalah **2**.
- Pastikan `user_id` cocok dengan ID "Budi" dan "Ani".

## 2. Pengujian: Integritas "Save to Draft"

### Langkah Pengujian:
1. Isi form usulan hanya bagian **Judul** saja: "Draft Pelatihan AI".
2. Klik tombol **"Simpan ke Draft"**.

### Verifikasi Database (DB Check):
Query: `SELECT * FROM tna_submissions ORDER BY id DESC LIMIT 1;`
**Ekspektasi Data:**
- Kolom `name`: "Draft Pelatihan AI".
- Kolom `status`: "draft".
- Kolom `tna_category_id`: Wajib **NULL** (karena tidak diisi).
- Kolom `description`: Wajib **NULL** atau string kosong.

## 3. Pengujian: Filter & Konsistensi Data (Display Test)

### Langkah Pengujian:
1. Di halaman **"Daftar Usulan"**, pilih filter Kategori: "Technical".
2. Hitung jumlah baris yang muncul di tabel (misal: 5 baris).

### Verifikasi Database (DB Check):
Query: `SELECT COUNT(*) FROM tna_submissions WHERE tna_category_id = [ID_TECHNICAL];`
- Pastikan angka hasil query DB (misal: 5) **SAMA PERSIS** dengan jumlah baris yang tampil di browser.
- Jika di DB ada 5 tapi di browser ada 3, maka sistem pagination atau query filter di controller bermasalah.

## 4. Pengujian: Keamanan Aksi (Security Test)
### Skenario: Paksa Edit data "Approved" lewat URL
1. Cari ID usulan yang statusnya sudah **"Approved"** di database (Misal ID: 99).
2. Paksa akses URL: `/learning-coordinator/tna/99/edit` lewat address bar browser.

### Ekspektasi:
- Sistem **HARUS** menampilkan halaman error 403 (Forbidden) atau dialihkan kembali ke daftar usulan dengan pesan: *"Data yang sudah disetujui tidak dapat diubah."*
- Pastikan di database, data dengan ID 99 **TIDAK BERUBAH** sedikitpun.
