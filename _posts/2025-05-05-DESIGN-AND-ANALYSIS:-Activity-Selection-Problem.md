---
layout: post
title: "Materi: Activity Selection Problem"
date: 2025-05-06 10:00:00 +0800
categories: algoritma optimasi
---

## Pendahuluan dan Definisi Masalah

**Activity Selection Problem** adalah masalah optimasi klasik untuk memilih jumlah aktivitas maksimum yang dapat dilakukan oleh satu sumber daya (misalnya, satu orang) dari sekumpulan aktivitas yang diberikan, di mana setiap aktivitas memiliki waktu mulai (s) dan waktu selesai (f). Syarat utamanya adalah aktivitas yang dipilih tidak boleh tumpang tindih. Dua aktivitas dianggap kompatibel jika waktu selesai satu aktivitas lebih awal dari atau sama dengan waktu mulai aktivitas lainnya.

Tujuannya adalah untuk menemukan subset terbesar dari aktivitas yang semuanya kompatibel satu sama lain.

## Konsep Dasar Algoritma Greedy

Untuk menyelesaikan masalah ini, kita menggunakan **Algoritma Greedy**. Pendekatan ini bekerja dengan membuat pilihan yang paling optimal pada setiap langkah tanpa mempertimbangkan konsekuensi di masa depan. Dalam konteks *Activity Selection Problem*, strategi "serakah" yang terbukti optimal adalah dengan selalu memilih aktivitas yang memiliki **waktu selesai (finish time) paling awal**.

Dengan memilih aktivitas yang selesai lebih cepat, kita memaksimalkan sisa waktu yang tersedia untuk aktivitas lain, sehingga memungkinkan lebih banyak aktivitas untuk dijadwalkan.

## Algoritma Activity Selection Problem

Langkah-langkah algoritma untuk menyelesaikan masalah ini adalah sebagai berikut:

1.  **Urutkan** semua aktivitas berdasarkan waktu selesainya (finish time) dari yang terkecil hingga terbesar.
2.  **Pilih** aktivitas pertama dari daftar yang sudah diurutkan. Aktivitas ini dijamin menjadi bagian dari solusi optimal.
3.  **Iterasi** ke aktivitas berikutnya. Untuk setiap aktivitas, periksa apakah waktu mulainya (start time) lebih besar dari atau sama dengan waktu selesai (finish time) dari aktivitas terakhir yang dipilih.
4.  Jika ya, **pilih** aktivitas tersebut dan perbarui aktivitas terakhir yang dipilih.
5.  Ulangi langkah 3 dan 4 sampai semua aktivitas telah diperiksa.

### Pseudocode

ACTIVITY-SELECTOR(s, f, n)// s: array waktu mulai// f: array waktu selesai (sudah diurutkan)// n: jumlah total aktivitasA = {a1}      // Pilih aktivitas pertamaj = 1         // Indeks aktivitas terakhir yang dipilihfor i = 2 to nif s[i] >= f[j]   // Jika tidak tumpang tindihA = A ∪ {ai}  // Tambahkan aktivitas ini ke solusij = i         // Perbarui indeks aktivitas terakhirreturn A
## Contoh Kasus dan Solusi

Diberikan sekumpulan aktivitas sebagai berikut:

| Aktivitas | Mulai (s) | Selesai (f) |
| :-------- | :-------- | :---------- |
| A1        | 1         | 4           |
| A2        | 3         | 5           |
| A3        | 0         | 6           |
| A4        | 5         | 7           |
| A5        | 8         | 9           |
| A6        | 5         | 9           |

**Langkah Solusi:**

1.  **Urutkan berdasarkan waktu selesai:** [A1, A2, A3, A4, A5, A6]. (Dalam contoh ini, urutan setelah diurutkan berdasarkan waktu selesai adalah A1, A2, A3, A4, A5, A6).
2.  **Pilih A1** (selesai paling awal, f=4). Solusi: `{A1}`.
3.  **Iterasi:**
    * Cek A2 (mulai=3, selesai=5): `3 < 4` (tumpang tindih). Abaikan.
    * Cek A3 (mulai=0, selesai=6): `0 < 4` (tumpang tindih). Abaikan.
    * Cek A4 (mulai=5, selesai=7): `5 >= 4` (kompatibel). **Pilih A4**. Solusi: `{A1, A4}`. Waktu selesai terakhir sekarang adalah 7.
    * Cek A5 (mulai=8, selesai=9): `8 >= 7` (kompatibel). **Pilih A5**. Solusi: `{A1, A4, A5}`. Waktu selesai terakhir sekarang adalah 9.
    * Cek A6 (mulai=5, selesai=9): `5 < 9` (tumpang tindih dengan A5, karena waktu selesai terakhir adalah 9, namun jika dibandingkan dengan A4, `5 < 7`). Abaikan.

**Solusi Optimal:** `{A1, A4, A5}` (Total 3 aktivitas).

## Analisis Kompleksitas

* **Kompleksitas Waktu:** **O(n log n)**.
    * Langkah dominan adalah pengurutan aktivitas berdasarkan waktu selesai, yang memakan waktu O(n log n).
    * Proses iterasi untuk memilih aktivitas hanya memakan waktu O(n).
* **Kompleksitas Ruang:** **O(n)**.
    * Dibutuhkan ruang untuk menyimpan daftar aktivitas (input) dan hasilnya (output).

## Aplikasi Dunia Nyata

* **Penjadwalan:** Alokasi ruang kelas, ruang rapat, atau jadwal penggunaan fasilitas olahraga.
* **Sistem Operasi:** Penjadwalan proses pada CPU.
* **Logistik:** Perencanaan jadwal pengiriman barang untuk memaksimalkan penggunaan kendaraan.
* **Telekomunikasi:** Alokasi bandwidth untuk transmisi data.

## Kesimpulan

*Activity Selection Problem* adalah masalah penjadwalan fundamental yang dapat diselesaikan secara efisien menggunakan algoritma *greedy*. Dengan mengurutkan aktivitas berdasarkan waktu selesainya dan memilih secara serakah, kita dapat mencapai solusi optimal dengan kompleksitas waktu **O(n log n)**. Meskipun sederhana, algoritma ini sangat kuat dan memiliki banyak aplikasi praktis. Namun, untuk masalah dengan batasan yang lebih kompleks (seperti prioritas atau biaya), mungkin diperlukan pendekatan lain seperti pemrograman dinamis.
