# Hafta 4 — Dosya Sistemleri ve Depolama Mantığı

## Dosya Sistemi (File System) Nedir?

Hangi dosyanın nerede depolandığını, ne kadar yer kapladığını ve kime ait olduğunu takip eden sistemdir.

> OS bir dosyayı okumak istediğinde "şu dosya nerede?" diye sorar. Dosya sistemi de "136. bloktan başlıyor, 12 blok kaplıyor" gibi bir cevap verir.

---

## Dosya Sistemi Türleri

### NTFS (New Technology File System)

Windows'un dosya sistemidir.

- Büyük dosyaları destekler. (Teorik limit: 16 TB)
- **Journaling** özelliği var: Yazma işlemi yarıda kesilirse sistem bozulmaz, geri alınabilir.
- Dosya izinleri ve şifreleme destekler.
- Linux'ta okuyabilir ama yazmak için ek araç gerekir.

### ext4 (Fourth Extended Filesystem)

Linux'un standart dosya sistemidir.

- Journaling destekler.
- Büyük disk ve dosya boyutlarını destekler.
- Hızlı ve kararlıdır.
- Windows'ta doğrudan okunamaz.

### APFS (Apple File System)

macOS ve iOS'un dosya sistemidir. 2017'de HFS+'ın yerini aldı.

- SSD'ye özel tasarlanmıştır.
- **Snapshot** özelliği var: Diskin belirli bir anını kaydedip geri dönebilirsin.
- Şifreleme varsayılan olarak açıktır.
- Clone işlemleri çok hızlıdır. (Aynı dosyayı kopyalamak neredeyse anında olur.)

| | NTFS | ext4 | APFS |
|---|---|---|---|
| İşletim Sistemi | Windows | Linux | macOS / iOS |
| Journaling | ✅ | ✅ | ✅ |
| Snapshot | ❌ | ❌ | ✅ |
| SSD Optimizasyonu | Kısmi | Kısmi | ✅ Tam |

---

## Blok Yapısı

Disk veriyi byte byte değil, **blok** adı verilen sabit boyutlu parçalar halinde saklar. Tipik blok boyutu **4 KB**'tır.

**Örnekler:**

- 1 KB'lık dosya bile 4 KB'lık bir blok kaplar → **internal fragmentation (iç parçalanma)**
- 10 KB'lık dosya → 3 blok kaplar `(4 + 4 + 4)`, son blokta 2 KB boşa gider

---

## Fragmentation (Parçalanma)

Dosyalar silinip yeni dosyalar yazıldıkça disk üzerindeki bloklar dağınık hale gelir. Bir dosyanın blokları diskte art arda değil, birbirinden uzak yerlerde olabilir.

- **HDD'de:** Okuma kafası fiziksel olarak hareket etmek zorunda → belirgin yavaşlama
- **SSD'de:** Elektronik erişim olduğu için fragmentation çok daha az sorun yaratır

Bu yüzden HDD'lerde **defragmentation (birleştirme)** yapılır, SSD'lerde yapılmaz.

---

## HDD (Hard Disk Drive)

Mekanik bir cihazdır. İçinde dönen diskler (**platter**) ve üzerinde hareket eden bir okuma kafası bulunur.

- Okuma kafasının doğru konuma gelmesi zaman alır → **seek time**
- Disk ne kadar hızlı dönerse okuma o kadar hızlı → RPM (standart: 7200 RPM)
- Darbeye karşı hassastır, mekanik parça içerir.
- GB başına daha ucuzdur.

---

## SSD (Solid State Drive)

Elektronik bir cihazdır. İçinde hareketli parça yoktur, flash bellek hücreleri kullanır.

- Fiziksel hareket olmadığı için erişim anında gerçekleşir.
- Darbeye karşı dayanıklıdır.
- GB başına daha pahalıdır ama hız farkı çok büyüktür.

**SSD neden bu kadar hızlı?**

Elektrik sinyaliyle çalışır, fiziksel hareket yoktur. HDD'de okuma kafasının doğru yere gitmesini beklemek gerekir. SSD'de ise "git bak" dersin, anında gelir.

| | HDD | SSD |
|---|---|---|
| Hız | 100 – 150 MB/s | 500 – 7000 MB/s |
| Seek Time | ~10 ms | ~0.1 ms |
| Dayanıklılık | Düşük (mekanik) | Yüksek |
| Fiyat (GB başına) | Ucuz | Pahalı |
| Fragmentation | Kritik | Önemsiz |
| Gürültü | Var | Yok |