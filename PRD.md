# Nefes Egzersizi Uygulaması — PRD

## 1. Proje Genel Bakışı

**Proje Adı:** Nefes Egzersizi (Breathe)

**Proje Tipi:** Mobil odaklı tek sayfalık web uygulaması (SPA)

**Çekirdek İşlev:** Animasyonlu nefes al/ver zamanlayıcı ile rehberli nefes egzersizleri. Kullanıcılar farklı nefes tekniklerini seçerek seans başlatabilir, geçmiş seanslarını ve istatistiklerini görüntüleyebilir.

**Hedef Kullanıcı:** Stres azaltma, uyku kalitesi iyileştirme, odaklanma ve genel wellness hedefleyen bireyler.

**Temel Değer Önerisi:** Sade, dikkat dağıtmayan, sakinleştirici bir arayüzle bilimsel nefes tekniklerini erişilebilir kılmak.

---

## 2. Hedef Platform

- **Birincil:** Mobil tarayıcı (iOS Safari, Android Chrome)
- **İkincil:** Masaüstü tarayıcı (tablet uyumlu)
- **PWA desteği:** Evet (offline çalışabilirlik için)
- **Native wrapper:** Gerekli değil (web tabanlı)

---

## 3. Fonksiyonel Gereksinimler

### 3.1 Nefes Teknikleri Modülü

| Teknik | Açıklama | Parametreler |
|--------|----------|--------------|
| **4-7-8 Tekniği** | Dr. Andrew Weil'in gevşeme tekniği | 4sn nefes al, 7sn tut, 8sn ver |
| **Kare Nefes (Box Breathing)** | 4 eşit parça | 4sn nefes al, 4sn tut, 4sn ver, 4sn tut |
| **Basit Nefes** | Rahatlatıcı temel teknik | 4sn nefes al, 4sn ver |
| **Rahatlatıcı Nefes** | Uzun exhale ağırlıklı | 4sn nefes al, 2sn tut, 6sn ver |

### 3.2 Egzersiz Oturumu

- **Egzersiz süresi seçimi:** 1, 3, 5, 10 dakika (varsayılan: 5 dakika)
- **Döngü sayısı:** Her teknik için belirlenen adımlar otomatik hesaplanır
- **Duraklatma/Devam:** Egzersiz sırasında geçici olarak duraklatılabilir
- **Erken bitirme:** Kullanıcı istediği zaman seansı sonlandırabilir
- **Tamamlama bildirimi:** Seans sonunda görsel + titreşim (mobil) bildirimi

### 3.3 Animasyon Sistemi

- **Ana animasyon:** Merkezi daire (nefes alırken büyür, verirken küçülür)
- **Animasyon türü:** CSS transitions + requestAnimationFrame
- **Renk geçişleri:** Her nefes aşamasına göre (al/tut/ver)
- **Yönergeler:** Ekranda "NEFES AL", "TUT", "VER" textleri

### 3.4 Oturum Geçmişi

- **Kaydedilen veriler:**
  - Tarih ve saat
  - Kullanılan teknik
  - Seans süresi
  - Tamamlanan döngü sayısı
- **Liste görünümü:** Ters kronolojik sıralama
- **Silme:** Tek tek veya tümünü temizle seçeneği
- **Depolama:** LocalStorage (yaygın tarayıcı desteği, kolay reset)

### 3.5 İstatistikler

| Metrik | Açıklama |
|--------|----------|
| **Toplam seans** | Tüm zamanların seans sayısı |
| **Toplam süre** | Dakika cinsinden toplam egzersiz süresi |
| **Bu hafta** | Haftalık seans sayısı ve süresi |
| **En çok kullanılan teknik** | Favori nefes tekniği |
| **Günlük seri** | Ardışık gün sayısı |
| **Ortalama seans süresi** | Hesaplanmış ortalama |

### 3.6 Ayarlar

- **Sesli yönerge:** Açık/Kapalı (nefes al/ver sesli komutları)
- **Titreşim:** Açık/Kapalı (seans tamamlama bildirimi)
- **Varsayılan süre:** 1/3/5/10 dakika seçimi
- **Veri yönetimi:** Tüm geçmişi temizle

