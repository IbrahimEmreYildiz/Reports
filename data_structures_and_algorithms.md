# Hafta 7 — Algoritma ve Veri Yapıları Mantığı

## Veri Yapıları Neden Var?

1000 kişilik bir listede adımızı nasıl buluruz?

- **Yöntem 1:** Baştan sona tek tek bak → 1000 adım sürebilir.
- **Yöntem 2:** Liste alfabetik sıralıysa ortadan başla, yarıyı ele → ~10 adım sürebilir.

Aynı problem, farklı yaklaşım, **100 kat fark.** Doğru veri yapısını seçmek, kodun hızını dramatik şekilde değiştirir.

---

## Big-O Notasyonu

"Bu algoritma ne kadar hızlı?" sorusunun cevabını verir. Dikkat: Big-O süreyi değil, **girdi arttıkça işlem sayısının nasıl büyüdüğünü** ölçer.

| Notasyon | İsim | Örnek |
|---|---|---|
| O(1) | Sabit — girdi artsa da hız sabit kalır | Dizinin ilk elemanına eriş |
| O(log n) | Logaritmik | Binary Search |
| O(n) | Doğrusal | Listede sırayla ara |
| O(n²) | Karesel | İç içe iki döngü |
| O(2ⁿ) | Üstel | Tüm kombinasyonları dene |

**Analojiler:**

- **O(1)** → Kitabın sayfa numarasını biliyorsun, direkt aç.
- **O(log n)** → Sözlükte kelime ara, her seferinde yarıya böl.
- **O(n)** → Kitabı baştan sona oku.
- **O(n²)** → Her sayfayı diğer her sayfayla karşılaştır.

---

## Array (Dizi)

Elemanlar bellekte **yan yana** durur. Index ile anında erişilir.

```
Index:  0    1    2    3    4
Değer: [10] [20] [30] [40] [50]
```

| İşlem | Big-O | Neden? |
|---|---|---|
| Erişim (`arr[2]`) | O(1) | Index ile direkt git |
| Arama | O(n) | Baştan sona bak |
| Ekleme (sona) | O(1) | Sona ekle |
| Ekleme (ortaya) | O(n) | Sağdakileri kaydır |
| Silme (ortadan) | O(n) | Sağdakileri kaydır |

> **Not:** Python'daki `list` bir **dynamic array**'dir. Doluluk sınırına ulaşınca daha büyük bir alan açar, tüm elemanları kopyalar. Bu yüzden `append()` genellikle O(1) ama zaman zaman O(n) olabilir — buna **amortized O(1)** denir.

---

## Linked List (Bağlı Liste)

Her eleman (**node**) hem veriyi hem de bir sonraki elemanın adresini tutar. Bellekte yan yana olmak zorunda değildir.

```
[10 | →] → [20 | →] → [30 | →] → [40 | null]
```

| İşlem | Big-O | Neden? |
|---|---|---|
| Erişim | O(n) | Baştan git, say |
| Ekleme (başa) | O(1) | Pointer değiştir |
| Ekleme (ortaya) | O(n) | Önce o noktaya git |
| Silme | O(n) | Önce bul, sonra sil |

**Array vs Linked List:**

| | Array | Linked List |
|---|---|---|
| Erişim | O(1) ✅ | O(n) ❌ |
| Ekleme / Silme (başa) | O(n) ❌ | O(1) ✅ |
| Bellek düzeni | Yan yana | Dağınık |

---

## Stack (Yığın)

**LIFO** — Last In, First Out. Son giren ilk çıkar.

```
      ┌──────┐
      │  30  │  ← push (ekle) / pop (çıkar)
      ├──────┤
      │  20  │
      ├──────┤
      │  10  │
      └──────┘
```

> Analoji: Tabak yığını. En üstteki tabağı alırsın, altındakine ulaşmak için önce üsttekini kaldırman gerekir.

**Kullanım alanları:**
- Fonksiyon çağrıları (call stack)
- Geri al / İleri al (undo / redo)
- Parantez eşleştirme

---

## Queue (Kuyruk)

**FIFO** — First In, First Out. İlk giren ilk çıkar.

```
← çıkış                        giriş →
[10]  [20]  [30]  [40]  [50]
```

> Analoji: Banka sırası. İlk gelen ilk işlem görür.

**Kullanım alanları:**
- İş kuyruğu (task queue)
- Yazıcı baskı sırası
- BFS (graph traversal)

---

## HashMap (Sözlük / Dictionary)

**Key → Value** eşleştirmesi yapar. Python'daki `dict` tam olarak budur.

```python
{"emre": 25, "kaan": 24, "mehmet": 23}
```

Arka planda **hash fonksiyonu** çalışır: key'i alır, bir index'e çevirir, oraya yazar.

| İşlem | Big-O | Neden? |
|---|---|---|
| Erişim (`d["emre"]`) | O(1) | Hash ile direkt git |
| Ekleme | O(1) | Hash hesapla, yaz |
| Silme | O(1) | Hash hesapla, sil |
| Arama | O(1) | Key hash'e çevrilir |

> **Önemli Not:** İki farklı key aynı hash değerine denk gelirse buna **hash collision** denir. Python bunu **chaining** yöntemiyle çözer — çakışan key'ler aynı index'te bir linked list ile zincirlenir. Çok fazla collision olursa O(1) garantisi bozulup O(n)'e dönebilir. Bu yüzden Python, doluluk oranı belirli bir eşiği geçince `dict`i otomatik olarak **resize** eder.

**Ne zaman kullanılır?** Hızlı arama, key-value eşleştirmesi gereken her durumda. CV projelerinde label mapping, config tutma ve sayaç gibi işlemlerde sıkça karşılaşılır.

---

## Hangi Yapıyı Ne Zaman Kullanmalı?

| İhtiyaç | Kullan |
|---|---|
| Index ile hızlı erişim | Array |
| Sık ekleme / silme | Linked List |
| Son gireni ilk al | Stack |
| İlk gireni ilk al | Queue |
| Key ile hızlı arama | HashMap |