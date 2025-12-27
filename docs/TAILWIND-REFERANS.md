# TAILWIND CSS 4 REFERANS DOKÜMANI

**Oluşturulma:** 25 Aralık 2025  
**Son Güncelleme:** 25 Aralık 2025 (v2.0 - Kapsamlı Güncelleme)  
**Amaç:** Claude'un Tailwind 4 ile kod yazarken okuması gereken referans  
**Ne Zaman Okunmalı:** Tailwind CSS kodu yazılacak her oturumda  
**Tailwind Sürümü:** v4.1.x (Aralık 2025)

---

## ⚠️ KRİTİK: TAİLWİND 4 KULLANIYORUZ

Bu projede **Tailwind CSS v4.x** kullanılıyor. v3 syntax'ı KULLANMA!

**En Son Kararlı Sürüm:** v4.1.18 (Aralık 2025)  
**Tarayıcı Desteği:** Safari 16.4+, Chrome 111+, Firefox 128+

---

## 📋 İÇİNDEKİLER

1. [v3 vs v4 Temel Farklar](#-v3-vs-v4-temel-farklar)
2. [@theme Direktifi ve Namespace'ler](#-theme-direktifi-ve-namespaceler)
3. [Container Queries](#-container-queries-dahili)
4. [Yeniden Adlandırılan Utility'ler](#-yeniden-adlandırılan-utilityler)
5. [Kaldırılan Utility'ler](#-kaldırılan-utilityler)
6. [Yeni Utility'ler](#-yeni-utilityler-v4)
7. [Yeni Variant'lar](#-yeni-variantlar-v4)
8. [Davranış Değişiklikleri](#-davranış-değişiklikleri)
9. [Proje Konfigürasyonu](#-bu-projenin-konfigürasyonu)
10. [Build Komutları](#-build-komutları)
11. [@utility Direktifi](#-utility-direktifi-custom-utilities)
12. [@source Direktifi](#-source-direktifi-kaynak-dosya-yönetimi)
13. [Diğer Direktifler](#-diğer-direktifler) (@reference, @plugin, @config, @variant)
14. [Yapılmaması Gerekenler](#-yapilmamasi-gerekenler)
15. [Yapılması Gerekenler](#-yapilmasi-gerekenler)

---

## 🔄 v3 vs v4 TEMEL FARKLAR

### 1. CSS Import

```css
/* ❌ v3 (ESKİ - KULLANMA) */
@tailwind base;
@tailwind components;
@tailwind utilities;

/* ✅ v4 (YENİ - KULLAN) */
@import "tailwindcss";
```

### 2. Konfigürasyon

```javascript
/* ❌ v3 (ESKİ - KULLANMA) */
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: { 500: '#183f6d' }
      }
    }
  }
}
```

```css
/* ✅ v4 (YENİ - KULLAN) */
/* Doğrudan CSS'te @theme direktifi ile */
@import "tailwindcss";

@theme {
  --color-primary-100: #9dd8e6;
  --color-primary-500: #183f6d;
  --font-archivo: "Archivo", sans-serif;
}
```

### 3. CLI Komutu

```bash
# ❌ v3 (ESKİ)
npx tailwindcss -i input.css -o output.css

# ✅ v4 (YENİ)
npx @tailwindcss/cli -i input.css -o output.css
```

### 4. Paket Kurulumu

```bash
# ❌ v3 (ESKİ)
npm install -D tailwindcss postcss autoprefixer

# ✅ v4 (YENİ)
npm install tailwindcss @tailwindcss/cli
# NOT: postcss ve autoprefixer artık dahili, ayrıca kurmaya gerek yok
```

### 5. Important Syntax

```html
<!-- ❌ v3 (ESKİ) -->
<div class="!flex !bg-red-500">

<!-- ✅ v4 (YENİ) - Sonunda olmalı -->
<div class="flex! bg-red-500!">
```

### 6. CSS Variable Arbitrary Values

```html
<!-- ❌ v3 (ESKİ) - Köşeli parantez -->
<div class="bg-[--brand-color]">

<!-- ✅ v4 (YENİ) - Normal parantez -->
<div class="bg-(--brand-color)">
```

### 7. Stacked Variants Sırası

```html
<!-- ❌ v3 (ESKİ) - Sağdan sola -->
<ul class="first:*:pt-0 last:*:pb-0">

<!-- ✅ v4 (YENİ) - Soldan sağa -->
<ul class="*:first:pt-0 *:last:pb-0">
```

---

## 🎨 @theme DİREKTİFİ VE NAMESPACE'LER

### Theme Variable Sistemi

v4'te tasarım token'ları `@theme` direktifi içinde tanımlanır. Her namespace, belirli utility class'ları oluşturur.

### Tüm Namespace'ler (18 adet)

| Namespace | Oluşturulan Utility'ler | Örnek Kullanım |
|-----------|------------------------|----------------|
| `--color-*` | `bg-*`, `text-*`, `border-*`, `fill-*`, `stroke-*` | `bg-primary-500` |
| `--font-*` | `font-*` (font family) | `font-archivo` |
| `--text-*` | `text-*` (font size) | `text-xl` |
| `--font-weight-*` | `font-*` (weight) | `font-bold` |
| `--tracking-*` | `tracking-*` (letter spacing) | `tracking-wide` |
| `--leading-*` | `leading-*` (line height) | `leading-tight` |
| `--breakpoint-*` | Responsive variants | `sm:`, `md:`, `lg:` |
| `--container-*` | Container queries + max-width | `@sm:`, `max-w-md` |
| `--spacing-*` | `p-*`, `m-*`, `gap-*`, `w-*`, `h-*` | `px-4`, `gap-6` |
| `--radius-*` | `rounded-*` | `rounded-lg` |
| `--shadow-*` | `shadow-*` | `shadow-md` |
| `--inset-shadow-*` | `inset-shadow-*` ⭐ | `inset-shadow-sm` |
| `--drop-shadow-*` | `drop-shadow-*` | `drop-shadow-md` |
| `--text-shadow-*` | `text-shadow-*` ⭐ | `text-shadow-sm` |
| `--blur-*` | `blur-*` | `blur-md` |
| `--perspective-*` | `perspective-*` ⭐ | `perspective-near` |
| `--aspect-*` | `aspect-*` | `aspect-video` |
| `--ease-*` | `ease-*` | `ease-out` |
| `--animate-*` | `animate-*` | `animate-spin` |

### @theme Örnek Kullanımı

```css
@import "tailwindcss";

@theme {
  /* RENKLER */
  --color-primary-100: #9dd8e6;
  --color-primary-200: #83b5dc;
  --color-primary-300: #739ba9;
  --color-primary-400: #4e7195;
  --color-primary-500: #183f6d;
  
  /* Tek renk (tonsuz) */
  --color-brand: #183f6d;
  
  /* Durum renkleri */
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-error: #EF4444;
  
  /* FONT */
  --font-archivo: "Archivo", sans-serif;
  
  /* SPACING (base unit) */
  --spacing: 0.25rem;  /* 4px - tüm spacing bu baz ile çarpılır */
  
  /* CUSTOM BREAKPOINT */
  --breakpoint-3xl: 120rem;
  
  /* CUSTOM CONTAINER SIZE */
  --container-8xl: 88rem;
  
  /* BORDER RADIUS */
  --radius-4xl: 2rem;
  
  /* ANIMATION */
  --animate-fade-in: fade-in 0.3s ease-out;
  
  @keyframes fade-in {
    from { opacity: 0; }
    to { opacity: 1; }
  }
}
```

### @theme inline - Değişken Referansı

CSS değişkenlerini referans olarak kullanırken `inline` kullan:

```css
/* Dark mode için değişken referansı */
:root {
  --my-primary: #183f6d;
}

.dark {
  --my-primary: #9dd8e6;
}

@theme inline {
  --color-primary: var(--my-primary);
}
```

**Neden inline?** Utility class değişken *değerini* kullanır, referansı değil. Bu sayede CSS variable inheritance düzgün çalışır.

### Varsayılan Tema'yı Override Etme

```css
@theme {
  /* Tek bir değeri override et */
  --breakpoint-sm: 30rem;
  
  /* Tüm namespace'i sıfırla */
  --color-*: initial;
  --color-white: #fff;
  --color-black: #000;
  --color-primary: #183f6d;
}
```

### Tüm Varsayılanları Kaldırma

```css
@theme {
  --*: initial;
  /* Şimdi sadece senin tanımladıkların var */
  --color-brand: #183f6d;
}
```

---

## 📦 CONTAINER QUERIES (DAHİLİ)

v4'te container queries artık dahili, **plugin gerekmez!**

### Temel Kullanım

```html
<!-- Container tanımla -->
<div class="@container">
  <!-- Container boyutuna göre responsive -->
  <div class="grid grid-cols-1 @sm:grid-cols-2 @lg:grid-cols-4">
    <!-- İçerik -->
  </div>
</div>
```

### Container Breakpoint'leri

| Class | Boyut |
|-------|-------|
| `@xs` | 20rem (320px) |
| `@sm` | 24rem (384px) |
| `@md` | 28rem (448px) |
| `@lg` | 32rem (512px) |
| `@xl` | 36rem (576px) |
| `@2xl` | 42rem (672px) |
| `@3xl` | 48rem (768px) |
| `@4xl` | 56rem (896px) |
| `@5xl` | 64rem (1024px) |
| `@6xl` | 72rem (1152px) |
| `@7xl` | 80rem (1280px) |

### Max-Width Container Queries

```html
<div class="@container">
  <!-- Container küçükken gizle -->
  <div class="@max-md:hidden">
    Sadece @md ve üzerinde görünür
  </div>
</div>
```

### Min-Max Range

```html
<div class="@container">
  <!-- Sadece @sm ile @lg arasında -->
  <div class="@sm:@max-lg:bg-blue-100">
    Orta boyutlarda mavi
  </div>
</div>
```

### Named Containers

```html
<div class="@container/sidebar">
  <nav class="@lg/sidebar:flex-col">
    <!-- sidebar container'ı @lg'ye ulaştığında -->
  </nav>
</div>
```

### Arbitrary Container Breakpoints

```html
<div class="@container">
  <div class="@[500px]:flex @[800px]:grid">
    <!-- Tam 500px ve 800px değerlerinde -->
  </div>
</div>
```

### Container Query Units

```html
<div class="@container">
  <!-- Container genişliğinin %50'si -->
  <div class="w-[50cqw]">
    Yarı genişlik
  </div>
  
  <!-- Container-based font size -->
  <h2 class="text-[5cqw]">
    Container'a göre ölçeklenen başlık
  </h2>
</div>
```

### Custom Container Sizes

```css
@theme {
  --container-xs: 320px;
  --container-3xl: 1600px;
}
```

---

## 📏 YENİDEN ADLANDIRILAN UTILITY'LER

**ÖNEMLİ:** v3'teki bazı utility'ler v4'te yeniden adlandırıldı. v3 isimleri çalışmaya devam eder ama farklı değerler verir!

### Shadow Scale Kayması

| v3 Adı | v4 Adı | Açıklama |
|--------|--------|----------|
| `shadow` | `shadow-sm` | Eski `shadow` artık `shadow-sm` |
| `shadow-sm` | `shadow-xs` | **DİKKAT:** Farklı görünüm! |
| `shadow-md` | `shadow-md` | Değişmedi |
| `shadow-lg` | `shadow-lg` | Değişmedi |
| `shadow-xl` | `shadow-xl` | Değişmedi |
| `shadow-2xl` | `shadow-2xl` | Değişmedi |
| - | `shadow-2xs` | ⭐ YENİ (en küçük) |

### Rounded Scale Kayması

| v3 Adı | v4 Adı |
|--------|--------|
| `rounded` | `rounded-sm` |
| `rounded-sm` | `rounded-xs` |

### Blur Scale Kayması

| v3 Adı | v4 Adı |
|--------|--------|
| `blur` | `blur-sm` |
| `blur-sm` | `blur-xs` |

### Drop Shadow Scale Kayması

| v3 Adı | v4 Adı |
|--------|--------|
| `drop-shadow` | `drop-shadow-sm` |
| `drop-shadow-sm` | `drop-shadow-xs` |

### Diğer Değişiklikler

| v3 Adı | v4 Adı | Not |
|--------|--------|-----|
| `outline-none` | `outline-hidden` | Semantik değişiklik |
| `ring` | `ring-3` | Default 3px → 1px |

---

## 🗑️ KALDIRILAN UTILITY'LER

### Opacity Modifier'lara Geçiş

```html
<!-- ❌ v3 (KALDIRILD) -->
<div class="bg-red-500 bg-opacity-50">
<div class="text-blue-500 text-opacity-75">
<div class="border-green-500 border-opacity-25">

<!-- ✅ v4 (YENİ) - Slash syntax -->
<div class="bg-red-500/50">
<div class="text-blue-500/75">
<div class="border-green-500/25">
```

### Diğer Kaldırılanlar

| Eski Utility | Yeni Karşılığı |
|--------------|----------------|
| `overflow-ellipsis` | `text-ellipsis` |
| `flex-shrink-*` | `shrink-*` |
| `flex-grow-*` | `grow-*` |
| `decoration-slice` | `box-decoration-slice` |
| `decoration-clone` | `box-decoration-clone` |

---

## ✨ YENİ UTILITY'LER (v4)

### 3D Transforms

```html
<!-- 3D rotasyon -->
<div class="rotate-x-45 rotate-y-30 rotate-z-15">

<!-- 3D translate -->
<div class="translate-z-10">

<!-- 3D scale -->
<div class="scale-z-150">

<!-- Perspective -->
<div class="perspective-500 perspective-origin-center">
  <div class="transform-3d rotate-y-45">
    3D dönmüş eleman
  </div>
</div>

<!-- Perspective presets -->
<div class="perspective-dramatic">  <!-- 100px -->
<div class="perspective-near">      <!-- 300px -->
<div class="perspective-normal">    <!-- 500px -->
<div class="perspective-midrange">  <!-- 800px -->
<div class="perspective-distant">   <!-- 1200px -->
```

### Text Shadows

```html
<h1 class="text-shadow-2xs">En hafif gölge</h1>
<h1 class="text-shadow-xs">Çok hafif gölge</h1>
<h1 class="text-shadow-sm">Hafif gölge</h1>
<h1 class="text-shadow-md">Orta gölge</h1>
<h1 class="text-shadow-lg">Güçlü gölge</h1>
```

### Inset Shadows

```html
<div class="inset-shadow-2xs">En hafif inset</div>
<div class="inset-shadow-xs">Hafif inset</div>
<div class="inset-shadow-sm">Normal inset</div>
```

### Inset Ring

```html
<button class="inset-ring inset-ring-blue-500">
  İç çerçeveli buton
</button>
```

### Field Sizing (Auto-resize Textarea)

```html
<!-- JavaScript olmadan auto-resize! -->
<textarea class="field-sizing-content">
  İçeriğe göre otomatik boyutlanır
</textarea>

<textarea class="field-sizing-fixed">
  Sabit boyut
</textarea>
```

### Color Scheme (Dark Mode Scrollbar)

```html
<!-- Tarayıcı UI'ını dark mode'a çevir -->
<html class="color-scheme-dark">
  <!-- Artık scrollbar'lar da koyu -->
</html>

<html class="color-scheme-light">
  <!-- Açık tema UI -->
</html>
```

### Font Stretch (Variable Fonts)

```html
<p class="font-stretch-condensed">Sıkıştırılmış metin</p>
<p class="font-stretch-expanded">Genişletilmiş metin</p>
```

---

## 🆕 YENİ VARIANT'LAR (v4)

### @starting-style (Enter/Exit Transitions)

```html
<div class="starting:opacity-0 starting:scale-95 transition-all">
  Sayfaya girerken fade-in + scale
</div>
```

### not-* (Negative Matching)

```html
<div class="not-hover:opacity-50">
  Hover değilken yarı saydam
</div>

<div class="not-first:mt-4">
  İlk eleman hariç margin-top
</div>
```

### inert (Non-interactive Elements)

```html
<div inert class="inert:opacity-30 inert:pointer-events-none">
  Inert attribute'lu elemanlar
</div>
```

### nth-* (Nth Child Variants)

```html
<ul>
  <li class="nth-2:bg-blue-100">2. eleman</li>
  <li class="nth-odd:bg-gray-50">Tek numaralı</li>
  <li class="nth-even:bg-gray-100">Çift numaralı</li>
  <li class="nth-3n:font-bold">Her 3. eleman</li>
</ul>
```

### in-* (Group-like, group class olmadan)

```html
<!-- ❌ Eski yöntem -->
<div class="group">
  <span class="group-hover:text-blue-500">...</span>
</div>

<!-- ✅ Yeni yöntem -->
<div>
  <span class="in-hover:text-blue-500">...</span>
</div>
```

### popover-open (via open variant)

```html
<button popovertarget="menu">Menu</button>
<div popover id="menu" class="open:opacity-100 opacity-0">
  Açık popover
</div>
```

### descendant (* variant)

```html
<!-- Tüm child'ları hedefle -->
<div class="*:mb-4 *:rounded-lg">
  <div>Otomatik stil 1</div>
  <div>Otomatik stil 2</div>
</div>
```

---

## ⚡ DAVRANŞ DEĞİŞİKLİKLERİ

### Border Default Color

```css
/* v3: gray-200 */
/* v4: currentColor */
```

v3 davranışını korumak için:
```css
@layer base {
  *, ::after, ::before, ::backdrop, ::file-selector-button {
    border-color: var(--color-gray-200, currentColor);
  }
}
```

### Ring Default

```css
/* v3: 3px, blue-500 */
/* v4: 1px, currentColor */
```

### Placeholder Color

```css
/* v3: Configured gray-400 */
/* v4: Current text color at 50% opacity */
```

### Button Cursor

```css
/* v3: cursor-pointer */
/* v4: cursor-default (browser default) */
```

Pointer'ı korumak için:
```css
@layer base {
  button, [type="button"], [type="submit"], [type="reset"] {
    cursor: pointer;
  }
}
```

### Hover on Touch

v4'te hover, touch cihazlarda daha doğru çalışır. Touch'ta hover tetiklenmez.

### Transition + Outline

`transition` ve `transition-colors` artık `outline-color` da içeriyor.

---

## 🎯 BU PROJENİN KONFIGÜRASYONU

### Dosya Yapısı

```
themes/hibeportali/
├── assets/css/
│   ├── tailwind.css      # Input (tek CSS dosyası, @theme dahil)
│   └── style.css         # Output (derlenmiş)
├── package.json          # npm scripts
└── (tailwind.config.js YOK - v4'te gerekli değil)
└── (postcss.config.js YOK - v4'te dahili)
```

### tailwind.css (Input Dosyası)

```css
@import "tailwindcss";

@theme {
  /* Primary - Mavi Tonları (STYLE-GUIDE.md'den) */
  --color-primary-100: #9dd8e6;  /* Açık Turkuaz */
  --color-primary-200: #83b5dc;  /* Açık Mavi */
  --color-primary-300: #739ba9;  /* Gri-Mavi */
  --color-primary-400: #4e7195;  /* Orta Mavi */
  --color-primary-500: #183f6d;  /* Koyu Lacivert (ana renk) */
  
  /* Durum Renkleri */
  --color-success: #10B981;
  --color-warning: #F59E0B;
  --color-error: #EF4444;
  --color-info: #3B82F6;
  
  /* Font */
  --font-archivo: "Archivo", sans-serif;
  
  /* Custom Shadows (projeye özel) */
  --shadow-card: 0 2px 8px rgba(0,0,0,0.08);
}
```

---

## 📦 BUILD KOMUTLARI

### package.json

```json
{
  "scripts": {
    "build": "npx @tailwindcss/cli -i ./assets/css/tailwind.css -o ./assets/css/style.css --minify",
    "watch": "npx @tailwindcss/cli -i ./assets/css/tailwind.css -o ./assets/css/style.css --watch",
    "dev": "npx @tailwindcss/cli -i ./assets/css/tailwind.css -o ./assets/css/style.css"
  }
}
```

### Çalıştırma

```bash
# Development (watch mode)
npm run watch

# Production build (minified)
npm run build

# Tek seferlik dev build
npm run dev
```

---

## ⚠️ YAPILMAMASI GEREKENLER

1. ❌ `tailwind.config.js` dosyası oluşturma (v4'te CSS-first)
2. ❌ `@tailwind base/components/utilities` kullanma
3. ❌ `postcss.config.js` oluşturma (v4'te dahili)
4. ❌ `npx tailwindcss` komutu kullanma (`@tailwindcss/cli` kullan)
5. ❌ v3 utility isimlerini kullanma (shadow-sm → shadow-xs)
6. ❌ `bg-opacity-*`, `text-opacity-*` kullanma (slash syntax kullan)
7. ❌ `@tailwindcss/container-queries` plugin kurma (v4'te dahili)
8. ❌ Important için başa `!` koyma (sona koy: `flex!`)
9. ❌ CSS variable için köşeli parantez `[--var]` (parantez kullan: `(--var)`)
10. ❌ Stacked variants'ta sağdan sola yazma (soldan sağa yaz)

---

## ✅ YAPILMASI GEREKENLER

1. ✅ `@import "tailwindcss"` kullan
2. ✅ `@theme { }` ile renk/font/spacing tanımla
3. ✅ `npx @tailwindcss/cli` kullan
4. ✅ Sadece `tailwindcss` ve `@tailwindcss/cli` paketlerini kur
5. ✅ v4 utility isimlerini kullan (shadow-xs, rounded-xs)
6. ✅ Opacity için slash syntax: `bg-red-500/50`
7. ✅ Container queries için `@container` + `@sm:` kullan
8. ✅ Important için sonuna `!` koy
9. ✅ CSS variable için parantez: `bg-(--brand-color)`
10. ✅ 3D transforms, text-shadow gibi yeni utility'leri kullan

---

## 🔧 @utility DİREKTİFİ (Custom Utilities)

v3'teki `@layer utilities` yerine:

```css
/* ❌ v3 (ESKİ) */
@layer utilities {
  .tab-4 {
    tab-size: 4;
  }
}

/* ✅ v4 (YENİ) */
@utility tab-4 {
  tab-size: 4;
}
```

### Container Customization

```css
@utility container {
  margin-inline: auto;
  padding-inline: 2rem;
}
```

---

## 📂 @source DİREKTİFİ (Kaynak Dosya Yönetimi)

v4'te content array yerine otomatik algılama var, ama bazen manuel belirtmek gerekebilir.

### Ek Kaynak Ekleme

```css
@import "tailwindcss";

/* Ek klasörleri tara */
@source "../node_modules/flowbite";
@source "./custom-components/**/*.html";
```

### Kaynak Hariç Tutma (v4.1+)

```css
/* Legacy kodu hariç tut */
@source not "./src/legacy";
@source not "./vendor";
```

### Safelist (Her Zaman Üret)

```css
/* Dinamik class'lar için safelist */
@source inline("bg-red-500 bg-blue-500 bg-green-500");
@source inline("text-sm text-md text-lg");
```

### WordPress için Tipik Kullanım

```css
@import "tailwindcss";

/* WordPress tema dosyaları */
@source "./*.php";
@source "./templates/**/*.php";
@source "./template-parts/**/*.php";
@source "./inc/**/*.php";
```

---

## 📌 DİĞER DİREKTİFLER

### @reference (Vue/Svelte Style Blocks)

CSS Modules veya component style blocks'ta theme değişkenlerine erişim:

```vue
<template>
  <h1>Hello world!</h1>
</template>

<style>
  @reference "../../app.css";
  
  h1 {
    @apply text-2xl font-bold text-primary-500;
  }
</style>
```

### @plugin (Legacy JS Plugin)

Eski v3 plugin'lerini yüklemek için:

```css
@plugin "@tailwindcss/typography";
@plugin "@tailwindcss/forms";
```

**Not:** Bu compatibility için. Modern yaklaşım CSS dosyası import etmek.

### @config (Legacy Config)

Eski tailwind.config.js dosyasını yüklemek için:

```css
@config "./tailwind.config.js";
```

**Not:** Sadece geçiş döneminde kullan. Hedef: tamamen CSS-first.

### @variant (Variant Uygulama)

CSS içinde variant kullanmak için:

```css
@variant hover {
  .my-button {
    background-color: var(--color-primary-600);
  }
}
```

### @custom-variant (Custom Variant Oluşturma)

```css
@custom-variant theme-dark (&:where([data-theme="dark"], [data-theme="dark"] *));
```

Kullanım:
```html
<div class="theme-dark:bg-gray-900 theme-dark:text-white">
```

### prefix() (Class Prefix)

```css
@import "tailwindcss" prefix(tw);
```

Class'lar: `tw:flex`, `tw:bg-red-500`, `tw:hover:text-blue-500`

---

## 🔗 KAYNAKLAR

- **Tailwind CSS v4 Docs:** https://tailwindcss.com/docs
- **Theme Variables:** https://tailwindcss.com/docs/theme
- **Functions & Directives:** https://tailwindcss.com/docs/functions-and-directives
- **Upgrade Guide:** https://tailwindcss.com/docs/upgrade-guide
- **Container Queries:** https://tailwindcss.com/docs/container-queries

---

## 📝 HIZLI REFERANS KARTI

```
IMPORT:        @import "tailwindcss";
THEME:         @theme { --color-brand: #183f6d; }
SOURCE:        @source "./templates/**/*.php";
SAFELIST:      @source inline("bg-red-500 text-lg");
PLUGIN:        @plugin "@tailwindcss/typography";
UTILITY:       @utility my-class { ... }
REFERENCE:     @reference "../../app.css"; (Vue/Svelte)

CLI:           npx @tailwindcss/cli -i input.css -o output.css --watch
CONTAINER:     @container → @sm:grid-cols-2, @max-md:hidden
OPACITY:       bg-red-500/50 (slash syntax)
IMPORTANT:     flex! (sonunda)
CSS VAR:       bg-(--my-color) (parantez)

SHADOW:        shadow-2xs < shadow-xs < shadow-sm < shadow-md < shadow-lg
ROUNDED:       rounded-xs < rounded-sm < rounded-md < rounded-lg
3D:            rotate-x-45 perspective-500 transform-3d
TEXT-SHADOW:   text-shadow-sm, text-shadow-md, text-shadow-lg
```

---

**Son Güncelleme:** 25 Aralık 2025  
**Versiyon:** 2.1 (Kapsamlı Güncelleme - @source, @reference, @plugin, @config, @variant direktifleri eklendi)

*Bu doküman Tailwind CSS işi yapılacak her oturumda MUTLAKA okunmalıdır.*
