# Hafta 6 — Git & Versiyon Kontrolü

## Git Nedir?

Kod yazarken "Dün çalışıyordu, bugün bozdum, geri dönemiyorum" gibi sorunları çözen sistemdir. Kodun her değişikliğini kaydeden bir **zaman makinesidir**.

Ek avantajları:

- Ekip ile aynı kodda çalışılabilir.
- Farklı özellikler paralel şekilde geliştirilebilir.
- Kim ne zaman neyi değiştirdi görülür.

---

## Snapshot Mantığı

Git dosyaları değişiklik olarak değil, **anlık görüntü (snapshot)** olarak saklar. Her `commit` yapıldığında Git "şu an projenin hali böyle" diyerek tüm dosyaların o anki halini kaydeder.

```
Commit 1 → Commit 2 → Commit 3 → Commit 4
(ilk hal)  (login     (bug        (tasarım
            eklendi)   düzeltme)   güncelleme)
```

İstediğin herhangi bir commit'e geri dönülebilir.

---

## Temel Kavramlar

### Repository (Repo)

Projenin tüm dosyaları ve Git geçmişinin saklandığı klasördür.

```bash
git init            # Mevcut klasörü repo'ya çevirir
git clone <url>     # Uzaktaki repo'yu bilgisayara indirir
```

### Staging Area (İndeks)

Commit'lemeden önce değişikliklerin bekletildiği hazırlık alanıdır.

```
[ Çalışma Dizini ] → git add → [ Staging Area ] → git commit → [ Repo ]
```

Bu ara adım, çok sayıda değiştirilmiş dosya arasından yalnızca seçilenlerin commit'lenmesini sağlar.

```bash
git status                          # Hangi dosyalar değişti?
git add main.py                     # Tek dosyayı staging'e ekler
git add .                           # Tüm değişiklikleri staging'e ekler
git commit -m "feat: login eklendi" # Snapshot alır
git log --oneline                   # Commit geçmişini gösterir
```

### Branch (Dal)

Ana koddan bağımsız bir kopya oluşturmayı sağlar. Yeni özellik geliştirirken ana kodu bozmadan denemek için kullanılır.

```
main:    A → B → C
                  \
feature:           D → E → F
```

`feature` branch'inde çalışırken `main` etkilenmez.

```bash
git branch                      # Mevcut branch'leri listeler
git branch feature/login        # Yeni branch oluşturur
git checkout feature/login      # O branch'e geçer
git checkout -b feature/login   # Oluşturup direkt geçer (kısayol)
```

---

## Merge ve Rebase

İki branch'i birleştirmenin iki yolu vardır.

### Merge

İki branch'i birleştirip yeni bir "birleştirme commit'i" oluşturur. Geçmiş olduğu gibi korunur.

```
main:    A → B → C ──────────── M
                  \            /
feature:           D → E → F
```

```bash
git checkout main
git merge feature/login
```

Takım projelerinde, geçmişin korunması gerektiğinde tercih edilir.

### Rebase

Feature branch'indeki commit'leri alıp main'in en üstüne yeniden yazar. Geçmiş düz ve temiz görünür.

```
main:    A → B → C
                  \
feature:           D' → E' → F'  (yeniden yazıldı)
```

```bash
git checkout feature/login
git rebase main
```

Kişisel branch'lerde, temiz geçmiş istendiğinde tercih edilir.

| | Merge | Rebase |
|---|---|---|
| Geçmiş | Olduğu gibi korunur | Yeniden yazılır |
| Görünüm | Dallanma belli olur | Düz çizgi |
| Güvenlik | Güvenli | Paylaşılan branch'te tehlikeli |

> ⚠️ **Altın Kural:** Başkasının da kullandığı branch'te asla `rebase` yapma.

---

## Uzak Repo ile Çalışmak

```bash
git remote add origin <url>     # Uzak repo ile bağlar
git push origin main            # Yerel değişiklikleri gönderir
git pull origin main            # Uzaktaki değişiklikleri alır
git fetch                       # Değişiklikleri alır ama merge etmez
```

- **push** → Yerelden uzağa gönder
- **pull** → Uzaktan yerele al (`fetch` + `merge`)

---

## Pull Request (PR) İncelemesi

Bir branch'teki değişiklikleri ana branch'e almadan önce incelemeye açmaktır. GitHub / GitLab üzerinde yapılır.

### PR Nasıl İncelenir?

1. **Files changed** sekmesine bak — ne değişmiş?
2. Satır satır değişiklikleri incele.
3. Yorum ekle — "Bu fonksiyon neden böyle yazıldı?"
4. `Approve` (onayla) veya `Request changes` (değişiklik iste)

### İyi Bir PR Nasıl Olur?

- Küçük ve odaklı olmalı. (Tek özellik veya tek bug fix)
- Açıklayıcı başlık: `feat: kullanıcı girişi eklendi`
- Ne yaptığını açıklayan description içermeli.
- Test edilmiş olmalı.

---

## Conventional Commits Formatı

```
feat:      Yeni özellik
fix:       Hata düzeltme
docs:      Dokümantasyon
refactor:  Kod yeniden düzenleme
chore:     Bağımlılık güncelleme
```