# Belajar Dasar DOM JavaScript

Repository ini berisi catatan pembelajaran dasar **DOM (Document Object Model)** pada JavaScript, khususnya tentang pemilihan elemen, perubahan konten, serta manipulasi atribut dan style.

## Pengertian DOM

DOM atau **Document Object Model** adalah representasi dokumen HTML dalam bentuk objek yang dapat diakses oleh JavaScript.

Saat browser membaca halaman HTML, setiap elemen seperti `<h1>`, `<p>`, `<button>`, `<div>`, dan `<img>` direpresentasikan sebagai objek di dalam DOM. JavaScript dapat menggunakan objek tersebut untuk membaca, menambah, mengubah, atau menghapus bagian halaman secara dinamis.

Dengan DOM, JavaScript dapat melakukan hal-hal seperti:

- Mengambil elemen HTML berdasarkan `id`, `class`, nama tag, atau selector CSS lainnya.
- Mengubah teks pada halaman.
- Menambahkan elemen HTML baru.
- Mengubah atribut HTML.
- Mengubah tampilan CSS.
- Merespons interaksi pengguna, seperti klik tombol atau input pada form.

## 1. querySelector dan querySelectorAll

### `querySelector()`

`querySelector()` adalah method untuk memilih **satu elemen pertama** yang sesuai dengan selector CSS.

Sintaks:

```javascript
document.querySelector("selector");
```

Contoh:

```javascript
const judul = document.querySelector("#judul");
```

Kode tersebut mencari elemen pertama yang memiliki `id="judul"`.

Contoh selector yang dapat digunakan:

```javascript
document.querySelector("#id-elemen");
document.querySelector(".nama-class");
document.querySelector("p");
document.querySelector("button");
document.querySelector("input[type='text']");
```

Jika elemen ditemukan, hasilnya berupa objek elemen HTML. Jika tidak ditemukan, hasilnya adalah `null`.

Contoh pengecekan:

```javascript
const tombol = document.querySelector("#tombol-simpan");

if (tombol) {
  console.log("Tombol ditemukan");
}
```

`querySelector()` digunakan ketika hanya membutuhkan satu elemen tertentu, misalnya:

- Satu judul halaman.
- Satu tombol simpan.
- Satu input pencarian.
- Satu area notifikasi.
- Satu elemen menu.

`querySelector()` mengembalikan elemen pertama yang cocok dengan selector yang diberikan. [2]

### `querySelectorAll()`

`querySelectorAll()` adalah method untuk memilih **semua elemen** yang sesuai dengan selector CSS.

Sintaks:

```javascript
document.querySelectorAll("selector");
```

Contoh:

```javascript
const semuaParagraf = document.querySelectorAll("p");
```

Kode tersebut mengambil semua elemen `<p>` yang terdapat pada halaman.

Contoh lain:

```javascript
const semuaTombol = document.querySelectorAll(".btn");
```

Kode tersebut mengambil semua elemen yang memiliki class `btn`.

Hasil dari `querySelectorAll()` berupa `NodeList`, yaitu kumpulan elemen yang dapat diakses menggunakan perulangan seperti `forEach()`.

```javascript
const semuaTombol = document.querySelectorAll(".btn");

semuaTombol.forEach((tombol) => {
  console.log(tombol);
});
```

Contoh mengubah teks semua elemen:

```javascript
const semuaItem = document.querySelectorAll(".item");

semuaItem.forEach((item) => {
  item.textContent = "Data telah diperbarui";
});
```

`querySelectorAll()` cocok digunakan ketika ingin memanipulasi banyak elemen, misalnya:

- Semua card produk.
- Semua tombol.
- Semua item daftar.
- Semua gambar.
- Semua elemen dengan class yang sama.

`querySelectorAll()` menghasilkan `NodeList` statis, sehingga daftar hasil yang sudah diambil tidak otomatis berubah ketika elemen baru ditambahkan setelah method dipanggil. [1][3]

### Perbedaan `querySelector()` dan `querySelectorAll()`

| Aspek                      | `querySelector()`                   | `querySelectorAll()`                    |
| -------------------------- | ----------------------------------- | --------------------------------------- |
| Jumlah elemen yang diambil | Satu elemen pertama                 | Semua elemen yang cocok                 |
| Hasil                      | `Element` atau `null`               | `NodeList`                              |
| Cocok untuk                | Satu tombol, satu judul, satu input | Banyak item, banyak tombol, banyak card |
| Contoh                     | `document.querySelector("#judul")`  | `document.querySelectorAll(".item")`    |

## 2. innerHTML, textContent, dan innerText

`innerHTML`, `textContent`, dan `innerText` adalah properti yang digunakan untuk mengambil atau mengubah isi sebuah elemen HTML.

Walaupun terlihat mirip, ketiganya memiliki fungsi yang berbeda.

### `innerHTML`

`innerHTML` digunakan untuk membaca atau mengubah isi elemen dalam bentuk **HTML**.

Contoh:

```javascript
const container = document.querySelector("#container");

container.innerHTML = "<h2>Judul Baru</h2><p>Ini adalah paragraf baru.</p>";
```

Pada contoh tersebut, string HTML akan diproses oleh browser dan ditampilkan sebagai elemen `<h2>` serta `<p>`.

Contoh hasil:

```html
<div id="container">
  <h2>Judul Baru</h2>
  <p>Ini adalah paragraf baru.</p>
</div>
```

`innerHTML` dapat digunakan ketika ingin:

- Menambahkan struktur HTML baru.
- Menampilkan card atau daftar item.
- Membuat tombol atau elemen secara dinamis.
- Mengganti seluruh isi suatu container.

Contoh:

```javascript
const daftar = document.querySelector("#daftar");

daftar.innerHTML = `
  <li>Item pertama</li>
  <li>Item kedua</li>
  <li>Item ketiga</li>
`;
```

Perlu berhati-hati ketika menggunakan `innerHTML` dengan input dari pengguna. Data yang tidak divalidasi dapat memasukkan kode HTML yang tidak diinginkan. Untuk menampilkan teks biasa dari pengguna, gunakan `textContent`.

### `textContent`

`textContent` digunakan untuk membaca atau mengubah isi elemen sebagai **teks biasa**.

Contoh:

```javascript
const pesan = document.querySelector("#pesan");

pesan.textContent = "Data berhasil disimpan.";
```

Jika nilai yang dimasukkan berisi tag HTML, tag tersebut tidak akan dijalankan sebagai HTML.

```javascript
pesan.textContent = "<strong>Data berhasil disimpan.</strong>";
```

Hasil yang tampil pada halaman:

```text
<strong>Data berhasil disimpan.</strong>
```

`textContent` cocok digunakan untuk:

- Menampilkan notifikasi.
- Menampilkan status.
- Mengubah judul.
- Menampilkan jumlah data.
- Menampilkan teks dari input pengguna.
- Mengubah isi tombol.

Contoh:

```javascript
const jumlahData = document.querySelector("#jumlah-data");

jumlahData.textContent = "Jumlah data: 10";
```

### `innerText`

`innerText` juga digunakan untuk mengambil atau mengubah teks dari sebuah elemen.

Contoh:

```javascript
const judul = document.querySelector("h1");

console.log(judul.innerText);
```

