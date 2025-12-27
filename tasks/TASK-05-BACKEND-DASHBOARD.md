# TASK-05: Backend - Dashboard & Raporlar API

**Durum:** ✅ Tamamlandı  
**Öncelik:** ⚡ Yüksek  
**Tahmini Süre:** 1 oturum  
**Bağımlılıklar:** TASK-04

---

## 🎯 HEDEF

Dashboard istatistikleri ve rapor API'lerini oluşturmak. Excel export.

---

## 📋 ALT GÖREVLER

### A. Dashboard API

- [x] A.1 `GET /api/dashboard` - Özet istatistikler
  - Portföy toplamı (adet + tutar)
  - Tahsil edilen (adet + tutar)
  - Bu hafta vadesi gelen (adet + tutar)
  - Gecikmiş evraklar (adet + tutar)
  - Durum dağılımı (pie chart için)
  - Aylık vade dağılımı (bar chart için)

- [x] A.2 `GET /api/dashboard/son-hareketler` - Son 10 evrak hareketi

- [x] A.3 `GET /api/dashboard/vade-uyarilari` - Vade uyarıları
  - Bugün vadesi dolan
  - Bu hafta vadesi dolacak
  - Gecikmiş

### B. Raporlar API

- [x] B.1 `GET /api/raporlar/tarih-araligi` - Tarih aralığı raporu
  - Query: baslangic, bitis, durum, evrak_tipi
  - Response: özet + detaylı liste

- [x] B.2 `GET /api/raporlar/vade` - Vade raporu
  - Query: gun (7, 14, 30)
  - Response: önümüzdeki X gün vadesi dolacaklar

- [x] B.3 `GET /api/raporlar/cari/:id` - Cari bazlı rapor

### C. Excel Export

- [x] C.1 `GET /api/raporlar/excel` - Excel dosyası oluştur
  - Query: baslangic, bitis, durum, evrak_tipi
  - Response: .xlsx dosyası (Content-Disposition)

- [x] C.2 ExcelJS kütüphanesi entegrasyonu

### D. Yedekleme API (Admin Only)

- [x] D.1 `GET /api/backup` - Yedek listesi + istatistikler
- [x] D.2 `POST /api/backup` - Manuel yedek oluştur
- [x] D.3 `POST /api/backup/:filename/restore` - Yedekten geri yükle
- [x] D.4 `DELETE /api/backup/:filename` - Yedek sil
- [x] D.5 `POST /api/backup/cleanup` - Eski yedekleri temizle

### E. Ayarlar API

- [x] E.1 `GET /api/settings` - Ayarları getir
- [x] E.2 `PUT /api/settings` - Ayarları güncelle (admin)
- [x] E.3 `GET /api/settings/setup-status` - İlk kurulum yapıldı mı?
- [x] E.4 `POST /api/settings/setup` - İlk kurulum (admin oluştur)

### F. Test

- [x] F.1 Dashboard istatistikleri doğruluğu
- [x] F.2 Excel export indirilebilirliği
- [x] F.3 Tarih filtresi çalışıyor mu
- [x] F.4 Yedekleme/geri yükleme çalışıyor mu

---

## 📝 API DETAYLARI

### GET /api/dashboard

**Response:**
```json
{
  "portfoy": { "adet": 25, "tutar": 450000 },
  "tahsil": { "adet": 12, "tutar": 180000 },
  "vade": {
    "bugun": { "adet": 2, "tutar": 30000 },
    "buHafta": { "adet": 5, "tutar": 75000 },
    "gecikmis": { "adet": 2, "tutar": 30000 }
  },
  "tipDagilimi": [...]
}
```

### Dashboard Ek Endpoint'ler

```
GET /api/dashboard/kartlar         # Önceden formatlanmış kartlar
GET /api/dashboard/durum-dagilimi  # Pie chart verisi (label + renk)
GET /api/dashboard/aylik-dagilim   # Bar chart verisi (12 ay)
GET /api/dashboard/son-hareketler  # Son evrak hareketleri
GET /api/dashboard/vade-uyarilari  # Detaylı vade listeleri
GET /api/dashboard/top-cariler     # En çok evrakı olan cariler
```

