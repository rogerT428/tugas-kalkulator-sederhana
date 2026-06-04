# Panduan Implementasi Siklus Hidup Activity & Pengenalan UI Android

Repositori/Proyek ini merupakan contoh penerapan dasar pemrograman aplikasi bergerak (Android) yang mencakup:
1. Pengenalan desain antarmuka layout menggunakan XML.
2. Pengenalan perizinan (*permissions*) dan konfigurasi tema pada `AndroidManifest.xml`.
3. Integrasi antarmuka dan pengamatan Siklus Hidup (*Activity Lifecycle*) menggunakan Java.

Berikut adalah tahapan demi tahapan yang telah diterapkan dalam pengembangan proyek ini:

## Tahap 1: Konfigurasi Dependensi Gradle (`build.gradle.kts`)
Untuk dapat menggunakan komponen Android UI klasik berbasis XML di dalam *project template* bawaan (terutama jika awalnya menggunakan Jetpack Compose), kita harus menyertakan pustaka klasik Android.
- **Lokasi File:** `app/build.gradle.kts`
- **Aksi:** Menambahkan dependensi *AppCompat* dan *Material* ke dalam blok `dependencies`.
  ```kotlin
  dependencies {
      implementation("androidx.appcompat:appcompat:1.6.1")
      implementation("com.google.android.material:material:1.10.0")
      // dependensi lainnya...
  }
  ```
- **Tujuan:** Memungkinkan aplikasi untuk mewarisi `AppCompatActivity` dan memanfaatkan tema UI berbasis Material Design.

## Tahap 2: Pengaturan Deklarasi Aplikasi & Perizinan (`AndroidManifest.xml`)
Manifest berguna untuk mendaftarkan komponen inti aplikasi kepada sistem Android.
- **Lokasi File:** `app/src/main/AndroidManifest.xml`
- **Aksi:** 
  1. Menambahkan baris permohonan perizinan akses internet: 
     `<uses-permission android:name="android.permission.INTERNET" />`
  2. Mengubah properti tema (`android:theme`) pada tag `<application>` dan `<activity>` menjadi `@style/Theme.AppCompat.Light.DarkActionBar`.
- **Tujuan:** Menyediakan izin standar dan menghindari bentrok atau error tema, karena `AppCompatActivity` wajib menggunakan tema berturunan `Theme.AppCompat`.

## Tahap 3: Pembuatan Desain Antarmuka (`activity_main.xml`)
Desain antarmuka tradisional pada Android dikelola sepenuhnya oleh file XML.
- **Lokasi File:** `app/src/main/res/layout/activity_main.xml`
- **Aksi:** Menggunakan tata letak linier (`LinearLayout`) dengan susunan vertikal yang terdiri atas 3 komponen utama:
  - `TextView` (id: `tvGreeting`) untuk menampilkan teks sambutan statis.
  - `Button` (id: `btnKlik`) sebagai tombol interaksi klik.
  - `TextView` (id: `tvLog`) untuk menampilkan catatan (*log*) teks pergerakan aktivitas secara dinamis di layar.
- **Tujuan:** Menyiapkan struktur tata letak aplikasi yang nantinya dipanggil dan diisi oleh kode Java.

## Tahap 4: Implementasi Logika Pemrograman (`MainActivity.java`)
Di dalam pola arsitektur Android, Activity adalah titik masuk visual bagi pengguna.
- **Lokasi File:** `app/src/main/java/com/mobile/uph24si3/MainActivity.java`
- **Aksi:**
  1. Mengubah deklarasi kelas agar mewarisi `AppCompatActivity`.
  2. Memuat desain XML ke layar menggunakan perintah `setContentView(R.layout.activity_main)`.
  3. Mengaitkan elemen layar (tombol dan teks) dengan variabel *backend* menggunakan metode `findViewById()`.
  4. Menerapkan pendengar kejadian (`setOnClickListener`) pada tombol yang akan mencetak pesan ke `tvLog` jika ditekan.
  5. Melakukan *override* fungsi siklus hidup dasar sistem Android: `onStart()`, `onResume()`, `onPause()`, `onStop()`, dan `onDestroy()`.
  6. Memanggil utilitas `Log.d()` serta fungsi bantu buatan sendiri (`appendLog`) pada setiap perpindahan fase agar Anda bisa melihat urutan fase yang sedang aktif.
- **Tujuan:** Menyelaraskan seluruh bagian, menangani interaksi pengguna, dan secara aktif memantau perubahan status *Lifecycle* secara *real-time*.

---

## ⚠️ Troubleshooting (Penyelesaian Masalah)

Jika Anda melihat peringatan baris merah (*error*) pada `MainActivity.java` yang bertuliskan seperti:
- `Cannot resolve symbol 'appcompat'`
- `Cannot resolve symbol 'AppCompatActivity'`
- `Cannot resolve method 'onCreate(Bundle)'` (atau metode lainnya)

**Penyebab:**
Android Studio belum melakukan pembaruan/pengunduhan pustaka (*library*) `appcompat` yang baru saja ditambahkan ke dalam file konfigurasi. Karena pustakanya belum dimuat, maka kelas `AppCompatActivity` beserta seluruh metode turunan di dalamnya dianggap tidak ada atau tidak dikenali oleh sistem.

**Solusi:**
Anda perlu melakukan **Sinkronisasi Gradle (Gradle Sync)**:
1. Cari *banner* notifikasi di bagian atas teks editor Android Studio yang bertuliskan **"Gradle files have changed since last project sync"**.
2. Klik tombol **"Sync Now"** di sebelah kanan *banner* tersebut.
3. **Alternatif Manual:** Klik menu bar **File > Sync Project with Gradle Files** (atau klik ikon berbentuk kepala Gajah dengan panah kecil berwarna biru di pojok kanan atas).
4. Tunggu proses sinkronisasi dan *indexing* selesai (dapat dilihat pada *progress bar* di bagian bawah layar). Seluruh teks peringatan merah akan secara otomatis hilang ketika prosesnya selesai.
