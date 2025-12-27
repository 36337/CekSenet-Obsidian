# Özellikler (Features)

**Son Güncelleme:** 26 Aralık 2025

---

## 📊 DASHBOARD

### Özet Kartları
- **Portföy Toplamı:** Portföydeki evrakların toplam tutarı
- **Tahsil Edilen:** Tahsil edilmiş evrakların toplamı
- **Bu Hafta Vadesi Gelen:** Sayı + tutar
- **Gecikmiş Evraklar:** Vadesi geçmiş, tahsil edilmemiş (kırmızı uyarı)

### Vade Uyarıları
- 🔴 **Gecikmiş:** Vadesi geçmiş, henüz tahsil edilmemiş
- 🟠 **Bugün:** Bugün vadesi dolan evraklar
- 🟡 **Bu Hafta:** 7 gün içinde vadesi dolacak
- 🟢 **Önümüzdeki Ay:** 30 gün içinde vadesi dolacak

### Grafikler
- **Aylık Vade Dağılımı:** Bar chart (son 6 ay)
- **Durum Dağılımı:** Pie chart (portföy, bankada, ciro, tahsil, karşılıksız)

### Son Hareketler
- Son eklenen/düzenlenen 10 evrak
- Tarih, evrak no, tutar, durum

---

## 📄 EVRAK YÖNETİMİ

### Evrak Listeleme

**Tablo Kolonları:**
| Kolon | Açıklama | Sıralanabilir | Filtrelenebilir |
|-------|----------|---------------|-----------------|
| Durum | Badge (renk kodlu) | ✅ | ✅ |
| Evrak Tipi | Çek / Senet | ✅ | ✅ |
| Evrak No | - | ✅ | ✅ (arama) |
| Tutar | Formatlanmış (₺) | ✅ | ✅ (aralık) |
| Vade Tarihi | dd.MM.yyyy | ✅ | ✅ (aralık) |
| Keşideci | - | ✅ | ✅ (arama) |
| Banka | - | ✅ | ✅ |
| İşlem | Düzenle, Detay | - | - |

**Filtreler:**
- Durum (çoklu seçim)
- Evrak tipi (çek/senet/tümü)
- Tarih aralığı (vade tarihi)
- Tutar aralığı (min-max)
- Arama (evrak no, keşideci)

**Sayfalama:**
- Sayfa başına: 10, 25, 50, 100
- Sayfa navigasyonu

### Evrak Ekleme

**Form Alanları:**
| Alan | Tip | Zorunlu | Açıklama |
|------|-----|---------|----------|
| Evrak Tipi | Select | ✅ | Çek / Senet |
| Evrak No | Text | ✅ | Benzersiz numara |
| Tutar | Number | ✅ | Decimal (2 basamak) |
| Vade Tarihi | Date | ✅ | - |
| Banka Adı | Text | - | Çekler için |
| Keşideci | Text | ✅ | Veren kişi/firma |
| Cari Hesap | Select | - | Cariler listesinden |
| Durum | Select | ✅ | Varsayılan: Portföy |
| Notlar | Textarea | - | Serbest metin |

### Evrak Düzenleme

- Tüm alanlar düzenlenebilir
- Durum değişikliği ayrı bir akış (onay gerektirir)
- Değişiklik geçmişi tutulur

### Evrak Detay

- Tüm bilgiler görüntüleme
- Hareket geçmişi (durum değişiklikleri)
- Hızlı durum güncelleme butonları

### Durum Değişikliği

**Akış:**
```
Portföy ──┬── Bankada ──┬── Tahsil
          │             │
          │             └── Karşılıksız
          │
          └── Ciro ────── (Çıkış)
```

**Durum Değişikliğinde:**
1. Onay modalı göster
2. Değişiklik kaydedilir
3. Hareket geçmişine eklenir
4. Dashboard güncellenir

### Toplu İşlemler

- Çoklu seçim (checkbox)
- Toplu durum güncelleme
- Toplu silme (admin only)

---

## 👥 CARİ YÖNETİMİ

### Cari Listeleme

**Tablo Kolonları:**
| Kolon | Açıklama |
|-------|----------|
| Ad Soyad / Firma | - |
| Tip | Müşteri / Tedarikçi (badge) |
| Telefon | - |
| Toplam Evrak | Bu cariye ait evrak sayısı |
| Toplam Tutar | Bu cariye ait evrak toplamı |
| İşlem | Düzenle, Detay |

**Filtreler:**
- Tip (müşteri/tedarikçi/tümü)
- Arama (ad, telefon)

### Cari Ekleme/Düzenleme

**Form Alanları:**
| Alan | Tip | Zorunlu |
|------|-----|---------|
| Ad Soyad / Firma Adı | Text | ✅ |
| Tip | Select | ✅ |
| Telefon | Text | - |
| E-posta | Email | - |
| Adres | Textarea | - |
| Vergi No | Text | - |
| Notlar | Textarea | - |

### Cari Detay

- Cari bilgileri
- Bu cariye ait evraklar listesi
- Toplam borç/alacak özeti

---

## 📈 RAPORLAR

### Tarih Aralığı Raporu

**Filtreler:**
- Başlangıç tarihi
- Bitiş tarihi
- Durum (opsiyonel)
- Evrak tipi (opsiyonel)

**Çıktı:**
- Özet istatistikler (toplam, ortalama, adet)
- Detaylı liste
- Excel export butonu

### Excel Export

