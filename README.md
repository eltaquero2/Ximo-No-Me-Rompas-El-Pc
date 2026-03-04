## 🗺️ MapaCustos

Aplicación Android desarrollada en Kotlin con Jetpack Compose que permite visualizar calles con niveles de peligrosidad personalizados, configurar colores, sonido y gestionar usuarios mediante base de datos local.

---

## 🚀 Tecnologías utilizadas

- Kotlin
- Jetpack Compose (Material 3)
- Room Database
- DataStore / SharedPreferences
- OSMDroid (mapa OpenStreetMap)
- Navigation Compose
- Coroutines

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

## 🗄️ Base de Datos

Room contiene:

### 👤 UserEntity
- name
- email (clave única)
- password
- gender
- birthDate

### 🛣️ StreetEntity
- name
- latitude
- longitude
- dangerLevel (0-3)

---

## 🗺️ Sistema de Mapa

- Basado en OSMDroid.
- Carga inicial de calles desde un archivo JSON.
- Las áreas peligrosas se dibujan como `Polygon`.
- Los colores se almacenan en preferencias.
- Al modificar nivel → se limpian overlays y se redibujan.

---

## 🎨 Sistema de Colores

Se almacenan mediante:
ColorPreferences
SharedPreferences (cache rápido)


Cada nivel tiene un color personalizable mediante RGB.

---

## 🔊 Sistema de Sonido

Configuración de:
- Nivel 1
- Nivel 2
- Vibración

Actualmente gestionado como estado local (expandible a DataStore).

---

## 🔐 Sistema de Sesión

- Se guarda en SharedPreferences:
"session" -> current_user (email)


- Al cerrar sesión:
    - Se elimina `current_user`
    - Se redirige a Register

---

## 📦 Archivo JSON inicial

Se carga mediante:
DatabaseInitializer.loadJsonIfEmpty()


Solo se ejecuta si la tabla Street está vacía.

---

## ▶️ Cómo ejecutar el proyecto

1. Clonar repositorio
2. Abrir en Android Studio
3. Sync Gradle
4. Ejecutar en dispositivo con:
   - Permisos de ubicación
   - Android 8+

---

## ⚙️ Mejoras futuras recomendadas

- Migrar sesión a DataStore
- Añadir cifrado de contraseña
- Añadir ViewModel
- Añadir arquitectura MVVM completa
- Persistir configuración de sonido
- Sistema real de autenticación

---

## 👨‍💻 Convenciones importantes

- No animar `Surface` directamente si tiene sombra
- Limpiar overlays antes de redibujar mapa
- No mezclar `android.graphics.Color` con `compose.ui.graphics.Color` sin alias

---

# 📖 Manual de Usuario – MapaCustos

## 1️⃣ Registro

Al abrir la app:

- Introducir:
  - Nombre completo
  - Email
  - Contraseña
  - Fecha de nacimiento (selector de calendario)
  - Género
- Pulsar "Create Account"

Si el email ya existe, se mostrará error.

---

## 2️⃣ Inicio de sesión

- Introducir email y contraseña
- Si son correctos → se abre el mapa

---

## 3️⃣ Pantalla principal (Mapa)

Contiene:

### 🔘 Botón inferior izquierdo
Activa/desactiva modo ruta.

### ⚙️ Botón superior derecho
Panel de ajustes desplegable:
- Ajustes de usuario
- Ajustes sonido/color
- Ajustes de mapa

---

## 4️⃣ Ajustes de usuario

Permite:
- Ver email (no editable)
- Cambiar contraseña
- Cambiar fecha (selector calendario)
- Cambiar género
- Guardar cambios
- Cerrar sesión (texto rojo)

---

## 5️⃣ Ajustes de sonido y color

Permite:
- Activar/desactivar sonido para:
  - Nivel 1
  - Nivel 2
- Activar vibración
- Personalizar colores de:
  - Nivel 1
  - Nivel 2
  - Nivel 3

Colores editables mediante:
- Sliders RGB
- Vista previa en rectángulo superior
- Botón restablecer

---

## 6️⃣ Ajustes de mapa

Permite:
- Cambiar nivel de peligrosidad de calles
- Actualizar representación visual en el mapa

---

## 7️⃣ Sistema de colores

Los colores personalizados afectan directamente:
- Renderizado de áreas
- Representación visual en mapa

---

## 8️⃣ Cerrar sesión

Desde Ajustes de Usuario:
- Pulsar "Cerrar sesión"
- Se redirige a pantalla de registro

---

## 🛠️ Solución de problemas

- Si no se ven calles → verificar permisos ubicación
- Si los colores no cambian → guardar cambios
- Si el mapa no actualiza → reiniciar app

---
