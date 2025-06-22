```markdown
---
title: Fractional Knapsack Summary
layout: post
---

## 📦 Definisi  
**Fractional Knapsack** adalah masalah optimisasi untuk memaksimalkan nilai barang dalam tas dengan kapasitas terbatas, di mana barang **boleh diambil sebagian** (fraksional). Berbeda dengan *0/1 Knapsack* yang hanya mengizinkan pengambilan penuh.

---

## 🔍 Karakteristik Utama  
- Setiap barang memiliki *weight* (berat) dan *value* (nilai)  
- Kapasitas tas terbatas  
- Barang dapat dipotong/diambil sebagian (contoh: 0.5 unit)  
- **Dapat diselesaikan dengan algoritma greedy** (tidak perlu cek semua kombinasi)

---

## ⚙️ Strategi Penyelesaian (Algoritma Greedy)  
1. Hitung **rasio nilai/berat** tiap item: `value / weight`  
2. Urutkan item berdasarkan rasio (**descending**)  
3. Ambil item dengan rasio tertinggi secara penuh hingga tas hampir penuh  
4. Jika kapasitas tersisa kurang untuk satu item, **ambil fraksi** dari item tersebut  

```python
# Pseudocode
sorted_items = sort(items, key=value/weight, reverse=True)
for item in sorted_items:
    if capacity >= item.weight:
        take_full(item)
    else:
        take_fraction(item, capacity)
        break
```

---

## ⏱ Kompleksitas Waktu  
- `O(n log n)` untuk *sorting* item  
- `O(n)` untuk proses pengisian tas  
- **Total: O(n log n)**

---

## 💡 Aplikasi Praktis  
1. Penjadwalan tugas dengan sumber daya terbatas  
2. Alokasi investasi ke beberapa proyek  
3. Manajemen bandwidth jaringan  
4. Optimisasi logistik pengiriman  

---

## ⚠️ Catatan Algoritma Greedy  
| **Keunggulan**                          | **Keterbatasan**                     |
|-----------------------------------------|--------------------------------------|
| Sederhana & cepat implementasinya       | Tidak selalu optimal untuk semua tipe masalah |
| Optimal untuk fractional knapsack       | Pilihan lokal bisa menghalangi solusi global |
| Efisien (`O(n log n)`)                  | Kurang efektif untuk masalah kompleks (e.g., 0/1 Knapsack) |

---

## ✅ Kesimpulan  
Fractional Knapsack **terpecahkan secara optimal dengan algoritma greedy** karena:  
1. Sifat fraksional memungkinkan pengambilan parsial  
2. Rasio nilai/berat menjadi indikator optimal  
3. Solusi lokal = solusi global pada kasus ini  
```