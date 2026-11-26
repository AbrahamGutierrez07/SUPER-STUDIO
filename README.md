# 📱 Super Studio — Organización Académica Inteligente v1.0.0

> **Aplicación Multiplataforma (PWA Avanzada, Android APK y Windows EXE)**  
> Desarrollado y creado por **Abraham Elías Gutiérrez Pardo**. Todos los derechos reservados © 2026.

---

## 🚀 Descripción General

**Super Studio** es una suite académica integral 100% *Offline-First* diseñada para estudiantes de colegio y universidad. Integra:
1. **Horario Inteligente + Escáner OCR:** Permite subir fotos o PDFs del horario y replica automáticamente las asignaturas, días y horas en una cuadrícula semanal editable con alertas.
2. **Calculadora de Notas y Remontadas:** Estructura en 2 niveles (Cortes con ponderación fija + Sub-ítems de evaluación como parciales, quices y talleres) con cálculo en tiempo real de *"¿Cuánto necesito para aprobar?"* y semáforo visual (Verde, Amarillo, Rojo).
3. **Bolsillo Estudiantil (Presupuesto y Gastos):** Bolsa general distribuible en sobres/categorías (Transporte, Comida, Copias, etc.) con registro histórico y descuento de saldos en tiempo real.
4. **Salas de Estudio Globales por ID:** Comunicación grupal en vivo con mensajería instantánea, intercambio de archivos y canal de voz bidireccional WebRTC.

---

## 🎨 Identidad Visual y Logotipo Oficial

- **Ícono Oficial:** `public/logo-super-studio.png`
- **Gama de Colores:** Fondo oscuro `#090d16` y superficies `#111827`, acentos en índigo (`#4f46e5`), violeta (`#7c3aed`) y cian (`#06b6d4`).
- **Resoluciones generadas:**
  - `public/icons/icon-192x192.png` & `icon-maskable-192x192.png`
  - `public/icons/icon-512x512.png` & `icon-maskable-512x512.png`
  - `public/favicon.ico` & `favicon.png`
  - `electron/assets/icon.ico` & `icon.png`
  - Android mipmaps en `android/app/src/main/res/mipmap-*`

---

## 🌐 1. Instalación Universal como PWA (PC y Móviles)

Super Studio está configurado con **`vite-plugin-pwa`** y un Service Worker (`sw.js`) con estrategias de precaché y almacenamiento en caché en tiempo de ejecución (`CacheFirst` y `StaleWhileRevalidate`) para garantizar funcionamiento offline total:

- **En PC (Windows, macOS, Linux):**
  - Al abrir la aplicación en Chrome, Edge o Brave, aparecerá el botón **"Instalar Super Studio"** en la barra superior o en el banner.
  - Al hacer clic en instalar, el navegador genera el acceso directo en el **Escritorio** y en el **Menú Inicio**, ejecutándose en ventana limpia sin barras de navegación (`display: "standalone"`).
- **En Teléfono Móvil (Android / iOS):**
  - En Android (Chrome/Edge): Pulsa **"Instalar App"** o selecciona *"Instalar aplicación"* en el menú del navegador. Se agregará como app nativa a tu cajón de aplicaciones.
  - En iPhone/iPad (Safari): Toca el botón *"Compartir"* y selecciona *"Agregar a pantalla de inicio"*.

---

## 🤖 2. Empaquetado Nativo para Android (`.apk`)

El proyecto incluye la integración completa con **Capacitor 6** (`@capacitor/android`, `@capacitor/core`):

### Configuración:
- Archivo de configuración: `capacitor.config.json`
- Estructura Android nativa lista en el directorio: `android/`
- Permisos configurados en `AndroidManifest.xml`: Notificaciones locales, cámara, archivos y audio.

