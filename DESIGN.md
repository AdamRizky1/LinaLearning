# Catatan desain

Arah desain: **modern AI product** — seperti landing page LLM beneran (ChatGPT,
Gemini, Perplexity) tapi tetap siap-bisnis, bukan template asal-jadi. Aturannya:

- Latar terang #f7f8fc dengan **aura aurora** halus di atas halaman (radial
  gradient indigo–violet–biru, opacity rendah, fixed) — kesan "produk AI"
  tanpa mengganggu keterbacaan. Panel putih #ffffff, teks #0e1526.
- Satu warna merek: indigo #4f46e5, plus **gradient merek**
  `linear-gradient(120deg, #4f46e5, #7c3aed, #2563eb)` yang dipakai konsisten:
  logo kotak, tombol utama, nomor step, bar atas slide & laporan, dot pill.
  Hijau #0e9f6e cuma jawaban benar; merah #e5484d salah/bahaya; amber buat
  analogi & peringatan.
- Tipografi dua peran via Google Fonts: **Space Grotesk** buat display
  (wordmark, hero, heading, angka statistik) dan **Inter** buat body. Heading
  tracking rapat (-.02em), angka tabular. Fallback sistem kalau font gagal load.
- **Hero di layar pertama**: logo gradient besar, pill "Dibantu AI" dengan dot
  gradient, headline besar dengan kata kunci ber-gradient teks
  (.grad-text, background-clip), sub-lead tenang, lalu kartu form melayang.
  Beranda juga buka dengan hero: pill "Beranda", sapaan besar per-user
  (#hello), satu kalimat penjelas. Glow di belakang hero via ::before radial.
- Tombol: utama = **gradient pill** (radius 999px) dengan shadow glow lembut;
  kedua = pill putih border halus; pilih file = pill lembut (.filebtn) dengan
  input asli disembunyikan visual tapi tetap accessible (focus-within).
- Kartu putih radius 16-18px, border 1px #e3e7f1, bayangan halus — konsisten
  di semua section, fokus strip, tabel, slide, dan overlay.
- App shell: topbar sticky blur dengan logo gradient + wordmark + tagline;
  footer dengan identitas owner jelas ("© 2026 · Dibuat & dimiliki oleh
  AdamRizky1") — identitas owner TIDAK boleh diganti.
- Komponen khas: tab segmented control, tile statistik fokus (label uppercase
  mini + angka besar Space Grotesk), slide kartu presentasi dengan bar
  gradient atas, tabel laporan kolom-kunci berlatar abu.
- Meta lengkap: title deskriptif, meta description, theme-color #4f46e5,
  favicon SVG inline (kotak gradient huruf L).
- Prompt ilustrasi SVG disuruh pakai palet yang senada biar gambar model
  nyambung dengan UI; SVG cadangan slide juga.
- Semua kontrol bisa diakses keyboard (focus-visible kelihatan), tab pakai
  role tablist/tab/tabpanel, status live pakai role=status, tombol file
  pakai pola visually-hidden input.
- Responsif satu kolom. Di layar sempit tagline & who disembunyikan, hero
  dipadatkan (judul 26px), kartu dipadatkan, target sentuh tetap dijaga.
- Yang TETAP dijaga: nggak ada emoji, nggak ada glassmorphism berlebihan,
  bahasa santai tapi copy onboarding & branding profesional, dan semua ID
  yang dipakai JS (form-setup, in-nama, in-ortu, btn-rec, dll) nggak diubah
  supaya fungsi & uji otomatis tetap lolos.