---

## 4. Teknik Gereksinimler

### 4.1 Teknoloji Stack

| Katman | Teknoloji |
|--------|-----------|
| **Framework** | Vite + React 18 |
| **Dil** | TypeScript |
| **Stil** | CSS Modules + CSS Variables |
| **State yönetimi** | React useState/useReducer (basit state için yeterli) |
| **Animasyon** | CSS transitions + React state |
| **Veri depolama** | LocalStorage API |
| **PWA** | Vite PWA plugin |
| **Build** | vite build |

### 4.2 Proje Yapısı

```
src/
├── components/
│   ├── BreathCircle/        # Ana nefes animasyonu
│   ├── Timer/               # Geri sayım sayacı
│   ├── TechniqueSelector/   # Teknik seçim kartları
│   ├── SessionHistory/      # Geçmiş liste
│   ├── Stats/               # İstatistikler paneli
│   └── Settings/            # Ayarlar
├── hooks/
│   ├── useBreathTimer.ts    # Nefes zamanlayıcı mantığı
│   ├── useLocalStorage.ts   # LocalStorage hook
│   └── useStats.ts          # İstatistik hesaplama
├── types/
│   └── index.ts             # TypeScript tipleri
├── utils/
│   └── storage.ts           # Storage yardımcıları
├── styles/
│   ├── variables.css        # CSS değişkenleri (tema)
│   └── global.css           # Global stiller
├── App.tsx
└── main.tsx
```

### 4.3 Veri Modeli

```typescript
interface Session {
  id: string;           // UUID
  date: string;          // ISO 8601 timestamp
  technique: TechniqueType;
  durationMinutes: number;
  cyclesCompleted: number;
}

type TechniqueType = '4-7-8' | 'box' | 'simple' | 'relaxing';

interface Settings {
  soundEnabled: boolean;
  vibrationEnabled: boolean;
  defaultDuration: 1 | 3 | 5 | 10;
}
```

### 4.4 LocalStorage Şeması

| Key | Değer |
|-----|-------|
| `breathe_sessions` | `Session[]` JSON |
| `breathe_settings` | `Settings` JSON |

---

## 5. UI/UX Gereksinimleri

### 5.1 Tasarım Dili

**Tema:** Sakinleştirici koyu tema (dark mode only)

**Renk Paleti:**

| Renk | Hex | Kullanım |
|------|-----|----------|
| **Arka plan** | `#0D1117` | Ana zemin |
| **Yüzey** | `#161B22` | Kartlar, paneller |
| **Yüzey yükseltilmiş** | `#21262D` | Hover, active states |
| **Ana vurgu** | `#58A6FF` | CTA, aktif durumlar |
| **Nefes al** | `#7EE787` | İnhale aşaması |
| **Nefes ver** | `#F78166` | Exhale aşaması |
| **Tut** | `#D29922` | Hold aşaması |
| **Metin birincil** | `#E6EDF3` | Ana içerik |
| **Metin ikincil** | `#8B949E` | Yardımcı içerik |

**Tipografi:**
- Font: `Inter, system-ui, sans-serif`
- Başlık: 24-32px, font-weight 600
- Gövde: 16px, font-weight 400
- Küçük metin: 14px, font-weight 400

**Boşluk Sistemi:**
- Base unit: 4px
- Spacing scale: 4, 8, 12, 16, 24, 32, 48, 64px
- Border radius: 8px (kartlar), 50% (daireler)

### 5.2 Sayfa Akışı

```
[Ana Ekran] ─────► [Egzersiz Ekranı] ─────► [Tamamlama Ekranı]
     │                   │
     ▼                   ▼
[Geçmiş]           [Duraklatildi]
     │
     ▼
[İstatistikler]
     │
     ▼
[Ayarlar]
```

### 5.3 Etkileşim Kalıpları

