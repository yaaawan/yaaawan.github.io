# 🎯 Subset Sum Problem: Algoritma Klasik NP-Complete

### 📚 Desain dan Analisis Algoritma

## 📑 **Daftar Isi**

1. [🎯 Apa itu Subset Sum Problem?](#-apa-itu-subset-sum-problem)
2. [🧩 Definisi Formal](#-definisi-formal)
3. [🔀 Variasi Masalah](#-variasi-masalah)
4. [📐 Pendekatan Penyelesaian](#-pendekatan-penyelesaian)
5. [💻 Implementasi Code](#-implementasi-code)
6. [🌍 Aplikasi di Dunia Nyata](#-aplikasi-di-dunia-nyata)
7. [📊 Analisis Kompleksitas](#-analisis-kompleksitas)
8. [📝 Kesimpulan](#-kesimpulan)

---

## 🎯 **Apa itu Subset Sum Problem?**

> **Subset Sum Problem** adalah masalah klasik dalam ilmu komputer yang termasuk dalam kategori **NP-Complete**. Diberikan sebuah himpunan bilangan bulat dan sebuah nilai target, tugasnya adalah menentukan apakah ada subset dari himpunan tersebut yang jumlah elemennya sama dengan nilai target.

### 🎲 **Karakteristik Utama:**

- 🔍 **Decision Problem**: Hanya membutuhkan jawaban "ya" atau "tidak"
- 🧮 **NP-Complete**: Tidak ada algoritma polynomial time yang diketahui
- 🎯 **Goal**: Mencari subset dengan jumlah tepat sama dengan target

---

## 🧩 **Definisi Formal**

### 📝 **Notasi Matematika:**

```
Input:  Set S = {a₁, a₂, ..., aₙ} dan target T
Output: True jika ∃ subset S' ⊆ S dimana Σ(S') = T
        False jika tidak ada
```

### 💡 **Contoh Sederhana:**

<div align="center">

| Input | Target | Output | Penjelasan |
|:------|:------:|:------:|:-----------|
| `{1, 2, 3}` | 3 | ✅ True | Subset `{1, 2}` atau `{3}` |
| `{10, 20, 30, 40, 50}` | 25 | ❌ False | Tidak ada kombinasi = 25 |

</div>

### 🎨 **Visualisasi Subset:**

```python
Set = {1, 2, 3}
Semua subset dan jumlahnya:
┌─────────────┬──────────┐
│   Subset    │   Sum    │
├─────────────┼──────────┤
│     {}      │    0     │
│    {1}      │    1     │
│    {2}      │    2     │
│    {3}      │    3     │
│   {1,2}     │    3 ✓   │
│   {1,3}     │    4     │
│   {2,3}     │    5     │
│  {1,2,3}    │    6     │
└─────────────┴──────────┘
```

---

## 🔀 **Variasi Masalah**

### 1️⃣ **Bounded Subset Sum Problem**

> Setiap elemen hanya dapat digunakan **sekali**.

```python
📌 Contoh:
Set = {3, 34, 4, 12, 5, 2}, Target = 9
✅ Solusi: {4, 5} = 9
❌ Tidak boleh: {3, 3, 3} = 9 (elemen diulang)
```

**Aplikasi**: 
- 📦 Pemilihan barang dengan stok terbatas
- 💼 Alokasi resource yang unik

### 2️⃣ **Unbounded Subset Sum Problem**

> Setiap elemen boleh digunakan **berulang kali**.

```python
📌 Contoh:
Set = {1, 3, 4}, Target = 6
✅ Solusi-solusi valid:
   • 1 + 1 + 1 + 3 = 6
   • 3 + 3 = 6
   • 1 + 1 + 4 = 6
```

**Aplikasi**:
- 💰 Penukaran uang (coin change)
- 🏗️ Pemotongan material

### 3️⃣ **Partition Problem**

> Membagi himpunan menjadi dua subset dengan jumlah **sama**.

```python
📌 Contoh:
Set = {1, 5, 11, 5}
Total = 22 → Target per subset = 11
✅ Solusi: {11} dan {1, 5, 5}
```

**Aplikasi**:
- ⚖️ Load balancing
- 👥 Pembagian tim yang seimbang

### 4️⃣ **Exact k-Elements Subset Sum**

> Subset harus terdiri dari **tepat k elemen**.

```python
📌 Contoh:
Set = {1, 2, 3, 4, 5}, Target = 9, k = 2
✅ Valid: {4, 5} (2 elemen, jumlah = 9)
❌ Invalid: {2, 3, 4} (3 elemen, meski jumlah = 9)
```

**Aplikasi**:
- 🏀 Pemilihan tim dengan jumlah pemain tetap
- 🎯 Portfolio dengan jumlah aset tertentu

### 5️⃣ **Multi-Target Subset Sum**

> Memenuhi **lebih dari satu kriteria** sekaligus.

```python
📌 Contoh:
Barang A: harga = 40, berat = 1 kg
Barang B: harga = 60, berat = 1.5 kg
Barang C: harga = 30, berat = 0.5 kg

Target: total harga ≤ 100 DAN total berat ≤ 2 kg

✅ Valid: A + C (harga = 70, berat = 1.5 kg)
❌ Invalid: A + B (harga = 100, berat = 2.5 kg)
```

### 6️⃣ **Approximate Subset Sum**

> Mencari subset yang **mendekati** target (tidak harus tepat).

```python
📌 Contoh:
Set = {2, 5, 10, 14}, Target = 16
Tidak ada subset = 16
✅ Solusi terbaik: {5, 10} = 15 (paling dekat)
```

---

## 📐 **Pendekatan Penyelesaian**

### 🔁 **1. Pendekatan Rekursif**

#### 💡 **Ide Dasar:**
Untuk setiap elemen, ada 2 pilihan:
- ✅ Sertakan dalam subset
- ❌ Abaikan

#### 🌳 **Pohon Rekursi:**

```mermaid
graph TD
    A[arr={2,3,1,1}, target=4] --> B[Include 2]
    A --> C[Exclude 2]
    B --> D[Include 3]
    B --> E[Exclude 3]
    C --> F[Include 3]
    C --> G[Exclude 3]
    D --> H[Target=4-2-3=-1<br/>❌ False]
    E --> I[Include 1]
    E --> J[Exclude 1]
    I --> K[Include 1]
    I --> L[Exclude 1]
    K --> M[Target=0<br/>✅ True]
```

#### 📊 **Kompleksitas:**
- ⏱️ **Waktu**: O(2ⁿ)
- 💾 **Ruang**: O(n) untuk call stack

### 🧠 **2. Dynamic Programming**

#### 📌 **A. Memoization (Top-Down)**

```python
📦 Cache hasil submasalah: dp[n][sum]
🔄 Hindari perhitungan ulang
✨ Kombinasi rekursi + caching
```

#### 📌 **B. Tabulation (Bottom-Up)**

**Konsep**: Bangun solusi dari bawah ke atas

```python
dp[i][j] = True jika subset arr[0..i-1] bisa membentuk j
```

**Tabel DP Contoh**: `arr = {1, 2, 3}`, `target = 3`

| i\j | 0 | 1 | 2 | 3 |
|:---:|:-:|:-:|:-:|:-:|
| 0 | ✅ | ❌ | ❌ | ❌ |
| 1 | ✅ | ✅ | ❌ | ❌ |
| 2 | ✅ | ✅ | ✅ | ✅ |
| 3 | ✅ | ✅ | ✅ | ✅ |

#### 🔄 **C. Space Optimization**

Gunakan 2 array 1D: `prev` dan `curr`

```python
Iterasi 1: prev = [True, True, False, False]
Iterasi 2: prev = [True, True, True, True]
Iterasi 3: prev = [True, True, True, True]
```

**Kompleksitas**:
- ⏱️ **Waktu**: O(n × sum)
- 💾 **Ruang**: O(sum) ✨

---

## 💻 **Implementasi Code**

### 🐍 **Python Implementation:**### ⚙️ **C++ Implementation (Snippet):**

```cpp
#include <iostream>
#include <vector>
using namespace std;

// 📊 Tabulation approach
bool subsetSum(vector<int>& arr, int target) {
    int n = arr.size();
    vector<vector<bool>> dp(n + 1, vector<bool>(target + 1, false));
    
    // Base case: sum 0 is always possible
    for (int i = 0; i <= n; i++) {
        dp[i][0] = true;
    }
    
    // Fill the table
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= target; j++) {
            // Exclude current element
            dp[i][j] = dp[i-1][j];
            
            // Include current element if possible
            if (j >= arr[i-1]) {
                dp[i][j] = dp[i][j] || dp[i-1][j - arr[i-1]];
            }
        }
    }
    
    return dp[n][target];
}

// 🎯 Find actual subset
void findSubset(vector<int>& arr, int target) {
    // Implementation of backtracking to find subset
    // ... (similar to Python version)
}
```

---

## 🌍 **Aplikasi di Dunia Nyata**

### 1️⃣ **Kriptografi 🔐**

> Digunakan dalam sistem kriptografi berbasis **knapsack**

- 🔑 **Public Key Cryptography**: Kesulitan SSP menjadi basis keamanan
- 🛡️ **Merkle-Hellman Cryptosystem**: Salah satu aplikasi awal
- ⚠️ **Note**: Sebagian besar telah dipecahkan, tapi konsepnya penting

### 2️⃣ **Alokasi Sumber Daya 📊**

**Contoh Praktis:**
```python
Proyek A: Cost = $40k, ROI = 15%
Proyek B: Cost = $60k, ROI = 20%
Proyek C: Cost = $30k, ROI = 10%
Budget: $100k

Goal: Pilih proyek yang total cost ≤ budget
```

### 3️⃣ **Genetika & Bioinformatika 🧬**

- 🔬 **DNA Fragment Assembly**: Menggabungkan fragmen dengan panjang target
- 🧪 **Gene Expression Analysis**: Kombinasi gen dengan ekspresi tertentu
- 💊 **Drug Discovery**: Kombinasi molekul dengan sifat target

### 4️⃣ **Keuangan & Investasi 💰**

```python
Portfolio Optimization:
- Saham A: $20, Expected Return: 8%
- Saham B: $35, Expected Return: 12%
- Saham C: $15, Expected Return: 6%
Target Investment: $100
```

### 5️⃣ **Gaming & Puzzle 🎮**

- 🎯 **RPG Games**: Kombinasi item untuk mencapai stat tertentu
- 🧩 **Puzzle Games**: Level design dengan target score
- 🎲 **Board Games**: Kombinasi kartu/dice untuk target value

---

## 📊 **Analisis Kompleksitas**

### ⏱️ **Time Complexity Comparison:**

| Approach | Time Complexity | Space Complexity | Use Case |
|:---------|:---------------|:-----------------|:---------|
| 🔁 Recursive | O(2ⁿ) | O(n) | n < 20 |
| 🧠 Memoization | O(n × sum) | O(n × sum) | Medium datasets |
| 📊 Tabulation | O(n × sum) | O(n × sum) | General purpose |
| 💾 Space Optimized | O(n × sum) | O(sum) | Large n, limited memory |

### 📈 **Performance Graph:**

```
Time (ms) ▲
1000 ─┤                           ╱╱ Recursive
     │                       ╱╱╱╱
 100 ─┤                   ╱╱╱╱
     │               ╱╱╱╱
  10 ─┤           ╱╱╱╱────────── DP
     │       ╱╱╱╱─────
   1 ─┤   ╱╱╱╱─────
     │╱╱╱─────
   0 ─┴────┬────┬────┬────┬────▶
         5   10   15   20   25   n
```

### 🔑 **Key Insights:**

1. **Pseudo-polynomial**: O(n × sum) - polynomial dalam input numerik
2. **NP-Complete**: Tidak ada algoritma polynomial dalam ukuran input bit
3. **Trade-off**: Waktu vs memori dapat dioptimasi sesuai kebutuhan

---

## 📝 **Kesimpulan**

### 🎯 **Takeaway Utama:**

> **Subset Sum Problem** adalah masalah fundamental dalam ilmu komputer yang menjadi dasar pemahaman untuk berbagai masalah optimasi dan keputusan.

### 💡 **Poin-Poin Penting:**

1. **🏷️ NP-Complete**: Representatif dari kelas masalah yang sulit
2. **🔄 Multiple Approaches**: Dari brute force hingga DP optimal
3. **🎨 Banyak Variasi**: Setiap variasi punya aplikasi unik
4. **🌍 Real-World Impact**: Dari crypto hingga bioinformatics
5. **⚖️ Trade-offs**: Balance antara waktu dan memori

### 🚀 **Best Practices:**

- ✅ Gunakan **DP** untuk instance umum
- ✅ **Space optimization** untuk array besar
- ✅ Pertimbangkan **approximation** untuk dataset sangat besar
- ✅ **Preprocessing** dapat membantu (sorting, filtering)

### 🔮 **Future Directions:**

1. **Quantum Computing**: Potensi speedup eksponensial
2. **Approximation Algorithms**: FPTAS untuk kasus praktis
3. **Parallel Computing**: Distribusi komputasi DP
4. **Machine Learning**: Prediksi subset tanpa full computation

