# Authentication Module

## Layer 1: Overview & Aktor
Modul Autentikasi menangani verifikasi identitas, manajemen sesi, dan kontrol akses. Mengingat kompleksitas *Enterprise* dari Semen Indonesia Group (SIG), sistem autentikasi memegang teguh prinsip keamanan dan pemisahan logika bisnis. Sistem dibagi menjadi **Dua Portal Terpisah** untuk memisahkan lalu lintas peserta didik (Publik & Karyawan) dari area administratif yang sangat teknis.

**Aktor Terlibat:**
- Portal Learner (Peserta): `public`, `employee`
- Portal Manajemen (Teknis): `learning_administrator`, `learning_coordinator`, `admin_coordinator`, `sme`, `helpdesk_admin`

---

## Layer 2: Fitur Utama
**Portal Learner (Public & Employee):**
1. **Identifier-First Login:** Autentikasi menggunakan metode 1 kolom input awal (hanya menanyakan Email).
2. **Automasi Setup Karyawan Baru:** Pengiriman instan "Link Setup Password" via email apabila data Karyawan kosong/belum memiliki *password* dari HR.
3. Login via Google SSO (Oauth) dengan "Polisi Tidur" untuk mencegah Karyawan Gaptek salah pencet.
4. Register (Eksklusif hanya untuk Publik).
5. Session Persistence (Kembali ke Landing Page promo tanpa putus sesi).

**Portal Manajemen (Technical Roles):**
1. Login Backoffice Tersembunyi (Berbasis Email & Password kaku, UI statis tanpa Register/Google).

**Global Tools:**
1. Logout (Pemusnahan Session).
2. Reset Password Khusus Email.

---

## Layer 3: Fungsionalitas & Logika Bisnis (CRUD & Edge Cases)

### A. Logika Portal Learner (Front-End) - Identifier First
- **Top Bar Navigasi (Landing Page):** 
  - Saat mode *guest*, tombol pojok kanan atas bertuliskan **"Masuk / Learning Center"**. 
  - Setelah berhasil masuk, tombol "Masuk" berubah menjadi **"Profil / Dasbor [Nama User]"**. Session menempel, klik tombol ini akan kembali melempar ke Dashboard tanpa perlu login diulang.

- **Alur Login 1 Kolom (Identifier First):**
  - **Skenario Ideal:** Layar Login HANYA memunculkan 1 kolom bertuliskan: *"Email ID Anda"*. Tidak ada kotak pilihan peran, tidak ada kolom password.
  - **Langkah 1:** User (Misal: Karyawan) mengetik `budi@sig.id` lalu klik 'Lanjut'.
  - **Langkah 2 (Pengecekan):** Sistem mencari email di DB.
    - *Kasus A (Karyawan Belum Setup):* Sistem mendeteksi bahwa field `password` si Budi itu `null` (hasil import HR kosongan). Maka kolom password **tidak akan dimunculkan**. Sistem justru mengirim "Link Setup Akun Baru" ke email Budi via API Fortify, lalu layar UI berubah jadi pesan sukses: *"Silakan cek masuk email kantor Anda untuk membuat Password perdana."*
    - *Kasus B (Pengguna Normal/Sudah Setup):* Sistem mendeteksi `password` user sudah ada isinya. Barulah di langkah ini secara *seamless* (tanpa reload), kolom "Password" muncul untuk diisi normal.
    - *Kasus C (Technical Role Salah Portal):* Jika email yang dimasukkan adalah email dari role teknis (Admin Coordinator, SME, Learning Administrator, dll), maka sistem **otomatis memblokir** dan memunculkan *alert*: *"Akun Anda adalah akun Manajemen. Harap login melalui portal eksklusif (Back-Office)."*
    - *Kasus D (Publik Tidak Dikenal):* Budi mengetik `budi@gmail.com` dan belum ada di DB. Sistem menolak dan memunculkan notifikasi merah yang menunjuk ke menu "Register di Sini".

