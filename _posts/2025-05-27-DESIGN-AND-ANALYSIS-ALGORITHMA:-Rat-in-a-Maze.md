
# Rat in a Maze: Panduan Lengkap Algoritma Backtracking



## 🐭 Pengenalan

**Rat in a Maze** adalah salah satu masalah klasik dalam algoritma backtracking di mana seekor tikus harus menemukan jalan keluar dari labirin dengan mengikuti jalur tertentu dari titik awal ke titik tujuan.

### Definisi Problem

> **Problem Statement:** Seekor tikus ditempatkan di posisi (0,0) dalam matriks N×N dan harus mencapai tujuan di (N-1, N-1). Temukan semua kemungkinan jalur yang dapat diambil tikus untuk mencapai dari sumber ke tujuan.

### Aturan Dasar

- **Representasi Maze:** Matriks dengan nilai 0 dan 1
  - `1` = Sel dapat dilewati (jalan)
  - `0` = Sel terhalang (tembok)
- **Pergerakan:** 4 arah (Up, Down, Left, Right)
- **Constraint:** Tidak ada sel yang dapat dikunjungi lebih dari satu kali dalam satu jalur

---

## 🎯 Prinsip Kerja Algoritma

### Langkah-Langkah Backtracking

1. **Explorasi:** Tikus mencoba bergerak ke salah satu dari 4 arah
2. **Validasi:** Periksa apakah langkah valid (dalam batas maze dan bukan tembok)
3. **Rekursi:** Jika valid, lanjutkan pencarian dari posisi baru
4. **Backtrack:** Jika tidak ada jalan dari posisi saat ini, mundur ke posisi sebelumnya
5. **Ulangi:** Coba arah lain yang belum dieksplorasi

### Arah Pergerakan

| Simbol | Arah | Koordinat |
|--------|------|-----------|
| `U` | Up (Atas) | (x-1, y) |
| `D` | Down (Bawah) | (x+1, y) |
| `L` | Left (Kiri) | (x, y-1) |
| `R` | Right (Kanan) | (x, y+1) |

---

## 📊 Contoh Simulasi

### Input Maze 4×4

```
1 0 0 0
1 1 0 1  
1 1 0 0
0 1 1 1
```

- **Start:** (0,0) 
- **End:** (3,3)
- **Goal:** Temukan jalur dari START ke END

### Trace Execution

#### Langkah 1: Posisi (0,0)
```
[S] 0 0 0
 1  1 0 1  
 1  1 0 0
 0  1 1 1
```
**Pilihan:** Down (D) ✅, Right (R) ❌ (tembok)  
**Aksi:** Pilih Down → (1,0)

#### Langkah 2: Posisi (1,0)
```
 1  0 0 0
[*] 1 0 1  
 1  1 0 0
 0  1 1 1
```
**Pilihan:** Down (D) ✅, Right (R) ✅  
**Aksi:** Pilih Right → (1,1)

#### Langkah 3: Posisi (1,1)
```
 1  0 0 0
 *  [*] 0 1  
 1   1  0 0
 0   1  1 1
```
**Pilihan:** Down (D) ✅  
**Aksi:** Pilih Down → (2,1)

#### Langkah 4: Posisi (2,1)
```
 1  0 0 0
 *  * 0 1  
 1  [*] 0 0
 0   1  1 1
```
**Pilihan:** Down (D) ✅  
**Aksi:** Pilih Down → (3,1)

#### Langkah 5: Posisi (3,1)
```
 1  0 0 0
 *  * 0 1  
 1  * 0 0
 0  [*] 1 1
```
**Pilihan:** Right (R) ✅  
**Aksi:** Pilih Right → (3,2)

#### Langkah 6: Posisi (3,2)
```
 1  0 0 0
 *  * 0 1  
 1  * 0 0
 0  * [*] 1
```
**Pilihan:** Right (R) ✅  
**Aksi:** Pilih Right → (3,3) **TUJUAN TERCAPAI!**

### Hasil Akhir

**Jalur yang ditemukan:** `DDRDRR`
- D: (0,0) → (1,0)
- D: (1,0) → (2,0)... **BACKTRACK**
- R: (1,0) → (1,1)
- D: (1,1) → (2,1)
- R: (2,1) → (3,1)
- R: (3,1) → (3,2)
- R: (3,2) → (3,3) ✅

---

## 💻 Implementasi C++

