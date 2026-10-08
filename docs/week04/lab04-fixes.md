| Widget | Error / symptom | Rule broken | Fix |
|---|---|---|---|
| StoreHeader | overflowed by 219 px on the right at 320 dp | Column dalam Row mengukur lebar dari teks | Expanded + maxLines: 1 + ellipsis |
| CategoryBar | overflowed by 321 px on the right at 320 dp | Row berisi chip yang total lebarnya lebih dari layar, dan tidak ada yang bisa di-scroll | SingleChildScrollView horizontal, padding dipindah ke scroll view |
| PromoStrip/PromoCard | overflowed by 128 px on the right at 320 dp (2 × 200 + 16 = 416 dp vs 288 dp) | Kartu menetapkan lebarnya sendiri, padahal parent yang membagi ruang | Hapus width: 200, tiap kartu dibungkus Expanded, teks diberi maxLines + ellipsis |
| MenuTile | overflowed by 22 px on the right at 320 dp | Column nama menu mengukur lebar dari teks, tidak diberi batas oleh Row; Spacer tidak bisa mengecilkan apa pun | Expanded + maxLines: 2 + ellipsis, Spacer diganti SizedBox kecil |
| CartBar | overflowed by 96 px on the right at 320 dp (208 dp tetap + teks 176 dp > 288 dp) | teks tanpa batas dan tombol width: 160 berebut ruang, tidak ada yang mengalah | Expanded pada teks + maxLines: 2 + ellipsis, width: 160 dihapus |
| MenuCard (grid tablet) | overflowed by 62 px on the bottom at 800 dp (isi 202 dp di sel 140 dp) | Sel grid memberi tinggi tepat, tapi kartu memaksa kotak ikon height: 110 | Kotak ikon Expanded, teks maxLines + ellipsis, sel dibuat lebih tinggi (childAspectRatio: 0.75) |
| MenuScreen | tidak ada stripes, tapi breakpoint memakai MediaQuery dan > 600 | Breakpoint membaca ukuran layar, bukan ruang yang diberikan parent; batasnya meleset dari "600 dp atau lebih" | LayoutBuilder dengan constraints.maxWidth >= 600 |
| MenuScreen body | overflowed by 184 px (small) dan 74 px (large) on the bottom at landscape; tablet tidak error tapi daftar nyaris tidak terlihat | Column menumpuk ±376 dp bagian tak-gulir di atas daftar, tidak ada yang bisa mengalah saat tinggi layar kurang | satu CustomScrollView: header sampai promo jadi SliverToBoxAdapter, daftar jadi SliverList.builder / SliverGrid.builder (lazy) |

Catatan:
"Tes 2 lolos tanpa perubahan, karena fix Tes 1 memakai Expanded dan scroll, bukan angka tetap"