# CEN Logistics Group — Site Mimarisi (Single Source of Truth)

> Bu doküman, projenin **tek doğruluk kaynağıdır**. Yeni sayfa, bileşen veya
> özellik eklerken önce bu dosya referans alınır. Burada tanımlanmayan bir
> mimari karar verilmeden önce bu doküman güncellenmeli ve onaylanmalıdır.
> Doküman, projenin mevcut (üretimde olan) halinin analiz edilmesiyle
> oluşturulmuştur; yani burada yazılanlar zaten kodda uygulanmış durumdadır.

---

## 1. Proje Özeti

- **Marka:** CEN LOGISTICS GROUP (Caucasus Europe Network)
- **Grup şirketleri:** CME Intermodal&Shipping, ELS Intermodal Logistics, CEN Logistics Georgia
- **Sektör:** Uluslararası lojistik (denizyolu, demiryolu, karayolu, multimodal, liman handling, tahmil tahliye)
- **Dil:** Varsayılan Türkçe (`lang="tr"`); sağ üstteki dil seçici ile
  İngilizce ve Rusça'ya anında geçiş yapılabilir (bkz. §14 Çok Dilli
  Altyapı — i18n)
- **Site tipi:** Çok sayfalı (multi-page), statik, kurumsal tanıtım + teklif toplama sitesi
- **Kaynak içerik:** `CME e-katalog.pdf` — hizmet açıklamaları, misyon/vizyon,
  değerler ve iletişim bilgileri buradan alınmıştır; yeni metin yazarken bu
  PDF'teki ifadelerle çelişilmemelidir.

---

## 2. Teknoloji Yığını

