# OpenMusic API v2

Ini adalah versi lanjutan dari proyek **[OpenMusic API v1](https://github.com/irfandwisamudra/openmusic-api-v1)**, yang kini dilengkapi dengan fitur **autentikasi**, **otorisasi**, dan **playlist**. Proyek ini merupakan tugas _submission_ untuk kelas "Belajar Fundamental Back-End dengan JavaScript" di Dicoding.

API ini berfungsi sebagai _back-end_ untuk aplikasi pemutar musik, menangani manajemen data album, lagu, pengguna, dan playlist secara aman dan terstruktur.

## Fitur

- [x] **Manajemen Album & Lagu (v1)**: Mempertahankan semua fungsionalitas CRUD dari v1.
- [x] **Registrasi & Autentikasi Pengguna**: Sistem registrasi dan _login_ yang aman menggunakan **JWT (JSON Web Token)**.
- [x] **Manajemen Playlist**: Pengguna dapat membuat, melihat, dan menghapus _playlist_ miliknya.
- [x] **Manajemen Lagu dalam Playlist**: Pengguna dapat menambah dan menghapus lagu dari _playlist_ miliknya.
- [x] **Otorisasi Berbasis Kepemilikan**: Memastikan hanya pemilik yang dapat memodifikasi _playlist_ atau menambahkan kolaborator.
- [x] **(Opsional) Kolaborasi Playlist**: Pemilik _playlist_ dapat mengundang pengguna lain untuk berkolaborasi (menambah/menghapus lagu).
- [x] **(Opsional) Aktivitas Playlist**: Mencatat dan menampilkan riwayat penambahan dan penghapusan lagu dalam sebuah _playlist_.

## Teknologi yang Digunakan

- **Node.js**: Lingkungan eksekusi JavaScript.
- **Hapi**: _Framework_ untuk membangun RESTful API.
- **PostgreSQL**: Sistem manajemen database relasional.
- **Joi**: Validasi skema data.
- **bcrypt**: _Hashing password_.
- **@hapi/jwt**: Pembuatan dan verifikasi JSON Web Token.
- **node-pg-migrate**: Alat untuk migrasi skema database.
- **dotenv**: Mengelola _environment variable_.
- **ESLint**: _Linter_ untuk menjaga konsistensi dan kualitas kode.

## Persiapan & Instalasi

Untuk menjalankan proyek ini secara lokal, ikuti langkah-langkah berikut:

#### 1\. Prasyarat

Pastikan Anda sudah menginstal perangkat lunak berikut:

- Node.js (v18 atau lebih baru direkomendasikan)
- PostgreSQL

#### 2\. Kloning Repositori

```bash
git clone https://github.com/irfandwisamudra/openmusic-api-v2.git
cd openmusic-api-v2
```

#### 3\. Instalasi Dependensi

```bash
npm install
```

#### 4\. Konfigurasi Database

Buat database baru di PostgreSQL untuk proyek ini.

```sql
CREATE DATABASE openmusic_v2;
```

#### 5\. Konfigurasi Environment Variable

Salin _file_ `.env.example` menjadi `.env`.

```bash
cp .env.example .env
```

Kemudian, buka _file_ `.env` dan sesuaikan nilainya dengan konfigurasi database lokal Anda (terutama `PGPASSWORD`). Jangan lupa untuk membuat kunci rahasia yang kuat untuk `ACCESS_TOKEN_KEY` dan `REFRESH_TOKEN_KEY`.

#### 6\. Migrasi Database

Jalankan migrasi untuk membuat semua tabel yang dibutuhkan (`albums`, `songs`, `users`, `authentications`, `playlists`, `playlist_songs`, `collaborations`, `playlist_song_activities`).

```bash
npm run migrate up
```

#### 7\. Menjalankan Server

Jalankan server dalam mode pengembangan (dengan _hot-reload_).

```bash
npm run start-dev
```

Server akan berjalan di `http://localhost:5000`.

## Dokumentasi API

Berikut adalah daftar _endpoint_ yang tersedia di API ini.

### Albums API

| Method   | Endpoint       | Deskripsi                                |
| :------- | :------------- | :--------------------------------------- |
| `POST`   | `/albums`      | Menambahkan album baru.                  |
| `GET`    | `/albums/{id}` | Mendapatkan detail album berdasarkan ID. |
| `PUT`    | `/albums/{id}` | Mengubah data album berdasarkan ID.      |
| `DELETE` | `/albums/{id}` | Menghapus album berdasarkan ID.          |

### Songs API

| Method   | Endpoint      | Deskripsi                                                 |
| :------- | :------------ | :-------------------------------------------------------- |
| `POST`   | `/songs`      | Menambahkan lagu baru.                                    |
| `GET`    | `/songs`      | Mendapatkan semua lagu (mendukung pencarian via _query_). |
| `GET`    | `/songs/{id}` | Mendapatkan detail lagu berdasarkan ID.                   |
| `PUT`    | `/songs/{id}` | Mengubah data lagu berdasarkan ID.                        |
| `DELETE` | `/songs/{id}` | Menghapus lagu berdasarkan ID.                            |

### Users & Authentications API

| Method   | Endpoint           | Deskripsi                                               |
| :------- | :----------------- | :------------------------------------------------------ |
| `POST`   | `/users`           | Registrasi pengguna baru.                               |
| `POST`   | `/authentications` | Login untuk mendapatkan _access_ & _refresh token_.     |
| `PUT`    | `/authentications` | Memperbarui _access token_ menggunakan _refresh token_. |
| `DELETE` | `/authentications` | Logout (menghapus _refresh token_).                     |

### Playlists API

| Method   | Endpoint          | Deskripsi                                                       |
| :------- | :---------------- | :-------------------------------------------------------------- |
| `POST`   | `/playlists`      | Menambahkan _playlist_ baru.                                    |
| `GET`    | `/playlists`      | Melihat daftar _playlist_ milik pengguna (termasuk kolaborasi). |
| `DELETE` | `/playlists/{id}` | Menghapus _playlist_.                                           |

### Playlist Songs API

| Method   | Endpoint                | Deskripsi                                |
| :------- | :---------------------- | :--------------------------------------- |
| `POST`   | `/playlists/{id}/songs` | Menambahkan lagu ke _playlist_.          |
| `GET`    | `/playlists/{id}/songs` | Melihat daftar lagu di dalam _playlist_. |
| `DELETE` | `/playlists/{id}/songs` | Menghapus lagu dari _playlist_.          |

### (Opsional) Collaborations & Activities API

| Method   | Endpoint                     | Deskripsi                                    |
| :------- | :--------------------------- | :------------------------------------------- |
| `POST`   | `/collaborations`            | Menambahkan kolaborator ke _playlist_.       |
| `DELETE` | `/collaborations`            | Menghapus kolaborator dari _playlist_.       |
| `GET`    | `/playlists/{id}/activities` | Melihat riwayat aktivitas sebuah _playlist_. |

---

Dibuat oleh **Irfan Dwi Samudra**.

- [GitHub](https://github.com/irfandwisamudra)
