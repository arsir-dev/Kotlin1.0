# Ensiklopedia Kotlin 1.0: Modul `kotlin-reflect`

## Pendahuluan
Pada Kotlin 1.0 (dirilis 15 Februari 2016), `kotlin-reflect` adalah library opsional yang menyediakan API introspeksi untuk program Kotlin. Berbeda dengan refleksi standar Java, `kotlin-reflect` dirancang untuk memahami konsep unik Kotlin seperti *nullability*, *properties*, dan *extension functions*.

## Komponen Utama (Core API)

### 1. `KClass<T>`
Representasi utama dari sebuah kelas Kotlin.
- **Cara Mendapatkan:** `Person::class` atau `obj.javaClass.kotlin`.
- **Fitur 1.0:** Mengakses `simpleName`, `qualifiedName`, `members`, `constructors`, dan `nestedClasses`.

### 2. `KCallable<out R>`
Interface dasar untuk segala sesuatu yang bisa dipanggil di Kotlin (Fungsi dan Properti).
- **`KFunction<out R>`:** Mewakili fungsi. Memiliki metode `.call(*args)` untuk eksekusi dinamis.
- **`KProperty<out R>`:** Mewakili properti (val/var). Memungkinkan pembacaan (`get`) dan penulisan (`set` jika `KMutableProperty`).

### 3. `KType`
Representasi tipe data di runtime.
- **Nullability:** Salah satu fitur terpenting di 1.0 adalah `isMarkedNullable`, yang memungkinkan developer mengecek apakah suatu parameter/property boleh bernilai null.

---

## Interoperabilitas JVM (`kotlin.reflect.jvm`)
Bagian ini sangat krusial di Kotlin 1.0 untuk menjembatani antara ekosistem Kotlin dan Java. Berikut adalah API yang tersedia di paket `kotlin.reflect.jvm`:

| API | Deskripsi |
| :--- | :--- |
| `isAccessible` | Mengekstensi `KCallable` untuk mendapatkan atau mengatur aksesibilitas (seperti `setAccessible` di Java). |
| `javaConstructor` | Mendapatkan `java.lang.reflect.Constructor` dari `KFunction`. |
| `javaField` | Mendapatkan `java.lang.reflect.Field` yang mendasari sebuah `KProperty`. |
| `javaMethod` | Mendapatkan `java.lang.reflect.Method` dari `KFunction` atau getter dari `KProperty`. |
| `javaSetter` | Mendapatkan `java.lang.reflect.Method` untuk setter dari `KMutableProperty`. |
| `javaType` | Mendapatkan `java.lang.reflect.Type` dari `KType`. |
| `jvmName` | Mendapatkan nama asli anggota tersebut di dalam bytecode JVM. |
| `kotlinFunction` | Mengekstensi `Method` atau `Constructor` (Java) untuk dikonversi menjadi `KFunction`. |
| `kotlinProperty` | Mengekstensi `Field` (Java) untuk dikonversi menjadi `KProperty`. |
| `reflect()` | Fungsi ekstensi untuk lambda atau referensi fungsi untuk mendapatkan instance `KFunction`-nya. |

---

## Contoh Penggunaan (Sintaks 1.0)

### Akses Field Java dari Properti Kotlin
```kotlin
val prop = Person::name
val field = prop.javaField // Menggunakan kotlin.reflect.jvm
field?.isAccessible = true
```

### Konversi Tipe ke Java Type
```kotlin
val kType = prop.returnType
val javaType = kType.javaType
println("Tipe Java: ${javaType.typeName}")
```

---

## Karakteristik & Batasan di Versi 1.0
1. **Ukuran Library:** File `kotlin-reflect.jar` memiliki ukuran sekitar **2.1 MB**.
2. **Internal Metadata:** Bergantung pada anotasi `@Metadata`. Jika metadata dihapus (oleh ProGuard), refleksi tidak akan berjalan.
3. **Kinerja:** Layer abstraksi di atas refleksi Java memberikan beban performa tambahan pada saat runtime.

---

## Referensi Resmi
Dokumentasi ini disusun berdasarkan sumber referensi berikut:
1. **Official Kotlin Documentation (v1.0):** *Reflection*. [kotlinlang.org/docs/1.0/reference/reflection.html](https://kotlinlang.org/docs/1.0/reference/reflection.html)
2. **JetBrains Blog (2016):** *Kotlin 1.0 Released*. [blog.jetbrains.com/kotlin/2016/02/kotlin-1-0-released/](https://blog.jetbrains.com/kotlin/2016/02/kotlin-1-0-released-pragmatic-language-for-jvm-and-android/)
3. **API Reference:** `kotlin.reflect.jvm` package documentation for Kotlin 1.0.

---
*Dokumen ini adalah bagian dari Proyek Ensiklopedia Kotlin 1.0.*