Perbedaan utama `innerText` dan `textContent` adalah bahwa `innerText` berfokus pada teks yang benar-benar terlihat pada halaman.

Jika suatu elemen disembunyikan menggunakan CSS, misalnya:

```css
display: none;
```

teks di dalam elemen tersebut biasanya tidak akan ikut terbaca oleh `innerText`. Sebaliknya, `textContent` tetap dapat membaca teks tersebut.

### Perbedaan `innerHTML`, `textContent`, dan `innerText`

| Properti      | Fungsi                                      | HTML diproses | Memperhatikan elemen tersembunyi |
| ------------- | ------------------------------------------- | ------------: | -------------------------------: |
| `innerHTML`   | Membaca atau mengubah isi dalam bentuk HTML |            Ya |              Tidak menjadi fokus |
| `textContent` | Membaca atau mengubah teks biasa            |         Tidak |           Ya, teks tetap terbaca |
| `innerText`   | Membaca atau mengubah teks yang terlihat    |         Tidak |  Tidak, hanya teks yang terlihat |

Contoh sederhana:

```html
<p id="contoh">
  Teks terlihat
  <span style="display: none;">Teks tersembunyi</span>
</p>
```

```javascript
const contoh = document.querySelector("#contoh");

console.log(contoh.innerHTML);
console.log(contoh.textContent);
console.log(contoh.innerText);
```

Secara konsep:

```text
innerHTML    : Teks terlihat <span style="display: none;">Teks tersembunyi</span>
textContent  : Teks terlihat Teks tersembunyi
innerText    : Teks terlihat
```

`textContent` merepresentasikan isi teks sebuah node dan turunannya, sedangkan `innerText` merepresentasikan teks yang dirender atau terlihat pada halaman. [6][7]

## 3. Manipulasi Atribut dan Style

### Pengertian atribut

Atribut adalah informasi tambahan yang terdapat pada elemen HTML.

Contoh:

```html
<img src="gambar.jpg" alt="Contoh gambar" />
<a href="[https://example.com](https://example.com)">Kunjungi Website</a>
<input type="text" placeholder="Masukkan nama" />
<button disabled>Kirim</button>
```

Beberapa contoh atribut HTML:

| Atribut       | Fungsi                                                      |
| ------------- | ----------------------------------------------------------- |
| `id`          | Memberikan identitas unik pada elemen                       |
| `class`       | Memberikan nama class CSS pada elemen                       |
| `src`         | Menentukan sumber file, biasanya pada gambar atau video     |
| `href`        | Menentukan tujuan link                                      |
| `alt`         | Menentukan teks alternatif untuk gambar                     |
| `disabled`    | Menonaktifkan input atau tombol                             |
| `placeholder` | Menampilkan petunjuk pada input                             |
| `title`       | Menampilkan informasi tambahan saat elemen diarahkan cursor |

### `getAttribute()`

`getAttribute()` digunakan untuk mengambil nilai dari suatu atribut.

Sintaks:

```javascript
element.getAttribute("nama-atribut");
```

Contoh:

```javascript
const gambar = document.querySelector("img");

const sumberGambar = gambar.getAttribute("src");

console.log(sumberGambar);
```

Kode tersebut mengambil nilai atribut `src` dari elemen gambar.

### `setAttribute()`

`setAttribute()` digunakan untuk menambahkan atribut baru atau mengubah nilai atribut yang sudah ada.

Sintaks:

```javascript
element.setAttribute("nama-atribut", "nilai-atribut");
```

Contoh:

```javascript
const tombol = document.querySelector("button");

tombol.setAttribute("disabled", "true");
```

Kode tersebut menambahkan atribut `disabled` pada tombol, sehingga tombol tidak dapat diklik.

Contoh mengubah atribut gambar:

```javascript
const gambar = document.querySelector("img");

gambar.setAttribute("src", "gambar-baru.jpg");
gambar.setAttribute("alt", "Gambar baru");
```

Contoh mengubah placeholder input:

```javascript
const input = document.querySelector("input");

input.setAttribute("placeholder", "Masukkan email");
```

### `removeAttribute()`

`removeAttribute()` digunakan untuk menghapus atribut dari sebuah elemen.

Sintaks:

```javascript
element.removeAttribute("nama-atribut");
```

Contoh:

```javascript
const tombol = document.querySelector("button");

tombol.removeAttribute("disabled");
```

Kode tersebut menghapus atribut `disabled`, sehingga tombol dapat diklik kembali.

### Manipulasi style langsung

JavaScript dapat mengubah tampilan CSS elemen menggunakan properti `style`.

Contoh:

```javascript
const kotak = document.querySelector(".box");

kotak.style.backgroundColor = "blue";
kotak.style.color = "white";
kotak.style.padding = "15px";
kotak.style.borderRadius = "8px";
```

Perubahan tersebut setara dengan CSS berikut:

```css
.box {
  background-color: blue;
  color: white;
  padding: 15px;
  border-radius: 8px;
}
```

Namun, ketika menulis CSS melalui JavaScript, nama properti menggunakan format `camelCase`.

| CSS                | JavaScript        |
| ------------------ | ----------------- |
| `background-color` | `backgroundColor` |
| `font-size`        | `fontSize`        |
| `text-align`       | `textAlign`       |
| `border-radius`    | `borderRadius`    |
| `margin-top`       | `marginTop`       |

Contoh lain:

```javascript
const judul = document.querySelector("h1");

judul.style.color = "darkblue";
judul.style.fontSize = "32px";
judul.style.textAlign = "center";
```

### Manipulasi class dengan `classList`

Selain menggunakan `style` secara langsung, cara yang lebih rapi untuk mengubah tampilan adalah menggunakan `classList`.

Method yang sering digunakan:

| Method                 | Fungsi                                          |
| ---------------------- | ----------------------------------------------- |
| `classList.add()`      | Menambahkan class                               |
| `classList.remove()`   | Menghapus class                                 |
| `classList.toggle()`   | Menambah atau menghapus class secara bergantian |
| `classList.contains()` | Memeriksa apakah elemen memiliki class tertentu |

Contoh:

```javascript
const kotak = document.querySelector(".box");

kotak.classList.add("aktif");
```

Jika terdapat CSS berikut:

```css
.aktif {
  background-color: green;
  color: white;
}
```

Maka elemen `kotak` akan memiliki tampilan sesuai class `aktif`.

Contoh menghapus class:

```javascript
kotak.classList.remove("aktif");
```

Contoh toggle class:

```javascript
kotak.classList.toggle("aktif");
```

`toggle()` berguna untuk fitur seperti:

- Mode gelap dan mode terang.
- Menampilkan atau menyembunyikan menu.
- Memberi tanda elemen yang aktif.
- Mengubah status tombol.
- Membuka dan menutup modal.

## Kesimpulan

DOM memungkinkan JavaScript berinteraksi dengan elemen HTML secara dinamis.

Materi penting yang dipelajari adalah:

