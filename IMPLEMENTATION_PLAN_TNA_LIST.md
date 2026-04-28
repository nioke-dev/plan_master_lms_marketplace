# Implementation Plan: Enhancing TNA Submissions List (Phase 2)

Dokumen ini merinci rencana teknis lanjutan untuk menyempurnakan halaman **Daftar Usulan TNA**, termasuk fitur navigasi data, ekspor laporan, dan proteksi integritas data.

## User Review Required

> [!IMPORTANT]
> **Logika Tombol Aksi & Status**: 
> - **Detail**: Selalu aktif (Enabled) untuk semua status.
> - **Edit & Hapus**: Otomatis **Disabled** (Grayed out & Unclickable) jika status adalah **"Approved"** atau **"Rejected"**. Hanya aktif jika status **"Reviewing"** atau **"Draft"**.
> - **Export Excel**: Hanya mengekspor data yang muncul di tabel saat itu (sinkron dengan filter pencarian, kategori, dan status).

## Strategi Pengerjaan (Phased Execution)

Tugas ini dibagi menjadi 3 bagian (Part) agar pengerjaan lebih terukur dan mudah direview:

### **Part 1: UI Foundation & Filter Logic**
- **Target**: Memperbaiki sistem pencarian dan filter kategori.
- **Pekerjaan**:
    - [MODIFY] `index.blade.php`: Update Search Bar & Tambah Dropdown Kategori.
    - [MODIFY] `TnaSubmissionController.php`: Logika pencarian (ID TNA & Nama) dan filter kategori.
    - [MODIFY] `TnaSubmissionController.php`: Pengiriman data `categories` unik ke View.

### **Part 2: Advanced Table Logic (Pagination & Action Lock)**
- **Target**: Navigasi data dinamis dan proteksi integritas data.
- **Pekerjaan**:
    - [MODIFY] `index.blade.php`: Tambah "Entries Selector" (5, 10, 25, 50).
    - [MODIFY] `TnaSubmissionController.php`: Handle pagination dinamis (`per_page`).
    - [MODIFY] `index.blade.php`: Logika tombol **Edit & Hapus** (Disabled jika status Approved/Rejected).
    - [MODIFY] `index.blade.php`: Pastikan tombol **Detail** selalu aktif.

### **Part 3: Smart Export Excel**
- **Target**: Fitur laporan yang sinkron dengan filter.
- **Pekerjaan**:
    - [NEW] `TnaExport.php`: Definisi kolom dan styling file Excel.
    - [MODIFY] `index.blade.php`: Tambah tombol "Export Excel".
    - [MODIFY] `TnaSubmissionController.php`: Method `export()` yang menerapkan filter aktif sebelum unduh file.

### **Part 4: Dashboard Analytics & Premium UI Improvements**
- **Target**: Visualisasi data interaktif dan perbaikan responsivitas UI.
- **Pekerjaan**:
    - [MODIFY] `backoffice.blade.php`: Integrasi library ApexCharts.
    - [MODIFY] `index.blade.php` (Dashboard): Implementasi Trend Chart (Area) dan Distribusi Status (Donut).
    - [MODIFY] `index.blade.php` (Dashboard): Sinkronisasi data real-time untuk summary status (Draft, Review, Approved, Rejected).
    - [MODIFY] `daftar-usulan.blade.php`: Perbaikan sinkronisasi filter Litepicker dan mekanisme reset global.
    - [MODIFY] `daftar-usulan.blade.php`: Optimasi tabel responsif (horizontal scroll) dan text truncation.
    - [MODIFY] `lc-sidebar.blade.php`: Pembersihan navigasi sidebar (Hapus Katalog Kursus).

---

## Rencana Verifikasi

### Pengujian Fitur
- **Uji Pagination**: Ubah "Entries" ke 10, pastikan tabel menampilkan 10 data (jika tersedia).
- **Uji Dashboard**: Pastikan grafik Trend dan Donut muncul sempurna dengan data yang sinkron.
- **Uji Responsivitas**: Cek tabel di layar kecil, pastikan bisa di-scroll horizontal tanpa merusak layout.
- **Uji Filter Date**: Pastikan filter tanggal di Daftar Usulan berfungsi dan bisa di-reset dengan benar.
