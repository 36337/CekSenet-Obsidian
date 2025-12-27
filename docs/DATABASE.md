# Veritabanı Tasarımı

**Son Güncelleme:** 26 Aralık 2025  
**Veritabanı:** SQLite  
**Versiyon:** 2.0 (Hareket geçmişi eklendi)

---

## 📊 GENEL BİLGİLER

| Özellik | Değer |
|---------|-------|
| Veritabanı Tipi | SQLite |
| Dosya | `database/ceksenet.db` |
| Karakter Seti | UTF-8 |

**Neden SQLite?**
- Kurulum gerektirmez
- Tek dosya (kolay yedekleme)
- Küçük-orta ölçek için yeterli
- Cross-platform

---

## 🗄️ TABLO YAPILARI

### 1. users (Kullanıcılar)

| Kolon | Tip | Constraint | Açıklama |
|-------|-----|------------|----------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | - |
| username | TEXT | NOT NULL UNIQUE | Kullanıcı adı |
| password | TEXT | NOT NULL | bcrypt hash |
| ad_soyad | TEXT | NOT NULL | Ad soyad |
| role | TEXT | NOT NULL DEFAULT 'normal' | 'admin' veya 'normal' |
| created_at | TEXT | DEFAULT CURRENT_TIMESTAMP | Kayıt tarihi |
| last_login | TEXT | - | Son giriş |

```sql
CREATE TABLE users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    ad_soyad TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'normal' CHECK(role IN ('admin', 'normal')),
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    last_login TEXT
);
```

---

### 2. cariler (Cari Hesaplar)

| Kolon | Tip | Constraint | Açıklama |
|-------|-----|------------|----------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | - |
| ad_soyad | TEXT | NOT NULL | Ad soyad / Firma adı |
| tip | TEXT | NOT NULL | 'musteri' veya 'tedarikci' |
| telefon | TEXT | - | Telefon |
| email | TEXT | - | E-posta |
| adres | TEXT | - | Adres |
| vergi_no | TEXT | - | Vergi numarası |
| notlar | TEXT | - | Notlar |
| created_at | TEXT | DEFAULT CURRENT_TIMESTAMP | Kayıt tarihi |
| updated_at | TEXT | - | Güncelleme tarihi |

```sql
CREATE TABLE cariler (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    ad_soyad TEXT NOT NULL,
    tip TEXT NOT NULL CHECK(tip IN ('musteri', 'tedarikci')),
    telefon TEXT,
    email TEXT,
    adres TEXT,
    vergi_no TEXT,
    notlar TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    updated_at TEXT
);
```

---

### 3. evraklar (Çek ve Senetler)

| Kolon | Tip | Constraint | Açıklama |
|-------|-----|------------|----------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | - |
| evrak_tipi | TEXT | NOT NULL | 'cek' veya 'senet' |
| evrak_no | TEXT | NOT NULL | Evrak numarası |
| tutar | REAL | NOT NULL | Tutar (decimal) |
| vade_tarihi | TEXT | NOT NULL | Vade tarihi (ISO format) |
| banka_adi | TEXT | - | Banka adı |
| kesideci | TEXT | NOT NULL | Keşideci |
| cari_id | INTEGER | FOREIGN KEY | Cari referans |
| durum | TEXT | NOT NULL DEFAULT 'portfoy' | Durum |
| notlar | TEXT | - | Notlar |
| created_at | TEXT | DEFAULT CURRENT_TIMESTAMP | Kayıt tarihi |
| updated_at | TEXT | - | Güncelleme tarihi |
| created_by | INTEGER | FOREIGN KEY | Oluşturan kullanıcı |

```sql
CREATE TABLE evraklar (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    evrak_tipi TEXT NOT NULL CHECK(evrak_tipi IN ('cek', 'senet')),
    evrak_no TEXT NOT NULL,
    tutar REAL NOT NULL,
    vade_tarihi TEXT NOT NULL,
    banka_adi TEXT,
    kesideci TEXT NOT NULL,
    cari_id INTEGER,
    durum TEXT NOT NULL DEFAULT 'portfoy' CHECK(durum IN ('portfoy', 'bankada', 'ciro', 'tahsil', 'karsiliksiz')),
    notlar TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    updated_at TEXT,
    created_by INTEGER,
    FOREIGN KEY (cari_id) REFERENCES cariler(id) ON DELETE SET NULL,
    FOREIGN KEY (created_by) REFERENCES users(id) ON DELETE SET NULL
);
```

---

### 4. evrak_hareketleri (Durum Geçmişi) - YENİ

