# Plantilla de proyecto Android (julio 2026)

## Cómo usarla

1. Crea el proyecto en Android Studio: `File > New > New Project > Empty Activity`.
2. Reemplaza el archivo `gradle/libs.versions.toml` generado por el `libs.versions.toml` de esta plantilla.
3. Reemplaza el `build.gradle.kts` raíz con el contenido de `build.gradle.kts.root`.
4. Reemplaza `app/build.gradle.kts` con el contenido de `build.gradle.kts.app` (ajusta `namespace` y `applicationId`).
5. Actualiza el Gradle Wrapper a la última versión:
   ```
   ./gradlew wrapper --gradle-version 9.6.1 --distribution-type all
   ```
   (ejecútalo dos veces, la primera actualiza Gradle y la segunda el propio wrapper).
6. Sync del proyecto (`Sync Now` en la barra que aparece arriba).

## Cómo mantenerla al día (evita que se desactualice sola)

- Las versiones de librerías cambian cada pocas semanas. Antes de empezar un proyecto importante, corre:
  ```
  ./gradlew dependencyUpdates
  ```
  (requiere el plugin `com.github.ben-manes.versions`, opcional pero útil) o revisa manualmente:
    - AGP: https://developer.android.com/build/releases/agp-9-2-0-release-notes
    - Compose BOM: https://developer.android.com/develop/ui/compose/bom/bom-mapping
    - Kotlin: https://kotlinlang.org/docs/releases.html
- Android Studio también te avisa con un banner "Update available" cuando hay una versión nueva de AGP o Gradle compatible — acéptalo si no estás a mitad de un release.

## Notas de compatibilidad importantes

- AGP 9.2 requiere **Gradle 8.11 o superior** (usa 9.6.1 para ir sobre seguro).
- El compilador de Compose ahora se gestiona con el plugin `org.jetbrains.kotlin.plugin.compose`, con la MISMA versión que Kotlin (ya no existe `kotlinCompilerExtensionVersion` suelto).
- Si usas KSP en vez de KAPT (recomendado — KAPT está en modo mantenimiento), la versión de KSP debe llevar el sufijo que corresponde al patch de Kotlin (ej. `2.2.20-1.0.24`); revisa la versión exacta en https://github.com/google/ksp/releases antes de fijarla en un proyecto real.
- JDK 17 es el mínimo recomendado para el Gradle Daemon.

## Íconos en Material 3

- **Set básico** (`Icons.Default.X`): desde `material3` 1.4.0 (sept 2025) **ya NO viene incluido automáticamente**.
  Hace falta agregar `androidx-material-icons-core` a mano (ya está activa en esta plantilla).
- **Set completo** (`Icons.Outlined.X`, `Icons.Rounded.X`, etc.): descomenta `androidx-material-icons-extended`
  en `libs.versions.toml` y en `app/build.gradle.kts`. No lleva versión propia, la gobierna el Compose BOM.
  Es una librería grande — R8 la reduce en `release`, pero en `debug` puede alargar el build.
- **Solo un par de íconos puntuales**: mejor usa `File > New > Vector Asset > Clip Art` en Android Studio,
  que genera un `.xml` local sin agregar ninguna dependencia.
- Si `Icons.Default.X` no autocompleta después de agregar la dependencia y sincronizar: `File > Invalidate Caches / Restart`.

## Tip para ahorrar tiempo en el futuro

Guarda esta carpeta como **plantilla de proyecto** en Android Studio:
`File > New > New Project` (con tu proyecto ya configurado como quieres) → después usa
`File > Save as File Template` en cada archivo clave, o simplemente clona esta carpeta como base
y usa `Ctrl+Shift+A > "Refactor > Rename"` para renombrar el paquete cada vez.
Alternativa más robusta: sube esta carpeta a un repo Git privado tipo `android-starter-template`
y clónalo cada vez que empieces un proyecto (`git clone` + rename del paquete) en vez de crear desde cero.