# Synchronous & Asynchronous JavaScript

Repository ini berisi catatan dan praktik sederhana mengenai **Synchronous** dan **Asynchronous** pada JavaScript.

Materi yang dipelajari meliputi:

- Pengertian Synchronous
- Pengertian Asynchronous
- Perbedaan Synchronous dan Asynchronous
- Contoh Synchronous dan Asynchronous
- Callback
- Promise
- Async/Await

---

## 1. Synchronous

**Synchronous** adalah cara kerja JavaScript yang menjalankan kode **secara berurutan**.

Kode berikutnya akan menunggu kode sebelumnya selesai dijalankan.

Contoh:

```javascript
console.log("Pertama");
console.log("Kedua");
console.log("Ketiga");
```

Output:

```text
Pertama
Kedua
Ketiga
```

Artinya, JavaScript menjalankan kode dari atas ke bawah sesuai urutan.

### Gambaran sederhana

```text
Proses 1
   ↓
Proses 2
   ↓
Proses 3
```

Proses berikutnya menunggu proses sebelumnya selesai.

---

## 2. Asynchronous

**Asynchronous** adalah cara kerja JavaScript yang memungkinkan suatu proses berjalan tanpa harus menghentikan proses lainnya.

Contoh sederhana menggunakan `setTimeout()`:

```javascript
console.log("Pertama");

setTimeout(() => {
  console.log("Kedua");
}, 2000);

console.log("Ketiga");
```

Output:

```text
Pertama
Ketiga
Kedua
```

`setTimeout()` menunda kode di dalamnya selama 2 detik.

JavaScript tidak berhenti menunggu selama 2 detik, tetapi langsung menjalankan kode berikutnya.

### Gambaran sederhana

```text
Proses 1 ──────────────── selesai
   ↓
Proses 2 → menunggu

Proses 3 ──────────────── berjalan
```

---

## 3. Perbedaan Synchronous dan Asynchronous

| Synchronous                        | Asynchronous                              |
| ---------------------------------- | ----------------------------------------- |
| Berjalan secara berurutan          | Tidak selalu berjalan secara berurutan    |
| Menunggu proses sebelumnya selesai | Tidak harus menunggu proses sebelumnya    |
| Lebih sederhana                    | Cocok untuk proses yang membutuhkan waktu |
| Kode berikutnya menunggu           | Kode berikutnya dapat tetap berjalan      |
| Contoh: `console.log()`            | Contoh: `setTimeout()`                    |

### Contoh perbandingan

#### Synchronous

```javascript
console.log("A");
console.log("B");
console.log("C");
```

Output:

```text
A
B
C
```

#### Asynchronous

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 2000);

console.log("C");
```

Output:

```text
A
C
B
```

---

## 4. Contoh Asynchronous

Beberapa contoh proses asynchronous dalam JavaScript:

- `setTimeout()`
- `setInterval()`
- Fetch API
- Promise
- Async/Await
- Event Listener

Contoh sederhana:

```javascript
console.log("Mulai");

setTimeout(() => {
  console.log("Proses selesai");
}, 3000);

console.log("Program tetap berjalan");
```

Output:

```text
Mulai
Program tetap berjalan
Proses selesai
```

Pesan `"Program tetap berjalan"` muncul lebih dahulu karena JavaScript tidak menunggu `setTimeout()` selesai.

---

# 5. Tiga Cara Menulis Kode Asynchronous di JavaScript

Ada tiga cara utama yang perlu dipahami:

1. Callback
2. Promise
3. Async/Await

---

## A. Callback

**Callback** adalah sebuah fungsi yang diberikan sebagai argument ke fungsi lain untuk dijalankan nanti.

Contoh:

```javascript
setTimeout(() => {
  console.log("Proses selesai");
}, 2000);
```

Fungsi:

```javascript
() => {
  console.log("Proses selesai");
};
```

merupakan callback yang akan dijalankan setelah 2 detik.

Contoh callback sederhana:

```javascript
function proses(callback) {
  console.log("Proses dimulai");

  setTimeout(() => {
    callback();
  }, 2000);
}