**İçerik:**
| Kolon | Format |
|-------|--------|
| Durum | Metin |
| Evrak Tipi | Metin |
| Evrak No | Metin |
| Tutar | Sayı (2 decimal) |
| Vade Tarihi | dd.MM.yyyy |
| Banka | Metin |
| Keşideci | Metin |
| Kayıt Tarihi | dd.MM.yyyy |

**Dosya Adı:** `CekSenet_Rapor_2025-12-26.xlsx`

### Vade Raporu

- Önümüzdeki 7/14/30 gün vadesi dolacak evraklar
- Tutar bazlı sıralama
- Takvim görünümü (opsiyonel)

---

## 👤 KULLANICI YÖNETİMİ (Admin)

### Kullanıcı Listesi

| Kolon | Açıklama |
|-------|----------|
| Kullanıcı Adı | - |
| Ad Soyad | - |
| Rol | Admin / Normal (badge) |
| Son Giriş | Tarih/saat |
| İşlem | Düzenle, Sil |

### Kullanıcı Ekleme

| Alan | Tip | Zorunlu |
|------|-----|---------|
| Kullanıcı Adı | Text | ✅ |
| Ad Soyad | Text | ✅ |
| Şifre | Password | ✅ |
| Şifre Tekrar | Password | ✅ |
| Rol | Select | ✅ |

### Şifre Değiştirme

- Kendi şifresini herkes değiştirebilir
- Admin başkalarının şifresini sıfırlayabilir

---

## 🔔 BİLDİRİMLER

### Vade Uyarıları (Dashboard'da)

- Bugün vadesi dolan evraklar (sarı banner)
- Gecikmiş evraklar (kırmızı banner)

### Toast Mesajları

- Başarılı işlemler (yeşil)
- Hata mesajları (kırmızı)
- Uyarılar (sarı)

---

## 🔍 ARAMA

### Global Arama (Header)

- Evrak no ile arama
- Keşideci ile arama
- Cari ismi ile arama
- Sonuçlar dropdown'da gösterilir

---

## 📱 MOBİL UYUMLULUK

### Responsive Tasarım

- Sidebar → Bottom navigation (mobil)
- Tablo → Card görünümü (mobil)
- Touch-friendly butonlar
- Swipe actions (opsiyonel)

### Mobil Öncelikli Sayfalar

1. Dashboard (hızlı bakış)
2. Evrak listesi (filtreleme)
3. Evrak ekleme (hızlı giriş)

---

## 🔒 GÜVENLİK ÖZELLİKLERİ

- Oturum zaman aşımı (24 saat)
- Şifre gücü kontrolü
- Hatalı giriş denemesi sınırı (5 deneme → 15 dk bekleme)
- Aktivite logu (kim ne zaman ne yaptı) - opsiyonel

---

## 💾 YEDEKLEME

### Otomatik Yedekleme

- Günlük otomatik yedek (7 gün sakla)
- Manuel yedek alma butonu (Ayarlar)
- Yedekten geri yükleme (Admin)

### Yedek Formatı

```
backups/
├── ceksenet_2025-12-26_auto.db
├── ceksenet_2025-12-25_auto.db
└── ceksenet_2025-12-24_manual.db
```

---

## 🚀 İLK KURULUM SİHİRBAZI

İlk çalıştırmada gösterilir:

1. **Hoş Geldiniz** - Uygulama tanıtımı
2. **Admin Hesabı Oluştur**
   - Kullanıcı adı
   - Şifre (min 8 karakter)
   - Şifre tekrar
   - Ad Soyad
3. **Şirket Bilgileri** (opsiyonel)
   - Şirket adı
4. **Tamamlandı** - Dashboard'a yönlendirme

**Tekrar Gösterilmez:** Kurulum tamamlandıktan sonra bir daha gösterilmez.

---

## 📱 PWA (Progressive Web App)

### Özellikler

- Telefona "Ana ekrana ekle" ile kurulabilir
- Tam ekran açılır (browser çerçevesi yok)
- Uygulama ikonu
- Splash screen

### Manifest

```json
{
  "name": "ÇekSenet Takip",
  "short_name": "ÇekSenet",
  "start_url": "/",
  "display": "standalone",
  "theme_color": "#4F46E5",
  "background_color": "#ffffff"
}
```

### Offline Desteği

- Temel uygulama kabuğu cache'lenir
- Veriler için internet gerekli
- Offline modda "Bağlantı yok" mesajı

---

## 📊 LOGGING & AUDIT

### Sistem Logları

- API hataları
- Veritabanı hataları
- Authentication olayları
- Performans metrikleri

### Audit Trail (Opsiyonel - v2.0)

Kim ne zaman ne yaptı:
- Evrak ekleme/düzenleme/silme
- Durum değişiklikleri
- Kullanıcı işlemleri

---

## 🔄 EŞZAMANLI DÜZENLEME

### Senaryo

İki kullanıcı aynı evrakı açıp düzenlemeye çalıştığında:

### Çözüm: Uyarı Sistemi

1. Kayıt açılırken `updated_at` timestamp alınır
2. Kaydetme sırasında kontrol yapılır
3. Farklıysa uyarı gösterilir:
   
   ```
   ⚠️ Bu kayıt başka bir kullanıcı tarafından değiştirilmiş.
   
   [Değişikliklerimi Yaz] [Sayfayı Yenile]
   ```

4. Kullanıcı seçim yapar

---

**Son Güncelleme:** 26 Aralık 2025
