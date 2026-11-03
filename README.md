# Buku Kas Driver (ShopeeFood)
Web statis (satu file `index.html`) + database Supabase + hosting GitHub Pages. Bisa dibuka dari HP/laptop mana saja.

## 1. Supabase (±5 menit)
1. Buat akun & project baru di supabase.com (Free cukup).
2. SQL Editor → New query → tempel isi `supabase-schema.sql` → Run.
3. Project Settings → API: salin **Project URL** dan **anon public key** (bukan service_role).
4. Buka `index.html`, ganti `YOUR_SUPABASE_URL` dan `YOUR_SUPABASE_ANON_KEY` di baris awal script.
5. Authentication → Sign In / Providers → Email: untuk mudah, matikan "Confirm email" (atau biarkan aktif dan konfirmasi lewat email).

## 2. GitHub Pages
1. github.com → New repository, mis. `driver-ledger`.
2. Upload `index.html`, `supabase-schema.sql`, `README.md` (Add file → Upload files).
3. Settings → Pages → Deploy from a branch → `main` / root → Save.
4. Alamatmu: `https://USERNAME.github.io/driver-ledger/`.
5. Supabase → Authentication → URL Configuration: isi Site URL dengan alamat tersebut.

## 3. Pakai di HP
Buka alamat di Chrome → Masuk (email+password; kalau belum punya akun, ikuti tawaran daftar) → menu ⋮ → "Tambahkan ke layar utama".

## Alur harian
- Pagi: cek tab **Siap**, lihat target di **Beranda**.
- Setelah narik: tab **Catat** (cash & saldo awal terisi otomatis dari catatan terakhir).
- Isi "Nilai COD" dari aplikasi, "tunai diterima" dari customer, kembalian yang kamu keluarkan, dan tagihan Shopee yang dibayar.
- Ganti angka bensin/servis/target di **Rencana**, tersimpan dan sinkron antar perangkat.

Catatan: anon key memang boleh publik karena data dilindungi Row Level Security (tiap akun hanya melihat datanya sendiri). Data mode tamu tidak otomatis pindah ke cloud.
