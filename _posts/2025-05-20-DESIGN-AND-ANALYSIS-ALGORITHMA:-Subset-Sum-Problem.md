### Subset Sum Problem - Analisis dan Implementasi

## Pendahuluan

**Subset Sum Problem (SSP)** adalah masalah klasik dalam ilmu komputer yang termasuk dalam kategori **NP-Complete**. Masalah ini bertujuan untuk menentukan apakah terdapat subset dari sekumpulan bilangan bulat yang jumlahnya sama dengan nilai target tertentu.

### Definisi Formal

> Diberikan sebuah himpunan bilangan bulat dan sebuah nilai target, tentukan apakah ada subset dari himpunan tersebut yang jumlah elemennya sama dengan nilai target.

**Contoh:**
- Set = {1, 2, 5}, Target = 3
- **Jawaban:** Ya, subset {1, 2} menghasilkan jumlah 3
- Set = {3, 34, 4, 12, 5, 2}, Target = 25  
- **Jawaban:** Tidak, tidak ada subset yang jumlahnya 25

## Variasi Masalah

### 1. Bounded Subset Sum Problem
- Setiap elemen hanya dapat digunakan **sekali**
- Cocok untuk kasus barang/sumber daya terbatas

**Contoh:** Set = {3, 34, 4, 12, 5, 2}, Target = 9  
**Solusi:** {4, 5} = 9

### 2. Partition Problem
- Membagi himpunan menjadi dua subset dengan jumlah sama
- Target = Total/2

**Contoh:** Set = {1, 5, 11, 5}, Total = 22 → Target = 11  
**Solusi:** {11} dan {1, 5, 5}

### 3. Exact k Elements Subset Sum
- Subset harus terdiri dari **tepat k elemen**

**Contoh:** Set = {1, 2, 3, 4, 5}, Target = 9, k = 2  
**Solusi:** {4, 5} (2 elemen, jumlah 9)

### 4. Unbounded Subset Sum Problem
- Setiap elemen boleh digunakan **berulang kali**

**Contoh:** Set = {1, 3, 4}, Target = 6  
**Solusi:** {3, 3} atau {1, 1, 4}

### 5. Multi-target Subset Sum Problem
- Memenuhi lebih dari satu kriteria secara bersamaan

### 6. Approximate Subset Sum
- Mencari subset terbaik yang **mendekati** target

## Pendekatan Solusi

### 1. Pendekatan Rekursif

```cpp
bool isSubsetSum(int arr[], int n, int sum) {
    // Base cases
    if (sum == 0) return true;
    if (n == 0 && sum != 0) return false;
    
    // Jika elemen terakhir > sum, abaikan
    if (arr[n-1] > sum)
        return isSubsetSum(arr, n-1, sum);
    
    // Cek: sertakan ATAU abaikan elemen terakhir
    return isSubsetSum(arr, n-1, sum) || 
           isSubsetSum(arr, n-1, sum - arr[n-1]);
}
```

**Kompleksitas:**
- Waktu: O(2^n)
- Ruang: O(n) - stack rekursi

> ⚠️ **Tidak efisien** untuk array besar

### 2. Dynamic Programming

#### A. Memoization (Top-Down)

```cpp
int dp[MAX_N][MAX_SUM];

bool isSubsetSumMemo(int arr[], int n, int sum) {
    if (sum == 0) return true;
    if (n == 0) return false;
    
    if (dp[n][sum] != -1) return dp[n][sum];
    
    if (arr[n-1] > sum)
        return dp[n][sum] = isSubsetSumMemo(arr, n-1, sum);
    
    return dp[n][sum] = isSubsetSumMemo(arr, n-1, sum) || 
                        isSubsetSumMemo(arr, n-1, sum - arr[n-1]);
}
```

#### B. Tabulation (Bottom-Up)

```cpp
bool isSubsetSumDP(int arr[], int n, int sum) {
    bool dp[n+1][sum+1];
    
    // Inisialisasi
    for (int i = 0; i <= n; i++)
        dp[i][0] = true;  // subset kosong = 0
    
    for (int j = 1; j <= sum; j++)
        dp[0][j] = false; // tidak ada elemen
    
    // Isi tabel DP
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= sum; j++) {
            if (j < arr[i-1])
                dp[i][j] = dp[i-1][j];
            else
                dp[i][j] = dp[i-1][j] || dp[i-1][j - arr[i-1]];
        }
    }
    
    return dp[n][sum];
}
```

**Kompleksitas DP:**
- Waktu: O(n × sum)
- Ruang: O(n × sum)

### 3. Optimasi Ruang

Menggunakan hanya **dua array 1D** (prev dan curr):

```cpp
bool isSubsetSumOptimized(int arr[], int n, int sum) {
    vector<bool> prev(sum + 1, false);
    vector<bool> curr(sum + 1, false);
    
    prev[0] = true; // subset kosong
    
    for (int i = 1; i <= n; i++) {
        for (int j = 0; j <= sum; j++) {
            if (j < arr[i-1])
                curr[j] = prev[j];
            else
                curr[j] = prev[j] || prev[j - arr[i-1]];
        }
        prev = curr;
    }
    
    return prev[sum];
}
```

**Kompleksitas Optimasi:**
- Waktu: O(n × sum)
- Ruang: **O(sum)** ✨

## Contoh Trace Execution

Untuk arr = {1, 2, 3}, sum = 3:

| i\j | 0 | 1 | 2 | 3 |
|-----|---|---|---|---|
| 0   | T | F | F | F |
| 1   | T | T | F | F |
| 2   | T | T | T | F |
| 3   | T | T | T | **T** |

**Hasil:** dp[3][3] = True → Ada subset {1, 2} dengan jumlah 3

## Aplikasi di Dunia Nyata

### 🔐 Kriptografi
- Sistem kriptografi berbasis ransel (knapsack)
- Keamanan kunci publik

### 💰 Keuangan & Investasi
- Menentukan kombinasi aset untuk target investasi
- Portfolio optimization

### 🧬 Bioinformatika
- Analisis kombinasi gen/fragmen DNA
- Ekspresi atau panjang tertentu

### 📊 Alokasi Sumber Daya
- Pemilihan subset proyek sesuai anggaran
- Manajemen sumber daya

### 🎮 Gaming & Puzzle
- Permainan pencarian kombinasi angka
- Teka-teki matematika

## Perbandingan Pendekatan

| Pendekatan | Kompleksitas Waktu | Kompleksitas Ruang | Kelebihan |
|------------|-------------------|-------------------|-----------|
| Rekursif | O(2^n) | O(n) | Sederhana, mudah dipahami |
| Memoization | O(n × sum) | O(n × sum) | Menghindari perhitungan ulang |
| Tabulation | O(n × sum) | O(n × sum) | Iteratif, tidak perlu stack |
| Space Optimized | O(n × sum) | **O(sum)** | Hemat memori |

## Kesimpulan

**Subset Sum Problem** adalah masalah fundamental dalam computer science dengan karakteristik:

- **Kategori:** NP-Complete decision problem
- **Tujuan:** Menentukan keberadaan subset dengan jumlah target
- **Variasi:** Bounded, unbounded, partition, exact-k, multi-target, approximate
- **Solusi Optimal:** Dynamic Programming dengan space optimization
- **Aplikasi Luas:** Kriptografi, keuangan, bioinformatika, resource allocation

> 💡 **Key Insight:** Meskipun tergolong NP-Complete, SSP dapat diselesaikan efisien menggunakan Dynamic Programming untuk input berukuran sedang, menjadikannya relevan untuk aplikasi praktis di berbagai domain.

---