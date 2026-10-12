# EcoChain

EcoChain adalah dashboard web untuk admin ekspedisi skala menengah. Admin mencatat berat, tujuan, dan moda transportasi setiap pengiriman dalam satu form, lalu sistem langsung menghitung estimasi emisi karbon (CO2e) per pengiriman dan menyiapkan rekap bulanan yang bisa diunduh untuk mitra bisnis yang meminta laporan emisi (Scope 3).

## Alur Inti

Alur ini mengikuti pekerjaan admin pengiriman sehari-hari.

1. Buka halaman **Ringkasan** untuk melihat jumlah pesanan hari ini, jumlah pesanan yang moda transportasinya belum dicatat, dan emisi bulan ini.
2. Klik **Catat pengiriman baru** untuk membuka form pencatatan.
3. Isi referensi pesanan, pilih tujuan (jarak terisi otomatis untuk rute umum), isi berat muatan dalam kg, dan pilih moda transportasi. Semua kolom wajib diisi.
4. Periksa panel **Yang akan dicatat** di sisi kanan. Panel ini menampilkan ringkasan isian dan estimasi emisi (kg CO2e) yang diperbarui langsung saat form diisi.
5. Klik **Catat pengiriman ini**. Entri muncul di daftar **Tercatat hari ini** dan form kosong kembali untuk pesanan berikutnya.
6. Buka **Riwayat Pengiriman** untuk melihat daftar pesanan. Pesanan tanpa moda transportasi ditandai agar bisa dilengkapi.
7. Buka **Laporan Emisi** untuk melihat rekap bulanan (tren harian dan emisi per moda), lalu klik **Unduh laporan (CSV)**.

## Cara Menjalankan (How to Run)

### Prasyarat

- Node.js versi 20.9 atau lebih baru (cek dengan `node -v`)
- npm (ikut terpasang bersama Node.js)
- Koneksi internet saat pertama kali menjalankan, karena font (Space Grotesk, Inter, IBM Plex Mono) diunduh dari Google Fonts oleh Next.js
- Browser Chrome atau Firefox versi terbaru

### Langkah

1. Clone repositori:
   ```
   git clone https://github.com/hairuddinmcs/steampreneur-EcoChain.git
   ```
2. Masuk ke folder proyek:
   ```
   cd steampreneur-EcoChain
   ```
3. Install dependensi:
   ```
   npm install
   ```
4. Jalankan website:
   ```
   npm run dev
   ```
5. Buka alamat berikut di browser: http://localhost:3000 (otomatis diarahkan ke halaman Ringkasan)

### Akun atau data contoh

Website ini tidak memerlukan login. Semua angka di halaman Ringkasan, Riwayat Pengiriman, dan Laporan Emisi adalah **data contoh** yang tersimpan di `src/lib/mock-data.ts`, bukan data pengiriman sungguhan.

Untuk mencoba form Catat Pengiriman, Anda bisa memakai isian berikut:

| Kolom | Contoh isian | Estimasi emisi yang diharapkan |
|---|---|---|
| Referensi pesanan | Bpk. Andi, pesanan WA pagi ini | |
| Tujuan | Samarinda, Kalimantan Timur (122 km) | |
| Berat muatan | 1850 | |
| Moda transportasi | Truk Diesel | sekitar 24,15 kg CO2e |

### Jika terjadi masalah

| Masalah | Cara mengatasi |
|---|---|
| Port 3000 sudah dipakai program lain | Jalankan `npm run dev -- -p 3001`, lalu buka http://localhost:3001 |
| Muncul error saat memuat font atau `fonts.googleapis.com` | Pastikan komputer terhubung ke internet, lalu jalankan ulang `npm run dev` |
| `npm install` gagal atau ada pesan versi Node tidak cocok | Cek `node -v`. Perbarui Node.js ke versi 20.9 atau lebih baru |
| Tampilan error setelah berganti versi Node atau memperbarui dependensi | Hapus folder `node_modules` dan `.next`, lalu jalankan `npm install` dan `npm run dev` lagi |

## Tangkapan Layar Alur Inti

Gambar disimpan di `docs/bukti/`.

1. Langkah 1, halaman Ringkasan: ![Langkah 1](docs/bukti/langkah-1-ringkasan.png)
2. Langkah 2 dan 3, form Catat Pengiriman terisi: ![Langkah 3](docs/bukti/langkah-3-form-terisi.png)
3. Langkah 4, panel ringkasan dan estimasi emisi: ![Langkah 4](docs/bukti/langkah-4-estimasi-emisi.png)
4. Langkah 5, entri muncul di Tercatat hari ini: ![Langkah 5](docs/bukti/langkah-5-tercatat.png)
5. Langkah 6, Riwayat Pengiriman dengan penanda moda belum dicatat: ![Langkah 6](docs/bukti/langkah-6-riwayat.png)
6. Langkah 7, Laporan Emisi: ![Langkah 7](docs/bukti/langkah-7-laporan.png)

