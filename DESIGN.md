# Catatan desain

Arah desain: **bersih siap-bisnis** — seperti produk SaaS beneran, bukan template
asal-jadi. Aturannya:

- Netral dingin + satu warna merek. Latar #f5f6f8, panel putih #ffffff, teks
  #101828. Biru kobalt #1f54c9 buat tombol utama, tab aktif, nomor step, dan
  kicker; kuning emas #dfae3d cuma buat aksen diagram/analogi; hijau #12805c
  cuma buat jawaban benar; merah #cf3341 buat salah/bahaya.
- Tipografi sans satu keluarga (Inter + fallback sistem), heading 700 dengan
  tracking rapat (-.015em), angka statistik tabular. Nggak ada serif.
- Kartu putih radius 14px, border 1px #d8dde7, bayangan halus dua lapis —
  konsisten di semua section, fokus strip, tabel, slide, dan overlay.
- App shell: topbar sticky putih blur dengan logo kotak gradient + wordmark +
  tagline, footer dengan identitas owner jelas ("© 2026 · Dibuat & dimiliki
  oleh AdamRizky1"), kartu onboarding tengah dengan logo + kicker.
- Komponen khas: tab segmented control (bukan underline), tile statistik fokus
  (label uppercase mini + angka besar), slide kartu presentasi dengan bar
  gradient atas, tabel laporan kolom-kunci berlatar abu.
- Meta lengkap: title deskriptif, meta description, theme-color, favicon SVG
  inline (kotak kobalt huruf L).
- Prompt ilustrasi SVG disuruh pakai palet yang sama (#1f54c9/#dfae3d/#d8dde7/
  #223047) biar gambar model nyambung dengan UI; SVG cadangan slide juga.
- Semua kontrol bisa diakses keyboard (focus-visible kelihatan), tab pakai
  role tablist/tab/tabpanel, status live pakai role=status.
- Responsif satu kolom. Di layar sempit tagline & who disembunyikan, kartu
  dipadatkan, angka fokus diperkecil, target sentuh tetap dijaga.
- Yang TETAP dijaga dari versi lama: nggak ada emoji, nggak ada glassmorphism
  berlebihan, bahasa tetap santai tapi copy onboarding & branding profesional.
