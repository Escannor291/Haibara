# Ringkasan HTML & CSS — Product Card

## HTML

### Struktur Semantik

- **`<section>`** → pengelompokan konten besar (semua card)
- **`<article>`** → satu unit konten mandiri (tiap card)
- **`<div>`** → pembungkus tanpa makna semantik (media, body, dots)
- **`<span>`** → elemen inline tanpa makna semantik (dot warna, badge)
- **`<button type="button">`** → selalu tulis `type="button"` agar tidak submit form

### Class Naming

- Satu elemen bisa punya **banyak class**: `class="dot3 dc1"` → `.dot3` untuk bentuk, `.dc1` untuk warna
- Nama class di HTML **harus persis sama** dengan selector di CSS (huruf besar/kecil, tanda hubung)

### Elemen Block vs Inline

- **`<p>`, `<div>`, `<h3>`** = block → otomatis turun ke baris baru
- **`<span>`, `<i>`, `<button>`** = inline → sejajar dalam satu baris
- Jika ingin teks sejajar dengan icon, gunakan `<span>` bukan `<p>`

### `alt` pada `<img>`

- Wajib diisi untuk aksesibilitas — mendeskripsikan gambar jika tidak tampil

---

## CSS

### Flexbox (paling sering dipakai)

| Property                         | Fungsi                                          |
| -------------------------------- | ----------------------------------------------- |
| `display: flex`                  | aktifkan flexbox                                |
| `flex-direction: row/column`     | arah susunan (horizontal/vertikal)              |
| `justify-content: space-between` | sebar elemen ke kiri & kanan                    |
| `align-items: center`            | tengahkan secara vertikal                       |
| `gap: 8px`                       | jarak antar elemen                              |
| `flex-wrap: wrap`                | elemen turun baris jika tidak muat              |
| `flex: 1`                        | elemen mengisi sisa ruang yang tersedia         |
| `margin-top: auto`               | dorong elemen ke paling bawah (tombol Shop Now) |

### Ukuran & Responsif

- `width: min(100%, 340px)` → ambil nilai terkecil: maksimal 340px, tapi tidak lebih dari lebar layar
- `aspect-ratio: 4/3` → pertahankan rasio lebar:tinggi gambar otomatis
- `object-fit: cover` → gambar memenuhi kotak tanpa distorsi (dipotong jika perlu)
- `calc(100% - 28px)` → perhitungan dinamis (lebar penuh dikurangi 28px)

### Positioning

- `position: relative` pada parent + `position: absolute` pada child → anak bisa diposisikan bebas di dalam parent
- `top/left/bottom/right` hanya bekerja jika `position` bukan `static`

### CSS Variables

```css
:root {
  --warna-utama: #59606a;
}
background: var(--warna-utama); /* pakai variabel */
```

Ubah di satu tempat → berubah di seluruh file.

### Dot Warna — Pola Modifier Class

```css
.dot3 {
  /* bentuk: bulat, ukuran */
}
.dot3.dc1 {
  background: #1d4137;
} /* warna spesifik */
```

```html
<span class="dot3 dc1"></span>
<!-- dapat style dari keduanya -->
```

### Pseudo-class

- `:hover` → style saat mouse di atas elemen
- `:focus` → style saat elemen difokus (klik/tab)

### Satuan CSS

| Satuan                   | Dipakai untuk                          |
| ------------------------ | -------------------------------------- |
| `px`                     | ukuran tetap                           |
| `%`                      | relatif terhadap parent                |
| `rem`                    | relatif terhadap font-size root        |
| `vw` / `vh`              | relatif terhadap lebar/tinggi viewport |
| `50%` di `border-radius` | membuat lingkaran sempurna             |

### Hal Penting Lainnya

- `overflow: hidden` pada card → gambar tidak meluber keluar `border-radius`
- `white-space: nowrap` → teks tidak turun baris (berguna untuk harga)
- `border-radius: 50%` → bentuk lingkaran
- `cursor: pointer` → kursor berubah jadi tangan saat hover tombol

