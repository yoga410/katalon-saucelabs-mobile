Berikut adalah seluruh isi dokumentasi yang sudah digabung menjadi **satu blok kode utuh**. Anda cukup klik tombol **Copy** di pojok kanan atas blok kode berikut, lalu *paste* langsung ke file `README.md`:

````markdown
# 📱 Katalon Mobile Automation Testing - Sauce Labs (Swag Labs App)

[![Katalon Studio](https://img.shields.io/badge/Katalon%20Studio-Mobile%20Testing-blue.svg)](https://katalon.com/)
[![Appium](https://img.shields.io/badge/Appium-Automated-purple.svg)](https://appium.io/)
[![Platform](https://img.shields.io/badge/Platform-Android-green.svg)](https://www.android.com/)
[![SauceLabs](https://img.shields.io/badge/SauceLabs-Sample%20App%20v2.7.1-red.svg)](https://saucelabs.com/)

Repositori ini berisi proyek otomatisasi pengujian mobile (*Mobile Automation Testing*) menggunakan **Katalon Studio 10.4.3** berbasis **Appium** untuk aplikasi **Sauce Labs Sample Mobile App (Swag Labs Android App v2.7.1)**.

---

## 📌 Daftar Isi
- [Tentang Proyek](#-tentang-proyek)
- [Struktur Direktori Proyek](#-struktur-direktori-proyek)
- [Global Variables & Profiles](#-global-variables--profiles)
- [Skenario Pengujian](#-skenario-pengujian)
- [Prasyarat (Prerequisites)](#-prasyarat-prerequisites)
- [Panduan Instalasi & Setup](#-panduan-instalasi--setup)
- [Cara Menjalankan Pengujian](#-cara-menjalankan-pengujian)
- [Contoh Kode Script Test Case](#-contoh-kode-script-test-case)
- [Best Practices & Tips](#-best-practices--tips)

---

## 📖 Tentang Proyek

Proyek ini dibangun untuk memvalidasi alur fungsional pada aplikasi e-commerce demo Swag Labs dari Sauce Labs. Pengujian mencakup skenario otentikasi positif (*Happy Path*) dan negatif (*Negative Tests*), pengujian kestabilan elemen UI, dan integrasi pengujian multi-device.

- **Aplikasi Target:** Sauce Labs Sample App (`APK-sauce-labs.2.7.1.apk`)
- **Framework:** Katalon Studio 10.4.3 & Appium
- **Bahasa Script:** Groovy
- **Target OS:** Android (Lokal Emulator/Real Device)

---

## 📂 Struktur Direktori Proyek

```text
katalon-saucelabs-mobile/
├── 📁 Apps/                               # Penyimpanan file binary aplikasi
│   └── APK-sauce-labs.2.7.1.apk           # File APK aplikasi target
├── 📁 Checkpoints/                        # Snapshot verifikasi data (opsional)
├── 📁 Data Files/                         # External Test Data (Excel, CSV, Internal Data)
├── 📁 Drivers/                            # External custom JAR / drivers
├── 📁 Include/                            # BDD Cucumber features & step definitions
│   ├── 📁 features/                       # Berkas .feature (Gherkin)
│   └── 📁 scripts/groovy/                 # Step Definitions & Cucumber Hooks
├── 📁 Keywords/                           # Custom Groovy Keywords (Gesture, Helper)
├── 📁 Object Repository/                  # Mobile UI Test Objects (.rs)
│   ├── 📁 Common/                         # Elemen universal (Navbar, Alert, Modal)
│   ├── 📁 Login/                          # Elemen halaman Login (Username, Password, Button, Error)
│   └── 📁 Products/                       # Elemen katalog produk & cart
├── 📁 Profiles/                           # Profil eksekusi & Global Variables
│   ├── default.glbl                       # Profil default bawaan Katalon
│   └── profiles.glbl                      # Profil khusus skenario Swag Labs
├── 📁 Reports/                            # Hasil report eksekusi (HTML, PDF, log)
├── 📁 Scripts/                            # Kode script Groovy untuk Test Cases
│   └── 📁 Authentication/                 # Script logika login & validasi
├── 📁 Test Cases/                         # Definisi skenario test case (.tc)
│   └── 📁 Authentication/                 # TC Login (Success, Locked, Problem User)
├── 📁 Test Suites/                        # Kumpulan Test Suite (.ts) & Suite Collections (.tsg)
│   ├── TS_Smoke_Test.ts                   # Suite pengujian cepat
│   └── TS_Regression_Test.ts              # Suite pengujian menyeluruh
├── 📁 settings/                           # Konfigurasi proyek & Desired Capabilities
│   ├── 📁 internal/                       # Konfigurasi internal Katalon
│   └── 📁 external/                       # Custom Capabilities (Appium / SauceLabs)
├── .classpath                             # Classpath konfigurasi Groovy/Java
├── .project                               # Metadata proyek Eclipse
└── katalon-saucelabs-mobile.prj           # Berkas project utama Katalon Studio
```

---

## ⚙️ Global Variables & Profiles

Konfigurasi kredensial dan path file dikelola terpusat pada berkas profil **`Profiles/profiles.glbl`**:

| Nama Variabel | Tipe Data | Nilai Default | Keterangan / Fungsi |
| :--- | :--- | :--- | :--- |
| `valid_username` | `STRING` | `'standard_user'` | Kredensial akun aktif untuk skenario Login Sukses (*Happy Path*). |
| `locked_username` | `STRING` | `'locked_out_user'` | Akun terblokir untuk validasi pesan kesalahan otentikasi. |
| `problem_username` | `STRING` | `'problem_user'` | Akun khusus simulasi isu UI / functional problem. |
| `password` | `STRING` | `'secret_sauce'` | Password standar seluruh akun demo Swag Labs. |
| `apkName` | `STRING` | `'Apps/APK-sauce-labs.2.7.1.apk'` | Path lokasi berkas binary APK di dalam proyek. |

---

## 🧪 Skenario Pengujian

### 1. TC_loginValid_ - Login Success (Happy Path)
- **Deskripsi:** Memverifikasi pengguna dapat masuk ke aplikasi menggunakan username dan password valid.
- **Data Input:** `GlobalVariable.valid_username` & `GlobalVariable.password`.
- **Ekspektasi:** Aplikasi berhasil mengarahkan ke halaman katalog produk (*Products Screen*).

### 2. TC_loginLocked_ - Login Locked Out User (Negative Test)
- **Deskripsi:** Memverifikasi penanganan sistem saat user yang diblokir mencoba masuk.
- **Data Input:** `GlobalVariable.locked_username` & `GlobalVariable.password`.
- **Ekspektasi:** Muncul notifikasi error: `"Sorry, this user has been locked out."`.

### 3. TC_loginProblem_ - Login Problem User (Edge Case)
- **Deskripsi:** Menguji respons aplikasi saat login menggunakan akun problem user.
- **Data Input:** `GlobalVariable.problem_username` & `GlobalVariable.password`.
- **Ekspektasi:** Berhasil login dan memvalidasi respons katalog produk.

---

## 📋 Prasyarat (Prerequisites)

Sebelum menjalankan pengujian, pastikan lingkungan pengujian Anda telah terinstal:
- **Katalon Studio** (v10.4.3)
- **Node.js** (v18+ atau v20+)
- **Appium Server** (v2.x) & Driver `uiautomator2`:
  ```bash
  npm install -g appium
  appium driver install uiautomator2
  ```
- **Java Development Kit (JDK)**: JDK 8 atau JDK 17
- **Android SDK & Build Tools** terpasang dengan konfigurasi environment variable:
  - `ANDROID_HOME` atau `ANDROID_SDK_ROOT`
  - Tambahkan `%ANDROID_HOME%\platform-tools` ke `PATH` (untuk binary `adb`)

---

## 🛠️ Panduan Instalasi & Setup

1. **Clone Repositori:**
   ```bash
   git clone <URL_REPOSITORY_ANDA>
   cd katalon-saucelabs-mobile
   ```

2. **Verifikasi File APK:**
   Pastikan berkas APK tersedia pada direktori:
   ```text
   Apps/APK-sauce-labs.2.7.1.apk
   ```

3. **Hubungkan Device Android:**
   Pastikan emulator atau perangkat fisik Android terhubung dan terdeteksi oleh `adb`:
   ```bash
   adb devices
   ```

4. **Buka Proyek:**
   - Jalankan **Katalon Studio**.
   - Pilih menu **File** > **Open Project** > arahkan ke berkas `katalon-saucelabs-mobile.prj`.

---

## 🚀 Cara Menjalankan Pengujian

### A. Melalui Katalon Studio IDE (GUI)
1. Di panel *Test Explorer*, buka folder **Test Suites** > pilih `TS_Login`.
2. Pada toolbar kanan atas, pastikan profil eksekusi yang dipilih adalah **`profiles`**.
3. Klik tombol dropdown panah hijau di samping **Run**, lalu pilih **Android**.
4. Pilih perangkat (*Device ID*) yang terdeteksi, lalu klik **Start**.

### B. Melalui Terminal / CLI (Katalon Runtime Engine - KRE)
```bash
katalonc -noSplash -runMode=console \
  -projectPath="katalon-saucelabs-mobile.prj" \
  -testSuitePath="Test Suites/TS_Regression_Test" \
  -executionProfile="profiles" \
  -browserType="Android" \
  -retry=0
```