- `querySelector()` digunakan untuk memilih satu elemen pertama yang sesuai dengan selector.
- `querySelectorAll()` digunakan untuk memilih seluruh elemen yang sesuai dengan selector.
- `innerHTML` digunakan untuk memasukkan atau mengambil isi dalam bentuk HTML.
- `textContent` digunakan untuk memasukkan atau mengambil teks biasa.
- `innerText` digunakan untuk mengambil teks yang terlihat pada halaman.
- `getAttribute()` digunakan untuk mengambil nilai atribut.
- `setAttribute()` digunakan untuk menambah atau mengubah atribut.
- `removeAttribute()` digunakan untuk menghapus atribut.
- `style` digunakan untuk mengubah CSS secara langsung melalui JavaScript.
- `classList` digunakan untuk menambah, menghapus, atau mengatur class CSS elemen.

## 4. Membuat dan Menghapus Elemen

Selain mengubah elemen yang sudah ada, JavaScript juga dapat digunakan untuk membuat elemen HTML baru, menambahkannya ke halaman, serta menghapus elemen yang tidak diperlukan.

### `document.createElement()`

`document.createElement()` digunakan untuk membuat elemen HTML baru melalui JavaScript.

Sintaks:

```javascript
document.createElement("nama-tag");
```

Contoh membuat elemen paragraf:

```javascript
const paragrafBaru = document.createElement("p");

paragrafBaru.textContent = "Ini adalah paragraf baru.";
```

Pada kode tersebut, elemen `<p>` sudah dibuat, tetapi belum tampil di halaman karena belum dimasukkan ke dalam DOM.

### `appendChild()`

`appendChild()` digunakan untuk menambahkan elemen baru sebagai anak dari elemen lain.

Sintaks:

```javascript
parent.appendChild(child);
```

Contoh:

```javascript
const container = document.querySelector("#container");

const paragrafBaru = document.createElement("p");
paragrafBaru.textContent = "Paragraf ini dibuat dengan JavaScript.";

container.appendChild(paragrafBaru);
```

Hasilnya, elemen `<p>` baru akan dimasukkan ke dalam elemen dengan `id="container"`.

Contoh HTML awal:

```html
<div id="container"></div>
```

Setelah JavaScript dijalankan, hasilnya menjadi:

```html
<div id="container">
  <p>Paragraf ini dibuat dengan JavaScript.</p>
</div>
```

### Menambahkan atribut dan class pada elemen baru

Elemen yang baru dibuat juga dapat diberi atribut, class, atau style sebelum dimasukkan ke halaman.

Contoh:

```javascript
const daftar = document.querySelector("#daftar");

const itemBaru = document.createElement("li");

itemBaru.textContent = "Belajar DOM JavaScript";
itemBaru.classList.add("item");
itemBaru.setAttribute("title", "Materi DOM");

daftar.appendChild(itemBaru);
```

Contoh HTML awal:

```html
<ul id="daftar"></ul>
```

Hasil setelah kode dijalankan:

```html
<ul id="daftar">
  <li class="item" title="Materi DOM">Belajar DOM JavaScript</li>
</ul>
```

### `append()`

Selain `appendChild()`, terdapat method `append()` untuk menambahkan node atau teks ke dalam elemen.

Contoh:

```javascript
const container = document.querySelector("#container");

const judul = document.createElement("h2");
judul.textContent = "Judul Baru";

container.append(judul);
```

Perbedaan sederhana antara `appendChild()` dan `append()`:

| Method          | Fungsi                                          |
| --------------- | ----------------------------------------------- |
| `appendChild()` | Menambahkan satu node atau elemen               |
| `append()`      | Dapat menambahkan node, elemen, atau teks biasa |

Contoh `append()` dengan teks:

```javascript
const container = document.querySelector("#container");

container.append("Teks tambahan");
```

### `prepend()`

`prepend()` digunakan untuk menambahkan elemen atau teks pada bagian awal sebuah parent element.

Contoh:

```javascript
const daftar = document.querySelector("#daftar");

const itemPertama = document.createElement("li");
itemPertama.textContent = "Item paling atas";

daftar.prepend(itemPertama);
```

Jika sebelumnya daftar sudah memiliki beberapa item, item baru tersebut akan muncul di posisi paling awal.

### `remove()`

`remove()` digunakan untuk menghapus elemen langsung dari DOM.

Contoh:

```javascript
const pesan = document.querySelector("#pesan");

pesan.remove();
```

Kode tersebut akan menghapus elemen yang memiliki `id="pesan"` dari halaman.

Contoh HTML:

```html
<p id="pesan">Pesan ini akan dihapus.</p>
```

Setelah `pesan.remove()` dijalankan, elemen tersebut tidak lagi ada di halaman.

### Contoh membuat dan menghapus item daftar

HTML:

```html
<input type="text" id="input-item" placeholder="Masukkan nama item" />
<button id="tambah-item">Tambah Item</button>

<ul id="daftar-item"></ul>
```

JavaScript:

```javascript
const inputItem = document.querySelector("#input-item");
const tombolTambah = document.querySelector("#tambah-item");
const daftarItem = document.querySelector("#daftar-item");

tombolTambah.addEventListener("click", () => {
  const teksItem = inputItem.value;

  if (teksItem === "") {
    return;
  }

  const itemBaru = document.createElement("li");
  itemBaru.textContent = teksItem;

  daftarItem.appendChild(itemBaru);

  inputItem.value = "";
});
```

Pada contoh tersebut:

- Pengguna menulis teks pada input.
- Saat tombol diklik, JavaScript membuat elemen `<li>` baru.
- Isi `<li>` diambil dari nilai input.
- Elemen `<li>` ditambahkan ke dalam `<ul>`.
- Input dikosongkan kembali setelah item berhasil ditambahkan.

Contoh menambahkan tombol hapus pada setiap item:

```javascript
const inputItem = document.querySelector("#input-item");
const tombolTambah = document.querySelector("#tambah-item");
const daftarItem = document.querySelector("#daftar-item");

tombolTambah.addEventListener("click", () => {
  const teksItem = inputItem.value;

  if (teksItem === "") {
    return;
  }

  const itemBaru = document.createElement("li");
  const tombolHapus = document.createElement("button");

  itemBaru.textContent = teksItem;

  tombolHapus.textContent = "Hapus";

  tombolHapus.addEventListener("click", () => {
    itemBaru.remove();
  });

  itemBaru.appendChild(tombolHapus);
  daftarItem.appendChild(itemBaru);

  inputItem.value = "";
});
```

Dengan kode tersebut, setiap item yang dibuat akan memiliki tombol `Hapus`. Saat tombol tersebut diklik, item terkait akan dihapus dari halaman.

## 5. Event Listener: `click`, `input`, dan `submit`

Event adalah kejadian yang terjadi pada halaman web, baik karena tindakan pengguna maupun proses dari browser.

Contoh event:

- Pengguna mengklik tombol.
- Pengguna mengetik pada input.
- Pengguna mengirim form.
- Mouse diarahkan ke elemen tertentu.
- Halaman selesai dimuat.
- Tombol keyboard ditekan.

JavaScript dapat mendeteksi event menggunakan `addEventListener()`.

### `addEventListener()`

Sintaks dasar:

```javascript
element.addEventListener("nama-event", function () {
  // kode yang dijalankan saat event terjadi
});
```

Contoh:

```javascript
const tombol = document.querySelector("#tombol");

tombol.addEventListener("click", () => {
  console.log("Tombol diklik");
});
```

Ketika tombol dengan `id="tombol"` diklik, pesan akan tampil di console browser.

### Event `click`

