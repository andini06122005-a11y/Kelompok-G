# API Contract — KantinKu

## 1. Informasi API

**Nama Sistem:** KantinKu – Aplikasi Pemesanan Makanan Kantin Kampus  
**Versi API:** 1.0.0  
**Arsitektur:** RESTful API  
**Format Data:** JSON  

Base URL:

```text
/api/v1
```

API KantinKu digunakan untuk menghubungkan aplikasi dengan layanan autentikasi,
menu, kategori, pemesanan, pembayaran, status pesanan, dan notifikasi.

---

## 2. Authentication

API menggunakan autentikasi berbasis Bearer Token.

Endpoint yang bersifat publik:

- Register
- Login
- Melihat kategori
- Melihat daftar menu
- Melihat detail menu

Endpoint lainnya membutuhkan autentikasi.

Contoh header:

```http
Authorization: Bearer <token>
Content-Type: application/json
```

Role pengguna:

- `customer` — mahasiswa/pelanggan
- `admin` — pengelola/penjual kantin

---

## 3. Standard JSON Response

### Success Response

```json
{
  "status": "success",
  "message": "Request berhasil diproses",
  "data": {}
}
```

### Error Response

```json
{
  "status": "error",
  "message": "Request gagal diproses",
  "errors": {}
}
```

### HTTP Status Code

| Code | Keterangan |
|---|---|
| 200 | Request berhasil |
| 201 | Data berhasil dibuat |
| 400 | Bad Request |
| 401 | Belum terautentikasi |
| 403 | Tidak memiliki izin |
| 404 | Data tidak ditemukan |
| 409 | Konflik data |
| 422 | Validasi gagal |
| 500 | Internal Server Error |

---

# 4. Authentication Endpoints

## 4.1 Register

**POST** `/auth/register`

**Authentication:** Tidak diperlukan  
**Role:** Public

### Request

```json
{
  "name": "Muhammad Rizha",
  "email": "rizha@example.com",
  "password": "password123"
}
```

### Success — 201

```json
{
  "status": "success",
  "message": "Registrasi berhasil",
  "data": {
    "id": 1,
    "name": "Muhammad Rizha",
    "email": "rizha@example.com",
    "role": "customer"
  }
}
```

### Errors

- `409 Conflict` — email sudah digunakan.
- `422 Unprocessable Entity` — data registrasi tidak valid.

---

## 4.2 Login

**POST** `/auth/login`

**Authentication:** Tidak diperlukan  
**Role:** Public

### Request

```json
{
  "email": "rizha@example.com",
  "password": "password123"
}
```

### Success — 200

```json
{
  "status": "success",
  "message": "Login berhasil",
  "data": {
    "token": "example_access_token",
    "user": {
      "id": 1,
      "name": "Muhammad Rizha",
      "role": "customer"
    }
  }
}
```

### Errors

- `401 Unauthorized` — email atau password salah.
- `422 Unprocessable Entity` — email/password tidak diisi atau format salah.

---

## 4.3 Get Current User

**GET** `/auth/me`

**Authentication:** Ya  
**Role:** Customer, Admin

### Success — 200

```json
{
  "status": "success",
  "message": "Profil pengguna berhasil diambil",
  "data": {
    "id": 1,
    "name": "Muhammad Rizha",
    "email": "rizha@example.com",
    "role": "customer"
  }
}
```

### Errors

- `401 Unauthorized` — token tidak tersedia.
- `401 Unauthorized` — token tidak valid atau kedaluwarsa.

---

# 5. Category Endpoints

## 5.1 Get Categories

**GET** `/categories`

**Authentication:** Tidak diperlukan  
**Role:** Public

### Success — 200

```json
{
  "status": "success",
  "message": "Daftar kategori berhasil diambil",
  "data": [
    {
      "id": 1,
      "name": "Makanan"
    },
    {
      "id": 2,
      "name": "Minuman"
    }
  ]
}
```

### Errors

- `400 Bad Request` — parameter request tidak valid.
- `500 Internal Server Error` — terjadi kesalahan pada server.

---

## 5.2 Create Category

**POST** `/categories`

**Authentication:** Ya  
**Role:** Admin

### Request

```json
{
  "name": "Makanan Ringan"
}
```

### Success — 201

