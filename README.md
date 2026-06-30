# Portal de Soporte Emocional (PWA)

App web instalable (tipo app) de apoyo emocional ante la crisis sísmica de Venezuela.
La persona completa un test breve según cómo se siente (ansiedad, cansancio,
somnolencia, malestar gastrointestinal, etc.), recibe un resultado estimado y
una guía de orientación con un flyer descargable y compartible.

Basada en los patrones de `linktree-silva` (service worker versionado,
detección iOS/Android, modo standalone), con la estética navy + naranja del portal.

---

## Archivos

| Archivo | Qué es |
|---|---|
| `index.html` | La app completa (interfaz + lógica + los 8 tests). |
| `manifest.json` | Configura la instalación: nombre, iconos, colores. |
| `sw.js` | Service worker. Maneja la caché y la auto-actualización. |
| `icon-192.png`, `icon-512.png` | Iconos de la app. |
| `logo.png` | Logo blanco usado dentro del encabezado. |
| `flyer-emergencia.png` | Flyer de la Guía de Emergencia (ansiedad, estrés). |
| `flyer-energia.png` | Flyer de la Guía para la Recuperación de Energía (cansancio, fatiga). |
| `flyer-recuperacion.png` | Flyer de la Guía de Recuperación (somnolencia, depresión). |
| `flyer-gastro.png` | Flyer de la Guía de Alivio Gastrointestinal (gastro, malestar). |
| `CNAME` | El subdominio donde vive la app. |

---

## Actualizar la app (¡importante para que no quede caché vieja!)

Cada vez que cambies algo en `index.html`, subí el número de versión en `sw.js`
para forzar que todos los dispositivos con la app instalada reciban la actualización:

```js
// sw.js, línea superior
const VERSION = 'v1';   // ← cambialo: v2, v3, etc.
```

Después:

```bash
git add .
git commit -m "Actualización vX"
git push
```

Cuando cambia `VERSION`, los celulares con la PWA instalada detectan el cambio,
descargan la versión nueva en segundo plano y la activan solos, sin que el
usuario tenga que hacer nada.

---

## Cómo funciona para el usuario

1. Entra al link, acepta el consentimiento, completa su perfil (nombre, cédula,
   sexo, edad, teléfono — empresa/depto/cargo son opcionales).
2. Toca un botón de sensación (ej. "Cansancio", "Ansiedad") → se abre el test
   correspondiente paso a paso.
3. Al terminar, ve una **tarjeta de resultado** (severidad estimada por rangos)
   y un botón "Registrar y ver guía".
4. Se abre la guía: arriba el **flyer en imagen** (se puede ampliar, descargar
   o compartir) y, al scrollear, el texto de orientación completo.
5. Las respuestas se envían a Google Sheets vía Apps Script (no a EVA / WhatsApp).

---

## Datos de configuración (dentro de `index.html`)

- Botones y textos: array `SECTIONS`.
- Mapeo de qué flyer/guía corresponde a cada sensación: `ITEM_CATEGORY`.
- Contenido de cada guía (título, imagen, texto): `FLYER_SCREEN`.
- URLs de los webhooks de Google Sheets: constantes `SHEETS_*_URL`.
- Contacto de emergencia médica (WhatsApp): buscar `584241466595`.
