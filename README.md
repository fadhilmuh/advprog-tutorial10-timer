# Reflections

## 1.2 Understanding How it Works
![Experiment 1.2](assets/Experiment-1.2.png)

Dalam kode ini, sebuah executor dan spawner baru dibuat menggunakan fungsi `new_executor_and_spawner()`. Kemudian, sebuah task di-spawn secara asynchronous menggunakan konstruksi `spawner.spawn(async { ... })`. Task ini mencetak "2206083464's beloved device: howdy!", menunggu selama dua detik menggunakan `TimerFuture::new(Duration::new(2, 0)).await`, dan kemudian mencetak "2206083464's beloved device: done!".

Sementara itu, di luar task yang di-spawn, fungsi main mencetak "2206083464's beloved device: hey hey", menjatuhkan spawner menggunakan `drop(spawner)`, dan kemudian menjalankan executor sampai antrian task kosong menggunakan `executor.run()`.

Hasilnya adalah sebagai berikut:

1. "2206083464's beloved device: hey hey" dicetak pertama karena dieksekusi dalam fungsi main sebelum ada task asynchronous yang di-spawn.
2. "2206083464's beloved device: howdy!" dicetak selanjutnya karena berada di dalam task asynchronous yang di-spawn. Namun, karena ini adalah task asynchronous, itu tidak memblokir eksekusi fungsi main, sehingga fungsi main tetap berlanjut dieksekusi.
3. Setelah dua detik, "2206083464's beloved device: done!" dicetak, menandakan bahwa task asynchronous telah selesai.

Jadi, urutan pernyataan cetak mencerminkan sifat asynchronous dari task yang di-spawn, di mana task tersebut berjalan secara concurrent dengan function main dan tidak memblokir eksekusinya.