### Raporlar Endpoint'ler

```
GET /api/raporlar/tarih-araligi    # Tarih bazlı rapor
GET /api/raporlar/vade             # Vade raporu
GET /api/raporlar/cari/:id         # Cari bazlı rapor
GET /api/raporlar/cariler          # Tüm cariler özet
GET /api/raporlar/excel            # Excel export (.xlsx)
```

### Backup Endpoint'ler (Admin Only)

```
GET    /api/backup                 # Liste + istatistikler
GET    /api/backup/stats           # Sadece istatistikler
GET    /api/backup/:filename       # Yedek detay
POST   /api/backup                 # Yeni yedek oluştur
POST   /api/backup/:filename/restore  # Geri yükle
DELETE /api/backup/:filename       # Sil
POST   /api/backup/cleanup         # Eski yedekleri temizle
```

### Settings Endpoint'ler

```
GET  /api/settings/setup-status    # Kurulum durumu (public)
POST /api/settings/setup           # İlk kurulum (public)
GET  /api/settings/app-info        # Uygulama bilgisi (public)
GET  /api/settings                 # Tüm ayarlar
PUT  /api/settings                 # Ayarları güncelle (admin)
GET  /api/settings/:key            # Tek ayar
PUT  /api/settings/:key            # Tek ayar güncelle (admin)
```

---

## ✅ TAMAMLANMA KRİTERLERİ

- [x] Dashboard istatistikleri doğru hesaplanıyor
- [x] Vade uyarıları çalışıyor
- [x] Excel export indirilebiliyor
- [x] Tarih aralığı raporu çalışıyor
- [x] Yedekleme API'leri çalışıyor (admin)
- [x] İlk kurulum API'si çalışıyor

---

## 📝 OTURUM KAYITLARI

### Oturum 6 - 26 Aralık 2025

**Yapılanlar:**

1. **Dashboard Model & Routes:**
   - `models/dashboard.js` - 7 fonksiyon (getOzet, getDurumDagilimi, getAylikVadeDagilimi, getSonHareketler, getVadeUyarilari, getTopCariler, getKartlar)
   - `routes/dashboard.js` - 7 endpoint
   - Türkçe ay isimleri ve grafik renkleri dahil

2. **Raporlar Model & Routes:**
   - `models/raporlar.js` - 6 fonksiyon (tarihAraligiRaporu, vadeRaporu, cariRaporu, tumCarilerRaporu, excelVerisiHazirla, getDurumLabel)
   - `routes/raporlar.js` - 5 endpoint
   - Vade veya kayıt tarihine göre filtreleme

3. **Excel Export:**
   - ExcelJS kütüphanesi kuruldu
   - Profesyonel Excel formatı (header renkleri, kenarlıklar, toplam satırı)
   - Stream olarak gönderim

4. **Backup Model & Routes:**
   - `models/backup.js` - 7 fonksiyon (create, list, getByFilename, restore, delete, cleanup, getStats)
   - `routes/backup.js` - 7 endpoint (admin only)
   - WAL mode desteği, güvenlik yedeği, meta dosyası

5. **Settings Model & Routes:**
   - `models/settings.js` - 9 fonksiyon
   - `routes/settings.js` - 7 endpoint
   - İlk kurulum desteği, varsayılan ayarlar

6. **index.js Entegrasyonu:**
   - 4 yeni route eklendi (dashboard, raporlar, backup, settings)

7. **Test Scripti:**
   - `test-dashboard.js` - 31 test
   - Settings, Dashboard, Raporlar, Backup API testleri

**Oluşturulan Dosyalar:**
- `backend/src/models/dashboard.js`
- `backend/src/models/raporlar.js`
- `backend/src/models/backup.js`
- `backend/src/models/settings.js`
- `backend/src/routes/dashboard.js`
- `backend/src/routes/raporlar.js`
- `backend/src/routes/backup.js`
- `backend/src/routes/settings.js`
- `backend/test-dashboard.js`

**Kurulan Paketler:**
- `exceljs` - Excel dosyası oluşturma

---

**Tamamlanma:** 26 Aralık 2025 - Oturum 6
