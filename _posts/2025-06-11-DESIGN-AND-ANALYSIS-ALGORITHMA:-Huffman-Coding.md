
### Huffman Coding

## Pengantar
- **Definisi**: Algoritma kompresi data *lossless* (tanpa kehilangan data) yang dikembangkan oleh **David A. Huffman (1952)**.
- **Fungsi**: Mengurangi ukuran data dengan mengganti simbol berfrekuensi tinggi dengan kode bit pendek.
- **Aplikasi**: Digunakan dalam format kompresi seperti ZIP, JPEG, dan MP3.

---

## Konsep Dasar
- Berdasarkan **frekuensi karakter** dalam data:
  - Frekuensi tinggi → kode bit **pendek**.
  - Frekuensi rendah → kode bit **panjang**.
- Menggunakan struktur **pohon biner** untuk merepresentasikan kode.

---

## Proses Pembuatan Kode
1. Hitung frekuensi kemunculan tiap karakter.
2. Buat simpul untuk setiap karakter.
3. Gabungkan dua simpul dengan **frekuensi terkecil** menjadi simpul baru.
4. Ulangi langkah 3 hingga terbentuk satu pohon Huffman.
5. Beri label:
   - **0** untuk sisi kiri
   - **1** untuk sisi kanan
6. Tentukan kode biner tiap karakter dengan *traversal* dari akar ke daun.

---

## Contoh Kasus
- **Data**: `"ABBCCCDDDD"`
- **Frekuensi**:  
  `A:1`, `B:2`, `C:3`, `D:4`
- **Kode Huffman**:
  ```
  D: 0, C: 10, B: 110, A: 111
  ```
- **Hasil**: Ukuran terkompresi lebih kecil dari representasi asli.

---

## Simulasi Kompresi
- **Pesan**: `"BCCABBDDAECCBBAEDDCC"` (20 karakter)
- **Kompresi ASCII**:
  - 1 karakter = 8 bit → Total bit = `20 × 8 = 160 bit`.
- **Kompresi Huffman**:
  | Karakter | Frekuensi | Kode Bit | Total Bit |
  |----------|-----------|----------|-----------|
  | A        | 3         | 001 (3)  | 9         |
  | B        | 5         | 10  (2)  | 10        |
  | C        | 7         | ?   (2)  | 14        |
  | D        | 4         | ?   (2)  | 8         |
  | E        | 1         | ?   (3)  | 3         |
  - **Total Bit** = `9 + 10 + 14 + 8 + 3 = 44 bit` (atau **45 bit** sesuai slide).
- **Penghematan**: `160 bit` → `45 bit` (**reduksi ~72%**).

---

## Implementasi
- **Pseudocode**:
  ```python
  while len(simpul) > 1:
      ambil 2 simpul frekuensi terkecil
      buat simpul baru (frekuensi = jumlah keduanya)
      tambahkan ke antrian prioritas
  ```
- **Kompleksitas Waktu**: `O(N log N)` (N = jumlah karakter unik).

---

## Kelebihan & Kekurangan
| **Kelebihan**                          | **Kekurangan**                     |
|----------------------------------------|-------------------------------------|
| ✅ Lossless (data asli bisa direkonstruksi) | ❌ Kurang optimal jika frekuensi karakter merata |
| ✅ Efisien untuk data distribusi tidak merata | ❌ Memerlukan tabel kode untuk dekompresi |
| ✅ Digunakan luas di sistem kompresi    |                                     |

---

