# 🗺️ MapaCustos

![Kotlin](https://img.shields.io/badge/Kotlin-100000?style=for-the-badge&logo=kotlin&logoColor=white) ![Jetpack Compose](https://img.shields.io/badge/Jetpack%20Compose-100000?style=for-the-badge&logo=jetpack&logoColor=white) ![OSMDroid](https://img.shields.io/badge/OSMDroid-100000?style=for-the-badge) ![Room](https://img.shields.io/badge/Room-100000?style=for-the-badge)

Aplicación Android desarrollada en **Kotlin** con **Jetpack Compose**, que permite visualizar calles con niveles de peligrosidad personalizados, configurar colores, sonido, gestionar usuarios y rutas evitando calles peligrosas mediante mapas OpenStreetMap.

El mapa se centra **por defecto en Alzira** (`39.1747, -0.4232`), y si se concede permiso de ubicación, se centra automáticamente en la ubicación del usuario.

---

## 🚀 Tecnologías

- **Lenguaje:** Kotlin  
- **UI:** Jetpack Compose (Material 3)  
- **Base de datos:** Room  
- **Persistencia rápida:** SharedPreferences  
- **Mapas:** OSMDroid (OpenStreetMap)  
- **Navegación:** Navigation Compose  
- **Asincronía:** Coroutines  

---

## 📂 Estructura del proyecto


```
com.example.mapacustos
│
├── MainActivity.kt → Punto de entrada de la app
│
├── ui
│ ├── pantalla → Pantallas Compose
│ │ ├── LoginScreen.kt
│ │ ├── RegisterScreen.kt
│ │ ├── UserSettingsScreen.kt
│ │ ├── SoundColorSettingsScreen.kt
│ │ └── PanelPerformanceScreen.kt
│ │
│ └── datos → Capa de datos
│ ├── AppDatabase.kt
│ ├── UserEntity.kt
│ ├── StreetEntity.kt
│ ├── UserDao.kt
│ ├── StreetDao.kt
│ ├── Routes.kt
│ └── DatabaseInitializer.kt
```


---

## 🗄️ Base de datos

### UserEntity
- name  
- email (clave única)  
- password  
- gender  
- birthDate  

### StreetEntity
- name  
- latitude / longitude (GPS)  
- dangerLevel (0-3)  
- includesViolation  
- includesViolence  

---

## 🗺️ Sistema de mapa

- Basado en **OSMDroid**.  
- **Ubicación por defecto:** Centro de Alzira (`39.1747, -0.4232`).  
- Se centra automáticamente en la ubicación actual si se conceden permisos de GPS.  
- Carga calles desde JSON inicial.  
- Áreas peligrosas dibujadas con `Polygon` semi-transparente según nivel de peligrosidad.  
- Colores personalizables por nivel.  
- Al actualizar nivel → se limpian y redibujan overlays automáticamente.  

---

## 🎨 Sistema de colores

- Guardados en **SharedPreferences** (`color_cache`)  
- Cada nivel (1–3) tiene color editable RGB.  
- Cambios afectan **inmediatamente** al renderizado del mapa.  

---

## 🔊 Sistema de sonido

- Configuración para:
  - Nivel 1  
  - Nivel 2  
  - Vibración  
- Estado local en Compose (expandible a DataStore).  

---

## 🔐 Sistema de sesión

- Guardado en SharedPreferences: `"session" -> current_user (email)`  
- Al cerrar sesión:
  - Se elimina `current_user`  
  - Redirección a pantalla de registro  

---

## 📦 JSON inicial

- Cargado por `DatabaseInitializer.loadJsonIfEmpty()`  
- Solo si tabla `Street` está vacía.  

---

## 📍 Rutas y navegación

- Cálculo de rutas **evitando calles con nivel peligroso** no permitido.  
- Si no hay ruta segura → se muestra ruta directa.  
- Se muestran rutas con `Polyline` azul en el mapa.  
- Se calculan desvíos automáticos para evitar calles peligrosas.  

---

## ▶️ Ejecución

1. Clonar repositorio  
2. Abrir en Android Studio  
3. Sync Gradle  
4. Ejecutar en dispositivo con:
   - Permisos de ubicación  
   - Android 8+  

---

## ⚙️ Mejoras futuras

- Migrar sesión a DataStore  
- Cifrado de contraseñas  
- Añadir ViewModel  
- Implementar arquitectura MVVM completa  
- Persistir configuración de sonido  
- Sistema de autenticación real  

---

## 👨‍💻 Convenciones importantes

- Limpiar overlays antes de redibujar mapa  
- No animar `Surface` directamente si tiene sombra  
- No mezclar `android.graphics.Color` con `compose.ui.graphics.Color` sin alias  
- Centro de mapa por defecto: **Alzira (`39.1747, -0.4232`)**, fallback automático si GPS no disponible  

---

## 📖 Manual de usuario

### Registro
- Introducir nombre, email, contraseña, fecha de nacimiento y género.  
- Pulsar “Create Account”.  

### Inicio de sesión
- Email y contraseña → abre mapa.  

### Pantalla principal
- **Botón inferior izquierdo:** Activa/desactiva modo ruta.  
- **Botón superior derecho:** Panel de ajustes:
  - Usuario  
  - Sonido / color  
  - Mapa  

### Ajustes de usuario
- Ver email  
- Cambiar contraseña, fecha, género  
- Guardar cambios  
- Cerrar sesión  

### Ajustes de sonido / color
- Activar/desactivar sonido por nivel  
- Activar vibración  
- Colores por nivel (editable RGB, vista previa y restablecer)  

### Ajustes de mapa
- Modificar nivel de peligrosidad  
- Actualiza representación visual del mapa  

### Cerrar sesión
- Desde Ajustes de Usuario → “Cerrar sesión”  

---

## 🛠️ Solución de problemas

- Calles no visibles → verificar permisos GPS  
- Colores no cambian → guardar cambios  
- Mapa no actualiza → reiniciar app  
