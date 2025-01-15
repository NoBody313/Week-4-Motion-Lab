# 📖 Navigation dan Routing Menggunakan Jetpack Compose

Jetpack Compose adalah toolkit modern untuk membuat UI yang deklaratif di Android, termasuk fitur navigasi ke berbagai layar. Dengan menggunakan library **Navigation Compose**, pengelolaan rute dalam aplikasi menjadi lebih mudah, sederhana, efisien, dan dapat memenuhi berbagai kebutuhan seperti pengiriman data dari berbagai layar, deep linking, dan state management.

---

## 📌 Pengertian Navigation dan Routing

- **Navigation**:
  Navigation adalah proses perpindahan antar layar dalam aplikasi. Hal ini mencakup navigasi ke layar berikutnya, kembali ke layar sebelumnya, atau memanipulasi backstack aplikasi.

- **Routing**:
  Routing merujuk pada pengelolaan jalur atau alur yang dilewati pengguna dalam aplikasi, termasuk bagaimana data dipindahkan antara layar dan bagaimana setiap layar diakses.

Dalam Jetpack Compose, **Navigation Compose** memanfaatkan:
1. **Deklarasi berbasis composable**: Setiap layar didefinisikan sebagai fungsi composable.
2. **State dan Backstack otomatis**: Menyederhanakan pengelolaan layar tanpa memikirkan detail implementasi backstack.
3. **Pengiriman data antar layar**: Mendukung pengelolaan argument dan deep linking.

---

## ✨ Komponen Utama dalam Navigation Compose

1. **NavController**:
   Objek inti untuk mengontrol navigasi antar layar. Digunakan untuk berpindah antar layar (`navigate`) dan memanipulasi backstack (`popBackStack`).

2. **NavHost**:
   Kontainer utama yang mendefinisikan semua rute dalam aplikasi. Setiap layar ditentukan sebagai destination di dalam `NavHost`.

3. **Composable Destination**:
   Layar atau tujuan yang didefinisikan menggunakan fungsi `composable`.

4. **Arguments**:
   Data yang diteruskan antara layar melalui rute.

---

## 🔧 Menambahkan Dependensi

Untuk menggunakan navigasi di Compose, tambahkan dependensi berikut ke dalam file `build.gradle` aplikasi Kamu:

```gradle
dependencies {
    implementation ("androidx.navigation:navigation-compose:2.8.5")
}
```

---

## 🚀 Langkah-Langkah Implementasi

### 1️⃣ Menyiapkan `NavHost`

`NavHost` digunakan untuk mendefinisikan rute (route) atau layar yang ada didalam aplikasi. Kamu juga perlu membuat instance `NavController` menggunakan fungsi `rememberNavController`.

```kotlin
@Composable
fun MainNavigation() {
    val navController = rememberNavController()

    NavHost(
        navController = navController,
        startDestination = "home"
    ) {
        composable("home") { HomeScreen(navController) }
        composable("details/{itemId}") { backStackEntry ->
            val itemId = backStackEntry.arguments?.getString("itemId")
            DetailScreen(navController, itemId)
        }
    }
}
```

- **`startDestination`**: Menentukan layar awal aplikasi.
- **`composable(route)`**: Mendefinisikan setiap layar dengan rutenya masing-masing.
- **`backStackEntry.arguments`**: Digunakan untuk mengambil data yang diteruskan melalui rute.

---

### 2️⃣ Membuat Composable Screen

#### Contoh 1: HomeScreen
Layar pertama dengan tombol navigasi ke layar detail.

```kotlin
@Composable
fun HomeScreen(navController: NavController) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Home Screen", style = MaterialTheme.typography.h4)
        Button(onClick = { navController.navigate("details/{itemId}") }) {
            Text("Go to Detail Screen")
        }
    }
}
```

#### Contoh 2: DetailScreen
Layar kedua yang menerima data `itemId` dari layar pertama.

```kotlin
@Composable
fun DetailScreen(navController: NavController, itemId: String?) {
    Column(
        modifier = Modifier.fillMaxSize(),
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center
    ) {
        Text("Detail Screen")
        Text("Item ID: $itemId")
        Button(onClick = { navController.popBackStack() }) {
            Text("Back to Home")
        }
    }
}
```

---

### 3️⃣ Mengirim Data Antar Layar

Untuk mengirim data antar layar, tambahkan argument pada rute. Contoh:
- Rute: `details/{itemId}`
- Pengiriman data: `navController.navigate("details/itemId")`

Pada layar tujuan, data diterima melalui `backStackEntry.arguments`.

---

### 4️⃣ Mengelola Backstack

Gunakan `navController.popBackStack()` untuk kembali ke layar sebelumnya. Untuk menutup aplikasi atau keluar dari stack, gunakan parameter `inclusive = true`.

---

## 📘 Best Practices

1. **Modularisasi**: Memisahkan setiap layar ke dalam file atau modul terpisah untuk menjaga kebersihan kode.
2. **State Management**: Menggunakan `ViewModel` untuk mengelola data layar yang kompleks.
3. **Error Handling**: Melakukan validasi argument dan kondisi navigasi untuk menghindari crash.
4. **Testing Navigation**: Menggunakan Compose Testing untuk memastikan alur navigasi berjalan sesuai desain.

---

## 💡 Contoh Proyek

Berikut adalah contoh sederhana struktur navigasi:

```kotlin
@Composable
fun AppNavigation() {
    val navController = rememberNavController()

    NavHost(navController = navController, startDestination = "home") {
        composable("home") { HomeScreen(navController) }
        composable(
            route = "details/{itemId}",
            arguments = listOf(navArgument("itemId") { type = NavType.StringType })
        ) { backStackEntry ->
            val itemId = backStackEntry.arguments?.getString("itemId")
            DetailScreen(navController, itemId)
        }
    }
}
```

---

## 💬 Kesimpulan

Jetpack Compose Navigation mempermudah pengelolaan navigasi dalam aplikasi Android. Dengan menggunakan pendekatan deklaratif, integrasi dengan Compose UI menjadi seamless (mulus), memungkinkan pengelolaan data antara layar dan backstack secara otomatis. Navigation Compose juga mendukung deep linking, yang dapat meningkatkan fleksibilitas aplikasi lebih modern.

--- 
