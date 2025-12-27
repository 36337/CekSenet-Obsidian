# Erişim Bilgileri (Credentials)

**Son Güncelleme:** 26 Aralık 2025  
**⚠️ GİZLİ DOKÜMAN - Paylaşmayın!**

---

## 🌐 URL'LER

### Geliştirme Ortamı

| Sayfa | URL |
|-------|-----|
| Frontend (Vite) | http://localhost:5173 |
| Backend API | http://localhost:7475/api |
| Health Check | http://localhost:7475/api/health |

### Production

| Sayfa | URL |
|-------|-----|
| Uygulama | http://localhost:7474 |
| LAN Erişimi | http://[PC-IP]:7474 |
| Dış Erişim | http://[STATIK-IP]:7474 |

---

## 👤 UYGULAMA KULLANICILARI

### Admin Hesabı

İlk kurulumda **İlk Kurulum Sihirbazı** ile oluşturulur.

| Özellik | Değer |
|---------|-------|
| Kullanıcı Adı | *(kurulumda belirlenir)* |
| Şifre | *(kurulumda belirlenir)* |
| Rol | admin |

⚠️ **Güçlü şifre kullanın!** (min 8 karakter, harf + rakam)

---

## 🗄️ VERİTABANI BİLGİLERİ

### SQLite

| Özellik | Değer |
|---------|-------|
| Veritabanı Tipi | SQLite |
| Dosya Konumu | `database/ceksenet.db` |
| Yedek Konumu | `database/backups/` |

**Not:** SQLite dosya tabanlıdır, ayrı bir veritabanı sunucusu yoktur.

---

## 🔧 SERVİS BİLGİLERİ

### Windows Servisi (Production)

| Özellik | Değer |
|---------|-------|
| Servis Adı | CekSenet |
| Port | 7474 |
| Başlangıç | Otomatik |

### Servis Kontrolü

```bash
# Servisi başlat
net start CekSenet

# Servisi durdur
net stop CekSenet

# Servis durumunu kontrol et
sc query CekSenet
```

---

## 📁 DOSYA YOLLARI

### Geliştirme

| Bileşen | Yol |
|---------|-----|
| Proje Dizini | `F:\projects\ceksenet\` |
| Backend | `F:\projects\ceksenet\backend\` |
| Frontend | `F:\projects\ceksenet\frontend\` |
| Veritabanı | `F:\projects\ceksenet\database\` |
| Loglar | `F:\projects\ceksenet\logs\` |

### Production (Varsayılan Kurulum)

| Bileşen | Yol |
|---------|-----|
| Kurulum Dizini | `C:\Program Files\CekSenet\` |
| Veritabanı | `C:\Program Files\CekSenet\database\` |
| Yedekler | `C:\Program Files\CekSenet\database\backups\` |
| Loglar | `C:\Program Files\CekSenet\logs\` |

---

## 🔐 ŞİFRE DEĞİŞTİRME

### Kendi Şifresini Değiştirme

1. Giriş yap
2. Sağ üst → Profil → Şifre Değiştir
3. Eski şifre + Yeni şifre gir
4. Kaydet

### Admin: Başka Kullanıcının Şifresini Sıfırlama

1. Admin olarak giriş yap
2. Ayarlar → Kullanıcılar
3. Kullanıcıyı seç → Düzenle
4. "Şifre Sıfırla" → Yeni şifre gir
5. Kaydet

### Acil Durum: Admin Şifresini Unutma

Veritabanına doğrudan müdahale gerekir:

1. Uygulamayı durdur
2. SQLite browser ile `database/ceksenet.db` aç
3. `users` tablosunda admin satırını bul
4. Password alanını güncellemek için yeni bcrypt hash oluştur
5. Veya: Veritabanını sil, uygulama yeni kurulum sihirbazını gösterir

---

## 🔑 JWT TOKEN

| Özellik | Değer |
|---------|-------|
| Algoritma | HS256 |
| Geçerlilik | 24 saat |
| Secret | *(config'den okunur)* |

**Not:** Production'da JWT secret mutlaka değiştirilmeli!

---

## 🔒 GÜVENLİK AYARLARI

| Ayar | Değer |
|------|-------|
| Şifre Hash | bcrypt (12 rounds) |
| Rate Limiting | 5 hatalı giriş → 15 dk bekleme |
| Session Timeout | 24 saat |
| CORS | Sadece izinli origin'ler |

---

**Son Güncelleme:** 26 Aralık 2025