| Kolon | Tip | Constraint | Açıklama |
|-------|-----|------------|----------|
| id | INTEGER | PRIMARY KEY AUTOINCREMENT | - |
| evrak_id | INTEGER | FOREIGN KEY NOT NULL | Evrak referans |
| eski_durum | TEXT | - | Önceki durum |
| yeni_durum | TEXT | NOT NULL | Yeni durum |
| aciklama | TEXT | - | Değişiklik açıklaması |
| created_at | TEXT | DEFAULT CURRENT_TIMESTAMP | İşlem tarihi |
| created_by | INTEGER | FOREIGN KEY | İşlemi yapan |

```sql
CREATE TABLE evrak_hareketleri (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    evrak_id INTEGER NOT NULL,
    eski_durum TEXT,
    yeni_durum TEXT NOT NULL,
    aciklama TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    created_by INTEGER,
    FOREIGN KEY (evrak_id) REFERENCES evraklar(id) ON DELETE CASCADE,
    FOREIGN KEY (created_by) REFERENCES users(id) ON DELETE SET NULL
);
```

---

### 5. ayarlar (Sistem Ayarları) - OPSİYONEL

| Kolon | Tip | Constraint | Açıklama |
|-------|-----|------------|----------|
| key | TEXT | PRIMARY KEY | Ayar anahtarı |
| value | TEXT | - | Ayar değeri |

```sql
CREATE TABLE ayarlar (
    key TEXT PRIMARY KEY,
    value TEXT
);
```

---

## 📋 VERİ SÖZLÜĞÜ

### Evrak Durumları

| Kod | Açıklama | Akış |
|-----|----------|------|
| `portfoy` | Şirketin kasasında | Başlangıç durumu |
| `bankada` | Bankaya tahsile/teminata verildi | portfoy → bankada |
| `ciro` | Tedarikçiye ciro edildi | portfoy → ciro |
| `tahsil` | Para hesaba geçti | bankada → tahsil |
| `karsiliksiz` | Ödenmedi (sorunlu) | bankada → karsiliksiz |

### Cari Tipleri

| Kod | Açıklama |
|-----|----------|
| `musteri` | Bizden mal/hizmet alan |
| `tedarikci` | Bize mal/hizmet satan |

### Kullanıcı Rolleri

| Kod | Yetkiler |
|-----|----------|
| `admin` | Tüm işlemler + kullanıcı yönetimi |
| `normal` | Görüntüleme + ekleme/düzenleme |

---

## 🔧 SQL KURULUM SCRIPTİ

```sql
-- Kullanıcılar
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    username TEXT NOT NULL UNIQUE,
    password TEXT NOT NULL,
    ad_soyad TEXT NOT NULL,
    role TEXT NOT NULL DEFAULT 'normal' CHECK(role IN ('admin', 'normal')),
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    last_login TEXT
);

-- Cariler
CREATE TABLE IF NOT EXISTS cariler (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    ad_soyad TEXT NOT NULL,
    tip TEXT NOT NULL CHECK(tip IN ('musteri', 'tedarikci')),
    telefon TEXT,
    email TEXT,
    adres TEXT,
    vergi_no TEXT,
    notlar TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    updated_at TEXT
);

-- Evraklar
CREATE TABLE IF NOT EXISTS evraklar (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    evrak_tipi TEXT NOT NULL CHECK(evrak_tipi IN ('cek', 'senet')),
    evrak_no TEXT NOT NULL,
    tutar REAL NOT NULL,
    vade_tarihi TEXT NOT NULL,
    banka_adi TEXT,
    kesideci TEXT NOT NULL,
    cari_id INTEGER,
    durum TEXT NOT NULL DEFAULT 'portfoy' CHECK(durum IN ('portfoy', 'bankada', 'ciro', 'tahsil', 'karsiliksiz')),
    notlar TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    updated_at TEXT,
    created_by INTEGER,
    FOREIGN KEY (cari_id) REFERENCES cariler(id) ON DELETE SET NULL,
    FOREIGN KEY (created_by) REFERENCES users(id) ON DELETE SET NULL
);

-- Evrak Hareketleri
CREATE TABLE IF NOT EXISTS evrak_hareketleri (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    evrak_id INTEGER NOT NULL,
    eski_durum TEXT,
    yeni_durum TEXT NOT NULL,
    aciklama TEXT,
    created_at TEXT DEFAULT CURRENT_TIMESTAMP,
    created_by INTEGER,
    FOREIGN KEY (evrak_id) REFERENCES evraklar(id) ON DELETE CASCADE,
    FOREIGN KEY (created_by) REFERENCES users(id) ON DELETE SET NULL
);

-- Ayarlar
CREATE TABLE IF NOT EXISTS ayarlar (
    key TEXT PRIMARY KEY,
    value TEXT
);

-- İndexler (performans için)
CREATE INDEX IF NOT EXISTS idx_evraklar_durum ON evraklar(durum);
CREATE INDEX IF NOT EXISTS idx_evraklar_vade ON evraklar(vade_tarihi);
CREATE INDEX IF NOT EXISTS idx_evraklar_cari ON evraklar(cari_id);
CREATE INDEX IF NOT EXISTS idx_hareketler_evrak ON evrak_hareketleri(evrak_id);
```