Event `click` terjadi ketika pengguna mengklik elemen, misalnya tombol, gambar, link, atau card.

Contoh HTML:

```html
<button id="tombol-ubah">Ubah Warna</button>

<p id="pesan">Warna belum diubah.</p>
```

JavaScript:

```javascript
const tombolUbah = document.querySelector("#tombol-ubah");
const pesan = document.querySelector("#pesan");

tombolUbah.addEventListener("click", () => {
  pesan.textContent = "Warna berhasil diubah.";
  pesan.style.color = "green";
});
```

Saat tombol diklik, teks paragraf akan berubah dan warnanya menjadi hijau.

Contoh toggle class dengan event `click`:

```html
<button id="tombol-mode">Ubah Mode</button>

<div class="box" id="kotak">Isi kotak</div>
```

```css
.dark-mode {
  background-color: #222;
  color: white;
}
```

```javascript
const tombolMode = document.querySelector("#tombol-mode");
const kotak = document.querySelector("#kotak");

tombolMode.addEventListener("click", () => {
  kotak.classList.toggle("dark-mode");
});
```

Setiap kali tombol diklik, class `dark-mode` akan ditambahkan atau dihapus dari elemen `kotak`.

### Object event

Saat event terjadi, JavaScript dapat menerima informasi tentang event tersebut melalui parameter `event`.

Contoh:

```javascript
const tombol = document.querySelector("#tombol");

tombol.addEventListener("click", (event) => {
  console.log(event);
});
```

Object `event` berisi informasi seperti:

- Elemen yang memicu event.
- Jenis event yang terjadi.
- Posisi mouse untuk event tertentu.
- Tombol keyboard yang ditekan.
- Informasi form yang dikirim.

Untuk mengetahui elemen yang memicu event, gunakan `event.target`.

```javascript
const tombol = document.querySelector("#tombol");

tombol.addEventListener("click", (event) => {
  console.log(event.target);
});
```

### Event `input`

Event `input` terjadi ketika nilai pada elemen input berubah.

Event ini biasanya digunakan pada elemen seperti:

- `<input>`
- `<textarea>`
- `<select>`

Contoh HTML:

```html
<input type="text" id="nama" placeholder="Masukkan nama" />

<p id="hasil"></p>
```

JavaScript:

```javascript
const inputNama = document.querySelector("#nama");
const hasil = document.querySelector("#hasil");

inputNama.addEventListener("input", () => {
  hasil.textContent = `Halo, ${inputNama.value}`;
});
```

Ketika pengguna mengetik pada input, teks pada elemen `hasil` akan berubah secara langsung.

Contoh validasi panjang karakter:

```html
<input type="text" id="username" placeholder="Masukkan username" />

<p id="info-username"></p>
```

```javascript
const username = document.querySelector("#username");
const infoUsername = document.querySelector("#info-username");

username.addEventListener("input", () => {
  const jumlahKarakter = username.value.length;

  infoUsername.textContent = `Jumlah karakter: ${jumlahKarakter}`;
});
```

Event `input` sangat berguna untuk:

- Menampilkan preview teks secara langsung.
- Menghitung jumlah karakter.
- Membuat fitur pencarian langsung.
- Memvalidasi data saat pengguna mengetik.
- Mengaktifkan atau menonaktifkan tombol berdasarkan input.

### Event `submit`

Event `submit` terjadi ketika pengguna mengirim sebuah form.

Contoh HTML:

```html
<form id="form-nama">
  <input type="text" id="input-nama" placeholder="Masukkan nama" />
  <button type="submit">Kirim</button>
</form>

<p id="hasil-form"></p>
```

JavaScript:

```javascript
const formNama = document.querySelector("#form-nama");
const inputNama = document.querySelector("#input-nama");
const hasilForm = document.querySelector("#hasil-form");

formNama.addEventListener("submit", (event) => {
  event.preventDefault();

  hasilForm.textContent = `Data berhasil dikirim: ${inputNama.value}`;
});
```

Secara default, ketika form dikirim, browser biasanya akan memuat ulang halaman atau berpindah ke alamat yang ditentukan oleh atribut `action`.

Method `event.preventDefault()` digunakan untuk mencegah perilaku default tersebut.

```javascript
event.preventDefault();
```

Dengan `preventDefault()`, JavaScript dapat memproses data form tanpa halaman dimuat ulang.

Contoh validasi form sederhana:

```html
<form id="form-login">
  <input type="text" id="email" placeholder="Masukkan email" />
  <input type="password" id="password" placeholder="Masukkan password" />
  <button type="submit">Login</button>
</form>

<p id="pesan-login"></p>
```

```javascript
const formLogin = document.querySelector("#form-login");
const email = document.querySelector("#email");
const password = document.querySelector("#password");
const pesanLogin = document.querySelector("#pesan-login");

formLogin.addEventListener("submit", (event) => {
  event.preventDefault();

  if (email.value === "" || password.value === "") {
    pesanLogin.textContent = "Email dan password wajib diisi.";
    pesanLogin.style.color = "red";
    return;
  }

  pesanLogin.textContent = "Form berhasil dikirim.";
  pesanLogin.style.color = "green";
});
```

Pada contoh tersebut:

- Form tidak akan me-refresh halaman karena menggunakan `preventDefault()`.
- JavaScript memeriksa apakah input email dan password kosong.
- Jika masih kosong, tampil pesan error.
- Jika semua input terisi, tampil pesan berhasil.

### Perbedaan `click`, `input`, dan `submit`

| Event    | Terjadi ketika      | Contoh penggunaan                          |
| -------- | ------------------- | ------------------------------------------ |
| `click`  | Elemen diklik       | Tombol, card, gambar, menu                 |
| `input`  | Nilai input berubah | Live search, jumlah karakter, preview teks |
| `submit` | Form dikirim        | Login, registrasi, tambah data             |

## 6. Event Bubbling dan `stopPropagation()`

### Pengertian event bubbling

Event bubbling adalah proses ketika sebuah event terjadi pada elemen anak, lalu event tersebut juga dapat diteruskan ke elemen parent atau elemen pembungkusnya.

Dengan kata lain, event bergerak dari elemen paling dalam menuju elemen luar.

Contoh struktur HTML:

```html
<div id="parent">
  <button id="child">Klik Saya</button>
</div>
```

JavaScript:

```javascript
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

parent.addEventListener("click", () => {
  console.log("Parent diklik");
});

child.addEventListener("click", () => {
  console.log("Button diklik");
});
```

Jika tombol diklik, hasil pada console adalah:

```text
Button diklik
Parent diklik
```

Hal tersebut terjadi karena event `click` pertama kali dijalankan pada elemen tombol, kemudian event tersebut naik ke elemen parent.

### Ilustrasi event bubbling

Struktur elemen:

```html
<div id="luar">
  <div id="tengah">
    <button id="dalam">Klik</button>
  </div>
</div>
```

Jika semua elemen memiliki event `click`:

```javascript
const luar = document.querySelector("#luar");
const tengah = document.querySelector("#tengah");
const dalam = document.querySelector("#dalam");

luar.addEventListener("click", () => {
  console.log("Elemen luar diklik");
});

tengah.addEventListener("click", () => {
  console.log("Elemen tengah diklik");
});

dalam.addEventListener("click", () => {
  console.log("Tombol diklik");
});
```

