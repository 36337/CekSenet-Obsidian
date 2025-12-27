# Deployment & Operasyon Kılavuzu

**Son Güncelleme:** 26 Aralık 2025  
**Amaç:** Kurulum sonrası operasyonel konular

---

## 📦 KURULUM SONRASI

### İlk Çalıştırma

Kurulum tamamlandığında:

1. Uygulama Windows servisi olarak başlar
2. Tarayıcı otomatik açılır: `http://localhost:7474`
3. **İlk Kurulum Sihirbazı** gösterilir:
   - Admin kullanıcı adı belirleme
   - Admin şifresi belirleme
   - Şirket adı (opsiyonel)
4. Kurulum tamamlanınca Dashboard'a yönlendirme

### Diğer Cihazlardan Erişim

**Aynı Ağ (LAN):**
```
http://[ANA-PC-IP]:7474
Örnek: http://192.168.1.100:7474
```

**Dış Ağ (İnternet - Statik IP):**
```
http://[STATIK-IP]:7474
```

**Gerekli Ayarlar:**
1. Windows Firewall'da 7474 portuna izin ver
2. Router'da port forwarding (7474 → Ana PC)
3. Statik IP'yi DNS'e bağlama (opsiyonel)

---

## 💾 YEDEKLEME

### Otomatik Yedekleme

Uygulama günlük otomatik yedek alır:

- **Zaman:** Her gün 02:00
- **Konum:** `[KURULUM]/database/backups/`
- **Format:** `ceksenet_YYYY-MM-DD_auto.db`
- **Saklama:** Son 7 gün (eski yedekler otomatik silinir)

### Manuel Yedekleme

**Yöntem 1: Uygulama İçinden**
1. Ayarlar → Yedekleme
2. "Şimdi Yedekle" butonu
3. Dosya: `ceksenet_YYYY-MM-DD_manual.db`

**Yöntem 2: Dosya Kopyalama**
1. Uygulamayı durdur (servis)
2. `database/ceksenet.db` dosyasını kopyala
3. Uygulamayı başlat

### Geri Yükleme

**Yöntem 1: Uygulama İçinden (Admin)**
1. Ayarlar → Yedekleme
2. Yedek listesinden seç
3. "Geri Yükle" butonu
4. Onay → Uygulama yeniden başlar

**Yöntem 2: Manuel**
1. Uygulamayı durdur
2. `database/ceksenet.db` dosyasını yedekle (güvenlik için)
3. Yedek dosyasını `database/ceksenet.db` olarak kopyala
4. Uygulamayı başlat

---

## 🔄 GÜNCELLEME

### Güncelleme Stratejisi

1. Yeni versiyon çıkınca GitHub Releases'dan indir
2. Mevcut veritabanı **korunur** (güncelleme silmez)
3. Şema değişikliği varsa otomatik migration çalışır

### Güncelleme Adımları

```
1. CekSenet-Setup-v1.1.exe indir
2. Mevcut kurulumun üzerine kur (aynı dizin)
3. Kurulum sırasında:
   - Veritabanı yedeklenir (otomatik)
   - Yeni dosyalar kopyalanır
   - Servis yeniden başlatılır
   - Migration çalışır (gerekirse)
4. Tarayıcıyı yenile
```

### Versiyon Notları

Her güncelleme ile birlikte:
- CHANGELOG.md dosyası
- Yeni özellikler
- Bug fixes
- Breaking changes (varsa)
- Migration notları

---

## 🗄️ VERİTABANI MIGRATION

### Otomatik Migration

Uygulama başlarken:
1. Mevcut DB versiyonu kontrol edilir
2. Yeni migration'lar varsa sırayla çalıştırılır
3. DB versiyonu güncellenir

### Migration Dosyaları

```
database/migrations/
├── 001_initial.sql       # İlk kurulum
├── 002_add_notlar.sql    # v1.1 - notlar alanı eklendi
└── 003_add_index.sql     # v1.2 - performans index'leri
```

### Rollback

Migration başarısız olursa:
1. Otomatik yedekten geri yükleme seçeneği
2. Manuel rollback script'i (advanced)

---

## 📊 LOGGING

### Log Dosyaları

```
logs/
├── app.log           # Genel uygulama logları
├── error.log         # Sadece hatalar
├── access.log        # API erişim logları
└── audit.log         # Kullanıcı aktiviteleri (opsiyonel)
```

### Log Rotasyonu

