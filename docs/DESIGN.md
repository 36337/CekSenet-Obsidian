# Tasarım Kılavuzu (Design Guide)

**Son Güncelleme:** 26 Aralık 2025  
**UI Kit:** Tailwind UI Catalyst

---

## 🎨 CATALYST UI KIT

### Genel Bilgi

Catalyst, Tailwind CSS ekibinin oluşturduğu profesyonel React component kiti.

**Lokasyon:** `F:\projects\catalyst-ui-kit\catalyst-ui-kit\`

**Teknolojiler:**
- React 19
- Tailwind CSS v4
- Headless UI (@headlessui/react)
- Heroicons (@heroicons/react)
- Motion (Framer Motion)
- clsx

### Bağımlılıklar

```bash
npm install @headlessui/react @heroicons/react clsx motion
npm install tailwindcss@latest
```

---

## 📦 CATALYST COMPONENTLERİ

### Layout
| Component | Dosya | Kullanım |
|-----------|-------|----------|
| SidebarLayout | sidebar-layout.tsx | Ana uygulama layout'u |
| Sidebar | sidebar.tsx | Sol menü |
| Navbar | navbar.tsx | Üst navbar |
| StackedLayout | stacked-layout.tsx | Alternatif layout |
| AuthLayout | auth-layout.tsx | Login sayfası layout'u |

### Form
| Component | Dosya | Kullanım |
|-----------|-------|----------|
| Input | input.tsx | Text input |
| Select | select.tsx | Dropdown select |
| Textarea | textarea.tsx | Çok satırlı input |
| Checkbox | checkbox.tsx | Checkbox |
| Radio | radio.tsx | Radio button |
| Switch | switch.tsx | Toggle switch |
| Combobox | combobox.tsx | Aranabilir select |
| Listbox | listbox.tsx | Liste seçici |
| Fieldset | fieldset.tsx | Form grupları |

### UI
| Component | Dosya | Kullanım |
|-----------|-------|----------|
| Button | button.tsx | Butonlar |
| Badge | badge.tsx | Durum etiketleri |
| Avatar | avatar.tsx | Kullanıcı avatarı |
| Alert | alert.tsx | Uyarı mesajları |
| Dialog | dialog.tsx | Modal dialog |
| Dropdown | dropdown.tsx | Dropdown menü |
| Pagination | pagination.tsx | Sayfalama |
| Table | table.tsx | Tablo |

### Typography
| Component | Dosya | Kullanım |
|-----------|-------|----------|
| Heading | heading.tsx | Başlıklar (H1, H2...) |
| Text | text.tsx | Paragraf metni |
| Link | link.tsx | Linkler |
| DescriptionList | description-list.tsx | Key-value listesi |
| Divider | divider.tsx | Ayırıcı çizgi |

---

## 🎯 KULLANIM ÖRNEKLERİ

### Sidebar Menü

```tsx
import { Sidebar, SidebarBody, SidebarItem, SidebarLabel, SidebarSection } from '@/components/sidebar'
import { HomeIcon, DocumentIcon, UsersIcon } from '@heroicons/react/20/solid'

<Sidebar>
  <SidebarBody>
    <SidebarSection>
      <SidebarItem href="/" current={pathname === '/'}>
        <HomeIcon />
        <SidebarLabel>Dashboard</SidebarLabel>
      </SidebarItem>
      <SidebarItem href="/evraklar" current={pathname.startsWith('/evraklar')}>
        <DocumentIcon />
        <SidebarLabel>Evraklar</SidebarLabel>
      </SidebarItem>
      <SidebarItem href="/cariler" current={pathname.startsWith('/cariler')}>
        <UsersIcon />
        <SidebarLabel>Cariler</SidebarLabel>
      </SidebarItem>
    </SidebarSection>
  </SidebarBody>
</Sidebar>
```

### Form

```tsx
import { Field, FieldGroup, Label } from '@/components/fieldset'
import { Input } from '@/components/input'
import { Select } from '@/components/select'
import { Button } from '@/components/button'

<form>
  <FieldGroup>
    <Field>
      <Label>Evrak No</Label>
      <Input name="evrak_no" required />
    </Field>
    <Field>
      <Label>Evrak Tipi</Label>
      <Select name="evrak_tipi">
        <option value="cek">Çek</option>
        <option value="senet">Senet</option>
      </Select>
    </Field>
    <Button type="submit">Kaydet</Button>
  </FieldGroup>
</form>
```

### Tablo

```tsx
import { Table, TableHead, TableBody, TableRow, TableHeader, TableCell } from '@/components/table'
import { Badge } from '@/components/badge'

<Table>
  <TableHead>
    <TableRow>
      <TableHeader>Evrak No</TableHeader>
      <TableHeader>Tutar</TableHeader>
      <TableHeader>Durum</TableHeader>
    </TableRow>
  </TableHead>
  <TableBody>
    {evraklar.map((evrak) => (
      <TableRow key={evrak.id} href={`/evraklar/${evrak.id}`}>
        <TableCell>{evrak.evrak_no}</TableCell>
        <TableCell>{formatCurrency(evrak.tutar)}</TableCell>
        <TableCell>
          <Badge color={getDurumColor(evrak.durum)}>
            {evrak.durum}
          </Badge>
        </TableCell>
      </TableRow>
    ))}
  </TableBody>
