# TASKS SİSTEMİ - Kullanım Kılavuzu

**Oluşturulma:** 26 Aralık 2025  
**Amaç:** Task-based proje yönetimi sistemi açıklaması

---

## 📖 BU DOKÜMAN HAKKINDA

**TASKS-README.md**, task sisteminin nasıl çalıştığını açıklar.

---

## 🎯 TASK SİSTEMİ NEDİR?

Proje işleri **bağımsız, modüler task'lara** bölündü. Her task kendi dokümanında detaylı olarak planlanır ve takip edilir.

**Avantajları:**
- ✅ Context izolasyonu (sadece ilgili task okunur)
- ✅ Modüler yapı (task'lar bağımsız çalışır)
- ✅ Temiz arşiv (tamamlanan task → archive)
- ✅ Takip kolaylığı

---

## 📁 KLASÖR YAPISI

```
tasks/
├── TASKS-README.md      ← Bu dosya (sistem açıklaması)
├── TASK-XX-ISIM.md      ← Aktif/bekleyen task'lar
└── archive/             ← Tamamlanan task'lar
```

**Güncel task listesi için:** `DURUM.md` dosyasına bak.

---

## 📄 TASK DOKÜMAN YAPISI

Her task dokümanı şu bölümleri içerir:

### 1. Header (Metadata)
```
Durum, öncelik, tarih, süre, bağımlılıklar
```

### 2. Hedef
```
2-3 cümle: Bu task neyi başarmaya çalışıyor?
```

### 3. Alt Görevler
```
A. Grup İsmi
  A.1 Alt Görev İsmi
    - Ne yapılacak (checkbox'lı adımlar)
    - Test kriterleri
```

### 4. Oturum Kayıtları
```
Her oturum sonunda:
- Yapılanlar
- Sorunlar
- Kararlar
- Sonraki hedef
```

### 5. Tamamlanma Kriterleri
```
Task ne zaman tamamlanmış sayılır?
```

---

## 🔄 ÇALIŞMA PROTOKOLÜ

### **Her Oturum Başında:**

1. README.md oku
2. INDEX.md oku
3. DURUM.md oku (hangi task aktif?)
4. tasks/[AKTİF-TASK].md oku

### **Oturum Sonunda:**

1. **Task Dokümanını Güncelle:**
   - Yapılanlar
   - Sorunlar ve çözümler
   - Sonraki hedef

2. **DURUM.md'yi Güncelle:**
   - Kısa özet (3-4 cümle)
   - Aktif task pointer

---

## 📊 DURUM KODLARI

**Task Durumları:**
- 🟡 **Aktif**: Şu anda üzerinde çalışılıyor
- ✅ **Tamamlandı**: Tüm alt görevler bitmiş
- ⏳ **Bekliyor**: Henüz başlanmadı
- 🔴 **Bloke**: Bağımlılık sebebiyle bekliyor

**Öncelik Kodları:**
- 🔥 **Kritik**: En öncelikli
- ⚡ **Yüksek**: Önemli
- 📌 **Orta**: Normal
- 💡 **Düşük**: Nice-to-have

---

## 🎯 BAŞLANGIÇ TALİMATI

**Yeni bir oturuma başlarken:**

```
"DURUM.md oku, aktif task dokümanını oku ve 
kaldığımız yerden devam et."
```

---

**Son Güncelleme:** 26 Aralık 2025
