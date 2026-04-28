# Aturan Bisnis, Validasi, dan Pengecualian

Dokumen ini mendefinisikan logika validasi ketat yang harus dipatuhi oleh sistem untuk peran Learning Coordinator guna menjaga integritas data.

## 1. Validasi Pembuatan Usulan TNA

### A. Skenario: Simpan sebagai Draft (Save to Draft)
Sistem memberikan kelonggaran pada tahap draft, namun tetap memiliki batas minimum:
- **Wajib Diisi**: `Judul Pelatihan` (Minimal 5 karakter). Pelatihan tidak bisa diidentifikasi tanpa judul.
- **Boleh Kosong**: `Kategori`, `Kompetensi`, `Tujuan`, `Estimasi Biaya`, dan `Daftar Peserta`.
- **Exception**: Jika klik "Simpan ke Draft" tapi `Judul Pelatihan` kosong, sistem harus menampilkan error: *"Judul pelatihan wajib diisi meskipun hanya draft."*

### B. Skenario: Kirim Usulan (Submit for Review)
Saat usulan dikirim untuk direview, validasi menjadi **sangat ketat**:
- **Judul Pelatihan**: Wajib diisi (Minimal 10 karakter).
- **Kategori Pelatihan**: Wajib dipilih dari dropdown yang tersedia.
- **Tujuan Pelatihan**: Wajib diisi (Minimal 20 karakter) untuk menjelaskan alasan bisnis.
- **Kompetensi**: Wajib memilih minimal 1 kompetensi yang terkait.
- **Daftar Peserta**: Wajib memilih minimal 1 karyawan sebagai peserta.
- **Exception**: Jika salah satu poin di atas kosong, sistem akan memblokir pengiriman dan menandai field yang merah dengan pesan: *"Lengkapi data ini sebelum mengirim usulan."*

## 2. Pengecualian Aksi Berdasarkan Status (Action Locking)

Sistem akan secara otomatis mengunci data yang sudah mencapai tahap akhir untuk mencegah manipulasi data yang sudah disetujui.
- **Status "Approved"**: Tombol **Edit** dan **Hapus** wajib dinonaktifkan (disabled). LC hanya boleh melihat detail.
- **Status "Rejected"**: Tombol **Edit** dan **Hapus** wajib dinonaktifkan. Catatan penolakan harus bisa dilihat di halaman detail.
- **Status "Reviewing"**: Tombol **Edit** dan **Hapus** tetap aktif jika Admin belum melakukan tindakan apapun (opsional tergantung kebijakan bisnis).

## 3. Validasi Filter dan Pencarian
- **Rentang Tanggal**: Tanggal akhir tidak boleh lebih kecil dari tanggal awal.
- **Exception**: Jika user memilih tanggal akhir di masa lalu dibanding tanggal awal, sistem otomatis mereset tanggal akhir sama dengan tanggal awal.
- **Empty State**: Jika hasil filter tidak ditemukan, sistem wajib menampilkan ilustrasi/teks: *"Data tidak ditemukan dengan kriteria tersebut."* dan memunculkan tombol **Reset Semua Filter**.