Saat tombol diklik, hasilnya:

```text
Tombol diklik
Elemen tengah diklik
Elemen luar diklik
```

Urutan tersebut menunjukkan bahwa event bergerak dari elemen anak ke parent, lalu ke parent yang lebih luar.

### `event.target` dan `event.currentTarget`

Pada event bubbling, penting untuk memahami perbedaan `event.target` dan `event.currentTarget`.

- `event.target` adalah elemen yang benar-benar diklik atau memicu event.
- `event.currentTarget` adalah elemen yang sedang menjalankan event listener.

Contoh:

```html
<div id="container">
  <button id="tombol">Klik Saya</button>
</div>
```

```javascript
const container = document.querySelector("#container");

container.addEventListener("click", (event) => {
  console.log("Target:", event.target);
  console.log("Current target:", event.currentTarget);
});
```

Jika pengguna mengklik tombol:

- `event.target` akan merujuk ke elemen `<button>`.
- `event.currentTarget` akan merujuk ke elemen `<div id="container">`.

### `stopPropagation()`

`stopPropagation()` digunakan untuk menghentikan event bubbling.

Sintaks:

```javascript
event.stopPropagation();
```

Contoh:

```html
<div id="parent">
  <button id="child">Klik Saya</button>
</div>
```

```javascript
const parent = document.querySelector("#parent");
const child = document.querySelector("#child");

parent.addEventListener("click", () => {
  console.log("Parent diklik");
});

child.addEventListener("click", (event) => {
  event.stopPropagation();

  console.log("Button diklik");
});
```

Jika tombol diklik, hasilnya hanya:

```text
Button diklik
```

Pesan `Parent diklik` tidak muncul karena event dari tombol tidak diteruskan ke parent.

### Contoh penggunaan pada modal

Event bubbling sering digunakan pada fitur modal.

Contoh HTML:

```html
<div id="modal">
  <div id="isi-modal">
    <h2>Judul Modal</h2>
    <p>Isi modal berada di sini.</p>
    <button id="tutup-modal">Tutup</button>
  </div>
</div>
```

Contoh CSS:

```css
#modal {
  background-color: rgba(0, 0, 0, 0.5);
  padding: 30px;
}

#isi-modal {
  background-color: white;
  padding: 20px;
}
```

JavaScript:

```javascript
const modal = document.querySelector("#modal");
const isiModal = document.querySelector("#isi-modal");
const tombolTutup = document.querySelector("#tutup-modal");

modal.addEventListener("click", () => {
  modal.style.display = "none";
});

isiModal.addEventListener("click", (event) => {
  event.stopPropagation();
});

tombolTutup.addEventListener("click", () => {
  modal.style.display = "none";
});
```

Cara kerja kode tersebut:

- Jika pengguna mengklik area luar modal, modal ditutup.
- Jika pengguna mengklik isi modal, event tidak diteruskan ke area luar karena menggunakan `stopPropagation()`.
- Jika pengguna mengklik tombol tutup, modal akan ditutup.

### Event delegation

Event bubbling juga dapat dimanfaatkan untuk menangani banyak elemen menggunakan satu event listener. Teknik ini disebut event delegation.

Contoh HTML:

```html
<ul id="daftar-menu">
  <li>Beranda</li>
  <li>Profil</li>
  <li>Kontak</li>
</ul>
```

JavaScript:

```javascript
const daftarMenu = document.querySelector("#daftar-menu");

daftarMenu.addEventListener("click", (event) => {
  if (event.target.tagName === "LI") {
    console.log(`Menu yang diklik: ${event.target.textContent}`);
  }
});
```

Pada contoh tersebut, event listener tidak dipasang pada setiap elemen `<li>`. Event listener hanya dipasang pada elemen `<ul>`.

Ketika salah satu `<li>` diklik, event akan naik ke `<ul>` melalui event bubbling. JavaScript kemudian memeriksa apakah elemen yang diklik adalah `<li>` menggunakan `event.target`.

Event delegation bermanfaat ketika:

- Memiliki banyak elemen yang sama.
- Elemen baru dapat ditambahkan secara dinamis.
- Ingin mengurangi jumlah event listener.
- Membuat daftar item, tabel, menu, atau card yang interaktif.

## Kesimpulan Tambahan

Materi lanjutan DOM yang dipelajari adalah:

- `document.createElement()` digunakan untuk membuat elemen HTML baru.
- `appendChild()` digunakan untuk menambahkan elemen sebagai child dari elemen lain.
- `append()` digunakan untuk menambahkan elemen atau teks ke dalam sebuah elemen.
- `prepend()` digunakan untuk menambahkan elemen pada bagian awal parent element.
- `remove()` digunakan untuk menghapus elemen dari DOM.
- `addEventListener()` digunakan untuk menjalankan kode ketika event tertentu terjadi.
- Event `click` digunakan untuk mendeteksi klik pada elemen.
- Event `input` digunakan untuk mendeteksi perubahan nilai input secara langsung.
- Event `submit` digunakan untuk menangani pengiriman form.
- `event.preventDefault()` digunakan untuk mencegah perilaku bawaan browser, seperti reload saat form dikirim.
- Event bubbling adalah proses event yang bergerak dari elemen anak ke elemen parent.
- `event.stopPropagation()` digunakan untuk menghentikan event agar tidak diteruskan ke parent.
- Event delegation memanfaatkan event bubbling agar satu event listener dapat menangani banyak elemen.

## 7. Event Delegation

Pada materi 6 sudah dikenalkan sedikit tentang event delegation. Pada bagian ini dibahas lebih lengkap, karena teknik ini sangat sering dipakai pada daftar data yang elemennya dibuat secara dinamis.

### Masalah jika memasang listener pada setiap elemen

```html
<ul id="daftar">
  <li>Item 1</li>
  <li>Item 2</li>
</ul>
```

```javascript
const daftar = document.querySelector("#daftar");

daftar.querySelectorAll("li").forEach((li) => {
  li.addEventListener("click", () => {
    console.log("Item diklik");
  });
});

const itemBaru = document.createElement("li");
itemBaru.textContent = "Item 3";
daftar.appendChild(itemBaru);
```

Item 1 dan Item 2 merespons klik, tetapi **Item 3 tidak**. Penyebabnya, `querySelectorAll()` hanya mengambil elemen yang sudah ada saat kode dijalankan, sehingga elemen baru tidak memiliki listener.

### Solusi: satu listener pada parent

```javascript
const daftar = document.querySelector("#daftar");

daftar.addEventListener("click", (event) => {
  const item = event.target.closest("li");

  if (!item) return;

  console.log(`Diklik: ${item.textContent}`);
});
```

Sekarang semua `<li>`, termasuk yang ditambahkan nanti, otomatis ikut bekerja. Klik pada `<li>` naik ke `<ul>` melalui event bubbling, lalu listener pada `<ul>` yang memprosesnya.

### `event.target.closest()`

Pada materi 6, pengecekan dilakukan dengan `event.target.tagName === "LI"`. Cara itu bermasalah jika di dalam `<li>` ada elemen lain, misalnya `<span>` atau `<strong>`. Jika yang diklik adalah `<span>`, maka `event.target` adalah `<span>`, bukan `<li>`.