- **Alur "Login With Google" (SSO & Jaring Pengaman Gaptek):**
  - Fitur *"Login With Google"* ditempatkan di bawah form email.
  - **Skenario Publik:** Jika Publik sejati mengklik ini, sistem membuatkan akun Public Database dengan *password hash* acak *on the fly*, lalu langsung meloloskannya ke _Dashboard_.
  - **Skenario Karyawan Valid (`@sig.id`):** Jika dia masuk via Google SSO, sistem mengecek *Database*. Jika status `password` miliknya masih `NULL`, maka **Akses SSO ditahan sementara**. Sistem akan mengiriminya '*Email Setup Password*' dan menyuruhnya membuat *password* internal dulu demi kepatuhan keamanan, sebelum dia bisa mulai belajar.
  - **Skenario Jaring Pengaman:** Jika Karyawan Gaptek mengklik logo Google tapi ponselnya tertaut pada `ekagaptek@gmail.com` pribadinya, Server kita membunyikan alarm: *"Tunggu dulu! Akun email ini tidak terdeteksi sebagai karyawan kantor. Akun Anda akan kami jadikan warga Publik. Jika Anda Karyawan SIG yang berniat ikut kelas penugasan, silakan BATALKAN dan kembali isi menggunakan Email Perusahaan Anda"*.

### B. Logika Portal Manajemen (Technical Back-Office)
- **Pemisahan Jalur Subdomain:** Menggunakan arsitektur keamanan *Enterprise*, yaitu isolasi melalui **Subdomain** (Misal: `admin.lms.sig.com` atau `management.lms.sig.com`), bukan *subfolder/routing* (bukan `/admin/login`). Hal ini mencegah pencurian sesi *cookie* dan memungkinkan pemblokiran IP Intranet via *Firewall* SIG.
- **Keamanan Statis (Kaku):** Portal ini tidak mentolerir SSO atau Register mandiri. Form meminta Email + Password sejak detik pertama. Semua data penggunanya murni diinjeksi (*seeded*) oleh IT/Pusat.
- **Role Middleware Protection:** *User* dengan peran `public` atau `employee` yang iseng menebak *password* di portal belakang ini akan otomatis diblokir dengan *error 403 (Unauthorized)*.

---

## Layer 4: Skenario Testing Jaminan Keamanan (UAT)

Skenario-skenario ekstrem (*Edge Cases*) yang harus sukses diuji sebelum Modul ini dinyatakan lulus:

1. **Uji Coba Password Kosong (First-Time Login):**
   - Aktor menginput email karyawan yang *password*-nya *NULL* di form.
   - *Ekspektasi:* Form *password* tidak muncul, sistem memanggil *API Mailer* dan berhasil mengirim tautan *Setup Password* ke tujuan.
2. **Uji Coba Celah SSO Google (Pemaksaan Setup):**
   - Aktor *login Google* menggunakan `@sig.id` yang *password*-nya di *database* masih *NULL*.
   - *Ekspektasi:* Sistem mencekal (*intercept*) kemudahan SSO dan memaksa pengguna mengonfirmasi tautan keamanan di *email*-nya sebelum divalidasi sistem.
3. **Uji Coba Pencampuran Identitas (Karyawan Ikut Kelas Publik):**
   - Seorang karyawan *logout* akun komersilnya, lalu memasukkan email `@gmail.com` miliknya di layar utama.
   - *Ekspektasi:* Sistem tidak membentrokkan data. Sistem menganggapnya murni sebagai peran `public`.
4. **Uji Coba Penetrasi Subdomain Back-Office:**
   - Aktor mengetik URL `management.lms.sig.com` dan *login* menggunakan akun yang ber-role Publik/Karyawan Normal.
   - *Ekspektasi:* *Middleware* seketika menendang pengguna tersebut dengan *error Forbidden* (Akses Ditolak).
