# INDEX - Doküman Yol Haritası

**Son Güncelleme:** 26 Aralık 2025  
**Amaç:** Hangi durumda hangi dokümanı okuyacağını bilmek

---

## 📚 TÜM DOKÜMANLAR

### 🔴 Ana Dokümanlar (Her Oturum Başında OKU)

1. **README.md** - Genel proje özeti
2. **INDEX.md** - Bu dosya (navigasyon)
3. **DURUM.md** - Dashboard (aktif task + son oturum özeti)
4. **tasks/TASKS-README.md** - Task sistemi açıklaması
5. **tasks/[AKTİF-TASK].md** - O an üzerinde çalışılan task

---

### 🟢 Detay Dokümanlar (İhtiyaca Göre)

**Teknik:**
- **docs/TECH-STACK.md** → Sunucu mimarisi, Node.js, SQLite, API yapısı
- **docs/DATABASE.md** → Veritabanı şeması, tablolar, migration, SQL
- **docs/CREDENTIALS.md** → Erişim bilgileri, şifreler
- **docs/DEPLOYMENT.md** → Kurulum sonrası, yedekleme, güncelleme, sorun giderme

**Tasarım:**
- **docs/DESIGN.md** → Tailwind UI Catalyst, renk paleti, componentler
- **docs/FEATURES.md** → Tüm özelliklerin detaylı açıklaması
- **docs/TAILWIND-REFERANS.md** → Tailwind CSS v4 referans (v3'ten farklar)

---

## 🎯 SENARYOLAR

### **SENARYO 1: Yeni Oturum Başlangıcı**
1. README.md
2. INDEX.md
3. DURUM.md (hangi task aktif?)
4. tasks/TASKS-README.md
5. tasks/[AKTİF-TASK].md

### **SENARYO 2: Backend Geliştirme**
- Ana dokümanlar
- **docs/TECH-STACK.md** (API yapısı)
- **docs/DATABASE.md** (tablo yapısı, migration)

### **SENARYO 3: Frontend Geliştirme**
- Ana dokümanlar
- **docs/DESIGN.md** (Catalyst componentler)
- **docs/FEATURES.md** (özellik detayları)
- **docs/TAILWIND-REFERANS.md** (CSS referans)

### **SENARYO 4: Veritabanı İşlemleri**
- **docs/DATABASE.md** (tablo yapısı, SQL, migration)
- **docs/CREDENTIALS.md** (bağlantı bilgileri)

### **SENARYO 5: Deployment / Operasyon**
- **docs/DEPLOYMENT.md** (kurulum, yedekleme, güncelleme)
- **docs/CREDENTIALS.md** (erişim bilgileri)

---

## 🗂️ DOKÜMAN HİYERARŞİSİ

```
CekSenet/
├── README.md ⭐ (Proje özeti)
├── INDEX.md ⭐ (Navigasyon)
├── DURUM.md ⭐ (Dashboard)
│
├── tasks/
│   ├── TASKS-README.md (Task sistemi)
│   ├── TASK-01-PROJE-KURULUM.md
│   ├── TASK-02-BACKEND-AUTH.md
│   ├── TASK-03-BACKEND-CARILER.md
│   ├── TASK-04-BACKEND-EVRAKLAR.md
│   ├── TASK-05-BACKEND-DASHBOARD.md
│   ├── TASK-06-FRONTEND-LAYOUT.md
│   ├── TASK-07-FRONTEND-DASHBOARD.md
│   ├── TASK-08-FRONTEND-EVRAKLAR.md
│   ├── TASK-09-FRONTEND-CARI-USER.md
│   ├── TASK-10-FRONTEND-RAPORLAR.md
│   ├── TASK-11-TEST-BUGFIX.md
│   ├── TASK-12-INSTALLER.md
│   └── archive/ (Tamamlanan task'ler)
│
└── docs/
    ├── TECH-STACK.md (Teknik mimari)
    ├── DATABASE.md (Veritabanı + migration)
    ├── CREDENTIALS.md (Erişim bilgileri)
    ├── DEPLOYMENT.md (Operasyon kılavuzu)
    ├── DESIGN.md (Catalyst + tasarım)
    ├── FEATURES.md (Özellikler)
    └── TAILWIND-REFERANS.md (CSS v4 referans)
```

---

## 📊 DOKÜMAN İSTATİSTİKLERİ

| Kategori | Dosya Sayısı |
|----------|--------------|
| Ana dokümanlar | 3 |
| Task dokümanları | 12 + README |
| Detay dokümanlar | 7 |

---

## 🔗 HIZLI ERİŞİM

**Dokümanlar:**
- F:\projects\ObsidianVault\Babam\CekSenet\

**Catalyst UI Kit:**
- F:\projects\catalyst-ui-kit\catalyst-ui-kit\

**Kaynak Kod:**
- F:\projects\ceksenet\

---

**Son Güncelleme:** 26 Aralık 2025

*Bu INDEX her oturum başında README ile birlikte okunmalıdır.*
