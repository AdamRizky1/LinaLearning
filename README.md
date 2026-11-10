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
- Generate modul, slide, dan kuis dari transkrip + materi.
- Focus counter: tiap 5 detik dihitung fokus kalau tab kebuka dan, kalau
  kamera nyala, wajah ke-deteksi. Pindah tab dan wajah hilang dihitung
  gangguan.
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
Semua bisa diubah di menu Pengaturan:

- Base URL: https://kenari.id/v1
- Key: format kn-... dari https://kenari.id/keys (tagihan Rupiah)
- Model modul & slide: glm-5-3-flash. Dipilih karena karakter benchmark-nya:
  konteks 1M token (transkrip panjang + materi masuk semua), kemampuan resmi
  menyusun dokumen terstruktur, dan harga output lebih murah
- Model kuis: gpt-6-luna. Dipilih karena posisi benchmark-nya sebagai model
  cepat untuk tugas terstruktur berulang (JSON, ekstraksi, klasifikasi) dengan
  latensi rendah
- Dua model itu jalan paralel pas menekan "Buat modul". Ganti kapan pun di
  Pengaturan (kolom Model modul & slide / Model kuis); tombol "Tes koneksi"
  mengetes dua-duanya sekaligus
- Model ASR: whisper-large-v3-turbo

Dua hal yang perlu diketahui:

1. Kalau ganti ke provider lain yang nggak ngirim header preflight CORS
   (misal chat completions di api.z.ai), request generate modul bisa
   keblokir browser. Solusinya isi kolom proxy di Pengaturan (misal
   cors-anywhere yang kamu deploy sendiri) atau pakai provider yang
   support browser.
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
- PDF dibaca pakai pdf.js dari CDN.
- Profil, pengaturan API, dan riwayat sesi disimpan di localStorage browser.