### Comandos para compilar localmente:
```bash
# 1. Instalar dependencias
npm install

# 2. Compilar aplicación web
npm run build

# 3. Sincronizar cambios en el proyecto Android
npx cap sync android

# 4. Compilar instalador APK de depuración
cd android
./gradlew assembleDebug
# En Windows: .\gradlew.bat assembleDebug
```
El archivo ejecutable se genera en:
`android/app/build/outputs/apk/debug/app-debug.apk` (renombrable a `Super-Studio-Android.apk`).

---

## 💻 3. Empaquetado de Escritorio para Windows (`.exe`)

El proyecto incluye la configuración de **Electron** y **electron-builder** para generar el instalador oficial de Windows:

### Configuración:
- Proceso principal: `electron/main.js` (Ventana nativa con fondo `#090d16`, ícono personalizado en barra de tareas, menú oculto y aceleración por hardware).
- Script de precarga: `electron/preload.js` (Aislamiento de contexto seguro).
- Ícono del instalador y aplicación: `electron/assets/icon.ico`.

### Comandos para ejecutar y compilar localmente:
```bash
# Ejecutar Electron en modo desarrollo
npm run electron:dev

# Generar el instalador ejecutable de Windows (Super-Studio-Setup.exe)
npm run electron:build:win
```
El instalador se genera en la carpeta:
`release/Super-Studio-Setup.exe`

---

## ⚙️ 4. Flujo Automático en GitHub Actions (`.github/workflows/build-apps.yml`)

El proyecto incluye un pipeline de integración continua que compila ambos instaladores automáticamente al subir el código a GitHub:

```yaml
# Activación automática en push a ramas main/master o ejecución manual
on:
  push:
    branches: [ main, master ]
  workflow_dispatch:
```

### Trabajos que ejecuta el flujo:
1. **`build-android` (Ubuntu Runner):**
   - Configura Node.js 20, Java 17 Temurin y Android SDK.
   - Ejecuta `npm install` y `npm run build`.
   - Ejecuta `npx cap sync android`.
   - Compila con `./gradlew assembleDebug`.
   - Sube como artefacto de descarga: **`Super-Studio-Android.apk`**.

2. **`build-windows` (Windows Runner):**
   - Configura Node.js 20.
   - Ejecuta `npm install` y `npm run build`.
   - Ejecuta `npx electron-builder --win --x64`.
   - Sube como artefacto de descarga: **`Super-Studio-Setup.exe`**.

---

## 📂 Estructura del Repositorio

```
super-studio/
├── .github/
│   └── workflows/
│       └── build-apps.yml       # Flujo CI/CD automatizado en GitHub Actions
├── android/                     # Proyecto nativo Capacitor para Android
│   ├── app/
│   │   ├── src/main/AndroidManifest.xml
│   │   └── build.gradle
│   ├── build.gradle
│   ├── gradlew & gradlew.bat
│   └── variables.gradle
├── electron/                    # Configuración de Electron para PC
│   ├── assets/
│   │   ├── icon.ico             # Ícono para el instalador Windows
│   │   └── icon.png
│   ├── main.js                  # Proceso principal de Electron
│   └── preload.js               # Puente de contexto seguro
├── public/                      # Recursos públicos y PWA
│   ├── icons/
│   │   ├── icon-192x192.png
│   │   ├── icon-512x512.png
│   │   ├── icon-maskable-192x192.png
│   │   └── icon-maskable-512x512.png
│   ├── favicon.ico
│   ├── favicon.png
│   ├── logo-super-studio.png    # Logotipo oficial
│   ├── manifest.json            # PWA Web App Manifest
│   └── sw.js                    # Service Worker con precaché offline
├── capacitor.config.json        # Configuración de Capacitor
├── index.html                   # Aplicación principal Super Studio
├── package.json                 # Dependencias y scripts de Vite, Capacitor y Electron
├── vite.config.js               # Configuración de Vite + VitePWA
└── README.md                    # Documentación del proyecto
```

---

## 📜 Créditos y Derechos

Desarrollado y creado por **Abraham Elías Gutiérrez Pardo**.  
Todos los derechos reservados © 2026.
