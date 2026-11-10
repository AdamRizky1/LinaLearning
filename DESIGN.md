# Catatan desain

Biar nggak keliatan hasil template AI, arah desainnya ditetapin dulu:

- Kertas, bukan dashboard. Latar #f3f0f7, panel #fdfcff, teks #221d2c.
  Satu warna aksen (ungu #5c4694) buat tombol utama, tab aktif, dan status
  fokus, didampingi gold #9c7a2e buat garis aksen (masthead, slide, peringatan).
  Hijau sage #4a7c59 cuma dipakai buat penanda jawaban benar di kuis.
- Heading serif (Georgia), body pakai font sistem. Nggak ada gradient, emoji,
  badge, kartu bento, atau glassmorphism.
- Struktur halaman ngikutin alur kerja: 1 audio, 2 materi, 3 transkrip,
  terus masuk mode belajar. Nomor step-nya teks di kotak kecil, bukan ikon.
- Semua kontrol bisa diakses keyboard (focus-visible kelihatan), tab pakai
  role tablist/tab/tabpanel, status live pakai role=status.
- Responsif satu kolom. Di layar sempit slide dan angka fokus diperkecil,
  target sentuh tetap dijaga.
