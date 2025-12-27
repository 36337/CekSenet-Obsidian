# Teknik Mimari (Tech Stack)

**Son Güncelleme:** 26 Aralık 2025  
**Versiyon:** 2.0 (Modern stack)

---

## 🎯 MİMARİ KARARLARI

| Karar | Seçim | Alternatifler | Neden Bu? |
|-------|-------|---------------|-----------|
| Backend | Node.js + Express | PHP, Python | JS ekosistemi, hızlı geliştirme |
| Database | SQLite | MySQL, PostgreSQL | Kurulum yok, tek dosya, kolay yedek |
| Frontend | React | Vue, vanilla JS | Component-based, büyük ekosistem |
| Styling | Tailwind CSS | Bootstrap, CSS | Utility-first, hızlı, responsive |
| Auth | JWT + bcrypt | Session-based | Stateless, mobil uyumlu |
| Packaging | Inno Setup | NSIS, MSI | Kolay, profesyonel |

---

## 📦 BACKEND

### Node.js + Express

```javascript
// Ana bağımlılıklar
{
  "express": "^4.18.x",      // Web framework
  "better-sqlite3": "^9.x",  // SQLite (sync, hızlı)
  "jsonwebtoken": "^9.x",    // JWT auth
  "bcryptjs": "^2.4.x",      // Şifre hash
  "cors": "^2.8.x",          // CORS
  "helmet": "^7.x",          // Güvenlik headers
  "express-validator": "^7.x" // Input validation
}
```

### API Yapısı (REST)

```
POST   /api/auth/login          # Giriş
POST   /api/auth/logout         # Çıkış
GET    /api/auth/me             # Mevcut kullanıcı

GET    /api/evraklar            # Liste (filtreleme, sayfalama)
POST   /api/evraklar            # Yeni ekle
GET    /api/evraklar/:id        # Detay
PUT    /api/evraklar/:id        # Güncelle
DELETE /api/evraklar/:id        # Sil
PATCH  /api/evraklar/:id/durum  # Durum güncelle
GET    /api/evraklar/:id/gecmis # Hareket geçmişi

GET    /api/cariler             # Liste
POST   /api/cariler             # Yeni ekle
GET    /api/cariler/:id         # Detay
PUT    /api/cariler/:id         # Güncelle
DELETE /api/cariler/:id         # Sil

GET    /api/dashboard           # Özet istatistikler
GET    /api/raporlar/excel      # Excel export

GET    /api/users               # Kullanıcı listesi (admin)
POST   /api/users               # Kullanıcı ekle (admin)
PUT    /api/users/:id           # Kullanıcı güncelle (admin)
DELETE /api/users/:id           # Kullanıcı sil (admin)
```

---

## 🗄️ DATABASE

### SQLite

**Neden SQLite?**
- Kurulum yok (tek .db dosyası)
- Yedekleme = dosyayı kopyala
- Küçük-orta ölçek için yeterli performans
- ACID uyumlu

**Dosya Konumu:**
```
ceksenet/database/ceksenet.db
```

**Yedekleme:**
```
ceksenet/database/backups/ceksenet_2025-12-26.db
```

---

## 🎨 FRONTEND

### React + Vite

```javascript
// Ana bağımlılıklar
{
  "react": "^18.x",
  "react-router-dom": "^6.x",  // Routing
  "axios": "^1.x",             // HTTP client
  "react-hook-form": "^7.x",   // Form handling
  "react-query": "^5.x",       // Data fetching + cache
  "recharts": "^2.x",          // Grafikler
  "date-fns": "^3.x",          // Tarih işlemleri
  "xlsx": "^0.18.x"            // Excel export (client-side)
}
```

### Tailwind CSS v4

```javascript
// Tailwind CSS v4 bağımlılıkları
{
  "tailwindcss": "^4.1.x",
  "@tailwindcss/postcss": "^4.1.x"  // PostCSS entegrasyonu
}
```

**Not:** Tailwind v4'te form styling dahili olarak gelir, ayrı eklenti gerekmez.

### Sayfa Yapısı

```
src/
├── components/
│   ├── layout/
│   │   ├── Sidebar.jsx
│   │   ├── Header.jsx
│   │   └── Layout.jsx
│   ├── ui/
│   │   ├── Button.jsx
│   │   ├── Input.jsx
│   │   ├── Table.jsx
│   │   ├── Modal.jsx
│   │   ├── Badge.jsx
│   │   └── Card.jsx
│   └── forms/
│       ├── EvrakForm.jsx
│       └── CariForm.jsx
│
├── pages/
│   ├── Login.jsx
│   ├── Dashboard.jsx
│   ├── evraklar/
│   │   ├── EvrakListesi.jsx
│   │   ├── EvrakEkle.jsx
│   │   └── EvrakDetay.jsx
│   ├── cariler/
│   │   ├── CariListesi.jsx
│   │   └── CariEkle.jsx
│   ├── raporlar/
│   │   └── Raporlar.jsx
│   └── ayarlar/
│       └── Kullanicilar.jsx (admin)
│
├── hooks/
│   ├── useAuth.js
│   └── useEvraklar.js
│
├── services/
│   └── api.js
│
└── App.jsx
```

---

## 🔐 GÜVENLİK

### Authentication Flow

```
1. Kullanıcı login → POST /api/auth/login
2. Server JWT token döner (24 saat geçerli)
3. Client token'ı localStorage'da saklar
4. Her request'te Authorization: Bearer <token>
5. Server token'ı doğrular, user bilgisi ekler
```

### Güvenlik Önlemleri

- [x] Password hashing (bcrypt, salt rounds: 12)
- [x] JWT token (HS256, 24h expiry)
- [x] CORS (sadece izinli origin'ler)
- [x] Helmet (güvenlik headers)
- [x] Input validation (express-validator)
- [x] SQL injection koruması (parameterized queries)
- [x] Rate limiting (login endpoint)

---

## 📦 PAKETLEME

### Windows Installer (Inno Setup)

**Installer İçeriği:**
1. Node.js runtime (embedded)
2. Uygulama dosyaları (build edilmiş)
3. SQLite veritabanı (boş şablon)
4. Windows servisi kurulumu

**Kurulum Sonrası:**
- Servis otomatik başlar
- Sistem başlangıcında otomatik çalışır
- Tray icon (opsiyonel)

### Build Süreci

```bash
# Development
npm run dev           # Frontend + Backend (hot reload)

# Production build
npm run build         # Frontend build
npm run package       # Electron/pkg ile paketleme
npm run installer     # Inno Setup ile .exe oluştur
```

---

## 🌐 ERİŞİM

### Port Yapılandırması

| Servis | Port | Açıklama |
|--------|------|----------|
| Backend API | 7475 | Express server (dev) |
| Frontend | 5173 | Vite dev server |
| Production | 7474 | Tek port (backend serve eder) |

**Production'da:** Tek port (7474), backend static frontend'i serve eder.

### Erişim URL'leri

```
Lokal:        http://localhost:7474
LAN:          http://192.168.X.X:7474
İnternet:     http://[STATIK-IP]:7474
```

---

## 📋 GELİŞTİRME ORTAMI

### Gereksinimler

- Node.js 20.x LTS
- npm 10.x
- Git
- VS Code (önerilen)

### VS Code Eklentileri

- ESLint
- Prettier
- Tailwind CSS IntelliSense
- ES7+ React/Redux/React-Native snippets

---

**Son Güncelleme:** 26 Aralık 2025