`closest("selector")` mencari elemen terdekat yang cocok, mulai dari elemen itu sendiri lalu naik ke parent-nya.

```javascript
const item = event.target.closest("li");
```

Hasilnya `<li>` pemilik elemen yang diklik, atau `null` jika tidak ada.

### `event.target.matches()`

`matches("selector")` memeriksa apakah elemen cocok dengan selector. Hasilnya `true` atau `false`.

```javascript
if (event.target.matches(".tombol-hapus")) {
  console.log("Yang diklik adalah tombol hapus");
}
```

### Menandai aksi dengan `data-*`

Atribut `data-*` digunakan untuk menyimpan informasi tambahan pada elemen. Nilainya dibaca melalui properti `dataset`.

```html
<ul id="daftar-tugas">
  <li data-id="1">
    <span>Belajar DOM</span>
    <button data-aksi="selesai">Selesai</button>
    <button data-aksi="hapus">Hapus</button>
  </li>
  <li data-id="2">
    <span>Belajar Event</span>
    <button data-aksi="selesai">Selesai</button>
    <button data-aksi="hapus">Hapus</button>
  </li>
</ul>
```

```javascript
const daftarTugas = document.querySelector("#daftar-tugas");

daftarTugas.addEventListener("click", (event) => {
  const tombol = event.target.closest("button[data-aksi]");

  if (!tombol) return;

  const item = tombol.closest("li");
  const id = item.dataset.id;
  const aksi = tombol.dataset.aksi;

  if (aksi === "selesai") {
    item.classList.toggle("selesai");
  } else if (aksi === "hapus") {
    item.remove();
  }

  console.log(`Aksi ${aksi} pada tugas ${id}`);
});
```

Penjelasan:

- Satu listener menangani semua tombol `Selesai` dan `Hapus`.
- `closest("button[data-aksi]")` memastikan yang diklik adalah tombol aksi.
- `tombol.dataset.aksi` membaca nilai dari `data-aksi`.
- `item.dataset.id` membaca nilai dari `data-id`.

### Perbandingan

| Aspek                         | Listener pada setiap elemen | Event delegation                  |
| ----------------------------- | --------------------------- | --------------------------------- |
| Jumlah listener               | Banyak                      | Satu                              |
| Elemen yang dibuat belakangan | Tidak otomatis ikut         | Otomatis ikut                     |
| Penggunaan memori             | Lebih besar                 | Lebih hemat                       |
| Cocok untuk                   | Elemen tunggal dan tetap    | Daftar, tabel, card, menu dinamis |

### Catatan

Event delegation bergantung pada bubbling. Beberapa event tidak mengalami bubbling, misalnya `focus` dan `blur`. Sebagai gantinya dapat digunakan `focusin` dan `focusout`, yang mengalami bubbling.

## 8. Ambil Data dan Validasi

Pada aplikasi yang memiliki form, JavaScript perlu mengambil data yang diisi pengguna, lalu memeriksa apakah data tersebut layak diproses.

### Mengambil data dari input

Nilai input dibaca melalui properti `value`.

```javascript
const inputNama = document.querySelector("#nama");

console.log(inputNama.value);
```

Nilai `value` selalu berupa **string**, termasuk pada `<input type="number">`.

```javascript
const inputUmur = document.querySelector("#umur");

console.log(typeof inputUmur.value); // "string"

const umur = Number(inputUmur.value);
```

Cara mengambil data dari berbagai elemen:

| Elemen                    | Cara mengambil data                                        |
| ------------------------- | ---------------------------------------------------------- |
| `<input type="text">`     | `input.value`                                              |
| `<input type="number">`   | `Number(input.value)`                                      |
| `<textarea>`              | `textarea.value`                                           |
| `<select>`                | `select.value`                                             |
| `<input type="checkbox">` | `checkbox.checked` (`true` atau `false`)                   |
| `<input type="radio">`    | `document.querySelector("input[name='x']:checked")?.value` |

### Merapikan data dengan `trim()`

`trim()` menghapus spasi di awal dan akhir teks. Tanpa `trim()`, input yang hanya berisi spasi dianggap terisi.

```javascript
const judul = inputJudul.value.trim();
```

### Mengambil semua data form sekaligus dengan `FormData`

```html
<form id="form-buku">
  <input type="text" name="judul" />
  <input type="text" name="penulis" />
  <button type="submit">Simpan</button>
</form>
```

```javascript
const form = document.querySelector("#form-buku");

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const data = Object.fromEntries(new FormData(form));

  console.log(data); // { judul: "...", penulis: "..." }
});
```

Agar `FormData` bekerja, setiap input harus memiliki atribut `name`.

### Validasi data

Validasi adalah pemeriksaan data sebelum diproses. Tujuannya mencegah data kosong, tidak masuk akal, atau duplikat.

Validasi yang sering dilakukan:

| Jenis validasi                | Contoh                           |
| ----------------------------- | -------------------------------- |
| Wajib diisi                   | `judul === ""`                   |
| Panjang minimal atau maksimal | `judul.length < 3`               |
| Angka                         | `Number.isNaN(umur)`, `umur < 0` |
| Format                        | Email harus mengandung `@`       |
| Duplikat                      | Judul sudah ada pada daftar      |

### Contoh fungsi validasi

Fungsi validasi sebaiknya dipisahkan dari kode DOM. Fungsi mengembalikan **pesan error** jika ada masalah, atau `""` jika data valid.

```javascript
function validasiJudul(judul, daftarJudul) {
  if (judul === "") {
    return "Judul wajib diisi.";
  }

  if (judul.length < 3) {
    return "Judul minimal 3 karakter.";
  }

  if (daftarJudul.includes(judul.toLowerCase())) {
    return "Judul sudah ada.";
  }

  return "";
}
```

### Menampilkan hasil validasi

```html
<form id="form-judul">
  <input type="text" id="input-judul" placeholder="Judul buku" />
  <button type="submit">Tambah</button>
</form>

<p id="pesan"></p>
```

```javascript
const form = document.querySelector("#form-judul");
const inputJudul = document.querySelector("#input-judul");
const pesan = document.querySelector("#pesan");

const daftarJudul = ["laskar pelangi"];

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const judul = inputJudul.value.trim();
  const error = validasiJudul(judul, daftarJudul);

  if (error) {
    pesan.textContent = error;
    pesan.classList.add("error");
    inputJudul.focus();
    return;
  }

  daftarJudul.push(judul.toLowerCase());
  pesan.textContent = "Berhasil ditambahkan.";
  pesan.classList.remove("error");
  form.reset();
});
```

Alur kode tersebut:

- Ambil nilai input, lalu rapikan dengan `trim()`.
- Kirim ke fungsi validasi.
- Jika ada error, tampilkan pesan dan hentikan proses dengan `return`.
- Jika valid, proses data, lalu kosongkan form dengan `form.reset()`.

### Hati-hati: `Number("")` menghasilkan `0`

```javascript
Number(""); // 0
Number("abc"); // NaN
```

Input angka yang kosong akan terbaca sebagai `0`. Karena itu, periksa dulu apakah input kosong sebelum mengubahnya menjadi angka.

```javascript
if (inputStok.value === "") {
  return "Stok wajib diisi.";
}

const stok = Number(inputStok.value);

if (stok < 0) {
  return "Stok tidak boleh negatif.";
}
```

