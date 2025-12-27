# TASK-04: Backend - Evraklar API

**Durum:** ✅ Tamamlandı  
**Öncelik:** 🔥 Kritik  
**Tahmini Süre:** 1-2 oturum  
**Gerçekleşen Süre:** 1 oturum  
**Bağımlılıklar:** TASK-03

---

## 🎯 HEDEF

Çek/senet yönetimi API'lerini oluşturmak. Durum değişikliği, hareket geçmişi, filtreleme.

---

## 📋 ALT GÖREVLER

### A. Evrak Model

- [x] A.1 Evrak model fonksiyonları:
  - `getAll(filters)` - Filtreleme, sıralama, sayfalama ✅
  - `getById(id)` - Tek evrak + cari bilgisi ✅
  - `create(data, userId)` - Yeni evrak + hareket kaydı ✅
  - `update(id, data, userId)` - Güncelleme ✅
  - `updateDurum(id, durum, aciklama, userId)` - Durum değişikliği + hareket ✅
  - `delete(id)` - Silme ✅
  - `getHareketler(id)` - Evrak hareket geçmişi ✅
  - `bulkUpdateDurum(ids, durum, aciklama, userId)` - Toplu güncelleme ✅ (ek)
  - `getVadeOzeti()` - Dashboard için vade özeti ✅ (ek)
  - `getDurumOzeti()` - Dashboard için durum özeti ✅ (ek)

### B. Evrak Routes

- [x] B.1 `GET /api/evraklar` - Liste ✅
  - Filtreler: durum, evrak_tipi, vade_baslangic, vade_bitis, tutar_min, tutar_max, search, cari_id
  - Sıralama: vade_tarihi, tutar, created_at, evrak_no
  - Sayfalama
- [x] B.2 `POST /api/evraklar` - Yeni evrak ✅
- [x] B.3 `GET /api/evraklar/:id` - Evrak detayı ✅
- [x] B.4 `PUT /api/evraklar/:id` - Evrak güncelle ✅
- [x] B.5 `DELETE /api/evraklar/:id` - Evrak sil (admin only) ✅
- [x] B.6 `PATCH /api/evraklar/:id/durum` - Durum güncelle ✅
- [x] B.7 `GET /api/evraklar/:id/hareketler` - Hareket geçmişi ✅
- [x] B.8 `POST /api/evraklar/toplu-durum` - Toplu durum güncelleme ✅

### C. Validation

- [x] C.1 evrak_tipi: cek veya senet ✅
- [x] C.2 evrak_no: zorunlu, 1-50 karakter ✅
- [x] C.3 tutar: pozitif sayı (> 0.01) ✅
- [x] C.4 vade_tarihi: geçerli tarih (ISO8601) ✅
- [x] C.5 kesideci: zorunlu, 2-200 karakter ✅
- [x] C.6 durum: enum kontrolü ✅
- [x] C.7 banka_adi: opsiyonel, max 100 karakter ✅ (ek)
- [x] C.8 notlar: opsiyonel, max 1000 karakter ✅ (ek)
- [x] C.9 aciklama: opsiyonel, max 500 karakter ✅ (ek)

### D. Durum Akışı Kontrolü

- [x] D.1 Geçerli durum geçişlerini kontrol et: ✅
  - portfoy → bankada, ciro ✅
  - bankada → tahsil, karsiliksiz ✅
  - ciro → (son durum) ✅
  - tahsil → (son durum) ✅
  - karsiliksiz → tahsil (geri dönüş) ✅

### E. Test

- [x] E.1 Evrak ekleme test ✅
- [x] E.2 Filtreleme test (tüm kombinasyonlar) ✅
- [x] E.3 Durum değişikliği test ✅
- [x] E.4 Hareket geçmişi test ✅
- [x] E.5 Toplu durum güncelleme test ✅
- [x] E.6 Geçersiz durum geçişi testi ✅ (ek)
- [x] E.7 Son durum koruması testi ✅ (ek)
- [x] E.8 Authentication testi ✅ (ek)

---

## 📝 API DETAYLARI

### GET /api/evraklar

**Query Params:**
- `durum`: portfoy,bankada (virgülle ayrılmış)
- `evrak_tipi`: cek | senet
- `vade_baslangic`: 2025-01-01
- `vade_bitis`: 2025-12-31
- `tutar_min`: 1000
- `tutar_max`: 50000
- `search`: evrak no veya keşideci
- `cari_id`: belirli cari
- `sort`: vade_tarihi | tutar | created_at | evrak_no
- `order`: asc | desc
- `page`: 1
- `limit`: 20

### PATCH /api/evraklar/:id/durum

**Request:**
```json
{
  "durum": "bankada",
  "aciklama": "Garanti Bankası'na tahsile verildi"
}
```

**Response:**
```json
{
  "message": "Evrak durumu güncellendi",
  "evrak": { ... },
  "hareket": {
    "id": 5,
    "eski_durum": "portfoy",
    "yeni_durum": "bankada",
    "aciklama": "Garanti Bankası'na tahsile verildi",
    "created_at": "2025-12-26T10:30:00Z"
  }
}
```

### POST /api/evraklar/toplu-durum

**Request:**
```json
{
  "ids": [1, 2, 3],
  "durum": "bankada",
  "aciklama": "Toplu tahsile gönderim"
}
```

**Response:**
```json
{
  "message": "2 evrak güncellendi",
  "success": 2,
  "failed": [{"id": 3, "message": "..."}]
}
```

---

## ✅ TAMAMLANMA KRİTERLERİ

- [x] Evrak CRUD çalışıyor ✅
- [x] Tüm filtreler çalışıyor ✅
- [x] Durum değişikliği hareket kaydı oluşturuyor ✅
- [x] Geçersiz durum geçişi engelleniyor ✅
- [x] Toplu durum güncelleme çalışıyor ✅

---

## 📝 OTURUM KAYITLARI

### Oturum 5 - 26 Aralık 2025

**Yapılanlar:**

1. **Evrak Model Oluşturuldu** (`backend/src/models/evraklar.js`)
   - 10 fonksiyon: getAll, getById, create, update, updateDurum, bulkUpdateDurum, delete, getHareketler, getVadeOzeti, getDurumOzeti
   - Durum akışı validasyonu (DURUM_GECISLERI)
   - Transaction ile evrak + hareket kaydı
   - Gelişmiş filtreleme (7 filtre + sıralama + sayfalama)

2. **Evrak Routes Oluşturuldu** (`backend/src/routes/evraklar.js`)
   - 8 endpoint (CRUD + durum + hareketler + toplu)
   - express-validator ile input validation
   - Admin only delete endpoint
   - Reusable validation kuralları

3. **Route Entegrasyonu** (`backend/src/index.js`)
   - Import ve mount eklendi

4. **Test Scripti** (`backend/test-evraklar.js`)
   - 23 test, hepsi başarılı
   - CRUD, filtreleme, durum akışı, authentication testleri

**Oluşturulan Dosyalar:**
- `backend/src/models/evraklar.js` (13.8 KB)
- `backend/src/routes/evraklar.js` (13.5 KB)
- `backend/test-evraklar.js`

**Test Sonuçları:**
```
✅ Başarılı: 23
❌ Başarısız: 0
📊 Toplam: 23
```

**Sonraki Task:** TASK-05 (Backend Dashboard & Raporlar)

---

**Oluşturulma:** 26 Aralık 2025  
**Tamamlanma:** 26 Aralık 2025
