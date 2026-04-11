# Hafta 1 — Bilgisayar Mimarisi (CPU, RAM, Cache)

## CPU (Central Processing Unit)

Her türlü hesaplamanın yapıldığı birimdir. İki ana parçadan oluşur:

- **ALU (Arithmetic Logic Unit):** Matematiksel işlemleri ve karşılaştırma işlemlerini yapar. (`5 + 3`, `5 > 3` gibi)
- **CU (Control Unit):** Orkestra şefi gibi çalışır. "Sıradaki işlem ne?" sorusunu sorar, RAM'i yönlendirir ve veri akışını koordine eder.

---

## RAM (Random Access Memory)

- **Random Access** kavramı şunu ifade eder: Bellekteki herhangi bir konuma sırayla gitmek zorunda kalmadan anında erişilebilir.
- **Disk ile farkı:** Diskte her şey kalıcı olarak saklanır fakat veriye ulaşmak zaman alır. RAM ise işlemci için hazırda bekleyen, anlık erişilebilir veridir.
- Bilgisayar kapatıldığında RAM **sıfırlanır** — elektrikle çalıştığı için geçicidir. Disk ise kalıcıdır.
- Örnek: Python'da bir değişkene değer atandığında (`x = 5`), bu değer RAM'e yazılır.

---

## Cache

CPU bir veriyi istediğinde RAM bu isteğe cevap vermekte yavaş kalır (~100 nanosaniye). Cache, bu gecikmeyi ortadan kaldırmak için CPU'ya çok daha yakın konumlandırılmış, küçük ve hızlı bir bellek katmanıdır.

### Katmanlar

| Katman | Hız | Boyut | Konum |
|--------|-----|-------|-------|
| L1 | En hızlı | 32 KB – 64 KB | CPU çekirdeğinin içi |
| L2 | Hızlı | 256 KB – 1 MB | CPU çekirdeğine yakın |
| L3 | Orta | 8 MB – 32 MB | Tüm çekirdekler paylaşır |

**Hız sıralaması:** `L1 > L2 > L3 > RAM`

### Arama Sırası

CPU bir veriyi istediğinde şu sırayla bakar:

```
L1 → L2 → L3 → RAM
```

### Cache Hit / Cache Miss

- **Cache hit:** İstenen veri cache'te bulundu → Hızlı erişim.
- **Cache miss:** Veri cache'te yok, RAM'e gidilmek zorunda kalındı → Yavaş erişim.