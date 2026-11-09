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

Pakai endpoint Z.ai yang OpenAI-compatible. Semua bisa diubah di menu
Pengaturan:

- Base URL: https://api.z.ai/api/paas/v4
- Model teks: glm-4.6
- Model ASR: glm-asr-2512

Dua hal yang perlu diketahui:

1. Chat completions di api.z.ai (masih) nggak ngirim header preflight CORS,
   jadi request generate modul bisa keblokir browser. Transkripsi
   (audio/transcriptions) aman langsung dari browser. Solusinya isi kolom
   proxy di Pengaturan (misal cors-anywhere yang kamu deploy sendiri) atau
   pakai provider lain yang support browser.
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
