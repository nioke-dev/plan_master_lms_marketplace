# TO-BE PROCESS & MVP BLUEPRINT (LMS SIG)

*Dokumen ini merupakan Cetak Biru (Blueprint) dari rancangan sistem LMS Baru secara Minimum Viable Product (MVP). Sistem ini dirancang untuk memotong rantai manual AS-IS, menggunakan pendekatan "Strategi Dua Kaki" (Fleksibilitas Input), dan memfasilitasi proses dari hulu (TNA) hingga hilir (Komersialisasi Publik).*

---

## 1. Arsitektur Dasar MVP (2 Minggu UAT Scope)
- **Data Injeksi:** Menggunakan fitur *Batch Import Excel*. Jika ditemukan 1 baris eror, sistem menolak seluruh file (Strict Validation).
- **Role Switching:** Mendukung satu user memiliki banyak role dengan fitur "Switch Dashboard" tanpa *logout*.
- **Audit Logging:** Seluruh aktivitas teknis oleh **Helpdesk Administrator** dicatat secara rinci (siapa, apa, kapan).
- **Payment Gateway:** Integrasi **Midtrans**. Sistem mengirimkan **Email Pengingat** otomatis (misal: 2 jam sebelum batas akhir 24 jam) jika pembayaran masih *pending*.

---

## 2. MODUL 1: Hulu - Dasbor Pengajuan TNA (Learning Coordinator)
- **Form Input TNA:** Dropdown Kategori, Judul, Objektif (**Bullet Points**), dan Pemilihan Peserta (**Checkbox per Unit**).
- **Feedback Loop:** LC menerima notifikasi (Dashboard & Email) jika TNA-nya di-*merge*.

---

## 3. MODUL 2: Strategi Perencanaan (Admin Coordinator)
- **Merging & Blueprinting:** Menggabungkan usulan sejenis menjadi satu *Course Blueprint*.
- **Deadline Monitoring:** Admin Coordinator mengatur tenggat waktu pembuatan materi. Muncul indikator visual Merah jika SME melewati deadline. 

---

## 4. MODUL 3: Produksi Konten & Quiz Engine (SME)
- **Content Setup:** SME menyusun rincian Bab (Layer), mengunggah PPT, eBook, dan **Ringkasan Materi (Text/PDF)** untuk tiap bab.
- **Quiz Engine:** Kreator kuis fleksibel (per Bab atau Akhir Kelas).
- **Project Assessment:** SME menilai tugas praktikum secara manual.
- **Locking Mechanism:** Materi kuis & konten otomatis terkunci (Read-Only) bagi SME setelah status kelas "Published".

---

## 5. MODUL 4: Orkestrasi Hilir (Learning Administrator)
- **Onboarding Engine:** Saat import user, Admin memilih: (A) Kirim Email "Welcome & Set Password" Otomatis, atau (B) Aktivasi manual nanti.
- **Video Injection:** Dua opsi aktif; (1) **Private YouTube Embed**, (2) **Direct Upload Manual**.
- **Certificate Engine (No-Code):** Visual UI untuk mengatur posisi teks (Nama, Nomor, Tanggal) pada template gambar.
- **Survey & Financial:** Mengelola survei L1/L3 dan memantau histori transaksi Midtrans (Exportable to Excel).

---

## 6. MODUL 5: Pengalaman Belajar (Learner - Karyawan & Publik)
- **Access Duration:** 
  - **Digital Only:** Akses Selamanya (Lifetime).
  - **Digital + Workshop:** Wajib menyelesaikan materi digital sebelum tenggat waktu workshop dimulai. Jika terlambat, sertifikat workshop tidak dapat diraih.
- **Evaluation Flow:** Peserta wajib mengisi Survei L1 sebelum dapat mengunduh sertifikat digital.
- **Impact Measurement:** Sistem mengirimkan link Survei L3 otomatis via email tepat 3 bulan setelah kelulusan.

---

## 7. MODUL 6: Pengawasan & Analitik (Senior Manager CLD)
- **Executive Dashboard:** Memantau Revenue, Sales, dan Grafik Kepuasan.
- **Reporting Engine:** Fitur ekspor data ke **Excel** (untuk data mentah) dan **PDF** (untuk laporan visual/grafik).

---

## 8. MODUL 7: Technical Support (Helpdesk Administrator)
- **Full Access Power:** Koreksi teknis lintas role (Edit/Delete/Add data), Reset Password, dan pembatalan pendaftaran.

---
*(Buku Suci LMS SIG. Zero Gap. Siap untuk Eksekusi).*