### Validasi bawaan HTML

HTML memiliki atribut validasi sendiri, seperti `required`, `minlength`, `min`, dan `type="email"`.

```html
<input type="text" required minlength="3" />
```

Validasi bawaan dapat diperiksa dengan JavaScript menggunakan `form.checkValidity()`. Namun validasi manual dengan JavaScript tetap berguna karena pesan error dapat disesuaikan, dan aturan khusus seperti pengecekan duplikat dapat ditambahkan.

## 9. DOM Traversal: Parent dan Children

DOM Traversal adalah perpindahan dari satu elemen ke elemen lain yang berhubungan dengannya, seperti parent (induk), children (anak), dan sibling (saudara).

### Struktur pohon DOM

```html
<ul id="daftar">
  <li>Satu</li>
  <li id="dua">Dua</li>
  <li>Tiga</li>
</ul>
```

```text
ul#daftar          <- parent dari ketiga li
├── li "Satu"      <- sibling sebelum #dua
├── li#dua         <- elemen yang menjadi acuan
└── li "Tiga"      <- sibling sesudah #dua
```

### Properti traversal

| Properti                 | Fungsi                                   |
| ------------------------ | ---------------------------------------- |
| `parentElement`          | Elemen parent langsung                   |
| `children`               | Semua elemen anak (`HTMLCollection`)     |
| `firstElementChild`      | Elemen anak pertama                      |
| `lastElementChild`       | Elemen anak terakhir                     |
| `nextElementSibling`     | Elemen saudara berikutnya                |
| `previousElementSibling` | Elemen saudara sebelumnya                |
| `closest("selector")`    | Elemen terdekat yang cocok, naik ke atas |

### Parent: `parentElement`

```javascript
const dua = document.querySelector("#dua");

console.log(dua.parentElement); // <ul id="daftar">
```

Dapat digunakan berantai untuk naik lebih jauh.

```javascript
dua.parentElement.parentElement;
```

### Children: `children`

```javascript
const daftar = document.querySelector("#daftar");

console.log(daftar.children); // HTMLCollection(3)
console.log(daftar.children.length); // 3
console.log(daftar.children[0]); // <li>Satu</li>
console.log(daftar.firstElementChild); // <li>Satu</li>
console.log(daftar.lastElementChild); // <li>Tiga</li>
```

`children` berupa `HTMLCollection`, bukan array. Agar dapat memakai `forEach()`, ubah dulu menjadi array.

```javascript
[...daftar.children].forEach((li) => {
  console.log(li.textContent);
});
```

### `children` dan `childNodes`

`childNodes` ikut menghitung node teks, termasuk spasi dan baris baru di antara tag HTML. `children` hanya berisi elemen.

```javascript
console.log(daftar.childNodes.length); // lebih banyak, karena ikut menghitung node teks
console.log(daftar.children.length); // 3
```

Untuk kebutuhan sehari-hari, gunakan `children`.

### Sibling

```javascript
const dua = document.querySelector("#dua");

console.log(dua.previousElementSibling); // <li>Satu</li>
console.log(dua.nextElementSibling); // <li>Tiga</li>
```

Jika tidak ada saudara pada arah tersebut, hasilnya `null`.

### Contoh: menghapus card dari tombol di dalamnya

```html
<div class="card">
  <h3>Laskar Pelangi</h3>
  <button class="hapus">Hapus</button>
</div>
```

```javascript
const tombol = document.querySelector(".hapus");

tombol.addEventListener("click", () => {
  tombol.parentElement.remove();
});
```

Dari tombol, JavaScript naik ke parent-nya (`.card`) lalu menghapusnya.

### Contoh: mengambil data di dalam card

```javascript
const card = tombol.closest(".card");
const judul = card.querySelector("h3").textContent;

console.log(judul); // "Laskar Pelangi"
```

`querySelector()` juga dapat dipanggil pada sebuah elemen, sehingga pencarian hanya dilakukan di dalam elemen tersebut.

### `parentElement` atau `closest()`

`parentElement` bergantung pada susunan HTML. Jika suatu hari tombol dibungkus elemen lain, `parentElement` akan menunjuk elemen yang salah.

```html
<div class="card">
  <h3>Laskar Pelangi</h3>
  <div class="aksi">
    <button class="hapus">Hapus</button>
  </div>
</div>
```

```javascript
tombol.parentElement; // <div class="aksi">, bukan .card
tombol.closest(".card"); // tetap <div class="card">
```

Karena itu, `closest()` lebih aman jika struktur HTML dapat berubah.

## 10. Perpus: Render Buku dan Tombol Aksi

Bagian ini menggabungkan materi sebelumnya menjadi aplikasi perpustakaan sederhana yang dapat menampilkan daftar buku, menambah buku, meminjam atau mengembalikan buku, dan menghapus buku.

### Prinsip utama: data dulu, lalu render

Daripada mengubah elemen HTML satu per satu, simpan data dalam array, lalu buat fungsi `renderBuku()` yang menggambar ulang tampilan berdasarkan data.

```text
Pengguna klik tombol -> data di array diubah -> renderBuku() -> tampilan diperbarui
```

Dengan cara ini, data selalu menjadi sumber kebenaran, dan tampilan selalu mengikuti data.

### Materi yang digunakan

| Fitur                        | Materi                                                    |
| ---------------------------- | --------------------------------------------------------- |
| Menampilkan kartu buku       | `createElement`, `append`, `textContent` (materi 2 dan 4) |
| Menandai kartu yang dipinjam | `classList`, `setAttribute` (materi 3)                    |
| Form tambah buku             | Event `submit`, `preventDefault` (materi 5)               |
| Pengecekan input             | Ambil data dan validasi (materi 8)                        |
| Tombol Pinjam dan Hapus      | Event delegation, `dataset` (materi 7)                    |
| Mencari kartu dari tombol    | `closest()` (materi 9)                                    |

### HTML

```html
<h1>Perpustakaan</h1>
<p id="info"></p>

<form id="form-buku">
  <input type="text" id="input-judul" placeholder="Judul buku" />
  <input type="text" id="input-penulis" placeholder="Penulis" />
  <button type="submit">Tambah Buku</button>
</form>

<p id="pesan"></p>

<div id="daftar-buku"></div>
```

### CSS

```css
.card {
  border: 1px solid #ccc;
  border-radius: 8px;
  padding: 12px;
  margin-bottom: 8px;
}

.card.dipinjam {
  background-color: #fff3cd;
}

.error {
  color: red;
}
```

### JavaScript

**1. Data dan elemen**

```javascript
let daftarBuku = [
  { id: 1, judul: "Laskar Pelangi", penulis: "Andrea Hirata", dipinjam: false },
  {
    id: 2,
    judul: "Bumi Manusia",
    penulis: "Pramoedya Ananta Toer",
    dipinjam: true,
  },
  { id: 3, judul: "Atomic Habits", penulis: "James Clear", dipinjam: false },
];

const form = document.querySelector("#form-buku");
const inputJudul = document.querySelector("#input-judul");
const inputPenulis = document.querySelector("#input-penulis");
const pesan = document.querySelector("#pesan");
const info = document.querySelector("#info");
const wadah = document.querySelector("#daftar-buku");
```

