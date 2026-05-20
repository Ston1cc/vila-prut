# Website Template – Pensiune Moldova

> Give this file + the images folder to Claude and say:
> **"Creează un website pentru [NUME PENSIUNE] folosind acest template."**
> Claude va genera toate paginile identice ca structură, înlocuind doar conținutul specific.

---

## 1. Date pensiune (de înlocuit)

| Câmp | Valoare |
|---|---|
| **Nume** | Pensiunea Prut |
| **Slogan** | Destinația ta pentru natură, confort și relaxare. |
| **Adresă** | Strada Calea Prutului, Costești, Republica Moldova |
| **Telefon** | + (373) 673 67 363 |
| **Email** | office@pensiuneaprut.md |
| **Facebook** | https://www.facebook.com/pensiuneaprut |
| **Instagram** | https://www.instagram.com/ |
| **TikTok** | https://www.tiktok.com/ |
| **Google Maps coords** | 47.877761, 27.256687 |
| **An copyright** | 2026 |

---

## 2. Structura fișierelor

```
/
├── index.html          # Pagina principală (Home)
├── camere.html         # Pagina Camere
├── restaurant.html     # Pagina Restaurant
├── facilitati.html     # Pagina Facilități
├── galerie.html        # Galerie foto (toate imaginile)
├── mai-mult.html       # Despre noi, locație, FAQ
├── styles.css          # CSS comun tuturor paginilor
└── img/                # Toate imaginile pensiunii
    ├── hero.jpg            → imaginea principală (banner Home)
    ├── gallery_*.jpg       → imagini pentru galerie/slider
    ├── food_*.jpg          → poze cu mâncare (Restaurant)
    ├── menu_*.jpg          → carduri de meniu (Restaurant)
    └── rooms_*.jpg         → poze camere (Camere)
```

---

## 3. Design system

### Culori (CSS variables în fiecare pagină)
```js
colors: {
  accent:        '#75A9BF',   // albastru-teal – butoane, accente
  'accent-dark': '#4E7180',   // hover pe butoane
  'text-main':   '#242323',   // text principal
  'text-muted':  '#737373',   // text secundar / italic
  'bg-secondary':'#F5F5F5',   // fundal secțiuni gri
}
```

### Fonturi (Google Fonts CDN)
```html
<link href="https://fonts.googleapis.com/css2?family=EB+Garamond:ital,wght@0,400;0,700;1,400&family=Open+Sans:ital,wght@0,300;0,400;0,600;1,300;1,400&display=swap" rel="stylesheet" />
```
- **Titluri / headings**: `font-family: 'EB Garamond', Georgia, serif` → clasa Tailwind `font-serif`
- **Corp text / UI**: `font-family: 'Open Sans', sans-serif` → clasa Tailwind `font-sans`

### CDN-uri necesare (în `<head>` pe fiecare pagină)
```html
<script src="https://cdn.tailwindcss.com"></script>
<link rel="stylesheet" href="styles.css" />
```

---

## 4. Componentă reutilizabilă: NAV

Același nav pe toate paginile. Pagina curentă primește clasa `current` pe link-ul ei.

```html
<nav class="sticky top-0 z-50 bg-white border-b border-gray-100 shadow-sm">
  <div class="max-w-[980px] mx-auto px-4 h-12 flex items-center gap-6">
    <a href="index.html"      class="nav-link [current]">Home</a>
    <a href="camere.html"     class="nav-link hidden sm:inline [current]">Camere</a>
    <a href="restaurant.html" class="nav-link hidden md:inline [current]">Restaurant</a>
    <a href="facilitati.html" class="nav-link hidden md:inline [current]">Facilități</a>
    <a href="galerie.html"    class="nav-link hidden lg:inline [current]">Galerie</a>
    <a href="mai-mult.html"   class="nav-link hidden lg:inline [current]">Mai mult</a>
    <div class="flex-1"></div>
    <a href="index.html#contact"
       class="nav-link border border-[#242323] px-4 py-1.5 text-[10px] tracking-widest
              hover:bg-[#242323] hover:text-white transition-colors duration-200">
      REZERVARE
    </a>
    <div class="flex items-center gap-1 border border-gray-300 px-2 py-1 cursor-pointer select-none">
      <span style="font-size:10px;letter-spacing:.1em;">RO</span>
      <svg width="8" height="5" viewBox="0 0 8 5" fill="none">
        <path d="M1 1l3 3 3-3" stroke="#242323" stroke-width="1.2" stroke-linecap="round"/>
      </svg>
    </div>
  </div>
</nav>
```

