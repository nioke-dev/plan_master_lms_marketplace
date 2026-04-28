# Fitur Utama: Learning Coordinator (LC)

Dokumen ini merinci seluruh kapabilitas fungsional yang dimiliki oleh peran **Learning Coordinator** dalam sistem LMS SIG.

## 1. Dashboard Analytics (Pemantauan Visual)
LC memiliki akses ke dashboard eksekutif yang menyajikan data TNA secara visual untuk membantu pengambilan keputusan cepat.
- **Trend Pengajuan**: Grafik area yang menunjukkan volume usulan TNA per bulan.
- **Distribusi Status**: Grafik donut yang membagi usulan ke dalam 4 kategori status: Approved, Review, Draft, dan Rejected.
- **Summary Cards**: Ringkasan angka total usulan berdasarkan status yang dapat diklik untuk navigasi cepat.

## 2. Manajemen Usulan TNA (TNA Management)
Fitur inti untuk mengelola seluruh siklus hidup usulan pelatihan.
- **Pendaftaran Usulan Baru**: Form input detail pelatihan (Judul, Kategori, Kompetensi, Peserta, dll).
- **Daftar Usulan Interaktif**: Tabel yang mendukung pencarian, filter kategori, dan filter rentang tanggal.
- **Detail Usulan**: Melihat seluruh informasi usulan yang telah dikirim.
- **Edit Usulan**: Mengubah data usulan (Hanya berlaku untuk status **Draft** dan **Reviewing**).
- **Hapus Usulan**: Menghapus data usulan dari sistem (Hanya berlaku untuk status **Draft** dan **Reviewing**).

## 3. Ekspor Laporan (Reporting)
- **Smart Export Excel**: Mengunduh data usulan TNA ke format .xlsx yang hasilnya otomatis tersaring sesuai dengan filter (search, category, status, date) yang sedang aktif di layar.

## 4. Navigasi Role (Role Switching)
- **Ubah Peran**: Kemampuan untuk berpindah ke peran lain (jika user memiliki lebih dari satu peran) tanpa harus logout, memastikan fleksibilitas operasional.
