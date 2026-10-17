# FASIH Koreksi KBLI & Anomali — Panduan Pengguna (v2.0.0)

Userscript untuk mengoreksi KBLI di FASIH SE2026 secara massal berdasarkan Excel **Pengecekan KBLI**.

Alur per dokumen: **Buka → Cek di Review → Edit/Revoke → Ganti KBLI (+produk, kegiatan utama) → Tandai anomali KBLI → Kirim → Approve**

> **Privasi:** file Excel hanya dibaca di browser pengguna. Tidak ada data yang dikirim ke server lain selain isian ke FASIH.

## 1. Pemasangan
1. Pasang ekstensi **Tampermonkey** (Chrome / Edge / Firefox).
2. Buka Dashboard Tampermonkey → **Buat skrip baru** → hapus isinya → tempel seluruh isi `fasih-koreksi-kbli.user.js` → **Simpan**.
3. Buka `https://fasih-sm.bps.go.id`. Tombol **KBLI Koreksi KBLI** muncul di kiri bawah (pintasan **Alt+5**).

## 2. Format Excel
Unduh template: **[template-pengecekan-kbli.xlsx](template-pengecekan-kbli.xlsx)**. Isinya tiga sheet:
- **Pengecekan KBLI**: isi data di sini (sheet pertama, yang dibaca skrip).
- **Contoh**: contoh pengisian untuk berbagai kasus (tidak dibaca skrip).
- **Petunjuk**: keterangan setiap kolom.

Skrip mencari sheet yang punya kolom **KBLI Baru**. Penulisan nama kolom bebas (spasi, titik, huruf besar/kecil tidak berpengaruh).

| Kolom | Wajib | Keterangan |
|---|---|---|
| `link` | ✔ | Link dokumen FASIH |
| `nama_usaha` | ✔ | Untuk mencocokkan kartu usaha |
| `kbli_akhir` | ✔ | KBLI saat ini |
| `KBLI Baru` | ✔ | KBLI pengganti (kosong = KBLI tidak diubah) |
| `Edit KBLI (1=Ya)` | – | Kalau kolom ada, **hanya baris bernilai 1 / Ya** yang dikerjakan |
| `Produk`, `Produk Baru` | – | Produk diganti kalau `Produk Baru` terisi |
| `keg_utama`, `Kegiatan utama` | – | Kegiatan utama lama & baru |
| `kec`, `desa`, `nmsls`, `catatan` | – | Info tampilan & laporan |

## 3. Cara pakai
1. Buka halaman **daftar assignment SE2026** sekali (alamatnya diingat untuk wilayah Anda).
2. Buka panel → muat file Excel.
3. Klik **🧪 Uji 1 dokumen** dulu. Skrip berhenti sebelum Kirim; periksa hasilnya, lalu **Kirim sekarang** atau **Lewati**.
4. Jalankan **▶ 5**, **▶ 20**, atau **⚡ Jalankan semua**. Selama berjalan, jangan klik apa pun di tab FASIH. Kalau perlu berhenti sebentar, pakai **⏸ Jeda**.
5. Dokumen **Perlu cek** / **Gagal** bisa dikerjakan lewat tombol **✍** (panel bantu manual), lalu klik **Tandai selesai manual**.
6. Unduh **⬇ Laporan CSV** sebagai bukti pekerjaan.

## 4. Pengaturan
- **Kecepatan:** Turbo / Kilat / Cepat / Normal. Kalau server membalas 429, skrip berhenti sementara dan menurunkan kecepatan sendiri.
- **Cek dulu di Review:** dokumen yang sudah sesuai tidak di-revoke.
- **Approve setelah kirim**
- **Submit Paksa kalau ada galat:** mati secara bawaan (dokumen bergalat ditandai *Perlu cek*).
- **Tag catatan KBLI:** catatan di 13.g sebagai jejak pemeriksaan (bawaan `#1`; kosongkan untuk menonaktifkan).
- **Penjelasan anomali KBLI:** teks penjelasan saat anomali KBLI ditandai "Ya, Sesuai Kondisi Lapangan".

## 5. Arti status
| Status | Arti |
|---|---|
| Belum | Masih di antrean |
| Selesai | Dikoreksi, dikirim & approve |
| Sudah sesuai | Data di FASIH sudah sama dengan Excel (tidak diubah) |
| Terisi (uji) | Mode uji: terisi tapi belum dikirim |
| Perlu cek | Perlu diperiksa manual (galat, nama tidak cocok, link salah, dll.) |
| Gagal | Tidak bisa dikerjakan otomatis |
| Selesai manual | Dikerjakan sendiri di FASIH |
| Forbidden | Akun tidak punya akses ke dokumen |

## 6. Hal penting
- **Satu browser, satu antrean.** Progres tersimpan di browser (localStorage). Pakai **💾 Ekspor / 📂 Impor** untuk memindahkan ke komputer lain.
- Kalau FASIH terbuka di beberapa tab, **hanya satu tab** yang menjalankan antrean; tab lain hanya memantau.
- Kalau sesi FASIH berakhir (logout), skrip menjeda sendiri dan berbunyi. Setelah login ulang, skrip melanjutkan sendiri (atau klik **▶ Lanjut**).
- Skrip **tidak menebak** isian yang tidak ada di Excel (mis. 26.c biaya pembelian yang baru muncul). Dokumen seperti itu ditandai *Perlu cek*.
