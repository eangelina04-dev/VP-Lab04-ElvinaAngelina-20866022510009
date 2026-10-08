# Lab 04: Fix Log

Layar yang diperbaiki: `MenuScreen` (Warung Digital).
Aturan acuan: **constraints go down, sizes go up, parent sets position.**

Ringkasan: 9 fix di 5 tes, 5 tes lolos tanpa perubahan kode (lihat tabel kedua).

## 1. Fix yang dilakukan

| No | Tes | Widget | Error / symptom | Rule broken | Fix |
|---|---|---|---|---|---|
| 1 | 1 (320 dp) | `StoreHeader` | overflowed by 219 px on the right | `Column` di dalam `Row` mengukur lebar dari teks, tidak diberi batas oleh parent | `Expanded` + `maxLines: 1` + ellipsis |
| 2 | 1 (320 dp) | `CategoryBar` | overflowed by 321 px on the right | `Row` berisi chip yang total lebarnya lebih dari layar, dan tidak ada yang bisa di-scroll | `SingleChildScrollView` horizontal, padding dipindah ke scroll view |
| 3 | 1 (320 dp) | `PromoStrip` / `PromoCard` | overflowed by 128 px on the right (2 × 200 + 16 = 416 dp vs 288 dp) | kartu menetapkan lebarnya sendiri, padahal parent yang membagi ruang | hapus `width: 200`, tiap kartu dibungkus `Expanded`, teks diberi `maxLines` + ellipsis |
| 4 | 1 (320 dp) | `MenuTile` | overflowed by 22 px on the right | `Column` nama menu mengukur lebar dari teks, tidak diberi batas oleh `Row`; `Spacer` tidak bisa mengecilkan apa pun | `Expanded` + `maxLines: 2` + ellipsis, `Spacer` diganti `SizedBox` kecil |
| 5 | 1 (320 dp) | `CartBar` | overflowed by 96 px on the right (208 dp tetap + teks 176 dp > 288 dp) | teks tanpa batas dan tombol `width: 160` berebut ruang, tidak ada yang mengalah | `Expanded` pada teks + `maxLines: 2` + ellipsis, `width: 160` dihapus |
| 6 | 3 (800 dp) | `MenuCard` (grid tablet) | overflowed by 62 px on the bottom (isi 202 dp di sel 140 dp) | sel grid memberi tinggi tepat, tapi kartu memaksa kotak ikon `height: 110` | kotak ikon `Expanded`, teks `maxLines` + ellipsis, sel dibuat lebih tinggi (`childAspectRatio: 0.75`) |
| 7 | 3 (800 dp) | `MenuScreen` | tidak ada stripes, tapi breakpoint memakai `MediaQuery` dan `> 600` | breakpoint membaca ukuran layar, bukan ruang yang diberikan parent; batasnya meleset dari "600 dp atau lebih" | `LayoutBuilder` dengan `constraints.maxWidth >= 600` |
| 8 | 4 (landscape) | `MenuScreen` body | overflowed by 184 px (small) dan 74 px (large) on the bottom; tablet tidak error tapi daftar nyaris tidak terlihat | `Column` menumpuk ±376 dp bagian tak-gulir di atas daftar, tidak ada yang bisa mengalah saat tinggi layar kurang | satu `CustomScrollView`: header sampai promo jadi `SliverToBoxAdapter`, daftar jadi `SliverList.builder` / `SliverGrid.builder` (lazy) |
| 9 | 6 (zero items) | `MenuScreen` | `RangeError: no indices are valid: 0` saat items kosong | kode mengakses `promos[0]` dan `promos[1]` tanpa memastikan daftarnya cukup panjang | `PromoStrip` hanya dibangun jika `promos.length >= 2` |
| 10 | 6 (zero items) | `MenuScreen` / `EmptyState` | data kosong menampilkan layar blank tanpa penjelasan | layar mengasumsikan data selalu ada, tidak ada keadaan untuk daftar kosong | `EmptyState` (ikon, pesan, tombol, `Key('empty-state')`) lewat `SliverFillRemaining`, plus `TextEditingController` agar reset sinkron |
| 11 | Bonus (notch + gesture bar) | `CartBar` | bagian bawah tombol "Pesan" di 556 dp, melewati batas 548 dp (masuk zona gesture bar) | bar menentukan posisinya sendiri tanpa menghormati area yang ditutupi sistem (padding dari `MediaQuery`) | `SafeArea(top: false)` dan `minHeight: 72` menggantikan `height: 72` |

## 2. Tes yang lolos tanpa perubahan kode

| Tes | Hasil | Alasan lolos |
|---|---|---|
| 2 (430 dp) | lolos | fix Tes 1 memakai `Expanded` dan scroll, bukan angka tetap, jadi berlaku untuk lebar berapa pun |
| 5 (nama 200 karakter) | lolos | `PromoCard`, `MenuTile`, dan `MenuCard` sudah memakai `maxLines` + ellipsis dari fix sebelumnya |
| 7 (500 item) | lolos | daftar sudah memakai `SliverList.builder` / `SliverGrid.builder` (lazy) sejak Tes 4 |
| 8 (keyboard) | lolos | seluruh konten sudah berada dalam satu `CustomScrollView` sejak Tes 4, jadi saat tinggi menyempit isinya tinggal digulir |
| 9 (dark mode) | lolos | semua warna berasal dari `ColorScheme`, pasangan `container` / `onContainer` ikut berganti dengan tema |