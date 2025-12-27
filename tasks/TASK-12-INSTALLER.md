# TASK-12: Windows Installer

**Durum:** ✅ Tamamlandı  
**Öncelik:** 🔥 Kritik  
**Tahmini Süre:** 2 oturum  
**Bağımlılıklar:** TASK-11 ✅

---

## 🎯 HEDEF

Uygulamayı Windows installer (.exe) olarak paketlemek. Kurulum sonrası otomatik başlatma.

---

## 🔧 TEKNİK KARARLAR

| Karar | Seçim | Neden |
|-------|-------|-------|
| Paketleme | Node.js Embedded | better-sqlite3 uyumluluğu, güvenilirlik |
| Otomatik Başlatma | Windows Startup Kısayolu | node-windows VBScript sorunları nedeniyle değiştirildi |
| Installer | Inno Setup | Ücretsiz, profesyonel, Türkçe dil desteği |

---

## 📋 ALT GÖREVLER

### A. Production Build ✅ TAMAMLANDI

- [x] A.1 Frontend production build (`npm run build`)
- [x] A.2 Backend'in frontend'i serve etmesi
- [x] A.3 Environment variables (production)
- [x] A.4 Tek port'ta çalışma (7474)

### B. Node.js Packaging ✅ TAMAMLANDI

- [x] B.1 Node.js embedded yapısı oluşturma (v22.17.0)
- [x] B.2 SQLite native binding'leri dahil etme (node_modules ile)
- [x] B.3 prepare-build.js scripti oluşturuldu
- [x] B.4 migrations klasörü dahil edildi

### C. Otomatik Başlatma ✅ TAMAMLANDI

- [x] C.1 start-background.js oluşturuldu (detached mode)
- [x] C.2 Windows Startup klasörüne kısayol ekleniyor
- [x] C.3 Başlat menüsünde "Sunucuyu Başlat" kısayolu
- [x] C.4 Kaldırma sırasında process kapatılıyor

### D. Otomatik Yedekleme (Scheduler) ✅ TAMAMLANDI

- [x] D.1 node-cron paketi kuruldu
- [x] D.2 scheduler.js modülü oluşturuldu
- [x] D.3 Günlük 02:00 otomatik yedekleme
- [x] D.4 Günlük 03:00 eski yedek temizliği (7 yedek tutulur)
- [x] D.5 Graceful shutdown handler

### E. Inno Setup Installer ✅ TAMAMLANDI

- [x] E.1 Inno Setup script yazıldı (ceksenet.iss)
- [x] E.2 64-bit kurulum desteği
- [x] E.3 Firewall kuralı otomatik ekleniyor
- [x] E.4 Türkçe dil desteği
- [x] E.5 Masaüstü kısayolu (opsiyonel)
- [x] E.6 Kaldırma işlemi

### F. Test ✅ TAMAMLANDI

- [x] F.1 Windows Sandbox'ta kurulum testi
- [x] F.2 Uygulama başlatma testi
- [x] F.3 Login ve kullanım testi
- [x] F.4 Migration'lar çalışıyor
- [x] F.5 Admin kullanıcı oluşturuluyor

---

## 📁 FİNAL DOSYA YAPISI

```
installer/
├── build/
│   ├── node/                    # Node.js v22.17.0 (~85 MB)
│   │   └── node.exe
│   ├── app/                     # Backend + Frontend
│   │   ├── backend/
│   │   │   ├── src/
│   │   │   ├── config/
│   │   │   ├── database/
│   │   │   │   └── migrations/  # SQL migration dosyaları
│   │   │   ├── logs/
│   │   │   ├── node_modules/
│   │   │   └── .env
│   │   └── frontend/
│   │       └── dist/
│   └── service/
│       └── start-background.js  # Arka plan başlatıcı
├── output/
│   └── CekSenet-Setup-1.0.0.exe # Final installer (~32 MB)
├── ceksenet.iss                 # Inno Setup script
├── prepare-build.js             # Build hazırlık scripti
└── README.md
```

---

## 📦 INSTALLER ÖZELLİKLERİ

| Özellik | Değer |
|---------|-------|
| Dosya Adı | CekSenet-Setup-1.0.0.exe |
| Boyut | ~32 MB |
| Kurulum Dizini | C:\Program Files\CekSenet |
| Port | 7474 |
| Firewall | Otomatik kural eklenir |
| Otomatik Başlatma | Windows Startup kısayolu |
| Masaüstü Kısayolu | Opsiyonel (varsayılan: evet) |
| Türkçe | ✅ |

---

## 🔄 KURULUM AKIŞI

1. Kullanıcı installer'ı çalıştırır
2. Kurulum sihirbazı açılır (Türkçe)
3. Kurulum dizini seçilir (varsayılan: Program Files)
4. Opsiyonlar seçilir (masaüstü kısayolu, otomatik başlatma)
5. Dosyalar kopyalanır
6. Firewall kuralı eklenir
7. Uygulama arka planda başlatılır
8. Tarayıcı açılır (http://localhost:7474)
9. Admin kullanıcı otomatik oluşturulur (admin/123456)

---

## ✅ TAMAMLANMA KRİTERLERİ

- [x] Tek .exe installer oluşuyor (~32 MB)
- [x] Kurulum Next-Next-Finish ile tamamlanıyor
- [x] Uygulama arka planda çalışıyor
- [x] Windows açılışında otomatik başlıyor
- [x] Tarayıcıdan erişilebiliyor
- [x] Migration'lar çalışıyor
- [x] Admin kullanıcı oluşuyor
- [x] Kaldırma çalışıyor

---

## 📝 OTURUM KAYITLARI

### Oturum 20 - 27 Aralık 2025 (Devam)

**Yapılanlar:**

1. **İlk Test - Sorun Tespiti:**
   - Windows Sandbox'ta test edildi
   - VBScript hatası alındı (node-windows)
   - Dosyaların `Program Files (x86)` yerine `Program Files`'a kurulması sağlandı

2. **İkinci Test - Migration Sorunu:**
   - Dosyalar doğru kopyalandı
   - Migration klasörü eksikti → düzeltildi
   - `prepare-build.js` güncellendi

3. **Üçüncü Test - node-windows Sorunu:**
   - VBScript hatası devam etti
   - **Karar:** node-windows yerine Startup kısayolu kullanılacak

4. **Final Çözüm:**
   - `start-background.js` oluşturuldu (detached mode)
   - Windows Startup klasörüne kısayol ekleniyor
   - Inno Setup script yeniden yazıldı
   - **TEST BAŞARILI!** ✅

**Çözülen Sorunlar:**
- ✅ VBScript hatası → Startup kısayolu ile çözüldü
- ✅ Migration eksikliği → prepare-build.js güncellendi
- ✅ 32-bit kurulum → 64-bit ayarı eklendi

---

### Oturum 19 - 27 Aralık 2025

- Frontend Production Build
- Backend Static Serving
- Environment Variables
- Production Test

---

**Oluşturulma:** 26 Aralık 2025  
**Tamamlanma:** 27 Aralık 2025 - Oturum 20
