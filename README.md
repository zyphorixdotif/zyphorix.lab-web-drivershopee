# Buku Kas Driver (ShopeeFood)

Catatan keuangan pribadi untuk driver ShopeeFood: upah antar, tip, COD, uang kembalian, tagihan Shopee, bensin, servis, target harian, balik modal, tabungan, dan keinginan. Satu file web (`index.html`) + database Supabase + hosting GitHub Pages, jadi bisa dibuka dari HP atau laptop mana saja.

Isi paket:
- `index.html` : aplikasi web
- `supabase-schema.sql` : pembuat tabel database
- `README.md` : panduan ini

Waktu total sekitar 20-30 menit. Urutannya: siapkan file, buat database, hubungkan web, pasang di GitHub, atur Supabase, lalu coba pakai.

---

## Tahap 1. Siapkan file
1. Simpan ketiga file di satu folder yang mudah dicari.

## Tahap 2. Buat database di Supabase
1. Buka **supabase.com**, klik **Start your project**, daftar (paling mudah lewat GitHub atau Google).
2. Klik **New project** dan isi:
   - **Name:** `driver-ledger`
   - **Database Password:** klik *Generate a password*, lalu simpan di catatan HP.
   - **Region:** Southeast Asia (Singapore).
   - **Plan:** Free.
3. Pilihan pengaturan Data API yang muncul:
   - **Enable Data API:** aktif (wajib, web memakainya).
   - **Automatically expose new tables:** aktif (paling mudah).
   - **Enable automatic RLS:** aktif (pengaman tambahan).
4. Klik **Create new project**, tunggu 1-2 menit.
5. Menu kiri **SQL Editor**, lalu **New query**.
6. Buka `supabase-schema.sql` dengan Notepad, salin semuanya, tempel di SQL Editor, klik **Run**.
7. Harus muncul **Success. No rows returned**.
8. Cek di **Table Editor**: harus ada tabel `logs` dan `settings`.

Kalau tadi kamu mematikan "Automatically expose new tables", jalankan SQL ini sekali lagi di SQL Editor:

```sql
grant usage on schema public to authenticated;
grant select, insert, update, delete on public.logs to authenticated;
grant select, insert, update, delete on public.settings to authenticated;
grant usage, select on all sequences in schema public to authenticated;
```

## Tahap 3. Hubungkan web ke database
1. Di Supabase buka **Project Settings**, lalu **API** (atau **Data API**, atau tombol **Connect**).
2. Salin **Project URL**, bentuknya `https://abcdefgh.supabase.co`.
3. Salin kunci publik: **anon public** (berawalan `eyJ...`) atau **publishable key** (berawalan `sb_publishable_...`).
4. **Jangan** memakai kunci **service_role** atau **secret**. Itu rahasia dan tidak boleh masuk ke web.
5. Buka `index.html` dengan Notepad (di HP pakai aplikasi editor teks). Cari (Ctrl+F) `YOUR_SUPABASE_URL`:
   ```
   const SUPABASE_URL="YOUR_SUPABASE_URL",SUPABASE_ANON_KEY="YOUR_SUPABASE_ANON_KEY";
   ```
6. Ganti kedua tulisan YOUR_... dengan Project URL dan kunci publik. **Tanda kutip jangan dihapus.**
   ```
   const SUPABASE_URL="https://abcdefgh.supabase.co",SUPABASE_ANON_KEY="eyJhbGciOi...";
   ```
7. Simpan (Ctrl+S).

## Tahap 4. Pasang web di GitHub
1. Buka **github.com**, daftar, verifikasi email.
2. Klik **+** di kanan atas, lalu **New repository**.
3. Isi:
   - **Repository name:** `driver-ledger`
   - **Public** (GitHub Pages gratis butuh repo publik).
   - Centang **Add a README file**.
   - **.gitignore:** No .gitignore. **License:** None.
4. Klik **Create repository**.
5. Klik **Add file**, lalu **Upload files**. Tarik ketiga file (`index.html`, `supabase-schema.sql`, `README.md`). Kalau ditanya soal `README.md` yang sudah ada, timpa saja.
6. Klik **Commit changes**.
7. Buka **Settings**, lalu **Pages** di menu kiri.
8. Di **Build and deployment**, *Source* pilih **Deploy from a branch**. *Branch* pilih **main**, folder **/(root)**, lalu **Save**.
9. Tunggu 1-3 menit, refresh. Akan muncul **Your site is live at** `https://USERNAME.github.io/driver-ledger/`. Salin alamat ini, ini alamat webmu.

