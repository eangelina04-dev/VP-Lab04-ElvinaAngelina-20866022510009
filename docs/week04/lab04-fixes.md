| Widget | Error / symptom | Rule broken | Fix |
|---|---|---|---|
| StoreHeader | overflowed by 219 px on the right at 320 dp | Column dalam Row mengukur lebar dari teks | Expanded + maxLines: 1 + ellipsis |
| CategoryBar | overflowed by 321 px on the right at 320 dp | Row berisi chip yang total lebarnya lebih dari layar, dan tidak ada yang bisa di-scroll | SingleChildScrollView horizontal, padding dipindah ke scroll view |
| PromoStrip/PromoCard | overflowed by 128 px on the right at 320 dp (2 × 200 + 16 = 416 dp vs 288 dp) | Kartu menetapkan lebarnya sendiri, padahal parent yang membagi ruang | Hapus width: 200, tiap kartu dibungkus Expanded, teks diberi maxLines + ellipsis |
| MenuTile | overflowed by 22 px on the right at 320 dp | Column nama menu mengukur lebar dari teks, tidak diberi batas oleh Row; Spacer tidak bisa mengecilkan apa pun | Expanded + maxLines: 2 + ellipsis, Spacer diganti SizedBox kecil |
| CartBar | overflowed by 96 px on the right at 320 dp (208 dp tetap + teks 176 dp > 288 dp) | teks tanpa batas dan tombol width: 160 berebut ruang, tidak ada yang mengalah | Expanded pada teks + maxLines: 2 + ellipsis, width: 160 dihapus |

Catatan:
"Tes 2 lolos tanpa perubahan, karena fix Tes 1 memakai Expanded dan scroll, bukan angka tetap"