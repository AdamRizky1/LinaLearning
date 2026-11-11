# Catatan desain

Arah desain: **modern AI product dengan rasa NotebookLM** — source-centric,
kurikulum jelas, warna merek tetap indigo–violet. Aturannya:

- Latar terang #f7f8fc dengan **aura aurora** halus di atas halaman (radial
  gradient indigo–violet–biru, opacity rendah, fixed). Panel putih #ffffff,
  teks #0e1526.
- Satu warna merek: indigo #4f46e5, plus **gradient merek**
  `linear-gradient(120deg, #4f46e5, #7c3aed, #2563eb)` yang dipakai konsisten:
  logo kotak, tombol utama, nomor step, bar atas slide & laporan, dot chip,
  root peta materi, progress bar jalur. Hijau #0e9f6e jawaban benar; merah
  #e5484d salah/bahaya; amber buat analogi & peringatan.
- Tipografi dua peran via Google Fonts: **Space Grotesk** (display: wordmark,
  hero, heading, angka statistik, root peta) dan **Inter** (body). Fallback
  sistem kalau font gagal load.

Struktur ala NotebookLM:

- **Label seksi kecil uppercase** di atas tiap kartu home: "Sumber · Audio",
  "Sumber · Materi tulisan", "Studio · Transkrip", "Studio · Susun output" —
  menata alur sumber → output tanpa mengubah langkah lama.
- **Chip sumber** di kartu studio: pill kecil ber-dot gradient yang menunjukkan
  sumber terkumpul (Audio + nama file, Transkrip + jumlah kata, Materi tempel,
  File materi). Terisi otomatis dari updateGenReady.
- **Jalur belajar (kurikulum)** di layar belajar, muncul sebelum fokus strip:
  kartu tahapan horizontal (3-6 tahap dari model via kolom `kurikulum` JSON,
  fallback disusun lokal dari modul, tahap "Uji pemahaman" selalu nempel di
  ujung). Tiap tahap punya dot status (todo/current/done), estimasi menit, dan
  bisa diklik → lompat ke bagian modul terkait atau tab kuis. Progress bar
  gradient + catatan "x dari y tahap selesai". Tahap jadi done saat diklik;
  tahap kuis done otomatis saat kuis kelar.
- **Peta materi (mind map)**: tab keempat "Peta" di layar belajar. Tree CSS
  dengan konektor garis (ul/li ::before/::after): root gradient (judul +
  label "peta materi"), cabang per modul (chip jumlah poin, klik → lompat ke
  bagiannya), daun = poin penting (maks 4 per cabang), plus node kuis.
- **Navbar atas ala produk notebook**: di topbar ada dua link pill — "Beranda"
  dan "Mode belajar" — dengan state aktif (pill indigo lembut). Di layar setup
  navbar disembunyiin (body.nav-off), di layar sempit wordmark teks dimatiin
  biar muat. Klik "Mode belajar" langsung lompat ke layar belajar (sesi lama
  di-resume, nggak regenerate); kalau sumbernya kosong, dibalikin ke beranda
  dengan pesan arahan.
- **Pipeline auto-generate di layar belajar**: begitu masuk mode belajar
  (nav "Mode belajar" atau tombol "Mulai belajar"), muncul strip progres dengan
  4 chip status — Modul / Slide / Peta / Kuis — tiap chip berubah antre →
  jalan (dot indigo berdenyut) → done (hijau) → dilewati (redup, kalau user
  nggak milih) → error (merah). Selagi nyusun, pane diisi skeleton shimmer.
  Ilustrasi digambar belakangan, pane di-render ulang begitu dateng.
- **Pilihan bahan per-user**: chip toggle "Dibuat otomatis" di langkah 4
  beranda (Modul, Slide, Peta materi, Kuis; tersimpan di localStorage
  `lina_gensel`). Yang nggak dipilih nggak dipanggil ke API sama sekali —
  prompt modul disusun dinamis sesuai pilihan — dan panenya nampilin pesan
  ramah, bukan "Nggak ada slide." polos.
- Prompt modul sekarang minta kolom `kurikulum` (3-6 tahap dasar→penerapan,
  tanpa kuis) — kalau model nggak kasih, frontend menyusun sendiri dari
  struktur modul, jadi fitur nggak pernah mati.

Lanjutan yang sama seperti sebelumnya:

- Hero di layar pertama: logo gradient besar, pill "Dibantu AI", headline
  ber-gradient teks, sub-lead, kartu form melayang; beranda buka dengan hero
  sapaan per-user.
- Tombol: utama gradient pill (radius 999px) shadow glow; kedua pill putih;
  pilih file pill lembut (.filebtn) dengan input visually-hidden.
- Kartu putih radius 16-18px, border 1px #e3e7f1, bayangan halus, konsisten.
- App shell: topbar sticky blur; footer identitas owner jelas ("© 2026 ·
  Dibuat & dimiliki oleh AdamRizky1") — identitas owner TIDAK boleh diganti.
- Komponen khas: tab segmented control (kini 4: Modul/Slide/Kuis/Peta), tile
  statistik fokus, slide kartu presentasi bar gradient, tabel laporan.
- Meta lengkap: title deskriptif, meta description, theme-color #4f46e5,
  favicon SVG gradient huruf L.
- Semua kontrol bisa diakses keyboard (focus-visible), tab role
  tablist/tab/tabpanel, status role=status, tombol file pola
  visually-hidden input.
- Responsif satu kolom; di layar sempit hero & kartu dipadatkan, tahap jalur
  jadi tumpukan satu kolom, target sentuh dijaga.
- Yang TETAP dijaga: nggak ada emoji, nggak ada glassmorphism berlebihan,
  bahasa santai tapi copy profesional, dan semua ID yang dipakai JS nggak
  diubah supaya fungsi & uji otomatis tetap lolos (test_rekam 23 asersi,
  test_kurikulum 24 asersi, test_nav_pipeline 33 asersi).
