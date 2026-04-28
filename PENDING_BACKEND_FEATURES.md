# 📋 Checklist Fungsi Backend (Universal Features)

Dokumen ini mencatat fitur-fitur universal yang UI-nya sudah diimplementasikan namun fungsi backend-nya masih bersifat *mockup* atau belum dihubungkan ke database. Fitur-fitur ini akan diaktifkan secara menyeluruh di tahap akhir pengujian.

## 1. Keamanan & Akun
- [ ] **Ganti Kata Sandi**: 
    - UI: Selesai.
    - Validasi Real-time: Selesai.
    - Backend: Proses `Hash::make()` dan update kolom `password` di database.
- [ ] **Lupa Sandi**:
    - UI: Tombol sudah tersedia di halaman Ganti Password dan Login.
    - Backend: Integrasi dengan Laravel Fortify/Mail untuk pengiriman link reset.

## 2. Profil & Data Personal
- [ ] **Perbarui Data (Email Update)**:
    - UI Modal: Selesai.
    - Backend: Validasi email unik dan update kolom `email` di database.
- [ ] **Integrasi Struktur Organisasi**:
    - UI: Selesai (Dynamic Level 1-5).
    - Backend: Memastikan data diambil dari relasi tabel `organizations` yang benar.

## 3. Akses Sistem
- [ ] **Ubah Peran (Role Switcher)**:
    - UI Modal: Selesai (Global Overlay).
    - Backend: Logika perpindahan `guard` atau session role saat tombol "Gunakan Peran Ini" diklik.

## 4. Help Center
- [ ] **Pusat Bantuan**:
    - UI: Selesai (Link di footer).
    - Backend: Penentuan redirect ke WhatsApp Support atau Ticketing System.

---
> **Catatan**: Fitur ini sengaja ditunda agar fokus saat ini tetap pada pengembangan modul spesifik setiap Role (seperti TNA untuk Learning Coordinator).