proses(() => {
  console.log("Proses selesai");
});
```

Output:

```text
Proses dimulai
Proses selesai
```

---

## B. Promise

**Promise** adalah object yang digunakan untuk menangani hasil dari proses asynchronous.

Promise memiliki tiga kondisi:

| Kondisi     | Pengertian            |
| ----------- | --------------------- |
| `pending`   | Proses masih berjalan |
| `fulfilled` | Proses berhasil       |
| `rejected`  | Proses gagal          |

Contoh:

```javascript
const data = new Promise((resolve, reject) => {
  resolve("Data berhasil didapatkan");
});

data.then((hasil) => {
  console.log(hasil);
});
```

Output:

```text
Data berhasil didapatkan
```

### Contoh Promise dengan `setTimeout()`

```javascript
const ambilData = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Data berhasil diambil");
  }, 2000);
});

ambilData.then((hasil) => {
  console.log(hasil);
});
```

Setelah 2 detik:

```text
Data berhasil diambil
```

---

## C. Async/Await

`async` dan `await` digunakan untuk membuat kode asynchronous menjadi lebih mudah dibaca.

Contoh:

```javascript
function ambilData() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("Data berhasil diambil");
    }, 2000);
  });
}

async function jalankan() {
  const hasil = await ambilData();

  console.log(hasil);
}

jalankan();
```

`await` digunakan untuk menunggu Promise selesai.

### Cara membacanya

```javascript
const hasil = await ambilData();
```

Kurang lebih dapat dipahami sebagai:

> "Tunggu sampai `ambilData()` selesai, lalu simpan hasilnya ke dalam `hasil`."

---

# 6. Perbandingan Callback, Promise, dan Async/Await

| Cara        | Keterangan                           |
| ----------- | ------------------------------------ |
| Callback    | Menggunakan fungsi sebagai callback  |
| Promise     | Menggunakan `.then()` dan `.catch()` |
| Async/Await | Menggunakan `async` dan `await`      |

Contoh sederhana:

### Callback

```javascript
setTimeout(() => {
  console.log("Selesai");
}, 2000);
```

### Promise

```javascript
const proses = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Selesai");
  }, 2000);
});

proses.then((hasil) => {
  console.log(hasil);
});
```

### Async/Await

```javascript
async function jalankan() {
  const hasil = await proses;

  console.log(hasil);
}

jalankan();
```

---

# 7. Praktik Synchronous vs Asynchronous

Pada repository ini terdapat file praktik:

```text
index.html
└── js/
    └── sync-async.js
```

Buka `index.html` menggunakan browser.

Kemudian buka:

```text
F12 → Console
```

Untuk melihat hasil dari program.

---

## Praktik 1 — Synchronous

```javascript
console.log("Pertama");
console.log("Kedua");
console.log("Ketiga");
```

Hasil:

```text
Pertama
Kedua
Ketiga
```

---

## Praktik 2 — Asynchronous

```javascript
console.log("Pertama");

setTimeout(() => {
  console.log("Kedua");
}, 2000);

console.log("Ketiga");
```

Hasil:

```text
Pertama
Ketiga
Kedua
```

Perhatikan bahwa `"Kedua"` muncul terakhir walaupun ditulis sebelum `"Ketiga"`.

---

# 8. Kesimpulan

### Synchronous

> Jalankan proses → tunggu selesai → lanjut ke proses berikutnya.

### Asynchronous

> Jalankan proses → proses dapat menunggu → kode lain tetap dapat berjalan.

Tiga cara utama menangani asynchronous JavaScript:

1. **Callback**
2. **Promise**
3. **Async/Await**

Untuk memahami asynchronous JavaScript, pahami terlebih dahulu contoh sederhana menggunakan `setTimeout()` sebelum masuk ke Promise dan Async/Await.
