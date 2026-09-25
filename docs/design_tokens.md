# KantinKampus Design Tokens

## Project Information

**Application Name:** KantinKampus  
**Type:** Mobile Application  
**Description:** Aplikasi layanan kantin kampus untuk memudahkan mahasiswa mencari makanan, melakukan pemesanan, dan mengelola transaksi secara digital.

Design tokens ini digunakan sebagai standar visual agar desain Figma, prototype, dan implementasi aplikasi memiliki konsistensi yang sama.

---

# 1. Color Tokens

## Primary Colors

Warna utama aplikasi yang digunakan untuk tombol, navigasi aktif, dan elemen interaksi.

| Token | Value |
|---|---|
| primary-50 | #EFF6FF |
| primary-100 | #DBEAFE |
| primary-300 | #93C5FD |
| primary-500 | #3B82F6 |
| primary-700 | #1D4ED8 |
| primary-900 | #1E3A8A |

---

## Semantic Colors

Digunakan untuk memberikan informasi status pada aplikasi.

| Token | Value | Usage |
|---|---|---|
| success-500 | #16A34A | Status berhasil |
| warning-500 | #F59E0B | Status menunggu |
| error-500 | #DC2626 | Status gagal |
| info-500 | #2563EB | Informasi |

---

## Neutral Colors

Digunakan untuk background, teks, dan elemen pendukung.

| Token | Value |
|---|---|
| neutral-50 | #F9FAFB |
| neutral-100 | #F3F4F6 |
| neutral-200 | #E5E7EB |
| neutral-400 | #9CA3AF |
| neutral-600 | #4B5563 |
| neutral-800 | #1F2937 |

---

# 2. Typography Tokens

Font utama:

```
Inter
```

| Token | Size | Weight | Example |
|---|---|---|---|
| Display | 28px | Bold | Selamat Datang Kembali! |
| H1 | 24px | Bold | Halo, Dimas Pratama! 👋 |
| H2 | 20px | Semi Bold | Menu Populer Hari Ini |
| H3 | 16px | Semi Bold | Ayam Geprek Sambal Korek |
| Body | 14px | Semi Bold | Pedas • 1 porsi |
| Caption | 12px | Regular | Rp16.000 |

---

# 3. Spacing Tokens

Menggunakan sistem spacing berbasis 4px.

| Token | Value |
|---|---|
| space-4 | 4px |
| space-8 | 8px |
| space-16 | 16px |
| space-24 | 24px |
| space-32 | 32px |
| space-48 | 48px |

---

# 4. Component Tokens

## Button Primary

Component:

```
Button / Primary
```

Specification:

```
Height:
48px

Radius:
12px

Usage:
Masuk Ke Kantin
```

---

## Input Field

Component:

```
Input / Default
```

Specification:

```
Height:
48px

Radius:
12px

Usage:
EMAIL KAMPUS / NIM

Placeholder:
Masukan Email / Nim
```

---

## Search Bar

Component:

```
Search Bar
```

Specification:

```
Height:
48px

Radius:
12px

Content:
🔍 Cari makanan atau minuman
```

---

## Food Card

Component:

```
Card / Food
```

Content:

```
Foto makanan

Ayam Geprek

Sambal Korek

Rp16.000
```

Specification:

```
Radius:
16px
```

---

## Category Chip

Component:

```
Category / Chip
```

Items:

```
[ Semua ]

[ Makanan ]

[ Minuman ]

[ Snack ]
```

Specification:

```
Height:
55px

Radius:
12px
```

---

## Bottom Navigation

Component:

```
Bottom / Navigation
```

Items:

```
🏠 Home

🗒️ Katalog

🛒 Pesanan

👤 Profil
```

Specification:

```
Height:
119px

Radius:
12px
```

---

# 5. Usage Rules

## Do

- Gunakan warna berdasarkan Color Tokens.
- Gunakan spacing sesuai sistem 4px.
- Gunakan typography sesuai hierarchy.
- Gunakan component sebagai reusable instance.

## Don't

- Jangan menggunakan warna di luar token.
- Jangan menggunakan ukuran spacing acak.
- Jangan membuat komponen baru tanpa dokumentasi.

---

# Version

```
KantinKampus Design Tokens v1.0

High Fidelity Mockup & Design System Project
```
