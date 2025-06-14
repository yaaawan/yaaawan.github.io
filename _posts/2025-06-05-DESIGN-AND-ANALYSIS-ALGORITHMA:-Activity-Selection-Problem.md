# Activity Selection Problem
## Masalah Pemilihan Aktivitas Maksimum yang Tidak Saling Tumpang Tindih

---

## 📖 Daftar Isi
1. [Pendahuluan](#pendahuluan)
2. [Definisi Masalah](#definisi-masalah)
3. [Algoritma Greedy](#algoritma-greedy)
4. [Langkah-langkah Algoritma](#langkah-langkah-algoritma)
5. [Pseudocode](#pseudocode)
6. [Contoh Implementasi](#contoh-implementasi)
7. [Analisis Kompleksitas](#analisis-kompleksitas)
8. [Aplikasi dalam Dunia Nyata](#aplikasi-dalam-dunia-nyata)
9. [Kelebihan dan Keterbatasan](#kelebihan-dan-keterbatasan)
10. [Kesimpulan](#kesimpulan)

---

## 🎯 Pendahuluan

**Activity Selection Problem** merupakan masalah optimasi klasik dalam ilmu komputer yang bertujuan untuk memilih jumlah aktivitas maksimum yang dapat dilakukan oleh satu orang atau resource tanpa adanya tumpang tindih waktu.

### Tujuan Utama
- Memilih jumlah aktivitas maksimum yang tidak saling bertabrakan
- Mengoptimalkan penggunaan waktu dan resource
- Mencari solusi optimal dengan pendekatan yang efisien

---

## 🔍 Definisi Masalah

### Input
Diberikan sekumpulan aktivitas dengan:
- **Waktu mulai (s)**: Waktu dimulainya aktivitas
- **Waktu selesai (f)**: Waktu berakhirnya aktivitas

### Kondisi Kompatibilitas
Dua aktivitas dikatakan **kompatibel** jika:
- Mereka tidak tumpang tindih secara waktu
- Salah satu aktivitas selesai sebelum aktivitas lainnya dimulai
- Secara matematis: `f[i] ≤ s[j]` atau `f[j] ≤ s[i]`

### Output
Subset terbesar dari aktivitas yang semuanya kompatibel satu sama lain.

---

## ⚡ Algoritma Greedy

### Konsep Dasar
**Algoritma Greedy** adalah strategi pemecahan masalah optimasi yang:
- Membuat pilihan yang tampak paling baik pada setiap langkah
- Bersifat "serakah" dengan mengambil pilihan terbaik saat itu
- Tidak mempertimbangkan konsekuensi di masa depan
- Berharap rangkaian pilihan lokal terbaik menghasilkan solusi global optimal

### Strategi untuk Activity Selection
Dalam konteks Activity Selection Problem, algoritma greedy diterapkan dengan:
- **Memilih aktivitas yang memiliki waktu selesai paling awal** di setiap langkah
- Maksimalisasi sisa waktu yang tersedia untuk aktivitas lainnya
- Pemilihan yang optimal berdasarkan *earliest finish time*

---

## 📋 Langkah-langkah Algoritma

### Algoritma Activity Selection Problem

1. **Pengurutan Awal**
   - Urutkan semua aktivitas berdasarkan waktu selesai (finish time) secara ascending

2. **Inisialisasi**
   - Pilih aktivitas pertama (aktivitas dengan waktu selesai paling awal)
   - Tambahkan ke dalam himpunan solusi

3. **Iterasi Pemilihan**
   - Untuk setiap aktivitas berikutnya:
     - Cek apakah waktu mulainya ≥ waktu selesai aktivitas terakhir yang dipilih
     - Jika ya, pilih aktivitas tersebut
     - Update aktivitas terakhir yang dipilih

4. **Output**
   - Kembalikan himpunan aktivitas yang terpilih

---

## 💻 Pseudocode

```
ACTIVITY-SELECTOR(s, f, n)
    A = {a₁}                    // Pilih aktivitas pertama
    j = 1                       // Index aktivitas terakhir yang dipilih
    
    for i = 2 to n
        if s[i] >= f[j]         // Waktu mulai >= waktu selesai terakhir
            A = A ∪ {aᵢ}        // Pilih aktivitas aᵢ
            j = i               // Update aktivitas terakhir yang dipilih
    
    return A
```

### Penjelasan Parameter
- **s**: Array yang berisi waktu mulai dari setiap aktivitas
- **f**: Array yang berisi waktu selesai dari setiap aktivitas (sudah diurutkan)
- **n**: Jumlah total aktivitas
- **A**: Himpunan aktivitas yang terpilih
- **j**: Index aktivitas terakhir yang dipilih

---

## 🔧 Contoh Implementasi

### Contoh Kasus
```
Aktivitas: A₁  A₂  A₃  A₄  A₅  A₆
Mulai (s):  1   3   0   5   8   5
Selesai(f): 4   5   6   7   9  10
```

### Langkah Penyelesaian

1. **Setelah diurutkan berdasarkan waktu selesai:**
   ```
   A₁: [1,4], A₂: [3,5], A₃: [0,6], A₄: [5,7], A₅: [8,9], A₆: [5,10]
   ```

2. **Proses Pemilihan:**
   - Pilih A₁ (finish = 4)
   - A₂: start = 3 < 4 ❌ (tidak dipilih)
   - A₃: start = 0 < 4 ❌ (tidak dipilih)
   - A₄: start = 5 ≥ 4 ✅ (dipilih, finish = 7)
   - A₅: start = 8 ≥ 7 ✅ (dipilih, finish = 9)
   - A₆: start = 5 < 9 ❌ (tidak dipilih)

3. **Hasil:** {A₁, A₄, A₅} = 3 aktivitas

---

## 📊 Analisis Kompleksitas

### Kompleksitas Waktu

| Operasi | Kompleksitas | Penjelasan |
|---------|-------------|------------|
| Pengurutan aktivitas | O(n log n) | Menggunakan algoritma pengurutan efisien |
| Pemilihan aktivitas | O(n) | Satu kali iterasi melalui semua aktivitas |
| **Total** | **O(n log n)** | Didominasi oleh operasi pengurutan |

#### Detail Kompleksitas Waktu
- **Langkah pertama**: Mengurutkan aktivitas berdasarkan waktu selesai menggunakan Quick Sort atau Merge Sort membutuhkan waktu O(n log n)
- **Langkah kedua**: Sekali iterasi melalui n aktivitas untuk memilih aktivitas yang kompatibel membutuhkan waktu O(n)
- **Hasil**: Total kompleksitas waktu adalah O(n log n)

### Kompleksitas Ruang

| Komponen | Kompleksitas | Penjelasan |
|----------|-------------|------------|
| Menyimpan aktivitas | O(n) | Array untuk n aktivitas |
| Menyimpan hasil | O(n) | Dalam kasus terburuk semua aktivitas dipilih |
| Pengurutan tambahan | O(log n) - O(n) | Tergantung implementasi algoritma pengurutan |
| **Total** | **O(n)** | Tidak memerlukan ruang tambahan yang signifikan |

#### Detail Kompleksitas Ruang
- Algoritma memerlukan ruang O(n) untuk menyimpan n aktivitas dalam array atau vektor
- Ruang tambahan untuk menyimpan hasil (subset aktivitas yang dipilih)
- Operasi pengurutan mungkin memerlukan ruang tambahan O(log n) atau O(n)

---

## 🌍 Aplikasi dalam Dunia Nyata

### 🏢 Penjadwalan & Fasilitas
- **Jadwal ruang kelas**: Mengoptimalkan penggunaan ruang kelas di universitas
- **Meeting rooms**: Penjadwalan ruang meeting di kantor
- **Laboratorium**: Alokasi waktu penggunaan laboratorium
- **Fasilitas olahraga**: Penjadwalan lapangan dan gym

### 💻 Sistem Operasi
- **Jadwal proses CPU**: Scheduling algoritma untuk mengoptimalkan penggunaan CPU
- **Alokasi memori**: Manajemen memori dalam sistem operasi
- **Resource management**: Alokasi resource sistem secara optimal

### 🚛 Logistik
- **Jadwal pengiriman**: Optimalisasi rute dan waktu pengiriman
- **Rute kendaraan**: Vehicle routing problem
- **Supply chain management**: Koordinasi aktivitas dalam rantai supply

### 📡 Telekomunikasi
- **Alokasi bandwidth**: Pembagian bandwidth untuk berbagai aplikasi
- **Jadwal transmisi data**: Scheduling transmisi data dalam jaringan
- **Network resource allocation**: Alokasi resource dalam jaringan komunikasi

---

## ⚖️ Kelebihan dan Keterbatasan

### ✅ Kelebihan

| Aspek | Deskripsi |
|-------|-----------|
| **Kesederhanaan** | Mudah dipahami dan diimplementasikan |
| **Efisiensi** | Kompleksitas waktu O(n log n) efisien untuk dataset besar |
| **Optimalitas** | Memberikan solusi optimal untuk masalah dasar |
| **Praktis** | Dapat diterapkan dalam berbagai domain aplikasi |

### ❌ Keterbatasan

| Aspek | Deskripsi |
|-------|-----------|
| **Preprocessing** | Membutuhkan proses pengurutan awal |
| **Batasan Kompleks** | Tidak cocok untuk masalah dengan banyak constraint tambahan |
| **Fleksibilitas** | Tidak dapat menangani prioritas, jarak, atau biaya |
| **Variasi Masalah** | Perlu pendekatan lain untuk variasi yang lebih kompleks |

### Alternatif untuk Kasus Kompleks
- **Dynamic Programming**: Untuk masalah dengan overlapping subproblems
- **Metaheuristic**: Untuk masalah dengan constraint yang sangat kompleks
- **Integer Linear Programming**: Untuk optimasi dengan constraint linear

---

## 🎯 Kesimpulan

**Activity Selection Problem** adalah masalah optimasi fundamental yang dapat diselesaikan secara elegant menggunakan algoritma greedy. Pendekatan ini memberikan solusi optimal dengan kompleksitas waktu O(n log n), membuatnya sangat praktis untuk aplikasi dunia nyata.

### Poin Kunci
- ✨ **Strategi Greedy**: Memilih aktivitas dengan waktu selesai paling awal
- ⚡ **Efisiensi**: Kompleksitas waktu O(n log n) dan ruang O(n)
- 🌟 **Optimalitas**: Menghasilkan solusi optimal untuk kasus dasar
- 🔧 **Aplikabilitas**: Luas digunakan dalam penjadwalan dan alokasi resource

### Rekomendasi Penggunaan
- **Gunakan algoritma greedy** untuk masalah activity selection dasar
- **Pertimbangkan metode lain** untuk kasus dengan constraint tambahan yang kompleks
- **Evaluasi kebutuhan** spesifik aplikasi sebelum memilih pendekatan

---

**Dipresentasikan oleh Kelompok 1**
- Ahmad Hidayat
- Suci Sri Aulia
- Yud Bryawan
- Julio Rema Palotong
- Naailah Mazaya

*Tanggal: 6 Mei 2025*







Activity Selection Problem
Fractional Knapsack
Huffman Coding
N-Queens Problem
Subset Sum Problem
Rat in a Maze
Breadth-First Search (BFS)
Depth-First Search (DFS)
 Kahn’s Algorithm
Dijkstra’s Algorithm