```json
{
  "status": "success",
  "message": "Kategori berhasil ditambahkan",
  "data": {
    "id": 3,
    "name": "Makanan Ringan"
  }
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `422 Unprocessable Entity` — nama kategori tidak valid.

---

## 5.3 Update Category

**PATCH** `/categories/{id}`

**Authentication:** Ya  
**Role:** Admin

### Request

```json
{
  "name": "Snack"
}
```

### Success — 200

```json
{
  "status": "success",
  "message": "Kategori berhasil diperbarui",
  "data": {
    "id": 3,
    "name": "Snack"
  }
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `404 Not Found` — kategori tidak ditemukan.
- `422 Unprocessable Entity` — data tidak valid.

---

## 5.4 Delete Category

**DELETE** `/categories/{id}`

**Authentication:** Ya  
**Role:** Admin

### Success — 200

```json
{
  "status": "success",
  "message": "Kategori berhasil dihapus",
  "data": null
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `404 Not Found` — kategori tidak ditemukan.
- `409 Conflict` — kategori masih digunakan oleh menu.

---

# 6. Menu Endpoints

## 6.1 Get Menus

**GET** `/menus`

**Authentication:** Tidak diperlukan  
**Role:** Public

Contoh query:

```text
/menus?category_id=1&available=true
```

### Success — 200

```json
{
  "status": "success",
  "message": "Daftar menu berhasil diambil",
  "data": [
    {
      "id": 1,
      "category_id": 1,
      "name": "Nasi Goreng",
      "description": "Nasi goreng kantin",
      "price": 15000,
      "image_url": "/images/nasi-goreng.jpg",
      "is_available": true
    }
  ]
}
```

### Errors

- `400 Bad Request` — parameter filter tidak valid.
- `500 Internal Server Error` — gagal mengambil data menu.

---

## 6.2 Get Menu Detail

**GET** `/menus/{id}`

**Authentication:** Tidak diperlukan  
**Role:** Public

### Success — 200

```json
{
  "status": "success",
  "message": "Detail menu berhasil diambil",
  "data": {
    "id": 1,
    "category_id": 1,
    "name": "Nasi Goreng",
    "description": "Nasi goreng kantin",
    "price": 15000,
    "is_available": true
  }
}
```

### Errors

- `400 Bad Request` — ID menu tidak valid.
- `404 Not Found` — menu tidak ditemukan.

---

## 6.3 Create Menu

**POST** `/menus`

**Authentication:** Ya  
**Role:** Admin

### Request

```json
{
  "category_id": 1,
  "name": "Nasi Goreng",
  "description": "Nasi goreng kantin",
  "price": 15000,
  "image_url": "/images/nasi-goreng.jpg",
  "is_available": true
}
```

### Success — 201

```json
{
  "status": "success",
  "message": "Menu berhasil ditambahkan",
  "data": {
    "id": 1,
    "name": "Nasi Goreng",
    "price": 15000
  }
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `422 Unprocessable Entity` — data menu tidak valid.

---

## 6.4 Update Menu

**PATCH** `/menus/{id}`

**Authentication:** Ya  
**Role:** Admin

### Request

```json
{
  "price": 17000,
  "is_available": true
}
```

### Success — 200

```json
{
  "status": "success",
  "message": "Menu berhasil diperbarui",
  "data": {
    "id": 1,
    "name": "Nasi Goreng",
    "price": 17000,
    "is_available": true
  }
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `404 Not Found` — menu tidak ditemukan.
- `422 Unprocessable Entity` — data tidak valid.

---

## 6.5 Delete Menu

**DELETE** `/menus/{id}`

**Authentication:** Ya  
**Role:** Admin

### Success — 200

```json
{
  "status": "success",
  "message": "Menu berhasil dihapus",
  "data": null
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `404 Not Found` — menu tidak ditemukan.
- `409 Conflict` — menu memiliki referensi transaksi yang tidak dapat dihapus.

---

# 7. Order Endpoints

## 7.1 Create Order

**POST** `/orders`

**Authentication:** Ya  
**Role:** Customer

### Request

```json
{
  "items": [
    {
      "menu_id": 1,
      "quantity": 2
    },
    {
      "menu_id": 3,
      "quantity": 1
    }
  ],
  "notes": "Tidak pedas"
}
```

### Success — 201

```json
{
  "status": "success",
  "message": "Pesanan berhasil dibuat",
  "data": {
    "id": 10,
    "order_number": "ORD-20261006-001",
    "status": "pending",
    "total_amount": 40000
  }
}
```

### Errors

- `401 Unauthorized` — pengguna belum login.
- `422 Unprocessable Entity` — item kosong, jumlah tidak valid, atau menu tidak tersedia.

---

## 7.2 Get My Orders

**GET** `/orders`

**Authentication:** Ya  
**Role:** Customer

### Success — 200

```json
{
  "status": "success",
  "message": "Daftar pesanan berhasil diambil",
  "data": [
    {
      "id": 10,
      "order_number": "ORD-20261006-001",
      "status": "processing",
      "total_amount": 40000
    }
  ]
}
```

### Errors

- `401 Unauthorized` — pengguna belum login.
- `400 Bad Request` — parameter filter tidak valid.

---

## 7.3 Get Order Detail

**GET** `/orders/{id}`

**Authentication:** Ya  
**Role:** Customer, Admin

### Success — 200

```json
{
  "status": "success",
  "message": "Detail pesanan berhasil diambil",
  "data": {
    "id": 10,
    "order_number": "ORD-20261006-001",
    "status": "processing",
    "total_amount": 40000,
    "items": [
      {
        "menu_id": 1,
        "name": "Nasi Goreng",
        "quantity": 2,
        "unit_price": 15000,
        "subtotal": 30000
      }
    ]
  }
}
```

### Errors

- `403 Forbidden` — customer mencoba melihat pesanan milik pengguna lain.
- `404 Not Found` — pesanan tidak ditemukan.

---

## 7.4 Get All Orders

**GET** `/admin/orders`

**Authentication:** Ya  
**Role:** Admin

### Success — 200

```json
{
  "status": "success",
  "message": "Seluruh pesanan berhasil diambil",
  "data": []
}
```

### Errors

- `401 Unauthorized` — pengguna belum login.
- `403 Forbidden` — pengguna bukan admin.

---

## 7.5 Update Order Status

**PATCH** `/orders/{id}/status`

**Authentication:** Ya  
**Role:** Admin

Status yang digunakan:

`pending` → `processing` → `ready` → `completed`

### Request

```json
{
  "status": "ready"
}
```

### Success — 200

```json
{
  "status": "success",
  "message": "Status pesanan berhasil diperbarui",
  "data": {
    "id": 10,
    "status": "ready"
  }
}
```

Ketika status berubah menjadi `ready`, sistem dapat membuat notifikasi
"Pesanan siap diambil" untuk customer.

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `404 Not Found` — pesanan tidak ditemukan.
- `422 Unprocessable Entity` — transisi status tidak valid.

---

# 8. Payment Endpoints

## 8.1 Create Payment

**POST** `/orders/{id}/payments`

**Authentication:** Ya  
**Role:** Customer

### Request

```json
{
  "payment_method": "qris"
}
```

### Success — 201

```json
{
  "status": "success",
  "message": "Pembayaran berhasil dibuat",
  "data": {
    "id": 5,
    "order_id": 10,
    "payment_method": "qris",
    "amount": 40000,
    "payment_status": "pending"
  }
}
```

### Errors

- `404 Not Found` — pesanan tidak ditemukan.
- `409 Conflict` — pembayaran untuk pesanan sudah dibuat.
- `422 Unprocessable Entity` — metode pembayaran tidak valid.

---

## 8.2 Get Payment

**GET** `/orders/{id}/payment`

**Authentication:** Ya  
**Role:** Customer, Admin

### Success — 200

```json
{
  "status": "success",
  "message": "Data pembayaran berhasil diambil",
  "data": {
    "id": 5,
    "order_id": 10,
    "payment_method": "qris",
    "amount": 40000,
    "payment_status": "paid"
  }
}
```

### Errors

- `403 Forbidden` — tidak memiliki izin melihat pembayaran.
- `404 Not Found` — pembayaran tidak ditemukan.

---

## 8.3 Update Payment Status

**PATCH** `/payments/{id}/status`

**Authentication:** Ya  
**Role:** Admin

### Request

```json
{
  "payment_status": "paid",
  "transaction_reference": "TRX-001"
}
```

### Success — 200

```json
{
  "status": "success",
  "message": "Status pembayaran berhasil diperbarui",
  "data": {
    "id": 5,
    "payment_status": "paid"
  }
}
```

### Errors

- `403 Forbidden` — pengguna bukan admin.
- `404 Not Found` — pembayaran tidak ditemukan.
- `422 Unprocessable Entity` — status pembayaran tidak valid.

---

# 9. Notification Endpoints

## 9.1 Get Notifications

**GET** `/notifications`

**Authentication:** Ya  
**Role:** Customer

### Success — 200

```json
{
  "status": "success",
  "message": "Notifikasi berhasil diambil",
  "data": [
    {
      "id": 1,
      "title": "Pesanan Siap",
      "message": "Pesanan ORD-20261006-001 siap diambil.",
      "is_read": false
    }
  ]
}
```

### Errors

- `401 Unauthorized` — pengguna belum login.
- `400 Bad Request` — parameter filter tidak valid.

---

## 9.2 Mark Notification as Read

**PATCH** `/notifications/{id}/read`

**Authentication:** Ya  
**Role:** Customer

### Request

```json
{
  "is_read": true
}
```

### Success — 200

```json
{
  "status": "success",
  "message": "Notifikasi ditandai telah dibaca",
  "data": {
    "id": 1,
    "is_read": true
  }
}
```

### Errors

- `403 Forbidden` — notifikasi bukan milik pengguna.
- `404 Not Found` — notifikasi tidak ditemukan.

---

# 10. Role-Permission Matrix

| Endpoint | Method | Public | Customer | Admin |
|---|---|:---:|:---:|:---:|
| `/auth/register` | POST | ✓ | ✓ | ✓ |
| `/auth/login` | POST | ✓ | ✓ | ✓ |
| `/auth/me` | GET | ✗ | ✓ | ✓ |
| `/categories` | GET | ✓ | ✓ | ✓ |
| `/categories` | POST | ✗ | ✗ | ✓ |
| `/categories/{id}` | PATCH | ✗ | ✗ | ✓ |
| `/categories/{id}` | DELETE | ✗ | ✗ | ✓ |
| `/menus` | GET | ✓ | ✓ | ✓ |
| `/menus/{id}` | GET | ✓ | ✓ | ✓ |
| `/menus` | POST | ✗ | ✗ | ✓ |
| `/menus/{id}` | PATCH | ✗ | ✗ | ✓ |
| `/menus/{id}` | DELETE | ✗ | ✗ | ✓ |
| `/orders` | POST | ✗ | ✓ | ✗ |
| `/orders` | GET | ✗ | ✓ | ✗ |
| `/orders/{id}` | GET | ✗ | ✓* | ✓ |
| `/admin/orders` | GET | ✗ | ✗ | ✓ |
| `/orders/{id}/status` | PATCH | ✗ | ✗ | ✓ |
| `/orders/{id}/payments` | POST | ✗ | ✓* | ✗ |
| `/orders/{id}/payment` | GET | ✗ | ✓* | ✓ |
| `/payments/{id}/status` | PATCH | ✗ | ✗ | ✓ |
| `/notifications` | GET | ✗ | ✓ | ✗ |
| `/notifications/{id}/read` | PATCH | ✗ | ✓* | ✗ |

`✓*` = customer hanya dapat mengakses resource miliknya sendiri.

---

# 11. Aturan Bisnis

1. Setiap akun memiliki role `customer` atau `admin`.
2. Customer harus login sebelum melakukan pemesanan.
3. Satu order dapat memiliki satu atau lebih `order_items`.
4. Menu yang memiliki `is_available = false` tidak dapat dipesan.
5. Harga pada `order_items.unit_price` merupakan harga menu saat transaksi dibuat.
6. `total_amount` dihitung berdasarkan jumlah subtotal seluruh item.
7. Satu order hanya memiliki satu data pembayaran.
8. Customer hanya dapat melihat pesanan, pembayaran, dan notifikasi miliknya sendiri.
9. Admin dapat mengelola kategori, menu, pesanan, dan status pembayaran.
10. Perubahan status order dicatat pada `order_status_histories`.
11. Ketika status pesanan menjadi `ready`, sistem membuat notifikasi bahwa pesanan siap diambil.

---

# 12. Error Handling

Semua error menggunakan struktur JSON yang konsisten.

### Contoh 401 Unauthorized

```json
{
  "status": "error",
  "message": "Authentication required",
  "errors": {
    "auth": ["Token tidak ditemukan atau tidak valid"]
  }
}
```

### Contoh 403 Forbidden

```json
{
  "status": "error",
  "message": "Anda tidak memiliki izin untuk mengakses resource ini",
  "errors": {
    "authorization": ["Akses ditolak"]
  }
}
```

### Contoh 404 Not Found

```json
{
  "status": "error",
  "message": "Data tidak ditemukan",
  "errors": {
    "resource": ["Resource yang diminta tidak tersedia"]
  }
}
```

### Contoh 422 Validation Error

```json
{
  "status": "error",
  "message": "Validasi gagal",
  "errors": {
    "quantity": ["Quantity minimal 1"]
  }
}
```

---

# 13. Changelog

## Version 1.0.0

- Membuat API Contract awal KantinKu.
- Menambahkan autentikasi pengguna.
- Menambahkan manajemen kategori dan menu.
- Menambahkan proses pemesanan dan detail pesanan.
- Menambahkan pembayaran.
- Menambahkan perubahan status pesanan.
- Menambahkan notifikasi pesanan siap diambil.
- Menambahkan standard JSON response.
- Menambahkan error handling.
- Menambahkan role-permission matrix.

---

**Dokumen:** API Contract KantinKu  
**Versi:** 1.0.0  
**Status:** Draft System Design
