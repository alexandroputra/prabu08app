# Prabu Centre 08 - Android APK

Aplikasi Android WebView untuk membuka website [prabucentre08.id](https://prabucentre08.id) dalam tampilan native (tanpa browser chrome).

## Fitur

- Full-screen WebView tanpa address bar atau kontrol browser
- Splash screen dengan logo Prabu Centre 08 dan background biru
- Dukungan upload file (untuk foto profil, bukti transfer, dll.)
- Halaman offline dengan tombol "Coba Lagi" saat tidak ada koneksi
- Navigasi back button berfungsi di dalam WebView
- Link eksternal dibuka di browser, link internal tetap di dalam app
- Status bar berwarna biru (#4B84F3) sesuai tema website

## Persyaratan

- Android Studio (versi terbaru) atau Android SDK Command Line Tools
- JDK 8 atau lebih baru
- Min SDK: Android 5.0 (API 21)
- Target SDK: Android 14 (API 34)

## Cara Build APK

### Menggunakan Android Studio

1. Buka Android Studio
2. Pilih "Open an Existing Project"
3. Pilih folder `android/` ini
4. Tunggu Gradle sync selesai
5. Pilih **Build > Build Bundle(s) / APK(s) > Build APK(s)**
6. APK akan tersedia di: `app/build/outputs/apk/debug/app-debug.apk`

### Menggunakan Command Line

```bash
# Masuk ke folder android
cd android

# Build debug APK
./gradlew assembleDebug

# Build release APK (perlu signing key, lihat bawah)
./gradlew assembleRelease
```

Output APK:
- Debug: `app/build/outputs/apk/debug/app-debug.apk`
- Release: `app/build/outputs/apk/release/app-release.apk`

## Build Release APK (untuk distribusi)

### 1. Buat Signing Key

```bash
keytool -genkey -v -keystore prabucentre08.keystore -alias prabucentre08 -keyalg RSA -keysize 2048 -validity 10000
```

Isi informasi yang diminta (nama, organisasi, dll.)

### 2. Konfigurasi Signing di `app/build.gradle`

Tambahkan block berikut di dalam `android { }`:

```gradle
signingConfigs {
    release {
        storeFile file('../prabucentre08.keystore')
        storePassword 'PASSWORD_ANDA'
        keyAlias 'prabucentre08'
        keyPassword 'PASSWORD_ANDA'
    }
}

buildTypes {
    release {
        signingConfig signingConfigs.release
        minifyEnabled true
        proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
    }
}
```

### 3. Build Release APK

```bash
./gradlew assembleRelease
```

## Mengubah URL Website

Jika URL website berubah, edit satu baris di `MainActivity.java`:

```java
private static final String WEBSITE_URL = "https://prabucentre08.id";
```

## Mengubah App Icon

Ganti file `ic_launcher.png` dan `ic_launcher_round.png` di setiap folder `res/mipmap-*`:

| Folder          | Ukuran     |
|-----------------|------------|
| mipmap-mdpi     | 48x48 px   |
| mipmap-hdpi     | 72x72 px   |
| mipmap-xhdpi    | 96x96 px   |
| mipmap-xxhdpi   | 144x144 px |
| mipmap-xxxhdpi  | 192x192 px |

Atau gunakan Android Studio > Right click `res` > New > Image Asset.

## Struktur Project

```
android/
├── app/
│   ├── build.gradle
│   ├── proguard-rules.pro
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/id/prabucentre08/app/
│       │   ├── MainActivity.java      # WebView utama
│       │   └── SplashActivity.java    # Splash screen
│       └── res/
│           ├── drawable/              # Button shapes
│           ├── layout/                # XML layouts
│           ├── mipmap-*/              # App icons
│           └── values/                # Colors, themes
├── build.gradle
├── settings.gradle
├── gradle.properties
└── gradle/wrapper/
```
