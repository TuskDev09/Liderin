# Liderín — Asistente Virtual de Supermercado Líder

Aplicación móvil híbrida (iOS / Android / Web) que actúa como asistente inteligente para clientes del Supermercado Líder en Chile. Liderín responde consultas en lenguaje natural sobre precios, ofertas, recetas y ubicación de productos dentro de la tienda, usando voz o texto.

---

## Características principales

- **Chat con IA** — conversaciones en lenguaje natural impulsadas por Google Gemini
- **Reconocimiento de voz** — entrada por micrófono en español chileno (`es-CL`) con Web Speech API
- **Texto a voz** — respuestas leídas en voz alta con SpeechSynthesis, con control de silencio persistente
- **Escaneo de código de barras** — identificación de productos en tiempo real con la cámara usando ZXing
- **Análisis de intención** — clasifica automáticamente si el usuario pregunta por precio, ubicación, ofertas o recetas
- **Catálogo de productos** — búsqueda full-text sobre 22 productos con precios, ubicaciones y ofertas
- **Ofertas y promociones** — listado de 50+ ofertas con porcentaje de descuento y fecha de vencimiento
- **Recetas** — sugerencias de recetas con ingredientes disponibles en Líder y precios en tiempo real
- **Selector de avatar** — personalización del asistente virtual

---

## Stack tecnológico

| Capa | Tecnología |
|---|---|
| Framework | Angular 20 (Standalone Components) |
| UI / Mobile | Ionic 8 |
| Runtime nativo | Capacitor 7 (iOS & Android) |
| Lenguaje | TypeScript 5.8 |
| Programación reactiva | RxJS 7.8 |
| Estilos | SCSS + Ionic Theme |
| IA / LLM | Google Gemini (vía Triskele API) |
| Voz (web) | Web Speech API (SpeechRecognition + SpeechSynthesis) |
| Voz (nativo) | @capacitor-community/speech-recognition · text-to-speech |
| Escaneo | @zxing/browser · html5-qrcode · jsqr · quagga |
| Plugins nativos | Camera · Barcode Scanner · Haptics · Keyboard · Status Bar |
| Testing | Karma + Jasmine |
| Linting | ESLint 9 |

---

## Arquitectura

```
src/
├── app/
│   ├── home/              # Chat principal con Liderín (voz + texto)
│   ├── price-check/       # Escáner de código de barras + consulta de precios
│   ├── recipes/           # Catálogo de recetas
│   ├── offers/            # Ofertas y promociones vigentes
│   ├── store-locator/     # Localizador de productos por pasillo
│   ├── app-download/      # Landing de descarga de la app
│   └── services/
│       ├── chat.service.ts      # Integración con Google Gemini (Triskele API)
│       ├── products.ts          # Catálogo de productos con búsqueda
│       ├── offers.service.ts    # Gestión de ofertas y descuentos
│       ├── recipes.service.ts   # Catálogo de recetas con filtros
│       └── cart.service.ts      # Carrito de compras
├── data/
│   ├── offers.data.ts     # 50+ ofertas mock
│   └── recipes.data.ts    # Recetas mock con ingredientes y pasos
└── environments/          # Configuración por entorno
```

Todas las páginas son **standalone components** sin NgModules. Las rutas están configuradas con **lazy loading** y `PreloadAllModules`.

---

## Requisitos previos

- [Node.js](https://nodejs.org/) 18+
- [Angular CLI](https://angular.io/cli) 20+
- [Ionic CLI](https://ionicframework.com/docs/cli)
- [Android Studio](https://developer.android.com/studio) (solo para build Android)
- [Xcode](https://developer.apple.com/xcode/) (solo para build iOS — requiere macOS)

---

## Instalación

```bash
# Clonar el repositorio
git clone https://github.com/TuskDev09/liderin.git
cd linderin

# Instalar dependencias
npm install
```

---

## Ejecutar en desarrollo

```bash
# Navegador
ionic serve

# Android (requiere dispositivo/emulador conectado)
ionic capacitor run android

# iOS (requiere macOS + Xcode)
ionic capacitor run ios
```

---

## Build para producción

```bash
# Compilar la app web
ionic build --prod

# Sincronizar con Capacitor
npx cap sync

# Abrir en Android Studio
npx cap open android

# Abrir en Xcode
npx cap open ios
```

---

## Cómo funciona el chat con IA

1. El usuario escribe o dicta una pregunta
2. `analyzeIntent()` detecta la intención: precio, ubicación, oferta o receta
3. Los servicios locales recuperan los datos relevantes (productos, ofertas, recetas)
4. `createPreciseContext()` construye un prompt enriquecido con esos datos
5. El prompt se envía a Google Gemini con la personalidad de Liderín como system prompt
6. La respuesta se muestra en pantalla y se lee en voz alta automáticamente

---

## Funcionalidades de voz

| Acción | API utilizada |
|---|---|
| Entrada de voz (web) | `window.SpeechRecognition` / `webkitSpeechRecognition` |
| Entrada de voz (nativo) | `@capacitor-community/speech-recognition` |
| Lectura en voz alta | `window.speechSynthesis` |
| Idioma | Español de Chile (`es-CL`) |

> El reconocimiento de voz en navegador requiere HTTPS o `localhost`. Funciona mejor en Chrome y Edge.

---

## Escaneo de código de barras

`PriceCheckerPage` usa `BrowserMultiFormatReader` de ZXing para leer barcodes en tiempo real desde la cámara trasera del dispositivo. Una vez leído el código, busca el producto en el catálogo local y consulta el precio a Liderín.

---

## Licencia

Este proyecto es de uso educativo y de portafolio. No está afiliado oficialmente con Walmart Chile ni Supermercado Líder.
