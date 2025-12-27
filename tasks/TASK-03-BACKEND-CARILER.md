# TASK-03: Backend - Cariler API

**Durum:** ✅ Tamamlandı  
**Öncelik:** 🔥 Kritik  
**Tahmini Süre:** 1 oturum  
**Gerçekleşen Süre:** 1 oturum  
**Bağımlılıklar:** TASK-02 ✅

---

## 🎯 HEDEF

Cari hesap (müşteri/tedarikçi) yönetimi API'lerini oluşturmak.

---

## 📋 ALT GÖREVLER

### A. Cari Model

- [x] A.1 Cari model fonksiyonları yaz:
  - `getAll(filters)` - Filtreleme ve sayfalama ile ✅
  - `getById(id)` - Tek cari ✅
  - `create(data)` - Yeni cari ✅
  - `update(id, data)` - Güncelleme ✅
  - `delete(id)` - Silme (evrak kontrolü ile) ✅
  - `getWithStats(id)` - Cari + evrak istatistikleri ✅
  - `getEvraklar(cariId, filters)` - Cariye ait evraklar ✅

### B. Cari Routes

- [x] B.1 `GET /api/cariler` - Liste
  - Query params: tip, search, page, limit ✅
  - Response: cariler + toplam sayı ✅
- [x] B.2 `POST /api/cariler` - Yeni cari ekle ✅
- [x] B.3 `GET /api/cariler/:id` - Cari detayı (istatistiklerle) ✅
- [x] B.4 `PUT /api/cariler/:id` - Cari güncelle ✅
- [x] B.5 `DELETE /api/cariler/:id` - Cari sil ✅
- [x] B.6 `GET /api/cariler/:id/evraklar` - Cariye ait evraklar ✅

### C. Validation

- [x] C.1 ad_soyad zorunlu (2-200 karakter) ✅
- [x] C.2 tip zorunlu ve enum kontrolü (musteri/tedarikci) ✅
- [x] C.3 email format kontrolü (opsiyonel alan) ✅
- [x] C.4 telefon format kontrolü (opsiyonel, max 50 karakter) ✅

### D. Test

- [x] D.1 Cari ekleme test ✅
- [x] D.2 Cari listeleme + filtreleme test ✅
- [x] D.3 Cari güncelleme test ✅
- [x] D.4 Cari silme test ✅
- [x] D.5 Validation hataları test ✅
- [x] D.6 Authentication test (401) ✅

---

## 📝 API DETAYLARI

### GET /api/cariler

**Query Params:**
- `tip`: musteri | tedarikci | (boş = hepsi)
- `search`: ad_soyad veya telefon içinde arama
- `page`: sayfa numarası (default: 1)
- `limit`: sayfa başına kayıt (default: 20, max: 100)

**Response:**
```json
{
  "data": [
    {
      "id": 1,
      "ad_soyad": "ABC Ticaret Ltd.",
      "tip": "musteri",
      "telefon": "0212 555 1234",
      "email": "info@abc.com",
      "evrak_sayisi": 5,
      "toplam_tutar": 125000.00
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 45,
    "totalPages": 3
  }
}
```

### POST /api/cariler

**Request:**
```json
{
  "ad_soyad": "ABC Ticaret Ltd.",
  "tip": "musteri",
  "telefon": "0212 555 1234",
  "email": "info@abc.com",
  "adres": "İstanbul",
  "vergi_no": "1234567890",
  "notlar": "Önemli müşteri"
}
```

### GET /api/cariler/:id

**Response:** Cari bilgileri + `istatistikler` objesi (durum bazlı evrak adet/tutar)

---

## ✅ TAMAMLANMA KRİTERLERİ

- [x] Cari CRUD işlemleri çalışıyor ✅
- [x] Filtreleme ve sayfalama çalışıyor ✅
- [x] Cari detayında evrak istatistikleri görünüyor ✅
- [x] Validation hataları düzgün dönüyor ✅

---

## 📝 OTURUM KAYITLARI

### Oturum 4 - 26 Aralık 2025

**Yapılanlar:**

1. **Cari Model Oluşturuldu** (`backend/src/models/cariler.js`):
   - 7 fonksiyon: getAll, getById, getWithStats, create, update, delete, getEvraklar
   - Liste sorgusunda evrak_sayisi ve toplam_tutar JOIN ile geliyor
   - Detay sorgusunda durum bazlı istatistikler
   - Silme işleminde evrak bağımlılık kontrolü

2. **Cari Routes Oluşturuldu** (`backend/src/routes/cariler.js`):
   - 6 endpoint: liste, detay, ekle, güncelle, sil, evraklar
   - express-validator ile input validation
   - Authentication zorunluluğu
   - Logger ile işlem kaydı

3. **Route Entegrasyonu** (`backend/src/index.js`):
   - carilerRoutes import ve kullanım eklendi

4. **Test** (`backend/test-cariler.js`):
   - 14 test yazıldı ve hepsi başarılı
   - CRUD, filtreleme, arama, validation, auth testleri

**Oluşturulan Dosyalar:**
- `backend/src/models/cariler.js`
- `backend/src/routes/cariler.js`
- `backend/test-cariler.js`

**Test Sonuçları:** 14/14 başarılı ✅

---

**Tamamlanma:** 26 Aralık 2025 - Oturum 4