</Table>
```

### İstatistik Kartları (Dashboard)

```tsx
// Stat component'i demo'dan
function Stat({ title, value, change }) {
  return (
    <div className="rounded-lg bg-white p-6 shadow">
      <div className="text-sm text-zinc-500">{title}</div>
      <div className="mt-2 text-3xl font-semibold">{value}</div>
      <div className={`mt-2 text-sm ${change.startsWith('+') ? 'text-green-600' : 'text-red-600'}`}>
        {change}
      </div>
    </div>
  )
}
```

---

## 🎨 RENK PALETİ

### Durum Badge Renkleri

Catalyst Badge component'i `color` prop'u alır:

| Durum | Color | Görünüm |
|-------|-------|---------|
| Portföy | `blue` | Mavi badge |
| Bankada | `purple` | Mor badge |
| Ciro | `orange` | Turuncu badge |
| Tahsil | `green` | Yeşil badge |
| Karşılıksız | `red` | Kırmızı badge |

```tsx
<Badge color="blue">Portföy</Badge>
<Badge color="green">Tahsil</Badge>
<Badge color="red">Karşılıksız</Badge>
```

### Evrak Tipi Badge

| Tip | Color |
|-----|-------|
| Çek | `cyan` |
| Senet | `amber` |

### Cari Tipi Badge

| Tip | Color |
|-----|-------|
| Müşteri | `green` |
| Tedarikçi | `blue` |

---

## 📐 LAYOUT YAPISI

### Uygulama Layout'u

```
┌─────────────────────────────────────────────────────┐
│ ┌──────┐ ┌────────────────────────────────────────┐ │
│ │      │ │ Navbar (user dropdown)                 │ │
│ │ Side │ ├────────────────────────────────────────┤ │
│ │ bar  │ │                                        │ │
│ │      │ │           Main Content                 │ │
│ │ 256px│ │           (children)                   │ │
│ │      │ │                                        │ │
│ └──────┘ └────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────┘
```

### Mobil Layout

```
┌─────────────────────┐
│ Navbar (hamburger)  │
├─────────────────────┤
│                     │
│   Main Content      │
│   (full width)      │
│                     │
│                     │
└─────────────────────┘
   ↓ Hamburger click
┌─────────────────────┐
│ Sidebar (overlay)   │
│ ├── Dashboard       │
│ ├── Evraklar        │
│ ├── Cariler         │
│ └── Ayarlar         │
└─────────────────────┘
```

---

## 🔤 HEROICONS

Catalyst, Heroicons kullanır. İki boyut var:

```tsx
// 20px (solid) - Sidebar, menü ikonları
import { HomeIcon, DocumentIcon } from '@heroicons/react/20/solid'

// 16px (solid) - Dropdown, küçük ikonlar
import { ChevronDownIcon } from '@heroicons/react/16/solid'

// 24px (outline) - Büyük ikonlar (opsiyonel)
import { HomeIcon } from '@heroicons/react/24/outline'
```

### Kullanılacak İkonlar

| Sayfa | İkon | Import |
|-------|------|--------|
| Dashboard | HomeIcon | @heroicons/react/20/solid |
| Evraklar | DocumentTextIcon | @heroicons/react/20/solid |
| Cariler | UsersIcon | @heroicons/react/20/solid |
| Raporlar | ChartBarIcon | @heroicons/react/20/solid |
| Ayarlar | Cog6ToothIcon | @heroicons/react/20/solid |
| Kullanıcılar | UserGroupIcon | @heroicons/react/20/solid |
| Çıkış | ArrowRightStartOnRectangleIcon | @heroicons/react/16/solid |

---

## 📁 COMPONENT KOPYALAMA

Catalyst componentlerini projemize kopyalayacağız:

**Kaynak:** `F:\projects\catalyst-ui-kit\catalyst-ui-kit\typescript\`

**Hedef:** `[PROJE]/src/components/ui/`

**Kopyalanacak Dosyalar:**
- alert.tsx
- avatar.tsx
- badge.tsx
- button.tsx
- checkbox.tsx
- dialog.tsx
- dropdown.tsx
- fieldset.tsx
- heading.tsx
- input.tsx
- link.tsx
- navbar.tsx
- pagination.tsx
- select.tsx
- sidebar.tsx
- sidebar-layout.tsx
- table.tsx
- text.tsx
- textarea.tsx

---

## 📚 DEMO REFERANS

Örnek kullanımlar için demo projesine bakılabilir:

**Demo Lokasyon:** `F:\projects\catalyst-ui-kit\catalyst-ui-kit\demo\typescript\`

**Önemli Dosyalar:**
- `src/app/(app)/application-layout.tsx` - Sidebar layout örneği
- `src/app/(app)/page.tsx` - Dashboard örneği
- `src/app/(app)/events/page.tsx` - Liste sayfası örneği
- `src/app/(auth)/login/page.tsx` - Login sayfası örneği

---

**Son Güncelleme:** 26 Aralık 2025