### Struktur Dasar

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class RatMaze {
private:
    vector<vector<int>> maze;
    vector<vector<bool>> visited;
    vector<string> solutions;
    int n;
    
    // Arah pergerakan: Down, Left, Right, Up
    int dx[4] = {1, 0, 0, -1};
    int dy[4] = {0, -1, 1, 0};
    string directions = "DLRU";

public:
    RatMaze(vector<vector<int>>& inputMaze) {
        maze = inputMaze;
        n = maze.size();
        visited.assign(n, vector<bool>(n, false));
    }
    
    bool isSafe(int x, int y) {
        return (x >= 0 && x < n && y >= 0 && y < n && 
                maze[x][y] == 1 && !visited[x][y]);
    }
    
    void solveMaze(int x, int y, string path) {
        // Base case: mencapai tujuan
        if (x == n-1 && y == n-1) {
            solutions.push_back(path);
            return;
        }
        
        visited[x][y] = true;
        
        // Coba semua 4 arah
        for (int i = 0; i < 4; i++) {
            int nextX = x + dx[i];
            int nextY = y + dy[i];
            
            if (isSafe(nextX, nextY)) {
                solveMaze(nextX, nextY, path + directions[i]);
            }
        }
        
        // Backtrack
        visited[x][y] = false;
    }
    
    vector<string> findAllPaths() {
        solutions.clear();
        if (maze[0][0] == 1 && maze[n-1][n-1] == 1) {
            solveMaze(0, 0, "");
        }
        return solutions;
    }
};
```

### Penggunaan

```cpp
int main() {
    vector<vector<int>> maze = {
        {1, 0, 0, 0},
        {1, 1, 0, 1},
        {1, 1, 0, 0},
        {0, 1, 1, 1}
    };
    
    RatMaze solver(maze);
    vector<string> paths = solver.findAllPaths();
    
    cout << "Jalur yang ditemukan:" << endl;
    for (const string& path : paths) {
        cout << path << endl;
    }
    
    return 0;
}
```

---

## 📈 Analisis Kompleksitas

### Kompleksitas Waktu

- **Worst Case:** O(4^(N²))
- **Penjelasan:** Untuk setiap sel, ada maksimal 4 pilihan arah
- **Best Case:** O(N) jika hanya ada satu jalur langsung

### Kompleksitas Ruang

- **Space Complexity:** O(N²)
- **Stack Space:** O(N²) untuk rekursi
- **Auxiliary Space:** O(N²) untuk matriks visited

### Optimasi yang Mungkin

1. **Pruning:** Hentikan pencarian jika sudah terlalu jauh dari tujuan
2. **Memoization:** Simpan hasil sub-problem yang sudah dihitung
3. **Heuristic:** Gunakan A* atau algoritma informed search

---

## ⚖️ Kelebihan dan Kekurangan

### ✅ Kelebihan

| Aspek | Deskripsi |
|-------|-----------|
| **Sederhana** | Mudah diimplementasikan dan dipahami |
| **Lengkap** | Menemukan semua solusi yang mungkin |
| **Fleksibel** | Dapat diadaptasi untuk berbagai variasi maze |
| **Memory Efficient** | Tidak memerlukan struktur data kompleks |

### ❌ Kekurangan

| Aspek | Deskripsi |
|-------|-----------|
| **Inefficient** | Kurang efisien untuk maze berukuran besar |
| **Stack Overflow** | Risiko stack overflow pada rekursi dalam |
| **Non-Optimal** | Tidak menjamin jalur terpendek |
| **Redundant** | Mungkin mengulang eksplorasi area yang sama |

---

## 🌍 Aplikasi di Dunia Nyata

### 1. 🤖 Robot Navigasi
- **Autonomous vehicles** mencari jalur di lingkungan dengan obstacle
- **Warehouse robots** navigasi di antara rak-rak penyimpanan
- **Drone path planning** untuk menghindari no-fly zones

### 2. 🗺️ GPS dan Navigasi
- **Route finding** dalam sistem navigasi
- **Alternative route** ketika ada jalan yang terblokir
- **Indoor navigation** di mall atau gedung besar

### 3. 🎮 Game Development
- **NPC pathfinding** dalam video games
- **Puzzle games** seperti maze solver
- **AI opponents** yang mencari jalur optimal

### 4. 📊 Simulasi dan Modeling
- **Network routing** dalam jaringan komputer
- **Circuit design** untuk menghindari interferensi
- **Urban planning** untuk desain jalan dan akses

---

## 🔄 Variasi Problem

### 1. Multiple Destinations
Tikus harus mengunjungi beberapa titik sebelum mencapai tujuan akhir.

### 2. Weighted Maze
Setiap sel memiliki cost berbeda, cari jalur dengan cost minimum.

### 3. Dynamic Maze
Maze berubah seiring waktu, perlu adaptasi real-time.

### 4. Multi-Agent
Beberapa tikus mencari jalur secara bersamaan tanpa collision.

---

## 🎯 Tips Optimasi

### 1. Direction Ordering
```cpp
// Prioritas arah berdasarkan kedekatan ke tujuan
string directions = "RDLU"; // Right-Down prioritas tinggi
```

### 2. Early Termination
```cpp
if (solutions.size() >= MAX_SOLUTIONS) {
    return; // Hentikan pencarian jika sudah cukup
}
```

### 3. Distance Heuristic
```cpp
int manhattanDistance(int x, int y) {
    return abs(x - (n-1)) + abs(y - (n-1));
}
```

---

## 📚 Kesimpulan

**Rat in a Maze** merupakan excellent introduction untuk memahami algoritma **backtracking**. Meskipun sederhana dalam konsep, problem ini mengajarkan prinsip-prinsip fundamental dalam:

- **Recursive problem solving**
- **State space exploration** 
- **Constraint satisfaction**
- **Optimization techniques**

Algoritma ini menjadi foundation untuk masalah pathfinding yang lebih kompleks dan memiliki aplikasi luas dalam robotics, game development, dan sistem navigasi.

> 💡 **Key Takeaway:** Backtracking adalah powerful technique untuk exploring all possible solutions, namun perlu dioptimasi untuk problem berukuran besar.

---
