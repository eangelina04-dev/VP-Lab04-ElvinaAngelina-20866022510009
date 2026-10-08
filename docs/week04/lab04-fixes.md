| Widget | Error / symptom | Rule broken | Fix |
|---|---|---|---|
| StoreHeader | overflowed by 219 px on the right at 320 dp | Column dalam Row mengukur lebar dari teks | Expanded + maxLines: 1 + ellipsis |
| CategoryBar | overflowed by 321 px on the right at 320 dp | Row berisi chip yang total lebarnya lebih dari layar, dan tidak ada yang bisa di-scroll | SingleChildScrollView horizontal, padding dipindah ke scroll view |