- **Teknik seçimi:** Kart tıklama → animate on selection → otomatik devam
- **Egzersiz başlatma:** Büyük merkezi buton (60px çaplı, yuvarlak)
- **Duraklatma:** Egzersiz sırasında herhangi bir yere dokunma veya buton
- **Geri navigasyon:** Alt bar navigasyonu (Ana, Geçmiş, İstatistikler, Ayarlar)

### 5.4 Animasyon Spesifikasyonları

**Nefes Dairesi:**
- İnhale: scale 1 → 1.5, 4000ms, ease-in-out
- Hold: scale sabit, pulse efekti (opacity 0.8 → 1)
- Exhale: scale 1.5 → 1, 4000-8000ms (tekniğe göre), ease-in-out
- Renk geçişi: her aşamaya özel renk

**Geçişler:**
- Sayfa geçişleri: fade + slide, 300ms
- Kart hover: scale 1.02, 150ms
- Buton press: scale 0.95, 100ms

---

## 6. Non-Fonksiyonel Gereksinimler

### 6.1 Performans

- **İlk yükleme:** < 2 saniye (3G)
- **Lighthouse puanı:** > 90 (Performance)
- **Animasyon FPS:** 60fps hedef
- **Bundle boyutu:** < 100KB gzipped

### 6.2 Erişilebilirlik

- **Renk kontrastı:** WCAG AA uyumlu
- **Klavye navigasyonu:** Tab, Enter, Space desteği
- **Ekran okuyucu:** ARIA label desteği
- **Kısmi karanlık:** Sadece koyu tema (primary design)

### 6.3 Güvenlik

- **Veri:** Sadece localStorage, kişisel tanımlayıcı yok
- **CORS:** Web uygulaması, sunucu yok
- **PWA:** Service Worker ile offline çalışma

### 6.4 Tarayıcı Desteği

- Chrome 90+
- Safari 14+
- Firefox 90+
- Edge 90+

---

## 7. Oyun Detayları (Varsa)

Bu proje bir oyun değildir.

---

## 8. Boş Durumlar

| Durum | Görünüm |
|-------|---------|
| **Geçmiş boş** | İllüstrasyon + "Henüz seans yok. İlk nefesini almaya başla!" |
| **İstatistik boş** | "Daha fazla egzersiz yap daha fazla veri gör" |
| **Yükleme** | Spinner yerine skeleton loader |

---

## 9. Hata Durumları

| Senaryo | Davranış |
|---------|----------|
| **LocalStorage dolu** | Hata mesajı + temizleme önerisi |
| **Geçersiz veri** | Silent reset, varsayılan değerler |
| **Timer drift** | requestAnimationFrame + drift compensation |

---

## 10. PRD Çıktı Formatı

```
STATUS: done
REPO: $HOME/projects/nefes-egzersizi-uygulamasi
BRANCH: feature/prd
TECH_STACK: vite-react
DB_REQUIRED: none
```

---

## 11. Ekranlar (Screens)

| # | Ekran Adı | Tür | Açıklama |
|---|-----------|-----|----------|
| 1 | Ana Dashboard | dashboard | Nefes dairesi, teknik seçimi, hızlı başlat butonu |
| 2 | Egzersiz Ekranı | full-screen | Animasyonlu nefes, zamanlayıcı, duraklatma |
| 3 | Tamamlama Ekranı | overlay/modal | Seans özeti, döngü sayısı, tekrar seçeneği |
| 4 | Geçmiş Liste | list-view | Tüm seanslar, tarih/saat/teknik/süre |
| 5 | Seans Detay | detail-view | Tek seans detayları (opsiyonel) |
| 6 | İstatistikler | dashboard | Haftalık grafik, toplamlar, favori teknik |
| 7 | Ayarlar | form | Ses, titreşim, varsayılan süre |
| 8 | Boş Durum (Geçmiş) | empty-state | İllüstrasyon + CTA |
| 9 | Hata Durumu | error-state | Global error boundary |
| 10 | 404 Sayfa | error-page | Sayfa bulunamadı |

---

**Ekran Sayısı: 10**

---

_Doküman versiyonu: 1.0_
_Oluşturulma tarihi: 2026-03-29_