---

## 5. Componentă reutilizabilă: FOOTER

Același footer pe toate paginile. Înlocuiește datele de contact.

```html
<footer class="bg-[#242323] text-white py-5 text-center">
  <p class="text-xs tracking-wide text-gray-300 mb-1">
    [EMAIL] &nbsp;|&nbsp; [ADRESĂ] &nbsp;|&nbsp; [TELEFON]
  </p>
  <p class="text-xs text-gray-500">© [AN] [NUME PENSIUNE]</p>
</footer>
```

---

## 6. Componentă reutilizabilă: PAGE BANNER (pagini interioare)

Folosit pe camere, restaurant, facilități, galerie, mai-mult.

```html
<div class="page-banner">
  <img src="img/[IMAGINE_BANNER].jpg" alt="[TITLU PAGINĂ]" />
  <div class="page-banner-overlay">
    <h1 class="page-banner-title">[TITLU PAGINĂ]</h1>
    <p class="page-banner-sub">[SUBTITLU]</p>
  </div>
</div>
```

CSS-ul pentru `.page-banner` este în `styles.css` și include animație Ken Burns automată.

---

## 7. Componentă reutilizabilă: SCROLL REVEAL

Orice element cu `data-reveal` apare animat când intră în viewport.
`data-d="1..6"` adaugă delay în trepte de 0.12s.

```html
<h2 data-reveal>Titlu</h2>
<p  data-reveal data-d="1">Paragraf cu delay 0.1s</p>
<div data-reveal data-d="2">Card cu delay 0.22s</div>
```

Script (același pe toate paginile, la finalul `<body>`):
```html
<script>
  const obs = new IntersectionObserver(entries => {
    entries.forEach(e => {
      if (e.isIntersecting) { e.target.classList.add('revealed'); obs.unobserve(e.target); }
    });
  }, { threshold: 0.12 });
  document.querySelectorAll('[data-reveal]').forEach(el => obs.observe(el));
</script>
```

---

## 8. Pagina: INDEX (Home)

### Secțiuni în ordine
1. **NAV** (sticky)
2. **HERO** – imagine full-width cu efect Ken Burns + overlay gradient
3. **TITLE SECTION** – H1 serif centrat, subtitlu italic, HR, buton "Rezervă Acum"
4. **GALLERY SLIDER** – 3 imagini vizibile, slide animat stânga/dreapta, autoplay 5s, dots
5. **TESTIMONIALS** – 3 carduri cu ghilimele teal, nume, citat
6. **SOCIAL ICONS** – Instagram, Facebook, TikTok
7. **CONTACT FORM** – câmpuri: Nume, Email, Subiect, Telefon, Mesaj, buton Trimite
8. **FOOTER**

### Imagini necesare pentru index
- `img/[hero].jpg` → hero (landscape, min 1440×430px)
- 16+ imagini pentru slider (orice dimensiune, se afișează 3 deodată)

### Gallery slider JS (în index.html)
```js
const images = [
  'img/imagine1.jpg',
  'img/imagine2.jpg',
  // ... toate imaginile
];
const VISIBLE = 3;
// Restul logicii este în index.html – nu modifica structura,
// doar actualizează array-ul `images[]`
```

### Testimoniale (de înlocuit cu recenzii reale)
```html
<span class="quote-mark">&ldquo;</span>
<h3>[Nume client]</h3>
<p>„[Text recenzie]"</p>
```

---

## 9. Pagina: CAMERE

