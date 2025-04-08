# Globe Multiplayer

[![Deploy ke Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/cloudflare/templates/tree/main/globe)

<!-- dash-content-start -->

Dengan memanfaatkan kekuatan [Durable Objects](https://developers.cloudflare.com/durable-objects/), contoh ini menampilkan lokasi pengunjung website secara real-time. Siapa pun di dunia yang mengunjungi [demo website](https://globe.dimas.fun) akan ditampilkan sebagai titik di globe! Template ini didukung oleh [PartyKit](https://www.partykit.io/).

## Cara Kerja

Setiap kali seseorang mengunjungi halaman, koneksi WebSocket dibuka dengan Durable Object yang mengelola status globe. Pengunjung ditempatkan di globe berdasarkan lokasi geografis alamat IP mereka, yang diekspos sebagai [properti pada permintaan HTTP awal](https://developers.cloudflare.com/workers/runtime-apis/request/#incomingrequestcfproperties) yang membangun koneksi WebSocket.

Instance Durable Object yang mengelola status globe berjalan di satu lokasi, dan menangani semua koneksi WebSocket yang masuk. Saat koneksi baru terbentuk, Durable Object menyiarkan lokasi pengunjung baru ke semua pengunjung aktif lainnya, dan klien menambahkan pengunjung baru ke visualisasi globe. Ketika seseorang meninggalkan halaman dan koneksinya ditutup, Durable Object juga menyiarkan penghapusan mereka. Instance Durable Object tetap aktif selama setidaknya ada satu koneksi terbuka. Jika semua koneksi ditutup, instance Durable Object dihapus dari memori dan tetap tidak aktif hingga pengunjung baru membuka halaman, membuka koneksi, dan memulai siklus hidup kembali.

## Lebih Lanjut tentang Durable Objects

Dalam template ini, hanya satu instance Durable Object yang dibuat, karena semua pengunjung harus melihat pembaruan dari pengunjung lain di seluruh dunia. Versi yang lebih kompleks dari aplikasi ini dapat menampilkan peta negara tempat pengunjung berada, dan hanya menampilkan penanda untuk pengunjung lain yang berada di negara yang sama. Dalam kasus ini, instance Durable Object akan dibuat untuk setiap negara, dan klien akan terhubung ke instance Durable Object yang sesuai dengan negara pengunjung.

Untuk contoh ini, Durable Objects digunakan untuk menyinkronkan status tetapi tidak menyimpan status. Karena koneksi pengunjung bersifat sementara dan terikat pada masa hidup instance Durable Object di memori, tidak perlu menggunakan penyimpanan persisten. Namun, versi yang lebih kompleks dari aplikasi ini dapat menampilkan jumlah pengunjung sepanjang waktu, yang perlu dipertahankan di luar masa hidup Durable Object di memori. Dalam kasus ini, kode Durable Object akan menggunakan [Durable Object Storage API](https://developers.cloudflare.com/durable-objects/api/storage-api/) untuk mempertahankan nilai penghitung.

<!-- dash-content-end -->

## Getting Started

Di luar repo ini, Anda dapat memulai proyek baru dengan template ini menggunakan [C3](https://developers.cloudflare.com/pages/get-started/c3/) (CLI `create-cloudflare`):

```
npm create cloudflare@latest -- --template=cloudflare/templates/globe
```

## Langkah Setup

1. Install dependensi proyek dengan package manager pilihan Anda:
   ```bash
   npm install
   ```
2. Deploy proyek!
   ```bash
   npx wrangler deploy
   ```