- Günlük rotasyon
- Son 30 gün saklanır
- Eski loglar sıkıştırılır (.gz)

### Log Seviyeleri

| Seviye | Açıklama | Örnek |
|--------|----------|-------|
| ERROR | Kritik hatalar | DB bağlantı hatası |
| WARN | Uyarılar | Hatalı login denemesi |
| INFO | Bilgi | Kullanıcı giriş yaptı |
| DEBUG | Detay (dev) | API request/response |

### Audit Log (Opsiyonel)

Kullanıcı aktiviteleri:
- Evrak ekleme/düzenleme/silme
- Durum değişiklikleri
- Kullanıcı oluşturma/silme

---

## 👥 MULTI-USER SENARYOLARI

### Aynı Anda Düzenleme

**Senaryo:** İki kullanıcı aynı evrakı düzenliyor.

**Çözüm: Last-Write-Wins + Uyarı**
1. Kayıt açılırken `updated_at` timestamp alınır
2. Kaydetme sırasında DB'deki `updated_at` kontrol edilir
3. Farklıysa: "Bu kayıt başkası tarafından değiştirilmiş" uyarısı
4. Kullanıcı seçer: Değişikliklerimi yaz / Yenile

### Concurrent Updates

```
User A: Evrak #1 aç (updated_at: 10:00)
User B: Evrak #1 aç (updated_at: 10:00)
User A: Kaydet → Başarılı (updated_at: 10:05)
User B: Kaydet → Uyarı: "Kayıt değişmiş, yenilemek ister misiniz?"
```

---

## 🔒 GÜVENLİK

### Dış Erişim Güvenliği

**Zorunlu:**
- Güçlü şifreler (min 8 karakter, harf+rakam)
- Rate limiting (5 hatalı giriş → 15 dk bekleme)
- Session timeout (24 saat)

**Önerilen:**
- Router'da sadece 3000 portunu aç
- VPN kullanımı (Tailscale alternatif olarak)
- Güvenilir IP listesi (opsiyonel)

**HTTPS:**
- Şu an HTTP (localhost güvenli)
- Dış erişim için HTTPS önerilir ama zorunlu değil
- İleride Let's Encrypt ile eklenebilir

### Firewall Kuralları

```
Windows Firewall:
- Inbound: TCP 7474 izin ver
- Sadece belirli IP'ler (opsiyonel)

Router:
- Port forwarding: 7474 → [ANA-PC-IP]:7474
- Sadece gerekli cihazlara izin ver (opsiyonel)
```

---

## 📱 MOBİL ERİŞİM (PWA)

### Progressive Web App

Uygulama PWA olarak çalışır:

**Telefona Ekleme (Chrome):**
1. Siteyi aç
2. Menü → "Ana ekrana ekle"
3. Uygulama ikonu oluşur
4. Tam ekran açılır (browser çerçevesi yok)

**Avantajları:**
- App store gerektirmez
- Otomatik güncelleme
- Offline temel görüntüleme (opsiyonel)

### Push Notification (İleri Versiyon)

Vade uyarıları için:
- Bugün vadesi dolan evraklar
- Yarın vadesi dolacak evraklar

*(v2.0'da eklenebilir)*

---

## 🛠️ SORUN GİDERME

### Uygulama Açılmıyor

1. Windows Services → CekSenet servisini kontrol et
2. Servis durmuşsa başlat
3. Hala çalışmıyorsa: `logs/error.log` kontrol et

### Veritabanı Hatası

1. Yedekten geri yükle
2. Veya: `database/ceksenet.db` dosyasını sil, uygulama yeni DB oluşturur (veriler kaybolur!)

### Port Çakışması

```
Hata: Port 7474 kullanımda
Çözüm: Başka uygulamayı kapat veya config'den port değiştir
```

### Dış Erişim Çalışmıyor

1. Windows Firewall kontrolü
2. Router port forwarding kontrolü
3. Statik IP doğruluğu
4. Ana PC'nin açık olduğundan emin ol

---

## 📋 BAKIM TAKVİMİ

| Görev | Sıklık | Otomatik |
|-------|--------|----------|
| Veritabanı yedekleme | Günlük | ✅ |
| Log temizliği | 30 gün | ✅ |
| Güncelleme kontrolü | Haftalık | ❌ (manuel) |
| Disk alanı kontrolü | Aylık | ❌ (manuel) |

---

**Son Güncelleme:** 26 Aralık 2025
