# Hafta 5 — Linux Temelleri

## Linux Nedir?

Linux, Windows gibi bir işletim sistemidir. Farkı açık kaynak ve terminal odaklı olmasıdır.

- Docker, cloud (AWS, GCP), CI/CD sistemleri Linux tabanlıdır.
- CV/ML modelleri deploy edildiğinde karşıya Linux çıkar.
- İşlerin büyük çoğunluğu **terminal** üzerinden yapılır.

---

## Dosya Sistemi Yapısı (File System Structure)

Linux'ta her şey bir dosyadır.

```
/                   ← Root (kök dizin, her şeyin başlangıcı)
├── home/emre/      ← Kullanıcı dosyaları (Windows'taki C:\Users gibi)
├── etc/            ← Sistem konfigürasyon dosyaları
├── var/            ← Log dosyaları, geçici veriler
├── usr/            ← Yüklü programlar
└── tmp/            ← Geçici dosyalar (reboot'ta silinir)
```

> Windows'ta `C:\Users\Emre\Documents` varsa, Linux'ta `/home/emre/documents` vardır.

---

## Temel Terminal Komutları

### Navigasyon

```bash
pwd             # Şu an hangi dizinde olduğunu gösterir (Print Working Directory)
ls              # Dizin içeriğini listeler
ls -la          # Gizli dosyalar dahil, detaylı listele
cd Desktop      # Desktop klasörüne gir
cd ..           # Bir üst dizine çık
cd ~            # Home dizinine git
```

### Dosya / Klasör İşlemleri

```bash
mkdir reports           # "reports" klasörü oluşturur
mkdir -p a/b/c          # İç içe klasör oluşturur
touch notes.txt         # Boş dosya oluşturur
cp notes.txt backup.txt # Dosya kopyalar
mv notes.txt docs/      # Dosya taşır (rename için de kullanılır)
rm notes.txt            # Dosyayı siler
rm -rf klasor/          # Klasörü içiyle birlikte siler (dikkatli kullanılmalı!)
```

### İçerik Görüntüleme

```bash
cat notes.txt           # Dosya içeriğini terminale yazdırır
less notes.txt          # Sayfa sayfa görüntüler (q ile çıkış)
head -n 10 notes.txt    # İlk 10 satırı gösterir
tail -n 10 notes.txt    # Son 10 satırı gösterir
```

### `grep` — Arama Komutu

Linux'un en güçlü araçlarından biridir. Dosya içinde veya komut çıktısında metin arama yapar.

```bash
grep "error" log.txt            # "error" geçen satırları bulur
grep -r "import" ./src/         # src/ klasöründe recursive arama yapar
grep -i "error" log.txt         # Büyük/küçük harf duyarsız arama yapar
ps aux | grep python            # Çalışan Python process'lerini bulur
```

> `|` işaretine **pipe** denir. Bir komutun çıktısını diğerine gönderir.

### `top` — Sistem Monitörü

```bash
top
```

Windows'taki Task Manager gibi düşünülebilir. Hangi process'in ne kadar CPU ve RAM kullandığını canlı olarak gösterir. `q` ile çıkılır.

---

## Dosya İzinleri (File Permissions)

Linux'ta her dosyanın izinleri vardır. `ls -la` çıktısında şu görünür:

```
-rwxr-xr-- 1 emre users 4096 Apr 12 main.py
```

Bu `-rwxr-xr--` kısmı sırasıyla:

```
-  rwx  r-x  r--
│   │    │    └── Others (diğer herkes): sadece okuyabilir
│   │    └─────── Group (grup): okuyabilir, çalıştırabilir
│   └──────────── Owner (sahip): okuyabilir, yazabilir, çalıştırabilir
└──────────────── Dosya türü (- = dosya, d = dizin)
```

| Harf | İzin | Anlamı |
|------|------|--------|
| `r` | read | Okuma |
| `w` | write | Yazma |
| `x` | execute | Çalıştırma |
| `-` | — | O izin yok |

### `chmod` — İzin Değiştirme

```bash
chmod +x script.sh      # Çalıştırma izni ekler
chmod 755 script.sh     # rwxr-xr-x
chmod 644 notes.txt     # rw-r--r--
```

**Sayısal sistem:**

| Sayı | İzin |
|------|------|
| 7 | rwx (4+2+1) |
| 6 | rw- (4+2) |
| 5 | r-x (4+1) |
| 4 | r-- (4) |

---

## Paket Yönetimi (Package Management)

Windows'ta program yüklemek için `.exe` indirilir. Linux'ta ise **paket yöneticisi** kullanılır — merkezi bir depodan güvenli kurulum yapar.

### apt (Debian / Ubuntu)

```bash
sudo apt update             # Paket listesini günceller
sudo apt install git        # git kurar
sudo apt remove git         # git kaldırır
sudo apt upgrade            # Yüklü paketleri günceller
```

### pacman (Arch Linux)

```bash
sudo pacman -Syu            # Sistemi günceller
sudo pacman -S git          # git kurar
sudo pacman -R git          # git kaldırır
```

### dnf (Fedora / RHEL)

```bash
sudo dnf install git        # git kurar
sudo dnf remove git         # git kaldırır
sudo dnf update             # Sistemi günceller
```

> `sudo` = **superuser do** — yönetici yetkisiyle çalıştır. Windows'taki "Yönetici olarak çalıştır" gibi.

---

## Servisler — `systemctl`

Linux'ta arka planda çalışan programlara **servis (daemon)** denir. Bunlar `systemctl` ile yönetilir.

```bash
sudo systemctl start nginx      # nginx servisini başlatır
sudo systemctl stop nginx       # nginx servisini durdurur
sudo systemctl restart nginx    # Yeniden başlatır
sudo systemctl status nginx     # Durumu kontrol eder
sudo systemctl enable nginx     # Sistem açılışında otomatik başlatır
sudo systemctl disable nginx    # Otomatik başlatmayı kapatır
```

**Daemon nedir?** Arka planda sessizce çalışan servislerdir. Web sunucusu, veritabanı, SSH sunucusu hepsi birer daemon'dır. Kullanıcı müdahalesi olmadan çalışırlar.