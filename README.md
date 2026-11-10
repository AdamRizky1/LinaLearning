# LinaLearning

Web buat belajar dari rekaman suara. Rekam pelajaran (atau upload file audio),
ubah jadi teks, terus teksnya disusun jadi modul baca, slide, dan kuis. Selama
sesi belajar ada penghitung fokus (webcam + deteksi pindah tab), dan di akhir
sesi ada laporan yang bisa dikirim ke email orang tua.

Satu file aja (index.html), nggak ada build step.

## Fitur

- Speech to text dari rekaman mic atau file audio. Audio panjang otomatis
  dipecah per 30 detik sesuai batas endpoint ASR, dire-encode jadi wav 16 kHz,
  lalu hasilnya digabung jadi satu transkrip.
- Materi tambahan opsional: tempel teks atau load file txt/pdf.
- Generate modul, slide, dan kuis dari transkrip + materi tambahan. Salah satu
  sumber aja udah cukup: bisa dari transkrip saja (tanpa materi), atau dari
  materi tulisan saja (tanpa rekaman audio sama sekali). Kalau dua-duanya ada,
  digabung: transkrip jadi sumber utama, materi dipatuhi.
- Alur nyusun 3 fase: (1) referensi dicari dulu di web — Wikipedia Indonesia
  yang API-nya CORS terbuka, kata kuncinya dipilih model — buat mastikan fakta,
  (2) modul & slide (GLM 5.3 Flash) paralel dengan kuis (GPT 6 Luna), dan
  (3) diagram SVG digambar. Gagal nyari web atau gambar? Sesi tetap jalan.
- Tiap bagian modul ada kolom "analogi": model memilih sendiri cara jelasin
  yang paling gampang dipahami (analogi kehidupan sehari-hari, cerita pendek,
  atau perbandingan sederhana), ditonjolkan dalam kotak khusus.
- Visual: satu diagram hero (peta konsep) di tab modul, diagram per bagian
  modul, dan visual di tiap slide (yang nggak butuh diagram pakai banner
  cadangan). Semua SVG dibikin GLM 5.3 Flash di panggilan terpisah.
- Focus counter: tiap 5 detik dihitung fokus kalau tab kebuka dan, kalau
  kamera nyala, wajah ke-deteksi. Pindah tab, wajah hilang, DAN kamera
  ketutup/laptop ditutup semua dihitung gangguan. Cek anti-frame-beku: track
  kamera yang mati/mute atau frame yang hampir rata warnanya (kamera tertutup
  fisik) otomatis dianggap wajah ilang — nilai lama nggak nyangkut. Kalau model
  wajah gagal load, fokus dihitung dari tab aja sesuai janji pesan errornya.
- Laporan sesi: durasi, skor fokus, gangguan, skor kuis. Bisa dikirim ke
  email orang tua, dibuka via Gmail, dikopi, atau diunduh jadi html.

## Cara jalanin

Mic dan webcam cuma jalan di secure context, jadi jangan dibuka langsung
dari file. Server lokal cukup:

    python -m http.server 8000

lalu buka http://localhost:8000. Mau online? Upload index.html ke mana aja
(GitHub Pages, Netlify, Vercel) asal https.

## API

Pakai Kenari (kenari.id), gateway LLM Indonesia yang OpenAI-compatible dan
CORS-nya terbuka, jadi semua request aman langsung dari browser tanpa proxy.
Konfigurasi di-hardcode langsung di index.html (tidak ada panel pengaturan
lagi), jadi siswa tinggal pakai:

- Base URL: https://kenari.id/v1
- Key: format kn-... dari https://kenari.id/keys (tagihan Rupiah)
- Model modul & slide: glm-5-3-flash. Dipilih karena karakter benchmark-nya:
  konteks 1M token (transkrip panjang + materi masuk semua), kemampuan resmi
  menyusun dokumen terstruktur, dan harga output lebih murah
- Model kuis: gpt-6-luna. Dipilih karena posisi benchmark-nya sebagai model
  cepat untuk tugas terstruktur berulang (JSON, ekstraksi, klasifikasi) dengan
  latensi rendah
- Dua model itu jalan paralel pas menekan "Buat modul"
- Model ASR: whisper-large-v3-turbo

Dua hal yang perlu diketahui:

1. Kalau ganti ke provider lain yang nggak ngirim header preflight CORS
   (misal chat completions di api.z.ai), request generate modul bisa
   keblokir browser. Konfigurasi ada di objek DEFAULT_CONFIG di index.html
   (kolom chatProxy), atau pakai provider yang support browser.
2. Key yang nempel default di file ini punya pemilik repo, buat ngobrol
   langsung tanpa setup. Taruh key asli di repo publik itu resiko sendiri;
   rotate kalau keceplosan.

## Email laporan

Kirim laporan pakai FormSubmit (formsubmit.co/ajax/email-ortu), gratis dan
tanpa key, tapi kirim pertama ke satu alamat bakal diminta aktivasi sekali
oleh FormSubmit. Kalau nggak mau ribet, ada tombol "Buka di Gmail" yang
langsung nyiapin draft laporan ke email orang tua.

## Catatan teknis

- Deteksi wajah pakai face-api.js (tiny face detector), dimuat dari CDN
  pas kamera dinyalakan. Kalau CDN-nya gagal, fokus dihitung dari tab aja.
- File materi dibaca langsung di browser: .txt/.md pakai File.text(), .pdf
  pakai pdf.js dari CDN (kalau teksnya nyaris kosong = PDF scan, otomatis
  dicoba OCR pakai tesseract.js, maks 6 halaman pertama, indo+inggris),
  .docx dibongkar manual (parser ZIP + DecompressionStream, nggak butuh
  library), .rtf dibersihin dari kode formatnya. File biner lain (.doc lama,
  .pptx, dll.) ditolak dengan pesan yang jelas, bukan dimasukin jadi sampah
  karakter. Batas 25 MB.
- SVG ilustrasi hasil model disaring dulu (tag script, atribut event, dan URL
  javascript:/data: dibuang) sebelum dirender ke halaman.
- Profil dan riwayat sesi disimpan di localStorage browser; config API lama
  otomatis dihapus sekali pas halaman dibuka (masa-masa ada panel pengaturan).
