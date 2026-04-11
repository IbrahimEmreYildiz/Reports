# Hafta 3 — Ağ Temelleri (Networking)

## Ağ (Network) Nedir?

Bilgisayarların birbirleriyle konuşabildiği yapıdır. Network'te her şey veri paketi göndermek ve almaktan ibarettir. Analojik olarak mektuplaşma gibi düşünülebilir.

---

## IP Adresi

Her cihazın internetteki adresidir. Mektuptaki alıcı adresi gibi düşünülebilir.

- **IPv4:** `192.168.1.1` — 4 sayı, nokta ile ayrılmış
- **IPv6:** `2001:0db8:85a3::8a2e:0370:7334` — IPv4 adresleri tükendi, yeni format

**İki tür IP vardır:**

- **Public IP:** İnternete çıkarken ISS (İnternet Servis Sağlayıcısı) tarafından verilen adres. Dış dünya cihazı bu adresle tanır.
- **Private IP:** Ev ve ofis ağı içindeki adrestir. (`192.168.x.x`) Router dışarıya tek bir public IP ile çıkar.

---

## Port

IP adresi doğru binaya götürüyorsa, port doğru daireye götürür. Bir sunucuda aynı anda birden fazla servis çalışabilir:

| Port | Servis |
|------|--------|
| 80 | HTTP (web) |
| 443 | HTTPS (güvenli web) |
| 22 | SSH |
| 5432 | PostgreSQL |

> `192.168.1.1:443` → Bu IP adresinin 443 numaralı portuna bağlan demektir.

---

## DNS (Domain Name System)

İnsanlar için IP adresi ezberlemek zordur. Bu yüzden IP adreslerine isim verilir. DNS bu çeviriyi yapar:

```
google.com → DNS sorgusu → 172.217.169.78
```

Telefon rehberi gibi düşünülebilir: ismi veriyorsun, numara (IP) karşına çıkıyor.

---

## TCP vs UDP

Veri internette iki farklı protokolle iletilir.

### TCP (Transmission Control Protocol)

Güvenilir ama yavaştır. Verinin karşıya tam olarak ulaşıp ulaşmadığını kontrol eder.

**Çalışma mantığı:**
1. Bağlantı kur
2. Veriyi gönder
3. "Aldın mı?" diye sor
4. Almadıysa tekrar gönder

Kullanım alanı: Web siteleri, e-posta, dosya transferi — **kayıp kabul edilemeyen** işlemler.

### UDP (User Datagram Protocol)

Hızlı ama güvenilmezdir. "Gönder ve unut" mantığıyla çalışır. Verinin ulaşıp ulaşmadığını umursamaz.

Kullanım alanı: Video streaming, online oyunlar, video konferans — **hızın önemli, birkaç kaybın kabul edilebilir** olduğu durumlar.

| | TCP | UDP |
|---|---|---|
| Güvenilirlik | ✅ Yüksek | ❌ Düşük |
| Hız | Yavaş | Hızlı |
| Bağlantı | Gerekli | Gereksiz |
| Kullanım | HTTP, SSH, FTP | Video, oyun, DNS |

---

## Paket Yapısı

Veri internette tek parça gitmez; küçük parçalara (paket) bölünür, her biri ayrı yoldan gidebilir ve karşı tarafta yeniden birleştirilir.

```
[ Header (Başlık)          | Data (Veri) ]
  -> Kaynak IP
  -> Hedef IP
  -> Port numarası
  -> Sıra numarası
  -> Hata kontrol bilgisi
```

- **Header** → Zarfın üzerindeki adres bilgisi
- **Data** → Zarfın içindeki mektup

---

## Tanılama Araçları

### `ping`
Hedef sunucunun ayakta olup olmadığını ve paketin gidip geri gelip gelmediğini kontrol eder.

```bash
ping google.com
```

Gönderilen paketin gidip geri dönme süresini gösterir (ms cinsinden). Buna **RTT (Round Trip Time)** denir.

### `traceroute` / `tracert`
Paketin hedefe giderken hangi sunuculardan (hop) geçtiğini gösterir.

```bash
tracert google.com
```

Her satır bir ara noktadır. Gecikmenin nerede oluştuğunu tespit etmek için kullanılır.

### `nslookup`
DNS sorgusunu elle yapar. Bir domain'in IP adresini öğrenmek için kullanılır.

```bash
nslookup google.com
```