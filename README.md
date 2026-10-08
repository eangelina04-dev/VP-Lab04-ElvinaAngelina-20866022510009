# Lab 04: Layout dan Responsivitas

Jawaban pertanyaan *Check yourself*. Pertanyaan serupa akan muncul di review [AFL 1](https://elearn.uc.ac.id/mod/assign/view.php?id=1228758).

Aturan yang dipakai: **constraints go down, sizes go up, parent sets position.**
Artinya: parent memberi batas ukuran ke child, child memilih ukurannya di dalam batas itu, dan parent yang menentukan posisinya.

## 1. Sebuah `Text` di dalam `Row` overflow. Bagian aturan mana yang dilanggar, dan oleh widget apa?

`Row` tidak membatasi lebar `Text`, jadi `Text` memakai lebar sesuai panjang tulisannya sendiri. Akibatnya ukuran yang "naik" ke parent lebih besar dari ruang yang ada, dan tidak ada yang menyuruhnya mengalah. Perbaikannya adalah membungkus `Text` dengan `Expanded` supaya `Row` yang menentukan lebarnya.

## 2. Kenapa menambah `width: 150` pada teks adalah perbaikan yang salah, walau stripes hilang?

Angka 150 hanya cocok untuk satu ukuran layar. Di layar lain, atau saat ukuran font diperbesar, masalahnya muncul lagi atau ruangnya terbuang. Jadi bug-nya cuma dipindah, bukan diperbaiki. Perbaikan yang benar adalah mengubah hubungan antara parent dan child, bukan mengganti angkanya.

## 3. Landscape diperbaiki dengan `SingleChildScrollView` dan `shrinkWrap: true` pada list. Tes mana yang gagal, dan kenapa penting kalau data dari API?

Tes 7 (500 item) yang gagal, karena `shrinkWrap: true` membuat semua item dibangun sekaligus, bukan hanya yang terlihat. Kalau data berasal dari API, jumlahnya tidak bisa kita tebak: bisa ribuan item. Aplikasi akan lambat, boros memori, dan bisa macet. Daftar yang jumlahnya tidak kita kontrol harus memakai `.builder` supaya dibangun sedikit demi sedikit.

## 4. Kenapa layout tablet memakai `LayoutBuilder`, bukan `MediaQuery.sizeOf(context)`?

`MediaQuery` memberi tahu ukuran seluruh layar, sedangkan `LayoutBuilder` memberi tahu ruang yang benar-benar diberikan parent ke widget itu. Misalnya widget ditaruh di panel selebar 400 dp pada layar 1200 dp: `MediaQuery` bilang "lebar 1200" dan widget salah memilih grid, sedangkan `LayoutBuilder` bilang "lebar 400" dan widget memilih daftar. Dengan `LayoutBuilder`, widget bisa dipakai ulang di tempat mana pun.

## 5. Crash karena data kosong bukan error layout. Kenapa tetap masuk lab layout?

Layar yang bagus harus tampil benar untuk semua keadaan data: kosong, sedikit, banyak, atau teksnya panjang. Crash karena data kosong membuat layar merah dan tidak bisa dipakai, sama buruknya dengan stripes. Karena itu keadaan kosong (*empty state*) juga bagian dari desain layar.