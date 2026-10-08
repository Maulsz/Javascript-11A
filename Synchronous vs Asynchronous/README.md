# Synchronous & Asynchronous JavaScript

Repository ini berisi catatan dan praktik sederhana mengenai **Synchronous** dan **Asynchronous** pada JavaScript, mulai dari dasar sampai Fetch API.

## Daftar Isi

1. [Synchronous](#1-synchronous)
2. [Asynchronous](#2-asynchronous)
3. [Perbedaan Synchronous dan Asynchronous](#3-perbedaan-synchronous-dan-asynchronous)
4. [Contoh Proses Asynchronous](#4-contoh-proses-asynchronous)
5. [Tiga Cara Menulis Kode Asynchronous](#5-tiga-cara-menulis-kode-asynchronous)
6. [Callback Function dan Callback Hell](#6-callback-function-dan-callback-hell)
7. [Promise: Konsep dan Cara Membuatnya](#7-promise-konsep-dan-cara-membuatnya)
8. [Promise: then, catch, dan finally](#8-promise-then-catch-dan-finally)
9. [Promise.all, race, allSettled, any](#9-promiseall-race-allsettled-any)
10. [Promise.withResolvers (ES2024)](#10-promisewithresolvers-es2024)
11. [Async dan Await](#11-async-dan-await)
12. [Error Handling di Async Function](#12-error-handling-di-async-function)
13. [Event Loop dan Cara Kerjanya](#13-event-loop-dan-cara-kerjanya)
14. [Timer: setTimeout dan setInterval](#14-timer-settimeout-dan-setinterval)
15. [Fetch API: Ambil Data dari Server](#15-fetch-api-ambil-data-dari-server)
16. [Fetch: POST, PUT, PATCH, dan DELETE](#16-fetch-post-put-patch-dan-delete)
17. [Error Handling Fetch dan Network](#17-error-handling-fetch-dan-network)
18. [Cara Menjalankan Semua Contoh](#18-cara-menjalankan-semua-contoh)
19. [Kesimpulan](#19-kesimpulan)

---

## 1. Synchronous

**Synchronous** adalah cara kerja JavaScript yang menjalankan kode **secara berurutan**. Kode berikutnya akan menunggu kode sebelumnya selesai dijalankan.

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

Gambaran sederhana:

```text
Proses 1
   ↓
Proses 2
   ↓
Proses 3
```

---

## 2. Asynchronous

**Asynchronous** adalah cara kerja JavaScript yang memungkinkan suatu proses berjalan tanpa harus menghentikan proses lainnya.

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

`setTimeout()` menunda kode di dalamnya selama 2 detik. JavaScript tidak berhenti menunggu, tetapi langsung menjalankan kode berikutnya.

Gambaran sederhana:

```text
Proses 1 → jalan → selesai
Proses 2 → dititipkan (menunggu 2 detik) ........→ selesai
Proses 3 → jalan → selesai
```

Analogi: kamu memesan kopi di kafe. Setelah memesan, kamu tidak berdiri diam menunggu di kasir, tetapi duduk dan main HP. Saat kopi siap, kamu dipanggil. Itulah asynchronous.

---

## 3. Perbedaan Synchronous dan Asynchronous

| Synchronous                        | Asynchronous                           |
| ---------------------------------- | -------------------------------------- |
| Berjalan secara berurutan          | Tidak selalu berjalan secara berurutan |
| Menunggu proses sebelumnya selesai | Tidak harus menunggu proses sebelumnya |
| Lebih sederhana                    | Cocok untuk proses yang butuh waktu    |
| Kode berikutnya menunggu           | Kode berikutnya tetap berjalan         |
| Contoh: `console.log()`            | Contoh: `setTimeout()`                 |

Synchronous:

```javascript
console.log("A");
console.log("B");
console.log("C");
// A B C
```

Asynchronous:

```javascript
console.log("A");
setTimeout(() => console.log("B"), 2000);
console.log("C");
// A C B
```

---

## 4. Contoh Proses Asynchronous

Beberapa proses asynchronous di JavaScript:

- `setTimeout()` dan `setInterval()`
- Fetch API (mengambil data dari server)
- Promise
- Async/Await
- Event Listener (klik, input, dll.)

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

---

## 5. Tiga Cara Menulis Kode Asynchronous

| Cara        | Keterangan                           |
| ----------- | ------------------------------------ |
| Callback    | Fungsi yang dikirim ke fungsi lain   |
| Promise     | Menggunakan `.then()` dan `.catch()` |
| Async/Await | Menggunakan `async` dan `await`      |

Ketiganya melakukan hal yang sama. Yang berbeda adalah **cara menulis** dan **kemudahan membacanya**. Urutan evolusinya: Callback → Promise → Async/Await.

---

## 6. Callback Function dan Callback Hell

### Penjelasan

**Callback** adalah fungsi yang dikirim sebagai argumen ke fungsi lain, untuk dijalankan **nanti** (misalnya setelah proses selesai).

Callback tidak selalu asynchronous. `forEach`, `map`, dan `filter` juga memakai callback, tetapi berjalan synchronous.

### Penggunaan

```javascript
// Callback sederhana
function sapa(nama, callback) {
  console.log(`Halo, ${nama}!`);
  callback();
}

sapa("Budi", () => {
  console.log("Callback dipanggil setelah menyapa");
});
```

Callback asynchronous dengan pola **error-first** (konvensi Node.js: argumen pertama adalah error, kedua adalah hasil):

```javascript
function bagi(a, b, callback) {
  setTimeout(() => {
    if (b === 0) {
      return callback(new Error("Tidak bisa dibagi 0"));
    }
    callback(null, a / b);
  }, 500);
}

bagi(10, 2, (err, hasil) => {
  if (err) return console.log("Error:", err.message);
  console.log("Hasil:", hasil); // 5
});
```

### Callback Hell

**Callback Hell** (disebut juga _pyramid of doom_) terjadi saat ada banyak callback bersarang di dalam callback. Kode bergeser terus ke kanan, sulit dibaca, dan sulit menangani error.

```javascript
ambilUser(1, (user) => {
  ambilPostingan(user.id, (posts) => {
    ambilKomentar(posts[0].id, (komentar) => {
      console.log(komentar);
      // makin dalam... makin pusing
    });
  });
});
```

Solusinya: gunakan **Promise** atau **Async/Await** (dibahas di bagian berikutnya).

### Cara menjalankan

Lihat [bagian 18](#18-cara-menjalankan-semua-contoh). Nama demo: `demo.callback()` dan `demo.callbackHell()`.

---

## 7. Promise: Konsep dan Cara Membuatnya

### Penjelasan

**Promise** adalah object yang mewakili hasil dari proses asynchronous yang **belum selesai sekarang, tetapi akan selesai nanti**.

Analogi: kamu pesan makanan online dan dapat struk pesanan. Struk itu adalah "janji" (promise). Hasilnya bisa:

| Kondisi     | Pengertian                            |
| ----------- | ------------------------------------- |
| `pending`   | Proses masih berjalan                 |
| `fulfilled` | Proses berhasil (`resolve` dipanggil) |
| `rejected`  | Proses gagal (`reject` dipanggil)     |

Sekali Promise berubah dari `pending` ke `fulfilled` atau `rejected`, statusnya **tidak bisa berubah lagi**.

### Cara membuat

```javascript
const janji = new Promise((resolve, reject) => {
  // lakukan proses di sini
  // berhasil → resolve(nilai)
  // gagal    → reject(error)
});
```

### Penggunaan

```javascript
function cekUmur(umur) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (umur >= 18) {
        resolve("Boleh masuk");
      } else {
        reject(new Error("Umur belum cukup"));
      }
    }, 1000);
  });
}

cekUmur(20)
  .then((hasil) => console.log(hasil)) // Boleh masuk
  .catch((err) => console.log(err.message));

cekUmur(15)
  .then((hasil) => console.log(hasil))
  .catch((err) => console.log(err.message)); // Umur belum cukup
```

Membuat Promise yang langsung selesai:

```javascript
Promise.resolve("langsung berhasil");
Promise.reject(new Error("langsung gagal"));
```

> Tips: selalu `reject` dengan `new Error(...)`, bukan string biasa, supaya ada informasi _stack trace_.

### Cara menjalankan

`demo.promiseDasar()`

---

## 8. Promise: then, catch, dan finally

### Penjelasan

| Method       | Kapan dijalankan                                 |
| ------------ | ------------------------------------------------ |
| `.then()`    | Saat Promise **berhasil**                        |
| `.catch()`   | Saat Promise **gagal** (atau ada error di chain) |
| `.finally()` | **Selalu** dijalankan, berhasil maupun gagal     |

### Penggunaan

```javascript
ambilData()
  .then((hasil) => {
    console.log("Berhasil:", hasil);
  })
  .catch((err) => {
    console.log("Gagal:", err.message);
  })
  .finally(() => {
    console.log("Selesai (berhasil atau gagal)");
  });
```

`finally` biasanya dipakai untuk hal seperti menyembunyikan loading spinner.

### Promise Chaining

`.then()` mengembalikan Promise baru, sehingga bisa dirantai. Nilai yang di-`return` akan diteruskan ke `.then()` berikutnya.

```javascript
Promise.resolve(2)
  .then((n) => n * 2) // 4
  .then((n) => n + 1) // 5
  .then((n) => {
    console.log(n); // 5
  });
```

Jika `.then()` mengembalikan Promise lain, rantai akan menunggu Promise itu selesai. Inilah cara Promise menggantikan Callback Hell:

```javascript
ambilUser(1)
  .then((user) => ambilPostingan(user.id))
  .then((posts) => ambilKomentar(posts[0].id))
  .then((komentar) => console.log(komentar))
  .catch((err) => console.log("Error:", err.message));
```

Satu `.catch()` di akhir sudah menangkap error dari semua langkah di atasnya.

### Cara menjalankan

`demo.promiseThenCatchFinally()` dan `demo.keluarDariCallbackHell()`

---

## 9. Promise.all, race, allSettled, any

### Penjelasan

Method ini dipakai untuk menjalankan **beberapa Promise sekaligus** (paralel).

| Method               | Selesai ketika...                         | Hasil                                                                 |
| -------------------- | ----------------------------------------- | --------------------------------------------------------------------- |
| `Promise.all`        | **Semua** berhasil                        | Array semua hasil. Gagal jika **satu** gagal                          |
| `Promise.race`       | **Yang pertama** selesai (berhasil/gagal) | Hasil Promise tercepat                                                |
| `Promise.allSettled` | **Semua** selesai (apa pun hasilnya)      | Array `{status, value/reason}`. Tidak pernah gagal                    |
| `Promise.any`        | **Yang pertama berhasil**                 | Hasil berhasil pertama. Gagal jika **semua** gagal (`AggregateError`) |

Contoh helper untuk semua contoh di bawah:

```javascript
const tugas = (nama, ms, gagal = false) =>
  new Promise((resolve, reject) => {
    setTimeout(() => {
      gagal ? reject(new Error(`${nama} gagal`)) : resolve(`${nama} selesai`);
    }, ms);
  });
```

### Promise.all

Cocok jika semua data dibutuhkan sekaligus.

```javascript
const hasil = await Promise.all([
  tugas("A", 1000),
  tugas("B", 2000),
  tugas("C", 500),
]);
console.log(hasil); // ["A selesai", "B selesai", "C selesai"]
// Total waktu ≈ 2 detik (bukan 3,5 detik), karena berjalan bersamaan
```

Jika salah satu gagal, seluruhnya gagal:

```javascript
try {
  await Promise.all([tugas("A", 500), tugas("B", 300, true)]);
} catch (err) {
  console.log(err.message); // B gagal
}
```

### Promise.race

Cocok untuk membuat **timeout**.

```javascript
const hasil = await Promise.race([tugas("Lambat", 3000), tugas("Cepat", 500)]);
console.log(hasil); // Cepat selesai
```

### Promise.allSettled

Cocok jika kamu ingin tahu hasil **semua** tugas walaupun ada yang gagal.

```javascript
const hasil = await Promise.allSettled([
  tugas("A", 500),
  tugas("B", 300, true),
]);
console.log(hasil);
// [
//   { status: "fulfilled", value: "A selesai" },
//   { status: "rejected",  reason: Error: B gagal }
// ]
```

### Promise.any

Cocok jika ada beberapa sumber data dan kamu hanya butuh yang pertama berhasil (misalnya server cadangan).

```javascript
const hasil = await Promise.any([
  tugas("Server 1", 1000, true),
  tugas("Server 2", 2000),
  tugas("Server 3", 500, true),
]);
console.log(hasil); // Server 2 selesai
```

Jika semua gagal:

```javascript
try {
  await Promise.any([tugas("A", 100, true), tugas("B", 200, true)]);
} catch (err) {
  console.log(err.name); // AggregateError
  console.log(err.errors); // array semua error
}
```

### Cara menjalankan

`demo.promiseAll()`, `demo.promiseRace()`, `demo.promiseAllSettled()`, `demo.promiseAny()`

---

## 10. Promise.withResolvers (ES2024)

### Penjelasan

Biasanya `resolve` dan `reject` hanya bisa diakses **di dalam** constructor Promise. `Promise.withResolvers()` mengembalikan tiga hal sekaligus: `promise`, `resolve`, dan `reject`, sehingga bisa dipakai di luar.

Tanpa `withResolvers` (cara lama):

```javascript
let resolve, reject;
const promise = new Promise((res, rej) => {
  resolve = res;
  reject = rej;
});
```

Dengan `withResolvers` (lebih singkat):

```javascript
const { promise, resolve, reject } = Promise.withResolvers();
```

### Penggunaan

Cocok jika Promise diselesaikan oleh event lain, misalnya klik tombol atau pesan WebSocket.

```javascript
const { promise, resolve } = Promise.withResolvers();

promise.then((pesan) => console.log("Diterima:", pesan));

// di tempat lain, nanti...
setTimeout(() => resolve("Halo dari luar!"), 1000);
```

> **Catatan dukungan browser:** butuh browser modern (Chrome 119+, Firefox 121+, Safari 17.4+, Node.js 22+). Jika browser kamu lama, fungsi ini tidak ada. Contoh di `index.html` sudah memeriksanya dan memberi pesan jika tidak didukung.

### Cara menjalankan

`demo.promiseWithResolvers()`

---

## 11. Async dan Await

### Penjelasan

`async` dan `await` adalah cara menulis Promise agar terlihat seperti kode synchronous.

- `async` di depan function → function itu **selalu mengembalikan Promise**.
- `await` → menunggu Promise selesai, lalu mengambil hasilnya. Hanya boleh dipakai di dalam `async function` (atau di level atas _module_).

`await` hanya menunda function `async` itu sendiri, **bukan** seluruh program.

### Penggunaan

```javascript
function ambilData() {
  return new Promise((resolve) => {
    setTimeout(() => resolve("Data berhasil diambil"), 1000);
  });
}

async function jalankan() {
  console.log("Mulai");
  const hasil = await ambilData(); // tunggu 1 detik
  console.log(hasil);
  console.log("Selesai");
}

jalankan();
console.log("Kode di luar tetap jalan");
```

Output:

```text
Mulai
Kode di luar tetap jalan
Data berhasil diambil
Selesai
```

### Berurutan vs Paralel

Hati-hati, `await` satu per satu membuat proses berjalan **berurutan**:

```javascript
// Berurutan: total ≈ 3 detik
const a = await tugas("A", 1000);
const b = await tugas("B", 2000);
```

Jika kedua tugas tidak saling bergantung, jalankan **paralel**:

```javascript
// Paralel: total ≈ 2 detik
const [a, b] = await Promise.all([tugas("A", 1000), tugas("B", 2000)]);
```

### Cara menjalankan

`demo.asyncAwait()`

---

## 12. Error Handling di Async Function

### Penjelasan

Pada `async/await`, error ditangani dengan **try / catch / finally**, sama seperti kode biasa. Jika Promise yang di-`await` gagal (`reject`), ia akan "dilempar" sebagai error.

### Penggunaan

```javascript
async function proses() {
  try {
    const hasil = await cekUmur(15); // akan reject
    console.log(hasil);
  } catch (err) {
    console.log("Error ditangkap:", err.message);
  } finally {
    console.log("Selalu dijalankan");
  }
}

proses();
```

Async function yang melempar error mengembalikan Promise yang `rejected`, sehingga bisa ditangkap dari luar:

```javascript
async function gagal() {
  throw new Error("Ups!");
}

gagal().catch((err) => console.log(err.message)); // Ups!
```

Menangani beberapa tugas, tidak peduli ada yang gagal:

```javascript
const hasil = await Promise.allSettled([
  tugas("A", 100),
  tugas("B", 100, true),
]);
hasil.forEach((h) => {
  if (h.status === "fulfilled") console.log("OK:", h.value);
  else console.log("Gagal:", h.reason.message);
});
```

### Kesalahan umum

1. **Lupa `await`** → hasilnya Promise, bukan nilai.

   ```javascript
   const hasil = ambilData(); // ❌ Promise { <pending> }
   const hasil = await ambilData(); // ✅ nilai sebenarnya
   ```

2. **Lupa try/catch** → muncul `Uncaught (in promise)` di console.
3. **`await` di dalam `forEach`** tidak bekerja seperti yang diharapkan. Gunakan `for...of` (berurutan) atau `Promise.all` + `map` (paralel).

### Cara menjalankan

`demo.errorHandlingAsync()`

---

## 13. Event Loop dan Cara Kerjanya

### Penjelasan

JavaScript hanya punya **satu thread** (hanya bisa mengerjakan satu hal pada satu waktu). Lalu bagaimana `setTimeout` bisa menunggu tanpa menghentikan program? Jawabannya: **Event Loop**.

Komponennya:

| Komponen                   | Fungsi                                                                      |
| -------------------------- | --------------------------------------------------------------------------- |
| **Call Stack**             | Tempat kode dijalankan, satu per satu                                       |
| **Web APIs**               | Fitur browser (timer, fetch, event) yang bekerja di luar JavaScript         |
| **Microtask Queue**        | Antrean prioritas tinggi: `.then()`, `catch`, `await`, `queueMicrotask`     |
| **Task Queue** (Macrotask) | Antrean biasa: `setTimeout`, `setInterval`, event klik                      |
| **Event Loop**             | Petugas yang memindahkan tugas dari antrean ke Call Stack saat stack kosong |

```text
   Call Stack  ←─────── Event Loop ←─── Microtask Queue  (didahulukan)
                           ↑        ←─── Task Queue
                           │
                       Web APIs (setTimeout, fetch, ...)
```

Aturan urutannya:

1. Jalankan semua kode **synchronous** sampai Call Stack kosong.
2. Jalankan **semua microtask** (Promise).
3. Ambil **satu macrotask** (misalnya `setTimeout`), jalankan.
4. Ulangi dari langkah 2.

### Penggunaan

Tebak urutan outputnya:

```javascript
console.log("1. Sync awal");

setTimeout(() => console.log("2. setTimeout 0 ms (macrotask)"), 0);

Promise.resolve().then(() => console.log("3. Promise.then (microtask)"));

queueMicrotask(() => console.log("4. queueMicrotask (microtask)"));

console.log("5. Sync akhir");
```

Output sebenarnya:

```text
1. Sync awal
5. Sync akhir
3. Promise.then (microtask)
4. queueMicrotask (microtask)
2. setTimeout 0 ms (macrotask)
```

Penjelasan: `setTimeout(..., 0)` **bukan** berarti langsung jalan. Ia tetap harus antre di Task Queue dan menunggu semua kode sync dan microtask selesai.

> Peringatan: kode synchronous yang berat (misalnya loop jutaan kali) akan **memblokir** Event Loop sehingga halaman terasa freeze, karena tidak ada kesempatan bagi antrean untuk berjalan.

### Cara menjalankan

`demo.eventLoop()`

---

## 14. Timer: setTimeout dan setInterval

### Penjelasan

| Fungsi        | Kegunaan                                    | Menghentikan    |
| ------------- | ------------------------------------------- | --------------- |
| `setTimeout`  | Menjalankan kode **satu kali** setelah jeda | `clearTimeout`  |
| `setInterval` | Menjalankan kode **berulang** tiap jeda     | `clearInterval` |

Waktu dalam **milidetik** (1000 ms = 1 detik). Keduanya mengembalikan **ID** yang dipakai untuk membatalkan timer.

### setTimeout

```javascript
setTimeout(() => {
  console.log("Muncul setelah 2 detik");
}, 2000);

// Dengan argumen tambahan
setTimeout((nama) => console.log(`Halo, ${nama}`), 1000, "Budi");

// Dibatalkan sebelum sempat jalan
const id = setTimeout(() => console.log("Tidak akan muncul"), 3000);
clearTimeout(id);
```

### setInterval

```javascript
let hitung = 0;

const id = setInterval(() => {
  hitung++;
  console.log("Hitungan:", hitung);

  if (hitung === 3) {
    clearInterval(id); // WAJIB dihentikan, kalau tidak jalan selamanya
    console.log("Interval dihentikan");
  }
}, 1000);
```

### Tips

- **Selalu simpan ID** dan hentikan `setInterval` jika sudah tidak dibutuhkan, agar tidak terjadi kebocoran memori.
- Jeda pada timer adalah **waktu minimum**, bukan jaminan tepat waktu (karena harus menunggu Call Stack kosong).
- Mengubah `setTimeout` menjadi Promise agar bisa di-`await`:

```javascript
const tunda = (ms) => new Promise((resolve) => setTimeout(resolve, ms));

async function contoh() {
  console.log("Mulai");
  await tunda(2000);
  console.log("2 detik kemudian");
}
```

### Cara menjalankan

`demo.timer()`

---

## 15. Fetch API: Ambil Data dari Server

### Penjelasan

**Fetch API** adalah fitur bawaan browser untuk melakukan request HTTP ke server. `fetch()` mengembalikan **Promise**, sehingga bisa memakai `.then()` atau `async/await`.

Alurnya dua tahap:

1. `fetch(url)` → menghasilkan object **Response** (header dan status sudah datang).
2. `response.json()` → membaca body menjadi object JavaScript (ini juga Promise).

Properti penting pada Response:

| Properti / Method | Keterangan                       |
| ----------------- | -------------------------------- |
| `response.ok`     | `true` jika status 200–299       |
| `response.status` | Kode status (200, 404, 500, ...) |
| `response.json()` | Baca body sebagai JSON           |
| `response.text()` | Baca body sebagai teks           |

Pada contoh ini dipakai API gratis untuk latihan: **JSONPlaceholder** (`https://jsonplaceholder.typicode.com`). Butuh koneksi internet.

### Penggunaan

Dengan `.then()`:

```javascript
fetch("https://jsonplaceholder.typicode.com/posts/1")
  .then((response) => response.json())
  .then((data) => console.log(data))
  .catch((err) => console.log("Error:", err.message));
```

Dengan `async/await` (lebih disarankan):

```javascript
async function ambilPost() {
  const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  const data = await response.json();
  console.log(data);
}

ambilPost();
```

Mengambil banyak data dengan query string:

```javascript
const params = new URLSearchParams({ userId: 1, _limit: 3 });

const response = await fetch(
  `https://jsonplaceholder.typicode.com/posts?${params}`,
);
const daftar = await response.json();

daftar.forEach((post) => console.log(post.id, post.title));
```

### Cara menjalankan

`demo.fetchGet()`

---

## 16. Fetch: POST, PUT, PATCH, dan DELETE

### Penjelasan

Secara bawaan `fetch` memakai method **GET**. Untuk method lain, kirim option kedua:

```javascript
fetch(url, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ ... }),
});
```

| Method   | Kegunaan                   | Body? |
| -------- | -------------------------- | ----- |
| `GET`    | Mengambil data             | Tidak |
| `POST`   | **Membuat** data baru      | Ya    |
| `PUT`    | **Mengganti seluruh** data | Ya    |
| `PATCH`  | **Mengubah sebagian** data | Ya    |
| `DELETE` | **Menghapus** data         | Tidak |

Hal penting:

- `headers: { "Content-Type": "application/json" }` memberi tahu server bahwa body berupa JSON.
- `body` harus berupa string, jadi object diubah dengan `JSON.stringify()`.
- JSONPlaceholder hanya **mensimulasikan** perubahan, data aslinya tidak benar-benar berubah. Namun responsnya sama seperti API sungguhan.

### POST (membuat data)

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/posts", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    title: "Belajar Fetch",
    body: "Fetch itu mudah",
    userId: 1,
  }),
});

console.log(response.status); // 201 (Created)
console.log(await response.json()); // data baru beserta id
```

### PUT (ganti seluruh data)

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PUT",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    id: 1,
    title: "Judul baru",
    body: "Isi baru",
    userId: 1,
  }),
});

console.log(await response.json());
```

### PATCH (ubah sebagian)

Cukup kirim field yang berubah saja.

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "PATCH",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ title: "Hanya judul yang berubah" }),
});

console.log(await response.json());
```

### DELETE (hapus data)

```javascript
const response = await fetch("https://jsonplaceholder.typicode.com/posts/1", {
  method: "DELETE",
});

console.log(response.status); // 200
console.log(response.ok); // true
```

### PUT vs PATCH

Bayangkan data `{ nama: "Budi", umur: 20 }`:

- `PUT` dengan `{ nama: "Andi" }` → data menjadi `{ nama: "Andi" }` (umur hilang).
- `PATCH` dengan `{ nama: "Andi" }` → data menjadi `{ nama: "Andi", umur: 20 }`.

### Cara menjalankan

`demo.fetchPost()`, `demo.fetchPut()`, `demo.fetchPatch()`, `demo.fetchDelete()`

---

## 17. Error Handling Fetch dan Network

### Penjelasan

Ini jebakan paling umum: **`fetch` TIDAK reject saat server membalas status error** (404, 500, dll.).

| Situasi                                   | Hasil `fetch`                                   |
| ----------------------------------------- | ----------------------------------------------- |
| Internet mati, domain salah, CORS ditolak | **Reject** (masuk `catch`) dengan `TypeError`   |
| Server membalas 404 / 500                 | **Resolve** (tidak masuk `catch`), `ok = false` |

Maka kita harus memeriksa `response.ok` sendiri.

### 1. Error HTTP (404, 500)

```javascript
async function ambilData(url) {
  const response = await fetch(url);

  if (!response.ok) {
    throw new Error(`HTTP error! Status: ${response.status}`);
  }

  return await response.json();
}

try {
  await ambilData("https://jsonplaceholder.typicode.com/posts/99999");
} catch (err) {
  console.log(err.message); // HTTP error! Status: 404
}
```

### 2. Error jaringan (offline, domain salah)

```javascript
try {
  await fetch("https://domain-yang-tidak-ada.invalid/data");
} catch (err) {
  console.log(err.name); // TypeError
  console.log(err.message); // Failed to fetch
}
```

### 3. Timeout dengan AbortController

`fetch` tidak punya timeout bawaan. Gunakan `AbortController` untuk membatalkan request yang terlalu lama.

```javascript
async function fetchDenganTimeout(url, ms = 5000) {
  const controller = new AbortController();
  const timer = setTimeout(() => controller.abort(), ms);

  try {
    const response = await fetch(url, { signal: controller.signal });
    return response;
  } finally {
    clearTimeout(timer); // bersihkan timer
  }
}

try {
  await fetchDenganTimeout("https://jsonplaceholder.typicode.com/posts", 1); // 1 ms
} catch (err) {
  if (err.name === "AbortError") {
    console.log("Request dibatalkan karena terlalu lama");
  } else {
    console.log("Error lain:", err.message);
  }
}
```

### 4. Helper lengkap (siap dipakai ulang)

```javascript
async function fetchJson(url, options = {}) {
  try {
    const response = await fetch(url, options);

    if (!response.ok) {
      throw new Error(`HTTP ${response.status} ${response.statusText}`);
    }

    return await response.json();
  } catch (err) {
    if (err instanceof TypeError) {
      throw new Error("Masalah jaringan. Cek koneksi internet kamu.");
    }
    throw err;
  }
}
```

### Ringkasan

| Jenis error      | Cara mendeteksi                     |
| ---------------- | ----------------------------------- |
| Error HTTP       | `if (!response.ok)`                 |
| Error jaringan   | `catch`, `err instanceof TypeError` |
| Timeout/batal    | `err.name === "AbortError"`         |
| JSON tidak valid | `catch` dari `response.json()`      |

### Cara menjalankan

`demo.fetchError()`

---

## 18. Cara Menjalankan Semua Contoh

Struktur file:

```text
index.html     ← berisi semua contoh (hanya <script>)
README.md      ← catatan ini
```

Langkah:

1. Simpan `index.html` dan `README.md` dalam satu folder.
2. Buka `index.html` di browser (klik dua kali, atau klik kanan → _Open with Live Server_ di VS Code).
3. Tekan **F12 → tab Console**.
4. Semua demo akan berjalan **berurutan otomatis**, lengkap dengan judul tiap bagian.

### Menjalankan demo tertentu saja

**Cara 1: lewat Console.** Ketik nama demo, lalu Enter:

```javascript
demo.callbackHell();
demo.promiseAll();
demo.fetchGet();
```

**Cara 2: lewat file.** Di `index.html`, isi daftar `DEMO_YANG_DIJALANKAN`:

```javascript
const DEMO_YANG_DIJALANKAN = ["promiseAll", "eventLoop"];
// kosong [] = jalankan semua
```

Untuk melihat daftar nama demo, ketik `Object.keys(demo)` di Console.

### Tabel nama demo

| Materi                      | Nama demo                                                      |
| --------------------------- | -------------------------------------------------------------- |
| Sync vs Async dasar         | `syncVsAsync`                                                  |
| Callback & Callback Hell    | `callback`, `callbackHell`                                     |
| Promise dasar               | `promiseDasar`                                                 |
| then, catch, finally        | `promiseThenCatchFinally`, `keluarDariCallbackHell`            |
| Promise.all/race/...        | `promiseAll`, `promiseRace`, `promiseAllSettled`, `promiseAny` |
| Promise.withResolvers       | `promiseWithResolvers`                                         |
| Async/Await                 | `asyncAwait`                                                   |
| Error handling async        | `errorHandlingAsync`                                           |
| Event Loop                  | `eventLoop`                                                    |
| Timer                       | `timer`                                                        |
| Fetch GET                   | `fetchGet`                                                     |
| Fetch POST/PUT/PATCH/DELETE | `fetchPost`, `fetchPut`, `fetchPatch`, `fetchDelete`           |
| Error handling fetch        | `fetchError`                                                   |

> Demo Fetch membutuhkan internet. Jika offline, demo tersebut akan menampilkan pesan error di Console (dan itu bagus untuk melihat error handling bekerja).

---

## 19. Kesimpulan

### Synchronous

> Jalankan proses → tunggu selesai → lanjut ke proses berikutnya.

### Asynchronous

> Jalankan proses → proses boleh menunggu → kode lain tetap berjalan.

### Peta belajar

```text
setTimeout  →  Callback  →  Promise  →  Async/Await  →  Fetch API
(dasar)        (Callback     (.then,      (rapi &          (aplikasi
                Hell)         .catch)      mudah dibaca)    nyata)
```

### Hal yang perlu diingat

1. **Callback** → simple, tetapi bisa jadi _Callback Hell_.
2. **Promise** → punya 3 status: `pending`, `fulfilled`, `rejected`.
3. `.then()` berhasil, `.catch()` gagal, `.finally()` selalu jalan.
4. `Promise.all` (semua berhasil), `race` (tercepat), `allSettled` (semua hasil), `any` (berhasil pertama).
5. **Async/Await** = Promise dengan tampilan yang lebih rapi. Error ditangani dengan `try/catch`.
6. **Event Loop**: sync dulu → microtask (Promise) → macrotask (timer).
7. **`fetch` tidak reject untuk 404/500.** Selalu cek `response.ok`.
8. Gunakan `async/await` + `try/catch` sebagai pilihan utama dalam kode modern.
