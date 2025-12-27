# Çek/Senet Takip Sistemi

**Proje Sahibi:** [Baba]  
**Geliştirici:** Ender Yalçınkaya  
**Başlangıç Tarihi:** 26 Aralık 2025  
**Doküman Versiyonu:** 2.0

---

## 📖 BU DOKÜMAN HAKKINDA

**README.md** projenin genel referans dokümanıdır. Her oturum başında **mutlaka** okunmalıdır.

**Güncel durum için:** `DURUM.md`  
**Detaylı bilgi için:** `docs/` klasörü  
**Task yönetimi için:** `tasks/` klasörü

---

## 🎯 PROJE ÖZETİ

### Bu Sistem Nedir?

Şirket içi çek ve senet takip sistemi:
- Çek/senet kayıt ve takibi
- Vade takibi ve uyarılar
- Durum yönetimi (portföy → tahsil akışı)
- Cari hesap yönetimi
- Raporlama ve Excel export

### Hedef Kitle

- Şirket içi kullanıcılar (birkaç kişi)
- Ofis PC + telefon erişimi
- Tek şirket, üyelik sistemi yok

### Erişim Modeli

```
                    ┌─────────────────┐
                    │   ANA PC        │
                    │   (Windows)     │
                    │   Statik IP     │
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
    ┌────▼────┐        ┌─────▼─────┐       ┌─────▼─────┐
    │ Ofis PC │        │  Telefon  │       │  Ev PC    │
    │ (LAN)   │        │ (Statik IP)│      │(Statik IP)│
    └─────────┘        └───────────┘       └───────────┘
```

---

## 🛠️ TEKNOLOJİ

| Katman | Teknoloji | Neden |
|--------|-----------|-------|
| **Backend** | Node.js + Express | Hızlı, cross-platform |
| **Database** | SQLite | Tek dosya, kurulum yok, kolay yedekleme |
| **Frontend** | React + Tailwind CSS | Modern, responsive, mobil uyumlu |
| **Auth** | JWT + bcrypt | Güvenli, session-less |
| **Paketleme** | Inno Setup | Profesyonel Windows installer |
| **Servis** | node-windows | Windows servisi olarak çalışır |

**Detaylar:** `docs/TECH-STACK.md`

---

## 📁 PROJE YAPISI

```
ceksenet/
├── package.json
├── src/
│   ├── server/                 # Backend (Node.js + Express)
│   │   ├── index.js
│   │   ├── routes/
│   │   ├── models/
│   │   └── middleware/
│   │
│   └── client/                 # Frontend (React + Tailwind)
│       ├── src/
│       │   ├── components/
│       │   ├── pages/
│       │   └── App.jsx
│       └── package.json
│
├── database/
│   └── ceksenet.db             # SQLite veritabanı
│
├── installer/
│   └── setup.iss               # Inno Setup script
│
└── docs/                       # Dokümantasyon
```

---

## 👥 KULLANICI ROLLERİ

| Rol | Yetkiler |
|-----|----------|
| **Admin** | Tüm işlemler + kullanıcı yönetimi |
| **Normal** | Görüntüleme + ekleme/düzenleme (kullanıcı yönetimi hariç) |

---

## 🗄️ VERİTABANI YAPISI

**4 Ana Tablo:**

1. **users** - Kullanıcı yönetimi (admin/normal)
2. **cariler** - Cari hesaplar (müşteri/tedarikçi)
3. **evraklar** - Çek ve senetler
4. **evrak_hareketleri** - Durum değişiklik geçmişi (yeni!)

**Detaylar:** `docs/DATABASE.md`

---

## ⚡ ÖZELLİKLER

### Temel Özellikler (Doküman1'den)
- ✅ Kullanıcı girişi (JWT)
- ✅ Dashboard (özet istatistikler)
- ✅ Evrak ekleme/düzenleme/listeleme
- ✅ Cari hesap yönetimi
- ✅ Durum takibi (5 durum)
- ✅ Excel export
- ✅ Tarih aralığı raporları

### İyileştirmeler (Yeni)
- ✅ Responsive tasarım (mobil uyumlu)
- ✅ Evrak arama ve filtreleme
- ✅ Vade uyarıları (bugün, bu hafta, gecikmiş)
- ✅ Evrak hareket geçmişi
- ✅ Toplu durum güncelleme
- ✅ Dashboard grafikleri

**Detaylar:** `docs/FEATURES.md`

---

## 🎨 TASARIM

**UI Kit:** Tailwind UI  
**Renk Paleti:** Belirlenmedi (kullanıcı seçecek)  
**Layout:** Sidebar + Main content

**Detaylar:** `docs/DESIGN.md`

---

## 📦 KURULUM (Son Kullanıcı)

```
1. CekSenet-Setup.exe dosyasını al (USB veya dosya paylaşımı)
2. Çalıştır → Next → Next → Install → Finish
3. Tarayıcı açılır: http://localhost:7474
4. Admin hesabı oluştur
5. Diğer kullanıcıları ekle
6. Dış erişim için: http://[STATIK-IP]:7474
```

---

## 📤 DAĞITIM

**Kaynak Kod:** GitHub (private repo - versiyon takibi için)  
**Son Kullanıcıya:** Elden EXE verme (USB / WeTransfer)

Küçük ölçekli aile içi proje olduğu için son kullanıcıya doğrudan dosya paylaşımı tercih edildi. GitHub sadece geliştirme ve versiyon takibi için kullanılacak.

---

## 🔗 HIZLI ERİŞİM

**Dokümanlar:**
- F:\projects\ObsidianVault\Babam\CekSenet\

**Kaynak Kod:**
- F:\projects\ceksenet\
- GitHub: *(repo oluşturulacak)*

---

**Son Güncelleme:** 26 Aralık 2025  
**Versiyon:** 2.0 (Teknoloji kararları)

*Bu README her oturum başında mutlaka okunmalıdır.*
