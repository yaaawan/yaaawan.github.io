# 🔍 Breadth-First Search (BFS)
## 🚀 Algoritma Penelusuran Graf yang Efisien

### 📚 Desain dan Analisis Algoritma



## 📑 Daftar Isi

1. [🌟 Pengertian](#-pengertian)
2. [💡 Konsep Dasar](#-konsep-dasar)
3. [📋 Langkah-langkah Algoritma](#-langkah-langkah-algoritma)
4. [⚙️ Cara Kerja BFS](#️-cara-kerja-bfs)
5. [💻 Implementasi Kode](#-implementasi-kode)
6. [🌐 Aplikasi Nyata](#-aplikasi-nyata)
7. [⚖️ Kelebihan dan Kekurangan](#️-kelebihan-dan-kekurangan)
8. [📌 Kesimpulan](#-kesimpulan)

---

## 🌟 Pengertian

**Breadth-First Search (BFS)** adalah metode untuk menjelajahi graph atau pohon dengan menelusuri simpul-simpul berdasarkan jaraknya dari titik awal. Secara sederhana, BFS akan mengeksplorasi semua simpul yang paling dekat terlebih dahulu, baru kemudian beralih ke simpul yang lebih jauh.

### 🎯 Karakteristik Utama
> 📍 Pendekatannya menyebar ke seluruh arah secara merata dari pusat

### 🏢 Ilustrasi Sederhana
Bayangkan kita sedang mencari ruang kelas di sebuah gedung kampus berlantai 3:
- 🥇 **Lantai 1**: Periksa semua ruangan
- 🥈 **Lantai 2**: Lanjut setelah lantai 1 selesai
- 🥉 **Lantai 3**: Terakhir setelah lantai 2 selesai

*Itulah cara kerja BFS: menjelajah secara mendatar!*

### 🚀 Aplikasi BFS
BFS banyak digunakan dalam berbagai aplikasi:
- 🎮 Pencarian rute tercepat dalam game
- 👥 Penelusuran jaringan sosial
- 🗺️ Pencarian jalur optimal dalam sistem navigasi dan peta digital

---

## 💡 Konsep Dasar

### 🔑 Tiga Konsep Fundamental BFS

| 🔢 No | 📌 Konsep | 📝 Deskripsi |
|-------|-----------|--------------|
| **1** | **Queue (Antrian)** | BFS menggunakan struktur data queue untuk menyimpan simpul yang akan dikunjungi |
| **2** | **Level-Order Traversal** | Mulai dari simpul awal → kunjungi semua tetangga → lanjut ke tetangga dari tetangga |
| **3** | **Per Lapisan** | Bersifat level-order traversal, mengunjungi node per lapisan/level |

### 📊 Visualisasi Konsep

```
Level 0: [A]           (Start Node)
Level 1: [B] [C]       (Tetangga A)
Level 2: [D] [E] [F]   (Tetangga dari B dan C)
Level 3: [G] [H]       (Tetangga dari D, E, F)
```

---

## 📋 Langkah-langkah Algoritma

### 🔄 Proses BFS Step-by-Step

#### 1️⃣ **Inisialisasi**
- Tentukan simpul awal (start node)
- Buat queue kosong
- Buat set untuk menyimpan node yang sudah dikunjungi

#### 2️⃣ **Enqueue Simpul Awal**
- Masukkan simpul awal ke dalam queue
- Tandai simpul awal sebagai "visited"

#### 3️⃣ **Proses Utama**
Selama queue tidak kosong:
```
a. Ambil simpul dari depan queue (dequeue)
b. Cek apakah simpul adalah tujuan
c. Jika bukan tujuan, lanjutkan
```

#### 4️⃣ **Eksplorasi Tetangga**
Untuk setiap tetangga dari simpul aktif:
- ✅ Jika belum dikunjungi → tandai sebagai dikunjungi
- ➕ Tambahkan ke queue (enqueue)

#### 5️⃣ **Iterasi**
- 🔁 Ulangi langkah 3-4 sampai:
  - ✅ Simpul tujuan ditemukan, atau
  - ❌ Queue kosong (tidak ada solusi)

---

## ⚙️ Cara Kerja BFS

### 🎯 Contoh Penelusuran Graf

Mari kita lihat cara kerja BFS dengan contoh konkret:

```
       S (Start)
      / \
     A   B
    / \   \
   C   D   E
       |  / \
       F H   G (Goal)
```

### 📊 Tabel Penelusuran

| 🔄 Step | 📦 Queue | 🎯 Simpul Aktif | ✅ Visited | 
|---------|----------|-----------------|------------|
| 1 | [S] | S | S |
| 2 | [A, B] | A | S, A |
| 3 | [B, C, D] | B | S, A, B |
| 4 | [C, D, E, F] | C | S, A, B, C |
| 5 | [D, E, F] | D | S, A, B, C, D |
| 6 | [E, F] | E | S, A, B, C, D, E |
| 7 | [F, H, G] | F | S, A, B, C, D, E, F |
| 8 | [H, G] | H | S, A, B, C, D, E, F, H |
| 9 | [G] | **G** 🎉 | S, A, B, C, D, E, F, H, G |

**✅ Goal ditemukan!**

---

## 💻 Implementasi Kode

### 🔧 C++ Implementation

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <unordered_map>
#include <unordered_set>

using namespace std;

// Inisialisasi graf dengan adjacency list
unordered_map<char, vector<char>> graf;

// Menyimpan simpul yang sudah dikunjungi
unordered_map<char, bool> sudahDikunjungi;

// Menyimpan jalur: simpul sekarang -> simpul sebelumnya
unordered_map<char, char> parent;

void bfs(char start, char goal) {
    queue<char> q;
    q.push(start);
    sudahDikunjungi[start] = true;
    parent[start] = '-'; // Tidak ada parent untuk start
    
    while (!q.empty()) {
        char sekarang = q.front();
        q.pop();
        cout << "Kunjungi node: " << sekarang << endl;
        
        // Jika goal ditemukan, langsung berhenti
        if (sekarang == goal) break;
        
        for (char tetangga : graf[sekarang]) {
            if (!sudahDikunjungi[tetangga]) {
                q.push(tetangga);
                sudahDikunjungi[tetangga] = true;
                parent[tetangga] = sekarang;
            }
        }
    }
}

// Cetak jalur terpendek dari goal ke start
void cetakJalur(char start, char goal) {
    vector<char> jalur;
    char current = goal;
    
    while (current != '-') {
        jalur.push_back(current);
        current = parent[current];
    }
    
    cout << "\n=== HASIL ===" << endl;
    cout << "Start Node: " << start << endl;
    cout << "Goal Node: " << goal << endl;
    cout << "Jalur Terpendek: ";
    
    for (int i = jalur.size() - 1; i >= 0; i--) {
        cout << jalur[i];
        if (i != 0) cout << " -> ";
    }
    cout << endl;
}
```

### 🐍 Python Implementation

```python
from collections import deque

def bfs(graph, start, goal):
    """
    Implementasi BFS untuk mencari jalur dari start ke goal
    """
    # Inisialisasi
    queue = deque([start])
    visited = {start}
    parent = {start: None}
    path_found = False
    
    print(f"🚀 Memulai BFS dari {start} ke {goal}\n")
    
    while queue and not path_found:
        current = queue.popleft()
        print(f"📍 Mengunjungi: {current}")
        
        if current == goal:
            path_found = True
            break
            
        # Eksplorasi tetangga
        for neighbor in graph.get(current, []):
            if neighbor not in visited:
                visited.add(neighbor)
                parent[neighbor] = current
                queue.append(neighbor)
                print(f"   ➡️ Menambahkan {neighbor} ke queue")
    
    # Rekonstruksi jalur
    if path_found:
        path = []
        node = goal
        while node is not None:
            path.append(node)
            node = parent[node]
        path.reverse()
        
        print(f"\n✅ Jalur ditemukan: {' → '.join(path)}")
        return path
    else:
        print(f"\n❌ Tidak ada jalur dari {start} ke {goal}")
        return None

# Contoh penggunaan
if __name__ == "__main__":
    # Definisi graf
    graph = {
        'S': ['A', 'B'],
        'A': ['C', 'D'],
        'B': ['E', 'F'],
        'C': [],
        'D': ['F'],
        'E': ['H', 'G'],
        'F': [],
        'H': [],
        'G': []
    }
    
    # Jalankan BFS
    bfs(graph, 'S', 'G')
```

---

## 🌐 Aplikasi Nyata

### 1. 🎮 BFS dalam Game & Labirin

**Penggunaan**: Mencari jalan terpendek dari titik awal ke tujuan

#### 👾 Contoh: Pac-Man AI
- Ghost menghitung rute terpendek untuk mengejar pemain
- Mengeksplorasi semua jalur level demi level
- Menemukan exit dengan jumlah langkah minimal

```
Start → Level 1 → Level 2 → ... → Exit
  🟦      🟦🟦       🟦🟦🟦          🎯
```

### 2. 👥 BFS di Media Sosial

**Platform**: Facebook, LinkedIn, Twitter

#### 🔗 "Orang yang Mungkin Anda Kenal"
- **Level 1**: Teman langsung
- **Level 2**: Teman dari teman
- **Level 3**: Koneksi lebih jauh

```
You → Friends → Friends of Friends → Suggestions
👤  →   👥    →        👥👥        →     💡
```

### 3. 🖥️ BFS dalam Jaringan Komputer

**Fungsi**: Memetakan topologi jaringan

#### 🌐 Struktur Jaringan:
```
Router Utama (Level 0)
    ├── Switch 1 (Level 1)
    │   ├── PC1 (Level 2)
    │   └── PC2 (Level 2)
    └── Switch 2 (Level 1)
        ├── Server1 (Level 2)
        └── Server2 (Level 2)
```

### 4. 🗺️ BFS dalam Navigasi & Transportasi

**Aplikasi**: Google Maps, Grab, Gojek

#### 🚗 Pencarian Rute Optimal:
- Mencari jalur dengan paling sedikit persimpangan
- Asumsi: semua jalan memiliki "bobot" yang sama
- Ideal untuk navigasi dalam kota dengan blok-blok teratur

```
Start 📍 → Junction 1 → Junction 2 → ... → Destination 🏁
```

---

## ⚖️ Kelebihan dan Kekurangan

### ✅ Kelebihan BFS

| 🌟 Aspek | 📝 Deskripsi |
|----------|--------------|
| **🎯 Optimal** | Menjamin ditemukannya solusi terpendek (untuk graf tak berbobot) |
| **📊 Komplet** | Pasti menemukan solusi jika ada |
| **🔍 Sistematis** | Penelusuran terstruktur level per level |
| **🛡️ Predictable** | Perilaku yang dapat diprediksi |

### ❌ Kekurangan BFS

| ⚠️ Aspek | 📝 Deskripsi |
|----------|--------------|
| **💾 Memori** | Membutuhkan memori O(b^d) - eksponensial |
| **⏰ Waktu** | Kompleksitas waktu O(V + E) |
| **🔢 Tidak Efisien** | Untuk graf yang sangat besar atau dalam |
| **⚖️ Bobot** | Tidak optimal untuk graf berbobot |

### 📊 Kompleksitas Algoritma

| 📈 Aspek | 🔢 Kompleksitas |
|----------|-----------------|
| **Waktu** | O(V + E) |
| **Ruang** | O(V) |

*Dimana:*
- V = Jumlah vertex (simpul)
- E = Jumlah edge (sisi)

---

## 📌 Kesimpulan

### 🎯 Poin-Poin Penting

1. **📍 Definisi**: BFS adalah algoritma penelusuran graf yang menjelajahi simpul secara berlapis, dimulai dari simpul awal

2. **📦 Struktur Data**: Menggunakan queue untuk memastikan semua simpul dengan jarak terdekat dijelajahi lebih dulu

3. **🎯 Kegunaan Utama**: Sangat cocok untuk mencari jalur terpendek pada graf tak berbobot

4. **🌍 Aplikasi Luas**: Efektif digunakan dalam game, peta digital, jaringan sosial, dan analisis jaringan

### 💡 Kapan Menggunakan BFS?

#### ✅ Gunakan BFS ketika:
- Mencari jalur terpendek dalam graf tak berbobot
- Perlu menjelajahi semua node pada level yang sama
- Solusi berada dekat dengan root
- Membutuhkan semua solusi pada jarak tertentu

#### ❌ Hindari BFS ketika:
- Graf memiliki bobot pada edge
- Memori terbatas
- Graf sangat dalam dengan solusi jauh dari root
- Hanya perlu satu solusi (tidak harus terpendek)