---

## 📊 ÖRNEK SORGULAR

### Dashboard İstatistikleri

```sql
-- Portföy toplamı
SELECT SUM(tutar) as toplam FROM evraklar WHERE durum = 'portfoy';

-- Tahsil edilen
SELECT SUM(tutar) as toplam FROM evraklar WHERE durum = 'tahsil';

-- Bu hafta vadesi gelen
SELECT COUNT(*) as adet, SUM(tutar) as toplam 
FROM evraklar 
WHERE vade_tarihi BETWEEN date('now') AND date('now', '+7 days')
AND durum NOT IN ('tahsil', 'karsiliksiz');

-- Gecikmiş evraklar
SELECT COUNT(*) as adet, SUM(tutar) as toplam 
FROM evraklar 
WHERE vade_tarihi < date('now')
AND durum NOT IN ('tahsil', 'ciro');

-- Durum dağılımı
SELECT durum, COUNT(*) as adet, SUM(tutar) as toplam
FROM evraklar
GROUP BY durum;
```

### Evrak Listesi (Filtrelemeli)

```sql
SELECT e.*, c.ad_soyad as cari_adi
FROM evraklar e
LEFT JOIN cariler c ON e.cari_id = c.id
WHERE 
    (e.durum = ? OR ? IS NULL)
    AND (e.evrak_tipi = ? OR ? IS NULL)
    AND (e.vade_tarihi >= ? OR ? IS NULL)
    AND (e.vade_tarihi <= ? OR ? IS NULL)
    AND (e.evrak_no LIKE ? OR e.kesideci LIKE ? OR ? IS NULL)
ORDER BY e.vade_tarihi ASC
LIMIT ? OFFSET ?;
```

### Cari Özeti

```sql
SELECT 
    c.*,
    COUNT(e.id) as evrak_sayisi,
    COALESCE(SUM(e.tutar), 0) as toplam_tutar
FROM cariler c
LEFT JOIN evraklar e ON c.id = e.cari_id
GROUP BY c.id;
```

---

## 🔄 MIGRATION STRATEJİSİ

### Versiyon Tablosu

```sql
CREATE TABLE IF NOT EXISTS db_migrations (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    version TEXT NOT NULL UNIQUE,
    description TEXT,
    applied_at TEXT DEFAULT CURRENT_TIMESTAMP
);
```

### Migration Yapısı

```
backend/
└── database/
    ├── migrations/
    │   ├── 001_initial.sql
    │   ├── 002_add_notlar.sql
    │   └── ...
    └── migrate.js
```

### Migration Dosya Formatı

```sql
-- Migration: 002_add_notlar
-- Description: Evraklara notlar alanı eklendi
-- Version: 1.1.0

ALTER TABLE evraklar ADD COLUMN notlar TEXT;
```

### Uygulama Başlangıcında

```javascript
// migrate.js
async function runMigrations(db) {
  // 1. db_migrations tablosu yoksa oluştur
  // 2. Uygulanmış migration'ları al
  // 3. Yeni migration dosyalarını bul
  // 4. Sırayla uygula
  // 5. Her birini db_migrations'a kaydet
}
```

### Güvenlik

- Her migration işleminden önce otomatik yedek
- Hata durumunda transaction rollback
- Migration başarısız olursa uygulama başlamaz

---

## 💾 YEDEKLEME

### Yedek Alma

```bash
# Dosyayı kopyala
cp database/ceksenet.db database/backups/ceksenet_$(date +%Y-%m-%d).db
```

### Geri Yükleme

```bash
# Dosyayı geri kopyala
cp database/backups/ceksenet_2025-12-26.db database/ceksenet.db
```

---

## ⚠️ ÖNEMLİ NOTLAR

### SQLite Limitleri

- Eşzamanlı yazma: Tek yazma, çoklu okuma
- Dosya boyutu: Pratik limit ~100GB (bizim için yeterli)
- Bağlantı: Dosya tabanlı, network desteği yok (tek sunucu)

### Performans İpuçları

- Index'ler eklendi (durum, vade_tarihi, cari_id)
- Büyük listeler için sayfalama kullan
- VACUUM komutu (opsiyonel, disk alanı optimizasyonu)

### Veri Bütünlüğü

- Foreign key'ler tanımlı (ON DELETE SET NULL / CASCADE)
- Check constraint'ler (durum, tip enum'ları)
- NOT NULL zorunlu alanlar

---

**Son Güncelleme:** 26 Aralık 2025
