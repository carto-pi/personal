# CLAUDE.md — Akademik Kişisel Web Sitesi

## Proje Özeti
Doç. Dr. Abdulkadir Memduhoğlu'nun kişisel akademik web sitesi.  
Harran Üniversitesi Harita Mühendisliği Bölümü Öğretim Üyesi kimliğini yansıtacak, 
hem uluslararası akademik çevreye hem de öğrencilere hitap edecek.

---

## Hedef Kitle
1. **Akademik çevre** — İşbirliği arayan araştırmacılar, dergi editörleri, proje ortakları (Türkiye & Avrupa)
2. **Öğrenciler** — Lisans/lisansüstü, Harran Üniversitesi Harita Mühendisliği
3. **Kurumsal paydaşlar** — TÜBİTAK, belediyeler, bakanlıklar

---

## Teknik Kısıtlar
- **Statik site** — Sunucu gerektirmez; GitHub Pages, Netlify veya HÜ web hosting ile yayınlanabilir
- **Tek HTML dosyası** (Faz 1-2), harici CSS/JS opsiyonel
- **Mobil uyumlu** — Responsive tasarım zorunlu
- **Dil** — Varsayılan Türkçe + İngilizce toggle (Faz 3)
- **Çerez / GDPR** — Zorunlu değil (statik içerik)

---

## Dosya Yapısı

```
/
├── index.html          # Ana sayfa (Faz 1)
├── students.html       # Öğrenci köşesi (Faz 2, isteğe bağlı ayrı sayfa)
├── assets/
│   ├── css/
│   │   └── style.css
│   ├── js/
│   │   └── main.js
│   ├── img/
│   │   ├── profile.jpg         # Akademik profil fotoğrafı
│   │   ├── og-image.jpg        # OpenGraph paylaşım görseli
│   │   └── courses/            # Ders logoları (opsiyonel)
│   └── files/
│       ├── cv-tr.pdf
│       ├── cv-en.pdf
│       └── courses/            # Ödev / ders dosyaları
├── CLAUDE.md           # Bu dosya
└── README.md
```

---

## Tasarım Sistemi

### Renk Paleti
```css
:root {
  --color-bg:        #f8f7f4;   /* Kağıt beyazı — steril değil, sıcak */
  --color-surface:   #ffffff;
  --color-dark:      #1e293b;   /* Koyu slate — başlıklar, navbar */
  --color-accent:    #0d9488;   /* Teal — butonlar, linkler, vurgular */
  --color-accent-2:  #b45309;   /* Amber — öğrenci bölümü aksanı */
  --color-muted:     #64748b;   /* Gri metin */
  --color-border:    #e2e8f0;
}
```

### Tipografi
```css
/* Google Fonts import */
@import url('https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,600;0,700;1,400&family=Source+Serif+4:wght@300;400;600&display=swap');

--font-display: 'Cormorant Garamond', Georgia, serif;   /* Hero, H1, H2 */
--font-body:    'Source Serif 4', Georgia, serif;        /* Paragraflar */
--font-mono:    'JetBrains Mono', monospace;             /* Kod, tarihler */
```

### Arka Plan Motifi
Çok hafif, düşük kontrast **topoğrafik kontur çizgisi** SVG deseni.  
`background-image: url("data:image/svg+xml,...")` olarak inline eklenebilir.  
Opasite: %3–5 arası — fark edilir ama dikkati dağıtmaz.

### Animasyon Kuralları
- Sayfa yüklenişinde bölümler aşağıdan yukarı `fade-in + translate-y` ile açılır (staggered)
- Kartlar hover'da `box-shadow` derinleşir + 2px yukarı kayar
- Navigasyon scroll'da arka plan blur alır (glassmorphism)
- Hiçbir animasyon 400ms'yi geçmez; `prefers-reduced-motion` desteklenir

---

## Sayfa Bölümleri (index.html)

### 1. Navbar
- Sol: Ad + unvan (küçük) veya logo
- Sağ: Hakkımda, Araştırma, Yayınlar, Projeler, Öğrenci Köşesi, TR/EN
- Sticky + scroll blur efekti
- Mobilde hamburger menü

### 2. Hero Section
```
[Sol] Ad, Unvan, Bölüm, Üniversite
      Araştırma alanları (badge'ler)
      [CV İndir] [Google Scholar] [ORCID] [ResearchGate] [E-posta]
[Sağ] Profil fotoğrafı — yuvarlak, subtle border, hafif gölge
```
- Arka plan: Topoğrafik desen + gradient overlay

### 3. Hakkımda
- 3-4 paragraf akademik biyografi
- Eğitim timeline (Yıldız TU MSc 2015, PhD 2021)
- Yan panel: İstatistikler (yayın sayısı, proje, öğrenci)

### 4. Araştırma Alanları
- 6 kart: GIS, Uzaktan Algılama, Makine Öğrenmesi, Kentsel Analiz, CBS Kalite, Çevre İzleme
- Her kart: İkon + başlık + 1-2 cümle + ilgili yayına link