| Katman | Teknoloji | Not |
|---|---|---|
| Markup | Statik HTML5 | Build aracı, SSR, SPA yok |
| Stil | Tek dosya `css/style.css` (vanilla CSS) | Sass/PostCSS/Tailwind kullanılmaz |
| Script | Tek dosya `js/script.js` (vanilla JS, IIFE) | Framework/kütüphane yok (React, jQuery vb. YOK) |
| Fontlar | Google Fonts: **Inter** (tek aile — hem başlık hem gövde, ağırlık 400/500/600/700/800) | `<link>` ile `preconnect` + `display=swap`; her sayfanın `<head>`'inde birebir aynı `<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap">` satırı bulunur |
| İkonlar | Inline SVG (stroke tabanlı, `stroke-width="2"`) | İkon kütüphanesi (Font Awesome/Lucide paketi) yüklenmez, gerekli ikonlar elle inline SVG olarak yazılır |
| Harita | Elle yazılmış SVG + `js/script.js` içindeki `COUNTRY_DATA` / `initWorldMap()` | Harici harita kütüphanesi (Leaflet, Mapbox) kullanılmaz |
| Form gönderimi | `mailto:` tabanlı (`data-mailto`, `data-subject` attribute'ları) | Backend/API yok |
| Görseller | `assets/images/*.jpg|png` | WebP dönüşümü henüz yapılmadı (bkz. §11 Bilinen Teknik Borç) |

**Kritik kural:** Bu yığına yeni bir framework, CSS önişlemcisi veya JS
kütüphanesi eklenmez. İhtiyaç doğarsa önce bu dokümanı güncelleyip onay almak
gerekir.

---

## 3. Klasör Yapısı

```
CME e-katalog/
├── site-architecture.md        ← bu dosya (SSOT)
├── CME e-katalog.pdf           ← kaynak içerik referansı
├── index.html                  ← Anasayfa
├── hakkimizda.html
├── hizmetler.html               ← hizmet listeleme (hub sayfa)
├── hizmet-deniz-tasimaciligi.html
├── hizmet-demiryolu.html
├── hizmet-karayolu.html
├── hizmet-multimodal.html
├── hizmet-liman-handling.html
├── hizmet-tahmil-tahliye.html
├── faaliyet-bolgeleri.html
├── tasima-sureci.html
├── teklif-al.html
├── iletisim.html
├── sss.html
├── blog.html                   ← blog listeleme (hub sayfa)
├── blog-cen-logistics-iran-koridoru.html
├── blog-btk-hatti-orta-koridor.html
├── blog-konteyner-tasimaciligi-rehberi.html
├── blog-yeni-web-sitesi-duyurusu.html
├── kvkk.html
├── gizlilik-politikasi.html
├── cerez-politikasi.html
├── css/
│   └── style.css               ← TEK stil dosyası, tüm sayfalar ortak kullanır
├── js/
│   └── script.js               ← TEK script dosyası, tüm sayfalar ortak kullanır
└── assets/
    └── images/
        ├── logo-emblem.png     ← favicon + navbar amblemi
        ├── logo-full.png       ← tam logo (kurumsal kullanım)
        ├── icon-home.png
        ├── icon-innovation.png ← "Değerlerimiz" bölümünde kullanılır
        ├── icon-reliability.png← "Değerlerimiz" bölümünde kullanılır
        ├── aktau-marine-terminal.jpg
        ├── cargo-lashing-truck.jpg
        ├── container-hopper.jpg
        ├── crane-lifting.jpg
        ├── grain-silos-port.jpg
        ├── iso-tank-containers.jpg
        ├── port-cranes-containers.jpg
        ├── rail-container-cma.jpg
        ├── rail-pipes.jpg
        ├── ship-containers.jpg
        ├── vessel-deck-sea.jpg    ← hero arka plan görseli (index.html)
        ├── warehouse-storage.jpg
        └── wind-turbine-blade-crane.jpg
```

**Kurallar:**
- Yeni bir sayfa **her zaman kök dizinde** düz `.html` dosyası olarak
  oluşturulur (alt klasör açılmaz: `pages/`, `views/` vb. YOK).
- Yeni CSS her zaman `css/style.css`'e eklenir; ikinci bir `.css` dosyası
  açılmaz.
- Yeni JS her zaman `js/script.js`'e eklenir; ikinci bir `.js` dosyası
  açılmaz.
- Yeni görseller `assets/images/` altına, betimleyici kebab-case isimle
  eklenir (örn. `port-cranes-containers.jpg`).

---

## 4. Sayfa Mimarisi ve Site Haritası

### 4.1 Sayfa Envanteri

| # | Dosya | Sayfa | Üst Menüde mi? | Mobil Menüde mi? | Footer'da mı? |
|---|---|---|---|---|---|
| 1 | `index.html` | Anasayfa | ✅ | ✅ | ✅ (brand) |
| 2 | `hakkimizda.html` | Hakkımızda | ✅ | ✅ | ✅ (Hızlı Menü) |
| 3 | `hizmetler.html` | Hizmetlerimiz (hub) | ✅ | ✅ | — |
| 4 | `hizmet-deniz-tasimaciligi.html` | Deniz Taşımacılığı (detay) | — | — | ✅ (Hizmetler) |
| 5 | `hizmet-demiryolu.html` | Demiryolu (detay) | — | — | ✅ (Hizmetler) |
| 6 | `hizmet-karayolu.html` | Karayolu (detay) | — | — | ✅ (Hizmetler) |
| 7 | `hizmet-multimodal.html` | Multimodal Taşımacılık (detay) | — | — | ✅ (Hizmetler) |
| 8 | `hizmet-liman-handling.html` | Liman Handling (detay) | — | — | ✅ (Hizmetler) |
| 9 | `hizmet-tahmil-tahliye.html` | Tahmil Tahliye (detay) | — | — | ✅ (Hizmetler) |
| 10 | `faaliyet-bolgeleri.html` | Faaliyet Bölgeleri | ✅ | ✅ | ✅ (Hızlı Menü) |
| 11 | `tasima-sureci.html` | Taşıma Süreci | — | ✅ | ✅ (Hızlı Menü) |
| 12 | `teklif-al.html` | Teklif Al | ✅ | ✅ | ✅ (İletişim) |
| 13 | `blog.html` | Blog (hub) | — | ✅ | ✅ (Hızlı Menü) |
| 14–17 | `blog-*.html` | Blog makale detayları | — | — | related-posts içinde çapraz link |
| 18 | `sss.html` | Sıkça Sorulan Sorular | — | ✅ | ✅ (Hızlı Menü) |
| 19 | `iletisim.html` | İletişim | ✅ | ✅ | ✅ (İletişim) |
| 20 | `kvkk.html` | KVKK Aydınlatma Metni | — | — | ✅ (footer-bottom) |
| 21 | `gizlilik-politikasi.html` | Gizlilik Politikası | — | — | ✅ (footer-bottom) |
| 22 | `cerez-politikasi.html` | Çerez Politikası | — | — | ✅ (footer-bottom) |

> **Not:** Üst (desktop) navbar bilinçli olarak **6 öğeyle sınırlıdır**
> (Anasayfa, Hakkımızda, Hizmetlerimiz, Faaliyet Bölgeleri, Teklif Al,
> İletişim). Taşıma Süreci, Blog ve SSS gibi ikincil sayfalar sadece
> **mobil menüde** ve **footer "Hızlı Menü"**de yer alır. Bu ayrım korunmalıdır;
> masaüstü navbar'a yeni üst-seviye öğe eklemeden önce bu dengeyi göz önünde
> bulundur.

### 4.2 Hizmet Detay Sayfaları Kalıbı

`hizmetler.html` bir **hub sayfasıdır**; 6 hizmetin kartlarını listeler ve
her biri kendi detay sayfasına (`hizmet-*.html`) bağlanır. Yedinci bir hizmet
eklenecekse:
1. `hizmetler.html` içindeki `.service-card-grid`'e yeni kart eklenir.
2. `index.html` içindeki aynı gride de (anasayfada da gösteriliyor) eklenir.
3. Yeni `hizmet-<slug>.html` dosyası, §5'teki hizmet detay şablonu ile oluşturulur.
4. Tüm sayfaların **footer "Hizmetler" listesine** ve **navigasyona** (gerekliyse) eklenir.
5. `teklif-al.html` formundaki "Taşıma Türü" `<select>` seçeneklerine eklenir
   (bkz. §7.8).

### 4.3 Blog Kalıbı

`blog.html` hub sayfasıdır; kategori filtre çipleri (`data-filter`) ile
`.blog-card-grid` içindeki kartları (`data-category`) filtreler. Kategoriler:
`sirket` (Şirket Haberleri), `sektor` (Lojistik Sektörü), `rehber`
(Taşımacılık Rehberleri), `duyuru` (Duyurular). Yeni bir kategori
eklenmeden önce mevcut 4 kategoriden birine uyup uymadığı kontrol edilir.

Yeni bir blog yazısı eklerken:
1. `blog-<slug>.html` dosyası §5'teki blog makale şablonuyla oluşturulur.
2. `blog.html`'deki `.blog-card-grid`'e yeni kart eklenir (`data-category`
   doğru atanmalı).
3. İlgili makalenin `.related-posts` bölümüne en az 2-3 çapraz link eklenir.

---

## 5. Sayfa İskeleti (Page Skeleton) — Zorunlu Şablon

**Her `.html` dosyası** aşağıdaki iskeleti birebir izler. Bu proje bir include/
templating mekanizması **kullanmaz**; header, mobil menü ve footer her
dosyada elle kopyalanır. Bu nedenle header/footer içeriği değiştiğinde
**tüm sayfalarda senkron güncellenmelidir** (bkz. §12 Bakım Kuralları).

```html
<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>{Sayfa Başlığı} | CEN Logistics Group</title>
<meta name="description" content="{150-160 karakter özet}">
<link rel="icon" type="image/png" href="assets/images/logo-emblem.png">
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
<link rel="stylesheet" href="css/style.css">
</head>
<body>

<!-- 1. Sayfa geçiş perdesi (tüm sayfalarda birebir aynı) -->
<div class="page-veil" id="pageVeil"><img class="veil-logo" src="assets/images/logo-emblem.png" alt=""></div>

<!-- 2. Sticky Header (tüm sayfalarda birebir aynı, bkz. §6.1) -->
<header class="site-header" id="siteHeader"> ... </header>

<!-- 3. Mobil menü + backdrop (tüm sayfalarda birebir aynı, bkz. §6.2) -->
<div class="menu-backdrop" id="menuBackdrop"></div>
<nav class="mobile-menu" id="mobileMenu"> ... </nav>

<!-- 4. Sayfaya özel hero -->
<!--   Anasayfa: tam ekran .hero (video/görsel arka plan)      -->
<!--   İç sayfalar: kompakt .page-hero + .breadcrumb           -->

<!-- 5. Sayfa içeriği: section.section-pad bloklarının art arda dizilimi -->

<!-- 6. Footer (tüm sayfalarda birebir aynı, bkz. §6.3) -->
<footer class="site-footer"> ... </footer>

<!-- 7. Scroll-to-top butonu (tüm sayfalarda birebir aynı) -->
<button class="back-to-top" id="backToTop" aria-label="Yukarı çık"> ... </button>

<script src="js/script.js"></script>
</body>
</html>
```

### 5.1 İç Sayfa Hero Kalıbı (`.page-hero`)

Anasayfa dışındaki her sayfa şu kalıbı kullanır:

```html
<section class="page-hero">
  <div class="page-hero-overlay"></div>
  <div class="container page-hero-content">
    <nav class="breadcrumb reveal in-view">
      <a href="index.html">Anasayfa</a><span>/</span>
      <!-- ara seviye varsa: <a href="hizmetler.html">Hizmetlerimiz</a><span>/</span> -->
      <span>{Güncel Sayfa Adı}</span>
    </nav>
    <h1 class="reveal in-view">{Başlık}</h1>
    <p class="reveal in-view reveal-delay-1">{Alt açıklama}</p>
  </div>
</section>
```

### 5.2 Hizmet Detay Sayfası İçerik Sırası (zorunlu bölüm sırası)

1. `.page-hero` (breadcrumb: Anasayfa / Hizmetlerimiz / {Hizmet})
2. Açıklama bloğu — `.about-grid` (görsel + metin) **veya** `.handling-block`
   (chip-grid kullanılan hizmetler için, örn. Tahmil Tahliye)
3. Avantajlar — `.advantages-grid` (3 `.value-card`)
4. Süreç Diyagramı — `.process-steps.mini-process` (5 adım: Talep → Planlama
   → Operasyon → Takip → Teslimat)
5. İlgili Görseller — `.detail-gallery` (3 görsel)
6. (Opsiyonel) İlgili Hizmetler — `.related-services` (3 `.service-card`)
7. CTA şeridi — `.cta-strip` ("Teklif İsteyin" + "Diğer Hizmetler")
8. Footer

Teklif butonları her zaman `teklif-al.html?transport={TaşımaTürüDeğeri}`
formatında link verir (bkz. §7.8 için geçerli değerler).

### 5.3 Blog Makale Sayfası İçerik Sırası

1. `.page-hero` (breadcrumb: Anasayfa / Blog / {Kategori})
2. `<article class="blog-article">`: `.article-meta` (tarih, okuma süresi,
   kategori) → `.article-cover` (kapak görseli) → `.article-body` (h2 alt
   başlıklar + p + ul)
3. "İlginizi Çekebilir" — `.related-posts` (3 kart)
4. CTA şeridi
5. Footer

---

## 6. Ortak Bileşenler (Header / Mobil Menü / Footer)

### 6.1 Header (`.site-header`)

- Sticky (`position: fixed`), scroll sonrası `.scrolled` class'ı ile
  koyulaşır (`js/script.js` → `onScroll()`).
- Sol: `.brand` (logo-emblem.png + "CEN LOGISTICS" / "Caucasus Europe
  Network" iki satırlı metin).
- Orta: `.nav-links` — **sabit 6 link**: Anasayfa, Hakkımızda,
  Hizmetlerimiz, Faaliyet Bölgeleri, Teklif Al, İletişim.
- Sağ: `.header-actions` → "Hemen Ara" `.btn-primary` (`tel:+902166296615`)
  + `.nav-toggle` hamburger (yalnızca ≤860px'de görünür).
- Aktif sayfa vurgusu `js/script.js` → `setActiveNavByFile()` ile dosya adı
  eşleşmesine göre otomatik yapılır (`.active` class'ı JS tarafından eklenir,
  HTML'de elle yazılmaz).

### 6.2 Mobil Menü (`.mobile-menu`)

- **9 link + 1 buton**, bu sırayla: Anasayfa, Hakkımızda, Hizmetlerimiz,
  Faaliyet Bölgeleri, Taşıma Süreci, Teklif Al, Blog, Sıkça Sorulan Sorular,
  İletişim, ardından "Hemen Ara" butonu.
- Masaüstünde (`>860px`) `display: none`; `≤860px`'de `.nav-toggle` tıklanınca
  `.open` class'ı ile sağdan kayarak açılır (`transform: translateX`).
- `.menu-backdrop` her zaman DOM'da bulunur; masaüstünde görünmez, mobilde
  `.open` class'ıyla opacity/visibility geçişi yapar.
- **Kritik CSS kuralı:** `.mobile-menu` ve `.menu-backdrop` için masaüstü
  `display` değerleri `css/style.css` §"HEADER" bloğunda (media query
  dışında) tanımlıdır: `.mobile-menu { display: none; }` /
  `.menu-backdrop { display: none; }`. Mobil media query içinde bu değerler
  `display: flex` / `display: block` olarak override edilir. **Bu satırlar
  silinirse header/mobil menü masaüstünde düz metin olarak sayfanın en
  üstünde görünür (geçmişte tespit edilmiş regresyon) — asla kaldırılmamalı.**

### 6.3 Footer (`.site-footer`)

4 kolonlu grid (`.footer-grid`):

1. **`.footer-brand`** — logo + kısa marka açıklaması + `.social-icons`
   (LinkedIn, Instagram, WhatsApp).
2. **Hızlı Menü** — Anasayfa, Hakkımızda, Faaliyet Bölgeleri, Taşıma Süreci,
   Blog, Sıkça Sorulan Sorular.
3. **Hizmetler** — 6 hizmet detay sayfasının tam listesi (Deniz Taşımacılığı
   → Tahmil Tahliye sırasıyla).
4. **İletişim** — adres, telefon, e-posta, "Teklif Al →" linki.

Altında `.footer-bottom`: telif hakkı satırı (`© <span id="year"></span> ...`
— yıl `js/script.js` ile otomatik doldurulur) + KVKK / Gizlilik Politikası /
Çerez Politikası linkleri.

**Bu 4 kolonun içeriği tüm sayfalarda birebir aynıdır.** Yeni bir üst
seviye sayfa eklenirse "Hızlı Menü" kolonuna, yeni bir hizmet eklenirse
"Hizmetler" kolonuna eklenmesi zorunludur.

### 6.4 Sabit İletişim Bilgileri (her yerde aynı kullanılır)

| Alan | Değer |
|---|---|
| Adres | Küçükbakkalköy Mah. Brandium R2 Blok K:25 D:243, Ataşehir / İstanbul |
| Telefon (sabit) | 0(216) 629 66 15 → `tel:+902166296615` |
| Telefon/WhatsApp (mobil) | 0(532) 674 32 10 → `tel:+905326743210` / `https://wa.me/905326743210` |
| E-posta | ufuk.elsever@cenlogs.com → `mailto:ufuk.elsever@cenlogs.com` |

---

## 7. Tasarım Sistemi

> **Sürüm notu:** Bu bölüm 2026-07-28'de "premium/modern/kurumsal, DHL/Maersk/
> MSC/Kuehne+Nagel/DB Schenker seviyesinde" tasarım dili talebi doğrultusunda
> tamamen yenilenmiştir. Aşağıdaki palet, tipografi ve buton kuralları
> **güncel ve geçerli olandır**; bu revizyondan önceki teal/Poppins tabanlı
> tasarım artık kullanılmamaktadır.

### 7.1 Renk Paleti (`:root` değişkenleri, `css/style.css`)

```css
--navy: #0B3C5D;          /* Ana renk — SADECE büyük yüzeyler: hero, footer,
                              cta-strip, page-hero, stats band gradient'leri.
                              Metin veya ikon rengi olarak KULLANILMAZ. */
--navy-deep: #082A42;     /* Ana rengin koyu tonu — gradient derinliği */
--navy-rgb / --navy-deep-rgb: rgba() içinde alfa karışımı için kanal değişkenleri

--blue: #1E88E5;          /* İkincil renk — linkler, ikonlar, hover/active,
                              ikincil butonlar (.btn-dark), checkmark/chevron,
                              eyebrow, aktif sekme/harita düğümü (idle durum) */
--blue-dark: #1669B8;     /* mavi hover koyu tonu */

--orange: #FF8C00;        /* Vurgu rengi — SADECE şu 4 yerde kullanılır:
                              1) .btn-primary (ana CTA butonları)
                              2) hero h1 içindeki <em> vurgu kelimesi
                              3) .nav-links a.active (aktif menü göstergesi)
                              4) harita hub/aktif ülke düğümü (map-node-hub, .active)
                              Bunların dışında YENİ bir yere turuncu eklenmez —
                              vurgu rengi ancak "gerçekten nadir ve anlamlı" bir
                              highlight anında kullanılır, dekoratif olarak
                              çoğaltılmaz. */
--orange-dark: #E67E00;

--heading: #1F2937;       /* Başlık metni — h1-h4 ve tüm "kart başlığı/etiket"
                              niteliğindeki UI metinleri (form label, accordion
                              başlığı, testimonial-author adı vb.) */
--text: #6B7280;          /* Gövde metni — body varsayılanı ve tüm paragraf/
                              açıklama metinleri */
--text-faint: #9CA3AF;    /* En soluk / üçüncül metin (tarih, footnote, ipucu) */

--bg: #FFFFFF;
--bg-alt: #F8FAFC;
--bg-deep: #EAF2FB;       /* İkon kutucuğu arka planı (hafif mavi ton) */
--border: #E5E7EB;

/* Geriye dönük uyumluluk: birkaç sayfada gövde metni içinde
   style="color:var(--teal-accent)" kullanılıyor. Ad korunuyor,
   değeri --blue'ya eşitlendi. YENİ kod bu adı değil doğrudan var(--blue)
   kullanmalıdır. */
--teal-accent: var(--blue);
```

**Renk hiyerarşisi kuralı (çok önemli, her yeni bileşende uygulanır):**

| Renk | Ne zaman kullanılır | Ne zaman kullanılMAZ |
|---|---|---|
| `--navy` / `--navy-deep` | Yalnızca büyük yüzey/gradient arka planları (hero, page-hero, footer, cta-strip, stats band, harita canvas'ı) | Asla metin, ikon veya buton rengi olarak |
| `--blue` | Linkler, ikon strokeları, hover/focus/active durumları, ikincil buton (`.btn-dark`), eyebrow, checkmark, chevron, harita idle düğümleri, aktif sekme | — |
| `--orange` | Yalnızca yukarıda listelenen 4 sabit kullanım noktası | Hover glow'ları, dekoratif bullet'lar, ikon strokeları, tekrarlayan liste işaretleri (bunlar `--blue` kullanır) |
| `--heading` | h1-h4 ve kart/etiket başlıkları | Büyük yüzeylerde (o zaman beyaz kullanılır) |
| `--text` / `--text-faint` | Gövde metni / üçüncül metin | — |

Yeni bir renk eklenecekse mutlaka bu 3 ailenin (`--navy`, `--blue`, `--orange`)
bir tonu olarak türetilir; paletin dışına (kırmızı, mor, yeşil vb.) çıkılmaz.

### 7.2 Tipografi

- **Tek font ailesi: Inter** (hem başlıklar hem gövde metni,
  `var(--font-head)` ve `var(--font-body)` ikisi de Inter'e işaret eder).
  Google Fonts ağırlıkları: 400, 500, 600, 700, 800.
- Başlıklar (`h1-h4`): `font-weight: 700`; hero/page-hero gibi en büyük
  display başlıklar `font-weight: 800`.
- Gövde metni: 400 (varsayılan); vurgulu UI metinleri (chip, eyebrow, buton)
  500-600 arası kalabilir.
- `.hero h1`: `clamp(2.4rem, 5.6vw, 4.4rem)`; `.page-hero h1`:
  `clamp(2rem, 4.4vw, 3.1rem)`; `.section-head h2`:
  `clamp(1.7rem, 3.4vw, 2.5rem)` — tüm başlıklarda `clamp()` ile akıcı
  (fluid) tipografi kullanılır, sabit `px` başlık boyutu yazılmaz.
- Vurgulanan kelimeler `<em>` ile işaretlenir ve CSS'te `font-style: normal;
  color: var(--orange)` uygulanır (`.hero h1 em`) — bu, turuncunun izin
  verilen 4 kullanım noktasından biridir, başka `<em>` vurgusu eklenmez.
- Satır yüksekliği: gövde `line-height:1.65`, başlıklar `line-height:1.25`
  (rahat okunabilirlik için sabit tutulur, değiştirilmez).

### 7.3 Boşluk / Radius / Gölge Token'ları

```css
--radius-sm: 10px;  /* chip, contact-card/service-card ikon kutusu, input alanları */
--radius-md: 14px;  /* value-card, testimonial-card, timeline-dot çevresi */
--radius-lg: 16px;  /* mv-card, cta-strip, service-card, form-card, hero medya */
--shadow-sm: 0 2px 8px rgba(var(--navy-rgb),.06);   /* hafif gölge */
--shadow-md: 0 8px 24px rgba(var(--navy-rgb),.08);
--shadow-lg: 0 16px 40px rgba(var(--navy-rgb),.14);
--container: 1200px;               /* .container max-width */
--transition: .3s cubic-bezier(.4,0,.2,1);  /* TEK geçiş eğrisi, tüm hover/transition bunu kullanır */
.section-pad { padding: 96px 0; }  /* standart bölüm iç boşluğu (mobilde 70px) */
```

**Kural:** Kart/panel radius değerleri her zaman 10-16px aralığında kalır
("yumuşak köşeler" gereksinimi); yalnızca butonlar ve rozet/chip'ler pill
(`999px`) veya tam daire (`50%`) olabilir.

### 7.4 Butonlar

3 varyant, hepsi `.btn` temel class'ı + pill radius (`border-radius: 999px`):

- `.btn-primary` — **turuncu** dolgu (`var(--orange)`), ANA CTA'lar (Teklif
  Al, Gönder, Teklif İsteyin). Hover'da `var(--orange-dark)` + yukarı kayma.
- `.btn-outline` — sadece koyu/renkli arka plan üzerinde kullanılır (beyaz
  border + hafif glassmorphism: `backdrop-filter: blur(6px)`), açık arka
  planda KULLANILMAZ (kontrast düşük olur).
- `.btn-dark` — **mavi** dolgu (`var(--blue)`), İKİNCİL CTA (örn.
  "Hikayemizin Tamamı", "Süreci Detaylı İnceleyin", "Diğer Hizmetler").
  Hover'da `var(--blue-dark)`.

Her buton SVG ikonla biter (`stroke-width="2"`, `viewBox="0 0 24 24"`),
genelde ok ikonu `<path d="M5 12h14M13 5l7 7-7 7"/>`.

### 7.5 Cam Efekti (Glassmorphism) — Sadece 4 Noktada

Brief gereği "sadece gerektiği yerlerde hafif" kullanılır; aşağıdaki 4
bileşenin dışında YENİ bir yere `backdrop-filter` eklenmez:

1. `.site-header.scrolled` — scroll sonrası beyaz + `blur(14px)`.
2. `.hero-tag` — hero üzerindeki küçük rozet, `blur(6px)`.
3. `.about-badge` — görsel üzerine taşan yarı saydam istatistik kartı,
   `blur(14px)`.
4. `.counter-card` — koyu stats bandındaki sayaç kartları, `blur(10px)`.

### 7.6 Dalga (Wave) Bölüm Geçişi

`.hero::after` ve `.page-hero::after` ortak bir kural ile, bu iki bölümün
altına inline SVG data-URI kullanılarak yumuşak bir beyaz dalga eklenir
(HTML değişikliği gerekmez, salt CSS pseudo-element). Yeni bir tam-genişlik
koyu bölüm eklenirse aynı `::after` desenine dahil edilebilir; aksi halde
sabit tutulur.

### 7.7 Navbar Scroll Davranışı

`.site-header` sayfa başında **şeffaftır** (üzerinde koyu hero/page-hero
olduğu için marka adı ve nav linkleri beyaz render edilir). `js/script.js`
`scrollY > 40` olduğunda `.scrolled` class'ını ekler; bu durumda header
beyaza döner (`rgba(255,255,255,.92)` + blur) ve `.site-header.scrolled`
alt seçicileri marka metnini/nav linklerini `var(--heading)` rengine
çevirir. Aktif sayfa linkinin altında turuncu bir alt çizgi
(`.nav-links a.active::after`) gösterilir — bu, "aktif menü göstergesi"
gereksinimini karşılar ve turuncunun izinli 4 kullanım noktasından biridir.

### 7.8 Form Alanı Adlandırma Kuralı (`teklif-al.html`)

`js/script.js` şu satırla URL parametresinden taşıma türünü otomatik seçer:

```js
const transportSelect = document.querySelector('select[name="transportType"]');
```

Bu nedenle **"Taşıma Türü" `<select>` elemanının `name` attribute'u her
zaman `transportType` olmalıdır** (görünen `<label>` metni "Taşıma Türü"
olabilir, ikisi bağımsızdır). Geçerli `<option value="">` değerleri ve
hizmet sayfalarından gelen `?transport=` parametreleri birebir eşleşmelidir:

| value / URL parametresi | Hizmet sayfası |
|---|---|
| `Denizyolu` | hizmet-deniz-tasimaciligi.html |
| `Demiryolu` | hizmet-demiryolu.html |
| `Karayolu` | hizmet-karayolu.html |
| `Multimodal` | hizmet-multimodal.html |
| `Liman Handling` | hizmet-liman-handling.html (URL'de `%20` encode edilir) |
| `Tahmil Tahliye` | hizmet-tahmil-tahliye.html (URL'de `%20` encode edilir) |

Yeni bir hizmet eklenirse bu tabloya satır eklenir ve `<select>`'e karşılık
gelen `<option>` eklenir.

---

## 8. Bileşen Kütüphanesi (CSS Sınıfları)

Yeni bir bölüm/kart tasarlarken önce burada karşılığı olup olmadığı
kontrol edilir; birebir aynı ihtiyacı karşılayan bir sınıf varsa **yeniden
icat edilmez**, mevcut sınıf kullanılır.

| Sınıf | Kullanım Amacı | Örnek Sayfa |
|---|---|---|
| `.section-head` (+`.center`) | Bölüm üst başlığı: eyebrow + h2 + p | Tüm sayfalar |
| `.eyebrow` | Küçük üst etiket (teal, önünde çizgi) | Tüm sayfalar |
| `.counter-card` / `.counter-num[data-count][data-suffix]` | Animasyonlu sayaç | index.html "Sayılarla Biz" |
| `.service-card` | Hover'da yükselen hizmet kartı (görsel+ikon+başlık+CTA) | index.html, hizmetler.html |
| `.value-card` | İkon + başlık + açıklama kartı | "Neden Biz", Değerlerimiz, Avantajlar |
| `.mv-card` (`.mission` / `.vision`) | Tam kaplı görsel + karartma + metin kartı | Misyon/Vizyon |
| `.timeline` / `.timeline-item` | Dikey zaman çizelgesi | Hakkımızda |
| `.process-steps` (+`.mini-process`) | Ok ile ayrılmış adım adım süreç | Taşıma Süreci, hizmet detay sayfaları |
| `.accordion` / `.accordion-item` | SSS akordiyon | sss.html |
| `.map-section-grid` / `#worldMap` / `.map-panel` | İnteraktif SVG harita + detay paneli | index.html, faaliyet-bolgeleri.html |
| `.region-grid` / `.region-card` | Ülke/bölge özet kartı | faaliyet-bolgeleri.html, hizmet-liman-handling.html |
| `.testimonial-card` | Müşteri yorumu kartı | index.html |
| `.gallery-grid` / `.gallery-item` / `.lightbox` | Görsel galeri + lightbox | index.html |
| `.blog-card` / `.blog-card-grid` | Blog kartı | blog.html |
| `.blog-article` / `.article-meta` / `.article-cover` / `.article-body` | Blog makale detay şablonu | blog-*.html |
| `.form-card` / `.form-grid` / `.form-group` / `.form-success` | Form kartı (Teklif Al, İletişim) | teklif-al.html, iletisim.html |
| `.contact-card` | İkon + başlık + link iletişim kartı | iletisim.html, teklif-al.html |
| `.legal-content` | Yasal metin sayfası tipografisi | kvkk.html, gizlilik-politikasi.html, cerez-politikasi.html |
| `.chip-grid` / `.chip` | Etiket/özellik rozeti grid'i | hizmet-tahmil-tahliye.html |
| `.detail-gallery` | 3'lü görsel grid | hizmet-*.html detay sayfaları |
| `.related-services` / `.related-posts` | Çapraz link kart grid'i | hizmet/blog detay sayfaları |
| `.cta-strip` | Sayfa sonu koyu CTA şeridi | Her sayfanın sonunda |
| `.marquee-wrap` / `.partner-strip-wrap` | Sonsuz kayan şerit | index.html (ülkeler, iş ortakları) |
| `.page-hero` / `.breadcrumb` | İç sayfa üst banner | Anasayfa hariç tüm sayfalar |

---

## 9. JavaScript Modülleri (`js/script.js`)

Tek IIFE içinde, bağımsız modüller halinde. Yeni bir JS özelliği eklerken
aynı desen izlenir: DOM'da ilgili eleman var mı diye kontrol edilip
(`if (el) {...}`) yalnızca o zaman event bağlanır — sayfada eleman yoksa
hata fırlatılmaz.

| Modül | Ne yapar | Tetiklendiği elemanlar |
|---|---|---|
| Header scroll state | `.scrolled` class + back-to-top görünürlüğü | `#siteHeader`, `#backToTop` |
| Mobil menü | Hamburger aç/kapat, body scroll kilidi | `#navToggle`, `#mobileMenu`, `#menuBackdrop` |
| Aktif nav linki | Dosya adına göre `.active` class'ı | `.nav-links a`, `.mobile-menu a` |
| Scroll reveal | `IntersectionObserver` ile `.reveal` → `.in-view` | `.reveal`, `.reveal-delay-1..4` |
| Servis sekmeleri | Tab/panel geçişi (şu an aktif kullanımda değil ama altyapı mevcut) | `.service-tab`, `.service-panel` |
| Galeri lightbox | Büyütme, ok ile gezinme, ESC kapatma | `.gallery-item`, `#lightbox` |
| Footer yılı | `new Date().getFullYear()` | `#year` |
| Sayfa geçiş perdesi | İç link tıklamalarında fade + yönlendirme | `#pageVeil` |
| Animasyonlu sayaçlar | `data-count` / `data-suffix` ile sayı animasyonu | `.counter-num` |
| Akordiyon | Tek seferde bir panel açık (SSS) | `.accordion-item` |
| Blog filtre çipleri | `data-filter` / `data-category` eşleşmesiyle kart gösterme/gizleme | `.blog-filter-chips .chip-filter`, `.blog-card-grid .blog-card` |
| Mailto form gönderimi | Form verisini `mailto:` linkine dönüştürüp `.form-success` gösterir | `form[data-mailto][data-subject]` |
| İnteraktif dünya haritası | `COUNTRY_DATA` objesinden SVG node/rota/panel üretir | `#worldMap`, `#mapPanel` |
| Teklif formu ön-doldurma | URL `?transport=` parametresini `select[name="transportType"]`'a yazar | `teklif-al.html` |

`COUNTRY_DATA` içinde tanımlı 10 ülke: `turkiye` (hub), `avrupa`,
`gurcistan`, `azerbaycan`, `turkmenistan`, `kazakistan`, `ozbekistan`,
`cin`, `iran`, `rusya`. Her ülke objesi şu alanları taşır: `name`, `x`, `y`,
`desc`, `services[]`, `transport[]`, `ports[]`, `rail[]` (+ hub için `hub:
true`). Yeni bir ülke eklenirse bu objeye eklenir; harita otomatik olarak
node ve rotayı üretir (HTML'e elle SVG yazılmaz).

---

## 10. Responsive Kırılım Noktaları

| Breakpoint | Etkisi |
|---|---|
| `≤1080px` | Grid'ler 2 kolona iner (about-grid, values-grid, service-card-grid, blog-card-grid, testimonial-grid, region-grid, counter-grid, footer-grid vb.), service-panel/handling-block tek kolona iner |
| `≤860px` | Masaüstü nav gizlenir, hamburger görünür, mobil menü aktif olur, hero-stats 2 kolon |
| `≤680px` | Tüm grid'ler tek kolona iner, `.section-pad` 70px'e düşer, process-steps dikeye döner (ok 90° rotate) |

Yeni bir grid bileşeni eklenirken bu 3 kırılım noktasına uygun media query
eklenmesi zorunludur (`css/style.css` dosyasının sonundaki "RESPONSIVE"
bloklarına eklenir, sayfa-özel CSS dosyası açılmaz).

---

## 11. SEO ve Erişilebilirlik Standartları

- Her sayfada benzersiz `<title>{Sayfa} | CEN Logistics Group</title>` ve
  150-160 karakterlik `<meta name="description">`.
- `lang="tr"` her sayfada sabit.
- Görsellerde betimleyici Türkçe `alt` metni zorunlu (örn. `alt="Liman vinç
  ve konteyner operasyonu"`), boş `alt=""` yalnızca dekoratif görsellerde
  (page-veil logosu gibi) kullanılır.
- Semantik HTML5 etiketleri kullanılır: `<header>`, `<nav>`, `<section>`,
  `<article>`, `<footer>`.
- Breadcrumb `<nav class="breadcrumb">` her iç sayfada bulunur.
- İkon-only butonlarda `aria-label` zorunlu (`#navToggle`, `#backToTop`,
  `.lightbox-close`, sosyal ikonlar).
- Akordiyon tetikleyicilerinde `aria-expanded` durumu JS ile senkron
  tutulur.
- Form alanlarında `<label for="...">` + eşleşen `id` zorunlu.
- Harita node'ları `role="button"` + `tabindex="0"` + `aria-label` ile
  klavye erişilebilir.

**Bilinen teknik borç (henüz tamamlanmadı, gelecekte ele alınacak):**
- Görseller `.jpg`/`.png` formatında; WebP dönüşümü ve `<picture>`/`srcset`
  ile lazy-loading optimizasyonu henüz yapılmadı.
- `loading="lazy"` attribute'u görsellerde henüz sistematik uygulanmadı
  (yalnızca `iletisim.html` harita iframe'inde `loading="lazy"` var).
- robots.txt / sitemap.xml henüz oluşturulmadı.

Bu maddeler ileride ele alınacaksa önce bu dokümana **"Bilinen Teknik
Borç"tan çıkarıldı** notu düşülüp ilgili bölüme taşınmalıdır.

---

## 12. Bakım Kuralları (Header/Footer Senkronizasyonu)

Bu proje bir include mekanizması kullanmadığı için:

1. **Header, mobil menü veya footer'da bir değişiklik** (yeni nav linki,
   telefon numarası değişikliği, sosyal medya linki vb.) yapıldığında bu
   değişiklik **22 sayfanın tamamına** aynı şekilde uygulanmalıdır.
2. Böyle bir değişiklik yapmadan önce mevcut tüm sayfalardaki ilgili bloğun
   birebir aynı olduğu doğrulanmalı (regresyon riski).
3. Yeni sayfa oluştururken §5'teki iskelet + §6'daki header/footer/mobil
   menü **birebir kopyalanır**, elle yeniden yazılmaz veya "sadeleştirilmez".
4. Tüm sayfalarda dahili linkler (`href="*.html"`) ve görseller
   (`src="assets/images/*"`) eklendikten sonra kontrol edilmelidir; kırık
   link/görsel referansı bırakılmaz.

---

## 13. Yeni Geliştirme Kontrol Listesi

Her yeni özellik/sayfa eklendikten sonra şu kontrol listesi doğrulanır:

- [ ] Dosya kök dizinde, doğru kebab-case isimle mi oluşturuldu?
- [ ] §5'teki sayfa iskeleti (page-veil, header, mobil menü, footer,
      back-to-top, script) birebir kopyalandı mı?
- [ ] Header/footer/mobil menü içeriği diğer sayfalarla birebir aynı mı?
- [ ] Yeni renk/font/spacing yerine §7'deki mevcut token'lar mı kullanıldı?
- [ ] Yeni bir kart/bölüm için §8'deki mevcut bir bileşen sınıfı yeniden
      kullanılabiliyor muydu, yeniden mi icat edildi?
- [ ] 3 responsive kırılım noktasında (§10) test edildi/CSS eklendi mi?
- [ ] `alt`, `aria-label`, `label for/id` gibi erişilebilirlik gereksinimleri
      karşılandı mı (§11)?
- [ ] Yeni hizmet ise: hizmetler.html, index.html, footer "Hizmetler"
      kolonu ve teklif-al.html `<select>`'i güncellendi mi (§7.8)?
- [ ] Yeni üst-seviye sayfa ise: footer "Hızlı Menü" ve gerekiyorsa mobil
      menü güncellendi mi (§4.1 tablosu)?
- [ ] Tüm yeni dahili linkler ve görsel referansları gerçekten var olan
      dosyalara mı işaret ediyor (kırık link/görsel yok)?

---

## 14. Çok Dilli Altyapı (i18n) — Türkçe / İngilizce / Rusça

> **Eklendi:** 2026-07-29. Site tek sayfa kalıp, framework/build aracı
> eklenmeden, saf JS ile 3 dilli hale getirildi.

### 14.1 Çalışma Prensibi

Ayrı `/en/`, `/ru/` klasörleri veya `index-en.html` gibi kopya dosyalar
**oluşturulmadı** (bu, §3'teki "alt klasör açılmaz" ve "her sayfa kök dizinde
tek dosya" kurallarıyla çelişirdi). Bunun yerine:

- Çevrilebilir her metin, ilgili HTML elemanına eklenen
  `data-i18n="anahtar"` attribute'u ile işaretlenir. `js/script.js`
  içindeki `applyLanguage(lang)` fonksiyonu, dil değiştiğinde bu elemanların
  `innerHTML`'ini `I18N[lang][anahtar]` değeriyle değiştirir (`<em>`, `<strong>`
  gibi iç etiketler değerin içinde saklı kalır).
- `placeholder`, `alt`, `aria-label`, `title`, meta `content` gibi
  attribute'lar `data-i18n-attr="attr1:anahtar1,attr2:anahtar2"` ile
  çevrilir.
- Tüm çeviriler `js/script.js` içindeki **tek** `I18N` sabitinde
  `{ tr: {...}, en: {...}, ru: {...} }` şeklinde düz (flat) anahtar-değer
  sözlükleri olarak tutulur — ikinci bir JS/JSON dosyası açılmaz (§2
  "tek script dosyası" kuralına uyar).
- Anahtar adlandırma: ortak header/footer/nav/CTA metinleri `common.*`
  önekiyle (örn. `common.nav.home`, `common.service.sea`,
  `common.cta.requestQuote`), sayfaya özel metinler ilgili sayfanın kısa
  slug'ıyla (örn. `index.hero.title`, `svc-sea.hero.title`, `kvkk.s1.p1`)
  adlandırılır. Ortak bir metin birden fazla sayfada birebir aynıysa yeni
  bir anahtar icat edilmez, mevcut `common.*` anahtarı yeniden kullanılır.
- Dil tercihi `localStorage` (`cen_lang` anahtarı) ile saklanır; sayfa her
  açıldığında `applyLanguage(currentLang)` otomatik çalışır, `<html lang>`
  attribute'u da güncellenir.
- İkon + metin birlikte olan elemanlarda (örn. `<svg>...</svg> Teklif Al`)
  `data-i18n` **asla** ikonu da içeren üst elemana konmaz; yalnızca metni
  saran bir `<span data-i18n="...">` eklenir (aksi halde dil değişiminde
  ikon silinir).

### 14.2 Dil Seçici Bileşeni

- **Masaüstü:** `.header-actions` içinde `#langSwitch` — bayrak/glob ikonlu
  buton + TR/EN/RU açılır menü (`.lang-switch-menu`). `≤860px`'de
  `.nav-links` ve `.btn-primary` ile birlikte gizlenir.
- **Mobil:** `.mobile-menu` içinde en üstte `.mobile-lang-switch` — 3 eşit
  genişlikte TR/EN/RU butonu. Masaüstünde `display:none`.
- Her iki bileşendeki butonlar ortak `.lang-option` sınıfını ve
  `data-lang="tr|en|ru"` attribute'unu paylaşır; `js/script.js` tek bir
  `document` seviyesinde click listener'ı ile ikisini de yönetir.
- Bu bileşen **her 22 sayfada birebir aynıdır** — §12 Bakım Kuralları bu
  bileşen için de geçerlidir.

### 14.3 Faaliyet Haritası (COUNTRY_DATA) Çevirisi

`js/script.js` içindeki `COUNTRY_DATA` (koordinat/hub bilgisi, dilden
bağımsız) sabit kalır; `COUNTRY_I18N.en` / `COUNTRY_I18N.ru` aynı ülke
anahtarlarıyla `name/desc/services/transport/ports/rail` alanlarının
çevirilerini tutar. `getCountry(key, lang)` ikisini birleştirir.
Harita düğüm etiketleri ve `#mapPanel` içeriği, `onLangChange()` ile
kayıtlı bir hook üzerinden dil değişiminde yeniden çizilir.

### 14.4 Yeni Metin Eklerken Kontrol Listesi

- [ ] Yeni element `data-i18n` veya `data-i18n-attr` ile işaretlendi mi?
- [ ] Anahtar, mevcut bir `common.*` anahtarıyla birebir aynı metni
      tekrar etmiyor mu (varsa onu kullan)?
- [ ] `I18N.tr` / `I18N.en` / `I18N.ru` üçüne de aynı anahtar eklendi mi
      (üçünde de eksiksiz olmalı — bkz. §14.1)?
- [ ] İkon + metin karışık elemanlarda ikon `data-i18n` kapsamı dışında mı
      bırakıldı?
- [ ] `teklif-al.html` gibi işlevsel `value`/`name` attribute'ları
      (örn. `select[name="transportType"]` seçenek değerleri) yanlışlıkla
      çevrilmedi mi — yalnızca görünen metin çevrilir, `value` sabit kalır.

---

*Bu doküman, 2026-07-28 itibarıyla projenin mevcut (üretimdeki) mimarisinin
analiz edilmesiyle oluşturulmuştur. Mimaride bilinçli bir değişiklik
yapıldığında bu doküman da aynı oturumda güncellenmelidir.*

*Revizyon (2026-07-28, aynı gün ikinci güncelleme): §7 Tasarım Sistemi,
premium/modern/kurumsal yeniden tasarım talebi doğrultusunda tamamen
yenilendi — yeni renk paleti (navy #0B3C5D / mavi #1E88E5 / turuncu
#FF8C00), tek font ailesi (Inter, Poppins kaldırıldı), 10-16px radius
skalası, buton renk hiyerarşisi (ana CTA turuncu / ikincil mavi), şeffaf→
beyaz navbar, dalga (wave) bölüm geçişleri ve 4 noktayla sınırlı
glassmorphism eklendi. Değişiklikler `css/style.css` (tek dosya, tam
yeniden yazım) ve tüm 22 sayfanın `<head>` font linkinde uygulandı; hiçbir
HTML sınıf adı veya sayfa yapısı değişmedi.*
