# Analisis Proses Bisnis AS-IS (LMS SIG)

*Dokumen ini merupakan pencatatan langsung dari **business requirement** dan kondisi aktual operasional pelatihan di Semen Indonesia Group (SIG). Dokumen ini berfungsi sebagai fondasi pemahaman utama (Source of Truth) bagi pengembangan sistem LMS yang baru secara MVP.*

---

## 1. Latar Belakang & Asal Mula Proyek Skripsi (The Pivot)

Proyek pengembangan LMS ini bermula dari inisiatif internal ketika pengembang (mahasiswa) berstatus magang di PT Semen Indonesia (Persero) Tbk. Mentor perusahaan (yang di dalam sistem ini berperan sebagai *Learning Administrator*) memberikan tugas mandiri: **Menciptakan sistem untuk mengkomersialisasikan materi pelatihan teknis ke publik**. 

Pada tahap awal, target pembangunannya sangat lurus dan tidak muluk-muluk: Membuat aplikasi portal *e-learning* (B2C) standar, di mana admin mengunggah kelas, dan peserta publik bisa membelinya secara bebas. 

Namun, sebuah transisi besar (*pivot*) terjadi ketika Dosen Pembimbing Kampus menantang: **Sebuah skripsi tidak boleh lepas dari akar operasional internal, harus menyelesaikan masalah pada proses AS-IS yang sebenarnya terjadi.**

## 2. Penemuan Masalah Fundamental: Keterbatasan Ekosistem Saat Ini

Saat ini, operasi pelatihan korporat perusahaan didukung oleh sistem raksasa dari luar (*Catatan Rahasia: SAP SF LMS*, selanjutnya dipanggil **LMS Existing**).

Sistem raksasa ini memiliki dua titik lumpuh:
1. **Tidak Memiliki Kapabilitas Komersial:** Sama sekali tidak memiliki gerbang/API untuk menjual *courses* kepada khalayak publik eksternal.
2. **Under-utilisasi Fitur (Memicu Proses Manual):** Banyak fungsionalitas sistem yang tidak dioptimalkan oleh unit kerja karena kerumitan UI/UX atau kurangnya adaptasi. Berujung pada merebaknya proses manual yang "liar" di luar sistem (menggunakan Excel, Google Forms, WA).

Oleh karena itu, LMS Baru (skripsi) ini dirancang sebagai **Solusi Penengah (Middleware System)** yang fleksibel.

---

## 3. Pembedahan Alur Operasional (AS-IS) Internal

Berikut adalah peta jalan penuh operasional internal SIG dari hulu hingga ke hilir, yang sebagian besarnya terjadi di "ruang gelap" proses manual administratif.

### TAHAP 1: Hulu (The Learning Coordinator)
Terdapat ratusan *Learning Coordinator* yang merupakan perwakilan di masing-masing unit kerja/pabrik. Mereka wajib mengusulkan **Kebutuhan Pelatihan (Training Needs)** untuk stafnya.

* **Format Manual Liar:** Menggunakan Excel, Google Forms, Notes. Parameter yang dikumpulkan: (1) Kategori Pelatihan, (2) Deskripsi Singkat, (3) Nama Karyawan yang ditargetkan.
* **Fakta Konfirmasi (Tanpa Approval):** Tidak ada birokrasi *Approval* dari Manajer. *Learning Coordinator* diasumsikan bertindak selayaknya perpanjangan tangan manajer. File usulan TNA tersebut langsung dikirim mentah-mentah ke *Admin Coordinator*.

### TAHAP 2: Perencanaan & Sintesis (The Admin Coordinator)
*Admin Coordinator* menerima ratusan file liar dari *Learning Coordinator*. Tugas mereka merekap dan mencari irisan kebutuhan. Jika 5 pabrik minta kelas "Safety", digabung jadi 1 kelas.

* **Fakta Konfirmasi (Kamus Kompetensi Manual):** Dalam memetakan dan mengelompokkan hujan usulan ini, *Admin Coordinator* bekerja secara manual berdasarkan rekaan panduan *Kamus Kompetensi Baku* di luar sistem.
* **Course Framework:** Admin Coordinator mendefinisikan *Course Objective* (Tujuan kelas) dan *Course Content Layers* (Bab & Kurikulum) secara bebas/manual.

