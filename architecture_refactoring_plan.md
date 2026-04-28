# Enterprise-Grade Refactoring: Service Layer & DTO Strategy

## Deskripsi Goal
Meningkatkan standar arsitektur ke level Enterprise dengan menerapkan isolasi logika total, integritas data melalui transaksi, dan standarisasi pertukaran data menggunakan DTO terpusat.

## Standar Enterprise yang Diterapkan
> [!IMPORTANT]
> - **DTO Factory Pattern**: Konversi data dari Model ke DTO dilakukan secara internal di dalam DTO melalui static method (misal: `fromModel`).
> - **Transactional Integrity**: Penggunaan `DB::transaction` pada setiap operasi penulisan data (Write) di Service Layer.
> - **Strict Return Typing**: Setiap method Service wajib memiliki *return type* yang jelas (DTO, Collection of DTOs, atau spesifik object).
> - **Domain Exceptions**: Menggunakan Custom Exception untuk menangani kegagalan logika bisnis (bukan sekadar mengembalikan boolean).

## Rencana Perubahan Detail

### 1. DTO Layer (The Contract)
Setiap DTO akan memiliki pabrik data sendiri untuk menjamin konsistensi antara Database Model dan View.

#### [NEW] `App\DTOs\ParticipantDTO.php`
- Menampung data peserta: `name`, `nik`, `email`, `jabatan`, `initials`, `avatarColor`.
- Method `fromModel(User $user)`: Pabrik statis untuk konversi otomatis.

#### [NEW] `App\DTOs\TnaCategoryDTO.php`
- Menampung data kategori: `id`, `name`, `desc`.
- Method `fromModel(TnaCategory $category)`: Pabrik statis untuk konversi otomatis.

---

### 2. Service Layer (The Brain)
Pemisahan logika bisnis utama ke dalam Service yang terisolasi.

#### [NEW] `App\Services\OrganizationService.php`
- **`getParticipantsByLCScope(User $lc): Collection<ParticipantDTO>`**: Mengambil bawahan secara rekursif dan mengubahnya menjadi koleksi DTO.

#### [NEW] `App\Services\TnaService.php`
- **`storeSubmission(array $data): TnaSubmissionDTO`**:
    - Wajib menggunakan `DB::transaction()`.
    - Melempar `TnaSubmissionException` jika validasi bisnis (seperti kuota atau otorisasi) gagal.
- **`generateTnaId(): string`**: Logic standarisasi kode TNA.

---

### 3. Controller Refactoring (The Orchestrator)
Controller hanya bertugas menangkap request, memanggil service, dan menangkap Exception.

#### [MODIFY] `TnaSubmissionController.php`
- Menggunakan Dependency Injection untuk memanggil Service.
- Tidak ada lagi query Eloquent (`User::where...`) di dalam controller.
- Menggunakan blok `try-catch` untuk menangkap Custom Exception dari Service.

## Rencana Verifikasi
1. **Transaction Test**: Sengaja menggagalkan query di tengah proses untuk memastikan `rollback` berjalan.
2. **Type Consistency**: Memastikan semua data yang sampai ke Blade adalah instance dari DTO Class.
3. **Exception Handling**: Memastikan pesan error dari Service tampil dengan rapi di UI.