### 5. Seçili Yayınlar
- Filtreler: Tümü | Dergi | Konferans | Kitap Bölümü
- Her kayıt: Yazar(lar), Başlık (link), Dergi, Yıl, DOI butonu
- "Tüm yayınları gör → Google Scholar" linki

### 6. Projeler
- Grid kartlar: Proje adı, fon kaynağı, süre, rol, durum (Devam/Tamamlandı)
- TÜBİTAK, Horizon Europe, Bakanlık projeleri ayrı renkle etiketlenir

### 7. Öğrenci Köşesi ⭐
> **Ayrı bir section veya tab** — `#students` anchor ile erişilir  
> Renk aksanı: amber (`--color-accent-2`) ile akademik bölümden görsel ayrım

**Alt bölümler:**
- **📢 Duyurular** — Kart listesi (tarih + başlık + metin + opsiyonel link)
- **📚 Derslerim** — Accordion; her ders tıklanınca dosya listesi açılır
  - Ders adı, kod, dönem
  - Dosya linkleri: Syllabus, Ders notları, Ödevler, Sınav bilgisi
- **📅 Takvim** — Basit HTML tablosu: Hafta / Konu / Ödev / Sınav
- **📬 İletişim** — "Öğrenciyseniz, e-posta öncesi bu SSS'e göz atın" + mailto linki

### 8. Footer
- İletişim bilgileri (kurum adresi, e-posta, ofis no)
- Hızlı linkler
- Son güncelleme tarihi
- "Bu site Claude ile tasarlandı" (opsiyonel)

---

## İçerik Verileri (Dolduralacaklar)

Aşağıdaki bilgiler `index.html` içine yerleştirilecek:

```
AD SOYAD      : Abdulkadir Memduhoğlu
UNVAN         : Doç. Dr.
POZİSYON      : Öğretim Üyesi, Harita Mühendisliği
ÜNİVERSİTE    : Harran Üniversitesi
ŞEHİR         : Şanlıurfa, Türkiye
E-POSTA       : akadirm@harran.edu.tr
ORCID         : https://orcid.org/0000-0002-9072-869X
GOOGLE SCHOLAR: https://scholar.google.com.tr/citations?user=EWXdpogAAAAJ&hl=tr
RESEARCHGATE  : https://www.researchgate.net/profile/Abdulkadir-Memduhoglu?ev=hdr_xprf

EĞİTİM:
  - MSc 2015 — Uzaktan Algılama ve CBS, Yıldız Teknik Üniversitesi
  - PhD 2021 — Uzaktan Algılama ve CBS, Yıldız Teknik Üniversitesi

ARAŞTIRMA ALANLARI:
  - Coğrafi Bilgi Sistemleri (GIS)
  - Uzaktan Algılama
  - Makine Öğrenmesi / Yapay Zeka (Mekansal Uygulamalar)
  - OpenStreetMap / Kalabalık Kaynaklı Coğrafi Veri
  - Kentsel İklim & Çevre Analizi
  - Kartografik Mühendislik

DERSLER (güncel liste eklenecek):
  - [Ders Kodu] Ders Adı — [Lisans/Lisansüstü] — [Dönem]
  - ...

YAYINLAR: [Google Scholar'dan manuel veya API ile çekilecek]
PROJELER:  [TÜBİTAK proje numaraları ve başlıkları eklenecek]
```

---

## Geliştirme Fazları

| Faz | Kapsam | Durum |
|-----|--------|-------|
| **1** | index.html — Hero, Hakkımda, Araştırma, Yayınlar, Projeler, Footer | 🔲 Yapılacak |
| **2** | Öğrenci köşesi bölümü (duyurular, dosyalar, takvim) | 🔲 Yapılacak |
| **3** | TR/EN dil toggle, SEO meta tagları, OpenGraph | 🔲 Yapılacak |
| **4** | İsteğe bağlı: Google Scholar API entegrasyonu | 🔲 İleride |

---

## Yayınlama Seçenekleri

### A) GitHub Pages (Önerilen)
```bash
# Repo oluştur: github.com/kullanici-adi/kullanici-adi.github.io
# veya özel repo + Pages ayarı
# Sonuç: https://kullanici-adi.github.io
```

### B) Harran Üniversitesi Web Sunucusu
- Akademik.harran.edu.tr altında subpage
- Üniversite IT birimiyle koordinasyon gerekir

### C) Özel Domain
- `abdulkadirmemduhoglu.com` gibi bir domain (~200₺/yıl)
- Cloudflare Pages veya Netlify ile ücretsiz hosting

---

## Sonraki Adım

Bu CLAUDE.md dosyası hazır.  
**Şimdi Faz 1'e başlamak için:**
> "Faz 1'i kodla" veya "index.html'i oluştur" de — tasarım sistemine tam uygun, üretim kalitesinde tek dosya HTML/CSS/JS oluşturulur.

Öncesinde şu bilgileri paylaşırsan kişiselleştirme daha hızlı olur:
1. Profil fotoğrafı (opsiyonel, placeholder kullanılabilir)
2. E-posta, ORCID, Google Scholar linki
3. Öne çıkarmak istediğin 3-5 yayın
4. Aktif derslerinin listesi (kod + ad)