## Cara Menjalankan Pengujian

Versi ini belum memiliki unit test. Pemeriksaan yang tersedia:

```
npm run typecheck
npm run build
```

Hasil yang diharapkan: kedua perintah selesai tanpa error. `npm run build` menampilkan lima route (`/`, `/overview`, `/catat-pengiriman`, `/shipments`, `/laporan-emisi`).

## Fitur

- Form Catat Pengiriman dengan validasi: semua kolom wajib, berat harus lebih dari 0, moda transportasi tidak boleh kosong
- Pilihan rute umum dari Balikpapan dengan jarak terisi otomatis, atau jarak manual untuk tujuan lain
- Panel ringkasan "Yang akan dicatat" sebagai pengecekan ulang sebelum data disimpan
- Estimasi emisi per pengiriman secara langsung, dengan rumus: berat (ton) × jarak (km) × faktor emisi moda (kg CO2e per ton-km)
- Halaman Ringkasan: pesanan hari ini, pesanan tanpa moda, emisi bulan ini, tren emisi, emisi per moda
- Riwayat Pengiriman dengan penanda untuk pesanan yang moda transportasinya belum dicatat
- Laporan Emisi bulanan dengan unduhan CSV yang bisa dibuka di Excel

## Batasan yang Diketahui

- Belum ada backend dan database. Semua data di halaman adalah data contoh.
- Entri dari form Catat Pengiriman hanya disimpan di memori browser dan hilang saat halaman di-refresh. Entri tersebut juga belum muncul di halaman Riwayat Pengiriman.
- Faktor emisi (truk diesel 0,107, van listrik 0,045, kapal kargo 0,008, kargo udara 0,602, kereta barang 0,028 kg CO2e per ton-km) adalah angka ilustratif, belum diverifikasi dengan sumber resmi.
- Jarak pada daftar rute umum adalah perkiraan, belum diverifikasi dengan jarak rute sebenarnya.
- Belum ada login dan pembagian peran pengguna.
- Pelacakan Kurir (aplikasi mobile) belum dibangun karena kebutuhannya belum divalidasi lewat wawancara dengan kurir.
- Kolom pencarian dan ikon notifikasi di bagian atas masih tampilan saja. Menu Pengaturan belum memiliki halaman.
- Belum ada unit test.

## Struktur Folder

| File / folder | Isi |
|---|---|
| `docs/` | Dokumen proses kerja (proposal, dokumen output tugas turun lapangan, rancangan arsitektur dan skema database) |
| `docs/bukti/` | Tangkapan layar alur inti |
| `src/app/` | Halaman: `overview`, `catat-pengiriman`, `shipments`, `laporan-emisi` |
| `src/components/` | Komponen antarmuka: `layout`, `forms`, `charts`, `dashboard`, `ui` |
| `src/lib/` | Data contoh (`mock-data.ts`), daftar rute umum (`common-routes.ts`), fungsi bantu (`utils.ts`) |
| `tailwind.config.ts` | Warna, font, dan token desain |

## Link Demo (opsional)

Belum ada.

## Sumber dan Lisensi

| Nama | Sumber (link) | Lisensi |
|---|---|---|
| Next.js | https://nextjs.org | MIT |
| React | https://react.dev | MIT |
| Tailwind CSS | https://tailwindcss.com | MIT |
| Recharts | https://recharts.org | MIT |
| Lucide (ikon) | https://lucide.dev | ISC |
| class-variance-authority | https://github.com/joe-bell/cva | Apache-2.0 |
| clsx | https://github.com/lukeed/clsx | MIT |
| tailwind-merge | https://github.com/dcastil/tailwind-merge | MIT |
| Font Space Grotesk | https://fonts.google.com/specimen/Space+Grotesk | SIL Open Font License 1.1 |
| Font Inter | https://fonts.google.com/specimen/Inter | SIL Open Font License 1.1 |
| Font IBM Plex Mono | https://fonts.google.com/specimen/IBM+Plex+Mono | SIL Open Font License 1.1 |

## Tim

| Nama | Peran |
|---|---|
| Alfarezal Kenzi | [Navigator utama] |
| Muhammad Rafa Rizki | [Dokumentator] |
| Alya Putri Cahyawati | [Analis kebutuhan] |
| Hairuddin | [Penguji] |