**2. Fungsi render**

```javascript
function renderBuku() {
  wadah.innerHTML = ""; // kosongkan wadah sebelum menggambar ulang

  if (daftarBuku.length === 0) {
    const kosong = document.createElement("p");
    kosong.textContent = "Belum ada buku.";
    wadah.appendChild(kosong);
  }

  daftarBuku.forEach((buku) => {
    const card = document.createElement("div");
    card.className = "card";
    card.dataset.id = buku.id;
    card.classList.toggle("dipinjam", buku.dipinjam);

    const judul = document.createElement("h3");
    judul.textContent = buku.judul;

    const penulis = document.createElement("p");
    penulis.textContent = buku.penulis;

    const status = document.createElement("strong");
    status.textContent = buku.dipinjam ? "Dipinjam" : "Tersedia";

    const tombolPinjam = document.createElement("button");
    tombolPinjam.textContent = buku.dipinjam ? "Kembalikan" : "Pinjam";
    tombolPinjam.dataset.aksi = "pinjam";

    const tombolHapus = document.createElement("button");
    tombolHapus.textContent = "Hapus";
    tombolHapus.dataset.aksi = "hapus";

    if (buku.dipinjam) {
      tombolHapus.setAttribute("disabled", "true"); // buku yang dipinjam tidak boleh dihapus
    }

    card.append(judul, penulis, status, tombolPinjam, tombolHapus);
    wadah.appendChild(card);
  });

  const jumlahDipinjam = daftarBuku.filter((buku) => buku.dipinjam).length;
  info.textContent = `${daftarBuku.length} buku, ${jumlahDipinjam} dipinjam`;
}
```

Catatan:

- `textContent` dipakai untuk judul dan penulis karena datanya berasal dari input pengguna.
- `dataset.id` pada kartu dan `dataset.aksi` pada tombol dipakai agar event listener tahu buku mana dan aksi apa yang dimaksud.
- Tombol `Hapus` dinonaktifkan jika buku sedang dipinjam.

**3. Tombol aksi dengan event delegation**

```javascript
wadah.addEventListener("click", (event) => {
  const tombol = event.target.closest("button[data-aksi]");

  if (!tombol) return;

  const card = tombol.closest(".card");
  const id = Number(card.dataset.id);

  if (tombol.dataset.aksi === "pinjam") {
    const buku = daftarBuku.find((item) => item.id === id);
    buku.dipinjam = !buku.dipinjam;
  }

  if (tombol.dataset.aksi === "hapus") {
    daftarBuku = daftarBuku.filter((item) => item.id !== id);
  }

  renderBuku();
});
```

Penjelasan:

- Listener hanya dipasang sekali pada `wadah`, bukan pada setiap tombol. Walaupun kartu dibuat ulang setiap `renderBuku()`, tombol tetap berfungsi.
- `closest("button[data-aksi]")` memastikan hanya tombol aksi yang diproses.
- `tombol.closest(".card")` mencari kartu pemilik tombol, lalu `dataset.id` memberi tahu buku yang dimaksud.
- `find()` mencari buku berdasarkan `id`, sedangkan `filter()` membuat array baru tanpa buku yang dihapus.
- `renderBuku()` dipanggil di akhir agar tampilan mengikuti data terbaru.

**4. Form tambah buku dengan validasi**

```javascript
function validasiBuku(judul, penulis) {
  if (judul === "" || penulis === "") {
    return "Judul dan penulis wajib diisi.";
  }

  if (judul.length < 3) {
    return "Judul minimal 3 karakter.";
  }

  const sudahAda = daftarBuku.some(
    (buku) => buku.judul.toLowerCase() === judul.toLowerCase(),
  );

  if (sudahAda) {
    return "Buku dengan judul tersebut sudah ada.";
  }

  return "";
}

form.addEventListener("submit", (event) => {
  event.preventDefault();

  const judul = inputJudul.value.trim();
  const penulis = inputPenulis.value.trim();
  const error = validasiBuku(judul, penulis);

  if (error) {
    pesan.textContent = error;
    pesan.classList.add("error");
    return;
  }

  daftarBuku.push({ id: Date.now(), judul, penulis, dipinjam: false });

  pesan.textContent = "";
  form.reset();
  renderBuku();
});
```

**5. Tampilkan pertama kali**

```javascript
renderBuku();
```

### Alur kerja aplikasi

1. Halaman dibuka, `renderBuku()` menggambar kartu dari array `daftarBuku`.
2. Pengguna menekan **Pinjam**. Listener pada `wadah` menangkap klik melalui bubbling.
3. `closest()` dan `dataset` menentukan buku mana yang dimaksud.
4. Data pada array diubah (`dipinjam` menjadi `true`).
5. `renderBuku()` dipanggil lagi, kartu berubah menjadi **Dipinjam** dan tombol Hapus nonaktif.
6. Saat form dikirim, data divalidasi dulu. Jika lolos, buku dimasukkan ke array dan `renderBuku()` dipanggil kembali.

### Kesalahan umum

| Kesalahan                                             | Akibat                                             | Perbaikan                                   |
| ----------------------------------------------------- | -------------------------------------------------- | ------------------------------------------- |
| Memasang listener pada tombol di dalam `renderBuku()` | Listener menumpuk atau hilang tiap render          | Gunakan event delegation pada `wadah`       |
| Lupa `Number()` pada `dataset.id`                     | `"1" === 1` bernilai `false`, buku tidak ditemukan | Ubah dengan `Number(card.dataset.id)`       |
| Lupa memanggil `renderBuku()` setelah data berubah    | Data berubah, tampilan tidak                       | Panggil `renderBuku()` di akhir setiap aksi |
| Memakai `innerHTML` untuk judul dari input            | Risiko HTML tidak diinginkan masuk                 | Gunakan `textContent`                       |

## Kesimpulan Lanjutan

Materi lanjutan yang dipelajari adalah:

- Event delegation memasang satu listener pada parent untuk menangani banyak elemen, termasuk elemen yang dibuat belakangan.
- `event.target.closest()` mencari elemen terdekat yang cocok dengan selector, dan lebih aman daripada `tagName`.
- `event.target.matches()` memeriksa apakah elemen cocok dengan selector tertentu.
- `data-*` dan `dataset` digunakan untuk menyimpan serta membaca informasi tambahan pada elemen, seperti `id` dan jenis aksi.
- `input.value` selalu berupa string, sehingga perlu `trim()` untuk merapikan teks dan `Number()` untuk angka.
- `FormData` mengambil semua data form sekaligus, dan memerlukan atribut `name` pada setiap input.
- Validasi memeriksa data sebelum diproses, dan sebaiknya dibuat sebagai fungsi yang mengembalikan pesan error.
- DOM Traversal berpindah antar elemen menggunakan `parentElement`, `children`, `firstElementChild`, `lastElementChild`, `nextElementSibling`, dan `previousElementSibling`.
- `children` hanya berisi elemen, sedangkan `childNodes` ikut menghitung node teks.
- `closest()` lebih tahan terhadap perubahan struktur HTML dibanding `parentElement`.
- Pola render menyimpan data dalam array, mengubah data saat ada aksi, lalu memanggil `renderBuku()` untuk memperbarui tampilan.
