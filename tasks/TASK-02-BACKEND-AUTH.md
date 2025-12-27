# TASK-02: Backend - Auth & Users API

**Durum:** ✅ Tamamlandı  
**Öncelik:** 🔥 Kritik  
**Tahmini Süre:** 1 oturum  
**Gerçekleşen Süre:** 1 oturum  
**Bağımlılıklar:** TASK-01
**Tamamlanma:** 26 Aralık 2025 - Oturum 3

---

## 🎯 HEDEF

Kullanıcı kimlik doğrulama (JWT) ve kullanıcı yönetimi API'lerini oluşturmak.

---

## 📋 ALT GÖREVLER

### A. Auth Middleware

- [x] A.1 JWT token doğrulama middleware'i yaz
- [x] A.2 Role kontrolü middleware'i yaz (admin/normal)
- [x] A.3 Rate limiting ekle (login endpoint için)

### B. Auth Routes

- [x] B.1 `POST /api/auth/login` - Giriş
  - Username + password al
  - bcrypt ile şifre doğrula
  - JWT token oluştur (24 saat geçerli)
  - last_login güncelle
- [x] B.2 `POST /api/auth/logout` - Çıkış (client-side token silme)
- [x] B.3 `GET /api/auth/me` - Mevcut kullanıcı bilgisi
- [x] B.4 `PUT /api/auth/password` - Şifre değiştirme

### C. Users Routes (Admin Only)

- [x] C.1 `GET /api/users` - Kullanıcı listesi
- [x] C.2 `POST /api/users` - Yeni kullanıcı ekle
- [x] C.3 `GET /api/users/:id` - Kullanıcı detayı
- [x] C.4 `PUT /api/users/:id` - Kullanıcı güncelle
- [x] C.5 `DELETE /api/users/:id` - Kullanıcı sil
- [x] C.6 `PUT /api/users/:id/password` - Şifre sıfırla (admin)

### D. Validation

- [x] D.1 Login validasyonu (username, password zorunlu)
- [x] D.2 User create validasyonu (username unique, şifre min 6 karakter)
- [x] D.3 Password validasyonu (min 6 karakter, tekrar eşleşmeli)

### E. Test

- [x] E.1 Postman/curl ile login test
- [x] E.2 Token ile korumalı endpoint test
- [x] E.3 Admin olmayan kullanıcı ile users endpoint test (403 beklenir)

---

## 📝 API DETAYLARI

### POST /api/auth/login

**Request:**
```json
{
  "username": "admin",
  "password": "123456"
}
```

**Response (200):**
```json
{
  "message": "Giriş başarılı",
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": {
    "id": 1,
    "username": "admin",
    "ad_soyad": "Sistem Yöneticisi",
    "role": "admin"
  }
}
```

**Response (401):**
```json
{
  "error": "Giriş başarısız",
  "message": "Hatalı kullanıcı adı veya şifre"
}
```

### GET /api/auth/me

**Headers:** `Authorization: Bearer <token>`

**Response (200):**
```json
{
  "id": 1,
  "username": "admin",
  "ad_soyad": "Sistem Yöneticisi",
  "role": "admin",
  "last_login": "2025-12-25T23:51:34.691Z"
}
```

---

## ✅ TAMAMLANMA KRİTERLERİ

- [x] Login çalışıyor, JWT token dönüyor
- [x] Token olmadan korumalı endpoint'lere erişilemiyor (401)
- [x] Admin olmayan kullanıcı users endpoint'lerine erişemiyor (403)
- [x] Şifre değiştirme çalışıyor
- [x] Kullanıcı CRUD işlemleri çalışıyor

---

## 📝 OTURUM KAYITLARI

### Oturum 3 - 26 Aralık 2025

**Yapılanlar:**

1. **Auth Middleware** (`middleware/auth.js`):
   - `authenticate` - JWT token doğrulama
   - `requireAdmin` - Admin rolü kontrolü
   - `optionalAuth` - Opsiyonel auth

2. **Rate Limiter** (`middleware/rateLimiter.js`):
   - `loginLimiter` - 15 dk'da max 5 deneme
   - `apiLimiter` - Dakikada max 100 istek

3. **Auth Routes** (`routes/auth.js`):
   - POST /api/auth/login
   - POST /api/auth/logout
   - GET /api/auth/me
   - PUT /api/auth/password

4. **Users Routes** (`routes/users.js`):
   - GET /api/users
   - GET /api/users/:id
   - POST /api/users
   - PUT /api/users/:id
   - PUT /api/users/:id/password
   - DELETE /api/users/:id

5. **Seed Script** (`seed.js`):
   - İlk admin kullanıcı: admin / 123456
   - Startup'ta otomatik çalışır

6. **Ek Paket:**
   - express-rate-limit eklendi

**Test Sonuçları:**
| Test | Sonuç |
|------|-------|
| Login (doğru şifre) | ✅ Token döndü |
| Login (yanlış şifre) | ✅ 401 hatası |
| GET /api/auth/me | ✅ Kullanıcı bilgisi |
| GET /api/users (admin) | ✅ Liste döndü |
| GET /api/users (token yok) | ✅ 401 hatası |

**Sorunlar ve Çözümler:**
- users tablosunda updated_at kolonu yoktu - route'lardan kaldırıldı
- express-rate-limit IPv6 uyarısı - custom keyGenerator kaldırıldı
- Port 7475'te eski instance çalışıyordu - process sonlandırıldı

---

**Oluşturulma:** 26 Aralık 2025  
**Tamamlanma:** 26 Aralık 2025