### Secțiuni în ordine
1. NAV + PAGE BANNER
2. Paragraf intro (italic, centrat)
3. **GRID CAMERE** – 3 carduri (`.room-card`): imagine, titlu, capacitate, descriere, amenities tags, buton Rezervă
4. **INCLUS ÎN TOATE CAMERELE** – 4 icoane (mic dejun, parcare, Wi-Fi, natură)
5. FOOTER

### Structură card cameră
```html
<div class="room-card">
  <div class="overflow-hidden">
    <img src="img/[CAMERA].jpg" alt="[TITLU]" />
  </div>
  <div class="p-6">
    <h2 class="font-serif text-xl font-bold">[TITLU CAMERĂ]</h2>
    <p class="text-xs text-[#75A9BF] tracking-widest uppercase mb-3">Până la [N] persoane</p>
    <p class="text-[#737373] text-sm">[DESCRIERE]</p>
    <div class="flex flex-wrap gap-2 mb-5">
      <span class="amenity-tag">[EMOJI] [FACILITATE]</span>
    </div>
    <a href="index.html#contact" class="...">Rezervă</a>
  </div>
</div>
```

---

## 10. Pagina: RESTAURANT

### Secțiuni în ordine
1. NAV + PAGE BANNER (imagine masă festivă sau terasă)
2. **ABOUT** – H2, HR, paragraf italic centrat
3. **SPECIALITĂȚI** – grid 3 coloane, carduri `.dish-card` cu imagine și descriere
4. **MENIU** – grid 4 coloane cu carduri `.menu-card` (imagini tip poster/text cu lista de preparate)
5. **PROGRAM + INFORMAȚII** – 2 coloane: ore deschidere + bullet list info
6. FOOTER

### Carduri specialități (de înlocuit cu preparatele reale + poze reale)
```html
<div class="dish-card">
  <div class="img-wrap"><img src="img/[PREPARAT].jpg" alt="[TITLU]" /></div>
  <div class="p-5">
    <h3 class="font-serif text-lg font-bold">[TITLU PREPARAT]</h3>
    <p class="text-[#737373] text-sm">[DESCRIERE EXACTĂ bazată pe ce se vede în poză]</p>
  </div>
</div>
```

> ⚠️ **Important**: Descrierile trebuie să corespundă exact cu ce se vede în fotografie.

---

## 11. Pagina: FACILITĂȚI

### Secțiuni în ordine
1. NAV + PAGE BANNER
2. Paragraf intro
3. **FACILITY GRID** – 3×3 carduri `.facility-card` cu icon emoji, titlu, descriere scurtă
4. **SPLIT SECTION** – imagine stânga + text dreapta (CTA)
5. FOOTER

### Facilități standard (adaptează după pensiune)
Parcare gratuită · Wi-Fi · Acces la natură · Grătar/BBQ · Terasă · Mic dejun inclus · Pet-friendly · Transfer · Evenimente

---

## 12. Pagina: GALERIE

### Secțiuni în ordine
1. NAV + PAGE BANNER
2. **PHOTO GRID** – 4 coloane (3 pe tablet, 2 pe mobil), aspect ratio 1:1, hover zoom + overlay
3. **LIGHTBOXES** – câte un `<div id="lbN">` pentru fiecare imagine, cu prev/next
4. FOOTER

### Cum se adaugă o imagine nouă în galerie
1. Adaugă în grid:
```html
<a href="#lb[N]" class="photo-item" data-reveal>
  <img src="img/[IMAGINE].jpg" alt="[DESCRIERE]" loading="lazy" />
  <div class="photo-item-overlay"><span class="photo-item-icon">⊕</span></div>
</a>
```
2. Adaugă lightbox:
```html
<div id="lb[N]" class="lightbox">
  <a href="#" class="lb-close">×</a>
  <a href="#lb[N-1]" class="lb-nav lb-prev">‹</a>
  <img src="img/[IMAGINE].jpg" alt="" />
  <a href="#lb[N+1]" class="lb-nav lb-next">›</a>
</div>
```
3. Actualizează primul lightbox `lb1` să aibă `lb-prev` → `#lb[ULTIMUL]`
4. Actualizează ultimul lightbox să aibă `lb-next` → `#lb1`

