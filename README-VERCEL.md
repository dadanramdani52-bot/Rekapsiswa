# Deploy Rekapitulasi Siswa ke Vercel

## 1. Siapkan database PostgreSQL online

Buat database PostgreSQL di Neon atau Supabase. Salin connection string PostgreSQL dari dashboard provider. Untuk Vercel, gunakan pooled connection string bila provider menyediakannya. Pastikan parameter SSL (`sslmode=require`) tersedia jika diwajibkan provider.

## 2. Atur environment variable

Di Vercel: **Project → Settings → Environment Variables**, tambahkan:

- `DATABASE_URL` = connection string PostgreSQL online.

Aktifkan untuk Production, Preview, dan Development sesuai kebutuhan. Jangan menaruh password database di source code atau mengunggah file `.env`.

## 3. Buat tabel database

Di komputer lokal, buat file `.env` berdasarkan `.env.example`, isi `DATABASE_URL` dengan connection string database online, lalu jalankan:

```bash
npm install
npm run db:push
```

Perintah tersebut membuat/menyesuaikan tabel berdasarkan `src/db/schema.ts`. Lakukan pada database baru atau setelah membuat backup database.

## 4. Deploy

1. Unggah folder proyek ini ke repository GitHub.
2. Di Vercel pilih **Add New → Project**, lalu impor repository tersebut.
3. Framework akan terdeteksi sebagai Next.js. Build command: `npm run build`.
4. Pastikan `DATABASE_URL` sudah diisi pada Environment Variables.
5. Klik **Deploy**.

## 5. Verifikasi

Setelah deploy, buka `https://domain-anda.vercel.app/api/health`. Jika database tersambung, responsnya:

```json
{"ok":true}
```

Jika gagal, periksa `DATABASE_URL`, akses jaringan database, dan apakah tabel sudah dibuat dengan `npm run db:push`.

## Catatan penting

- Data aplikasi disimpan di PostgreSQL, bukan filesystem server Vercel.
- File upload/logo yang disimpan sebagai data URL di database dapat memperbesar ukuran database. Untuk skala besar, gunakan object storage.
- Jangan menghapus database lama sebelum memastikan backup berhasil dipulihkan ke database online.