### TAHAP 3: Penciptaan "Daging" Materi (SME & Tim CLD)
*Admin Coordinator* memilih **SME (Subject Matter Expert)**. (Bisa Internal/Karyawan atau Eksternal/Partner). 
SME merakit materi (PPT, Soal Quiz). Kemudian SME diundang oleh **Unit of CLD** ke dalam studio khusus SIG untuk rekaman *Shooting* video kelas layaknya *Masterclass profesional*. Video diedit hingga paripurna oleh tim CLD.

### TAHAP 4: Muara Eksekusi Digital (The Learning Administrator & Employee)
Video matang dan Soal Quiz diserahkan ke **Learning Administrator** yang kemudian meng-uploadnya ke *LMS Existing* dan menyetting *Target Employee*. Karyawan menonton video dan mengerjakan kuis di dalam *LMS Existing*.
* **Fakta Konfirmasi (Jembatan Komunikasi Manual):** Untuk mengetahui karyawan mana yang harus di-*assign* ke *Course* tersebut, *Admin Coordinator* memberitahunya via WA/Email (manual forward data Excel) kepada *Learning Administrator*. Tidak ada *flow* data otomatis.

*(Terkait Integrasi Data, untuk MVP sistem skripsi selanjutnya, diputuskan bahwa **TIDAK ADA integrasi API** ke sistem kepegawaian SAP SF. Karyawan akan diinjeksi ke sistem baru menggunakan fitur **Excel Batch Import**. Begitu pula dengan riwayat video lama, dibiarkan terekam di LMS Existing karena sistem MVP fokus memanajemen program yang sepenuhnya baru).*

---

## 4. Hilir Eksekusi Manual (Workshop, Tugas, & Survei)

Bagian paling hilir dari kelas tipe *"Learning Digital + Workshop"* kondisinya **100% Manual**.

### A. Kekacauan Kelas Tipe Workshop (Praktikum)
- **Jadwal & Absensi:** Dilakukan manual via grup WhatsApp / Email. Kehadiran diabsen secara manual.
- **Pengumpulan & Penilaian Tugas (Celah Kehilangan Data):** Karena LMS Existing tidak memiliki fitur pengumpulan berkas tugas proyek, karyawan mengumpulkan tugas via *Email pribadi SME*. 
- **Input Nilai:** *SME* bertugas mengoreksi tugas Workshop tersebut, **NAMUN** nilai akhir lokakarya ini tidak dientri/tidak diwadahi oleh pelacakan sistem *LMS Existing* (sistem terputus/loss).

### B. Evaluasi & Manajemen Kualitas (Survei L1 & L3)
Sebagai pengukur kesuksesan organisasi, SIG menerapkan Survei L1 & L3 di luar sistem.
- **Survei Level 1 (Post-Class Satisfaction):** Setelah karyawan usai belajar digital, instrumen kepuasan dikirimkan terpisah (Google Form).
- **Survei Level 3 (Behavioral & Performance Impact):** Tiga bulan pasca-pelatihan, tautan GForm disebar oleh **Tim Unit CLD**.
- **Fakta Konfirmasi (Bermuara di Manajer Senior CLD):** Seluruh laporan *dashboard* performa (GForm L1 & L3) tidak diserahkan ke *Admin Coordinator*. Laporan tersebut dilaporkan dan dipresentasikan murni kepada *Senior Manager of CLD* sebagai eksekutor tunggal evaluasi kualitas. Jika ada perbaikan, *Senior Manager* akan merapatkannya (*meeting*) dengan *Admin Coordinator* secara *offline*.

### C. Sertifikat Kelulusan
Pembuatan sertifikat kelulusan baik untuk digital maupun workshop dikelola sebagai desain grafis manual dan dikirim secara terpisah (tidak ada _auto-generation_ pasca lulus skor di sistem).

---
*(Dokumen AS-IS FINAL. Siap menjadi landasan Sistem TO-BE).*