---

## 13. Pagina: MAI MULT (Despre noi)

### Secțiuni în ordine
1. NAV + PAGE BANNER
2. **DESPRE NOI** – 2 coloane: text stânga + imagine dreapta
3. **STATS** – 4 carduri cu numere mari (camere, recenzii, %, 24h)
4. **CUM AJUNGEȚI** – 2 coloane: adresă + distanțe stânga, Google Maps embed dreapta
5. **FAQ** – accordion (click deschide/închide)
6. FOOTER

### Google Maps embed
```html
<iframe
  src="https://maps.google.com/maps?q=[LAT],[LNG]&output=embed&hl=ro&z=15"
  width="100%" height="280"
  style="border:0; display:block;"
  loading="lazy" allowfullscreen
></iframe>
<a href="https://maps.google.com/?q=[LAT],[LNG]" target="_blank">
  Deschide în Google Maps →
</a>
```

---

## 14. styles.css – clase cheie

| Clasă | Unde se folosește |
|---|---|
| `.nav-link` | Toate link-urile din nav |
| `.nav-link.current` | Link-ul paginii active (subliniat) |
| `.page-banner` | Banner pagini interioare (Ken Burns) |
| `.page-banner-title` | Titlu alb peste banner |
| `[data-reveal]` | Orice element cu animație scroll |
| `.gallery-wrap` | Container slider home |
| `.gallery-track` | Grid 3 coloane imagini slider |
| `.arrow-btn` | Butoane prev/next slider |
| `.dot` / `.dot.active` | Indicatori pagină slider |
| `.quote-mark` | Ghilimele teal mari (testimoniale) |
| `.testimonial-card` | Card testimonial |
| `.form-input` | Câmpuri formular contact |
| `.photo-item` | Item galerie foto |
| `.lightbox` | Overlay lightbox (`:target`) |
| `.dish-card` | Card preparat restaurant |
| `.menu-card` | Card meniu restaurant |
| `.facility-card` | Card facilitate |
| `.room-card` | Card cameră |
| `.split-section` | Secțiune 50/50 imagine + text |

---

## 15. Checklist pentru o pensiune nouă

- [ ] Înlocuiește **numele pensiunii** în toate titlurile și `<title>` tags
- [ ] Înlocuiește **adresa** în toate footer-ele și în `mai-mult.html`
- [ ] Înlocuiește **telefonul** în toate footer-ele și în `mai-mult.html`
- [ ] Înlocuiește **email-ul** în toate footer-ele
- [ ] Actualizează **coordonatele GPS** în iframe-ul Google Maps
- [ ] Înlocuiește **link-urile social media** (Facebook, Instagram, TikTok)
- [ ] Pune imaginea **hero** (`index.html` → `.hero-img`)
- [ ] Actualizează array-ul `images[]` din **slider** cu imaginile reale
- [ ] Înlocuiește imaginile din **camere.html** cu pozele camerelor
- [ ] Înlocuiește imaginile din **restaurant.html** cu pozele preparatelor + scrie descrieri exacte
- [ ] Adaugă imaginile **meniului** (carduri poster) în secțiunea Meniu
- [ ] Populează **galeria** cu toate imaginile disponibile
- [ ] Actualizează **textele** (descriere pensiune, testimoniale, FAQ, facilități)
- [ ] Verifică **anul** în footer (© 2026)
- [ ] Commit și push pe GitHub

---

## 16. Comandă pentru Claude

```
Creează un website complet pentru "[NUME PENSIUNE]" folosind TEMPLATE.md ca ghid.

Date:
- Nume: [NUME]
- Adresă: [ADRESĂ]
- Telefon: [TELEFON]
- Email: [EMAIL]
- Facebook: [URL]
- Coordonate GPS: [LAT, LNG]
- Hero image: img/[FISIER]
- Imagini galerie: toate fișierele din img/

Generează: index.html, camere.html, restaurant.html, facilitati.html, galerie.html, mai-mult.html, styles.css
Respectă exact structura, clasele CSS, animațiile și design system-ul din template.
```
