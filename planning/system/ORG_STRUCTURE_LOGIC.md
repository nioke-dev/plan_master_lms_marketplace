# Logika Struktur Organisasi (Leveling)

Sistem LMS SIG mendukung struktur organisasi yang fleksibel dengan maksimal 5 level:

- **Level 1**: Perusahaan (Company) — *Wajib ada*
- **Level 2**: Direktorat / Group Head / Direktur — *Opsional/Fleksibel*
- **Level 3**: Group Head — *Opsional*
- **Level 4**: Departemen — *Opsional*
- **Level 5**: Unit — *Opsional*

### Karakteristik Implementasi:
1. **Fleksibilitas**: Seorang karyawan bisa saja hanya terdaftar di Level 1 dan Level 2 (misal: Direktorat Human Capital) tanpa memiliki Departemen atau Unit di bawahnya.
2. **Penamaan**: Label untuk setiap level bisa berbeda tergantung pada entitas perusahaan (misal: di SIG Pusat vs SISI).
3. **Tampilan Profil**: Kartu informasi organisasi hanya boleh menampilkan level yang memiliki data. Jangan menampilkan label kosong jika karyawan tersebut tidak memiliki Unit atau Departemen.

### Keamanan Data:
- Hanya menampilkan informasi umum (Email, NIK, Telepon).
- Data sensitif (Tanggal Lahir, Alamat Rumah, dll) tidak ditampilkan untuk menjaga privasi karyawan.
- Tombol update data hanya ditujukan untuk pembaruan Email (untuk saat ini).
