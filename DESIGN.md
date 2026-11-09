# Catatan desain

Biar nggak keliatan hasil template AI, arah desainnya ditetapin dulu:

- Kertas, bukan dashboard. Latar #f7f5ef, panel #fffdf8, teks #211d16.
  Satu warna aksen (hijau tua #276247) cuma buat tombol utama dan status fokus.
- Heading serif (Georgia), body pakai font sistem. Nggak ada gradient, emoji,
  badge, kartu bento, atau glassmorphism.
- Struktur halaman ngikutin alur kerja: 1 audio, 2 materi, 3 transkrip,
  terus masuk mode belajar. Nomor step-nya teks di kotak kecil, bukan ikon.
- Semua kontrol bisa diakses keyboard (focus-visible kelihatan), tab pakai
  role tablist/tab/tabpanel, status live pakai role=status.
- Responsif satu kolom. Di layar sempit slide dan angka fokus diperkecil,
  target sentuh tetap dijaga.
