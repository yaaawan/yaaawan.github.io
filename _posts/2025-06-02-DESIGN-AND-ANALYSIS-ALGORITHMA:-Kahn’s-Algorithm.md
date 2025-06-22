```markdown
# Kahn's Algorithm

## Pengantar
- **Definisi**: Algoritma untuk melakukan *topological sorting* pada **Directed Acyclic Graph (DAG)** berbasis **BFS**.
- **Fungsi**: Menentukan urutan node berdasarkan ketergantungan (dependency) dengan aturan:  
  *Jika ada edge `u → v`, maka `u` harus muncul sebelum `v` dalam urutan.*
- **Dikembangkan oleh**: Arthur Kahn (1962).

---

## Konsep Dasar
- **In-degree**: Jumlah edge masuk ke suatu node.
- **Prinsip Kerja**:
  1. Node dengan `in-degree = 0` diproses terlebih dahulu.
  2. Edge yang terhubung dihapus, dan `in-degree` node tetangga diperbarui.
  3. Ulangi hingga semua node terproses atau terdeteksi siklus.

---

## Proses Algoritma
1. Hitung `in-degree` setiap node.
2. Masukkan node dengan `in-degree = 0` ke dalam *queue*.
3. Proses node dari *queue*:
   - Kurangi `in-degree` node tetangga.
   - Jika `in-degree` tetangga menjadi 0, tambahkan ke *queue*.
4. Jika hasil urutan tidak mencakup semua node → **graf memiliki siklus**.

---

## Contoh Kasus
### **Kasus 1**: Dependensi Mata Kuliah
- **Graf**:  
  `A → C`, `B → C`, `C → D`
- **Urutan Valid**:  
  `A → B → C → D` atau `B → A → C → D`

### **Kasus 2**: Manajemen Proyek
- **Graf**:  
  `A → D`, `F → B`, `B → D`, `F → A`, `D → C`
- **Urutan Valid**:  
  `F → A → B → D → C` atau `F → B → A → D → C`

---

## Implementasi Kode (C++)
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
using namespace std;

void topologicalSort(unordered_map<char, vector<char>>& graph, unordered_map<char, int>& inDegree) {
    queue<char> q;
    for (auto& node : inDegree) {
        if (node.second == 0) q.push(node.first);
    }

    while (!q.empty()) {
        char u = q.front(); q.pop();
        cout << u << " ";
        for (char v : graph[u]) {
            if (--inDegree[v] == 0) q.push(v);
        }
    }
}
```

---

## Kelebihan & Kekurangan
| **Kelebihan**                          | **Kekurangan**                     |
|----------------------------------------|-------------------------------------|
| ✅ Kompleksitas waktu **O(V + E)** (efisien) | ❌ Hanya bekerja pada **DAG** (tidak boleh ada siklus) |
| ✅ Dapat mendeteksi siklus             | ❌ Output tidak unik (banyak urutan valid) |
| ✅ Cocok untuk masalah dependensi (jadwal, kompilasi) | ❌ Memerlukan struktur data tambahan (`in-degree`, `queue`) |

---

## Aplikasi
- Penjadwalan tugas (project management).
- Kompilasi program (urutan kompilasi file).
- Penyusunan mata kuliah berdasarkan prasyarat.

---
