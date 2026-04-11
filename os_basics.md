# Hafta 2 — İşletim Sistemi Temelleri (OS Basics)

## İşletim Sistemi (Operating System) Nedir?

Bir kod yazdığımızda bu doğrudan CPU'ya gitmez. Arada bir tercüman ve yönetici vardır — bu işletim sistemidir.

- Uygulamalara ne kadar RAM kullanabileceklerini söyler.
- CPU doluysa uygulamalara bekleme emri verir.

**Katman yapısı:**

```
[ Uygulamalar: Chrome, Python, VS Code ]
              ↕
[ İşletim Sistemi: Windows, Linux, macOS ]
              ↕
[ Donanım: CPU, RAM, Disk ]
```

---

## Kernel

İşletim sisteminin kalbidir. İki taraf arasında köprü görevi görür:

- Uygulamalar ↔ İşletim Sistemi
- İşletim Sistemi ↔ Donanım

**Görevleri:**

- RAM'i uygulamalar arasında paylaştırır.
- CPU'ya sırada kimin olduğunu söyler.
- Disk okuma/yazma işlemlerini yönetir.
- Donanıma erişimi kontrol eder.

> Kernel bir güvenlik görevlisi gibidir. Uygulama donanıma doğrudan erişemez, her şey Kernel üzerinden geçer.

---

## Process (Süreç) ve Thread (İş Parçacığı)

### Process

Bir programın çalışan halidir. Bir `.py` dosyasını çalıştırdığımızda OS bir process oluşturur. Bu process:

- Kendine ait RAM alanına ve kaynaklara sahiptir.
- Diğer process'lerden tamamen izoledir.

Farklı uygulamalar için farklı process'ler oluşturulur.

### Thread

Process'in içindeki küçük çalışma birimleridir. Bir process birden fazla thread içerebilir.

**Analoji:**
- **Process** → Restoran (kendine ait bina ve mutfak)
- **Thread** → Restorandaki garsonlar (aynı mutfağı paylaşır)

| | Process | Thread |
|---|---|---|
| Bellek | Kendine ait | Process ile paylaşır |
| İzolasyon | Tam izole | Aynı belleği paylaşır |
| İletişim | Yavaş | Hızlı ama riskli |

---

## Bellek Yönetimi (Memory Management)

OS her process'e ayrı bir bellek alanı tahsis eder. Buna **Virtual Memory (Sanal Bellek)** denir.

Her process, RAM'in tamamı kendisine aitmiş gibi çalışır — ama bu bir yanılsamadır. OS arka planda fiziksel RAM'i sanal bellek olarak process'lere böler.

**Bu sistem neden var?**

10 uygulama aynı anda RAM'e yazmaya çalışırsa çakışma ve kaos oluşur. Sanal bellek bu sorunu çözer.

**RAM dolduğunda ne olur?**

OS bazı verileri geçici olarak diske (HDD/SSD) taşır. Buna **swap** denir. Disk RAM'e göre çok daha yavaş olduğundan bilgisayar bu durumda belirgin şekilde yavaşlar.

---

## Scheduler (CPU Zamanlayıcısı)

Tek çekirdekli bir CPU aynı anda yalnızca bir şey çalıştırabilir. Buna rağmen tüm uygulamalar aynı anda açık hissini verir — çünkü CPU process'ler arasında çok hızlı geçiş yapar. Buna **context switching** denir.

Bu geçişleri **scheduler** yönetir.

**Temel zamanlama stratejileri:**

- **Round Robin:** Herkese sırayla eşit süre verilir. (Adil ama her zaman verimli değil)
- **Priority Scheduling:** Önceliği yüksek olan önce çalışır. (Sistem görevleri öncelikli)
- **Shortest Job First:** En kısa sürecek iş önce tamamlanır.