Soal keamanan repo publik:
- Yang publik hanya kode web. Catatan keuanganmu ada di Supabase, bukan di GitHub.
- Kunci publik di `index.html` aman terlihat, karena data tiap akun dikunci oleh Row Level Security.
- Jangan pernah menaruh password atau kunci service_role/secret di repo.

## Tahap 5. Atur Supabase untuk alamat web
1. Supabase: **Authentication**, lalu **URL Configuration**.
2. Isi **Site URL** dengan alamat GitHub Pages, lalu **Save**.
3. Buka **Sign In / Providers**, pilih **Email**. Untuk mudah, matikan **Confirm email**, lalu **Save**. Kalau dibiarkan aktif, kamu harus klik link konfirmasi di email setelah daftar.

## Tahap 6. Coba pakai
**Login pertama**
1. Buka alamat web di HP, klik **Masuk**, isi email dan password (minimal 6 karakter, catat di tempat aman).
2. Karena belum punya akun, login gagal dan muncul tawaran *"Daftar akun baru dengan email ini?"*. Pilih **OK**.
3. Setelah masuk, banner "Mode tamu" hilang dan tombol berubah jadi **Keluar**.

**Pasang di layar utama HP**
4. Di Chrome buka menu ⋮, pilih **Tambahkan ke layar utama**.

**Isi pengaturan dulu**
5. Tab **Rencana**: isi bensin sekali isi dan tahan berapa hari, paket data, angin, biaya servis dan intervalnya, modal awal (jaket 130rb + holder 15rb + lainnya), tabungan, dan keinginan. Hasilnya adalah **Target upah kotor per hari**.
6. Tab **Siap**: centang barang yang sudah kamu punya.

**Catat data pertama (31 Okt)**
7. Tab **Catat**, isi:
   - Tanggal 31 Oktober
   - Upah antar 6400
   - Nilai COD semua order 28500
   - Uang tunai dari customer 28000
   - Cash awal dan saldo Shopee awal sesuai uang asli saat itu
8. Di pratinjau, **Selisih COD** harus -Rp500. Klik **Simpan**.
9. Lanjutkan 1 Nov dan 2 Nov. Cash dan saldo awal hari berikutnya terisi otomatis dari catatan terakhir.

**Isi form setiap hari**
- **Nilai COD semua order:** total nilai COD dari aplikasi.
- **Uang tunai dari customer:** uang yang benar-benar diterima.
- **Kembalian:** uang yang kamu keluarkan sendiri.
- **Bayar tagihan Shopee:** nominal yang kamu bayar, dan pilih dari cash atau saldo.
- Rumus selisih COD: (tunai diterima - kembalian) - nilai COD. Hasil negatif berarti kamu nombok.
- Bersih harian = upah + tip + selisih COD - bensin - biaya lain.

## Cara membaca Beranda
- **Hari ini:** bersih hari ini dibanding target, dan perkiraan sisa order.
- **Posisi uang:** cash di tangan, saldo Shopee, dan tagihan Shopee yang belum dibayar. Ini estimasi, cocokkan dengan saldo asli.
- **Pencapaian:** progres balik modal, grafik bersih 14 hari, rata-rata bersih per hari, upah rata-rata per order, dan total nombok COD.

## Backup
Di tab **Riwayat**, klik **Ekspor CSV** seminggu sekali dan simpan di Google Drive.

## Memperbarui web
Buka repo di GitHub, klik `index.html`, klik ikon pensil, edit, lalu **Commit changes**. Tunggu 1-2 menit lalu muat ulang paksa (Ctrl+Shift+R, atau tutup lalu buka lagi di HP).

## Kalau ada masalah
- **"Supabase belum diisi":** URL atau kunci di `index.html` belum terganti.
- **"relation logs does not exist":** SQL di Tahap 2 belum dijalankan.
- **"Invalid API key":** kunci salah atau terpotong, salin ulang utuh.
- **"permission denied for table logs":** jalankan SQL grant di Tahap 2.
- **GitHub Pages 404:** tunggu beberapa menit, pastikan file bernama persis `index.html` dan ada di folder utama repo.
- **Perubahan tidak muncul:** muat ulang paksa.
- **Lupa password:** Supabase, **Authentication**, **Users**, atur ulang atau buat akun baru.
- **Project Supabase dijeda karena lama tidak dipakai:** masuk dashboard dan klik **Restore**, data tidak hilang.
- **Data mode tamu:** hanya ada di perangkat itu dan tidak otomatis pindah ke cloud. Masuk dulu sebelum mulai mencatat.

## Catatan
Angka saldo dan target adalah estimasi dari data yang kamu isi. Aturan ShopeeFood/SPX (on-bid, saldo, pindah kota) bisa berubah. Cek ke aplikasi driver atau pusat bantuan.