---

---

## Garis Horizontal (Divider)

### Cara 1 — Tag `<hr>` (paling simpel)

```html
<hr />
```

```css
hr {
  border: none;
  border-top: 1px solid #ccc;
  margin: 20px 0;
}
```

### Cara 2 — `border-bottom` pada elemen

```css
.navbar {
  border-bottom: 1px solid #ccc;
}
```

Cocok untuk garis di bawah header/navbar tanpa elemen tambahan.

### Cara 3 — Pseudo-element `::after` (tidak perlu HTML tambahan)

```css
.section::after {
  content: "";
  display: block;
  height: 1px;
  background: #ccc;
  margin: 20px 0;
}
```

---

## Memindahkan Elemen — Jangan Selalu Pakai `margin`

`margin` boleh dipakai, tapi ada alat yang lebih tepat tergantung situasi:

### Kapan pakai `margin`

- Memberi **jarak antar elemen** yang berbeda
- Contoh: `margin-bottom: 12px` antara judul dan teks

### Kapan pakai `padding`

- Memberi **ruang di dalam** elemen (antara konten dan border)
- Contoh: tombol terasa lapang → `padding: 10px 20px`
- **Perbedaan kunci:** `margin` = jarak luar, `padding` = jarak dalam

### Kapan pakai Flexbox (lebih direkomendasikan)

Daripada geser manual pakai margin, atur posisi lewat **parent**:

```css
/* Tengahkan horizontal & vertikal */
.parent {
  display: flex;
  justify-content: center; /* horizontal */
  align-items: center; /* vertikal */
}

/* Dorong satu elemen ke kanan */
.parent {
  display: flex;
}
.child-kanan {
  margin-left: auto; /* trik: dorong ke ujung kanan */
}
```

### Kapan pakai `position`

Untuk elemen yang perlu **dilepas dari alur normal** (badge, overlay, tooltip):

```css
.parent {
  position: relative;
}

.badge {
  position: absolute;
  top: 12px; /* jarak dari atas parent */
  left: 12px; /* jarak dari kiri parent */
}
```

### Kapan pakai `gap` (di dalam flex/grid)

Daripada `margin` pada tiap child:

```css
/* ❌ Cara lama */
.card {
  margin-right: 16px;
}

/* ✅ Lebih bersih */
.container {
  display: flex;
  gap: 16px; /* otomatis berlaku ke semua child */
}
```

### Ringkasan Pilihan Alat

| Situasi                                            | Gunakan                           |
| -------------------------------------------------- | --------------------------------- |
| Jarak antar elemen berbeda                         | `margin`                          |
| Ruang dalam elemen (tombol, card)                  | `padding`                         |
| Susun & posisikan banyak elemen                    | `flexbox` + `gap`                 |
| Elemen mengambang di atas yang lain (badge, modal) | `position: absolute`              |
| Tengahkan di layar                                 | `flexbox` atau `margin: 0 auto`   |
| Geser sedikit dari posisi normal                   | `position: relative` + `top/left` |

---

## Kesalahan Umum yang Harus Dihindari

| Kesalahan             | Yang Benar                                         |
| --------------------- | -------------------------------------------------- |
| `gap: auto`           | `gap: 8px` (harus nilai terukur)                   |
| `font-weight: 10px`   | `font-weight: 400` (tidak pakai px)                |
| `border-radius: 20`   | `border-radius: 20px` atau `50%`                   |
| `font: size 12px`     | `font-size: 12px`                                  |
| `box-shadow: #282827` | `box-shadow: 0 2px 4px #282827`                    |
| `<p>` sejajar icon    | Gunakan `<span>` karena `<p>` adalah block element |
| Class HTML ≠ CSS      | `class="dot3"` di HTML harus ada `.dot3` di CSS    |
