# VIDEOIA STUDIO

App web que convierte un guion de texto en un video MP4, renderizando y grabando todo **dentro del navegador** (canvas + [ffmpeg.wasm](https://ffmpegwasm.netlify.app/)). No hay backend ni servidor de render: es un único archivo estático (`index.html`), ideal para desplegar en Vercel directamente desde GitHub.

## Qué hace

1. Pegas un guion (reconoce automáticamente el formato `[GANCHO]`, `[PUNTO 1]`... `[FINAL]`, o separa por párrafos si usas otro formato).
2. Lo convierte en una lista de escenas editables: texto, duración, e imagen de fondo opcional por escena (si no subes imagen, genera un fondo animado tipo motion graphics).
3. Eliges formato (9:16 TikTok/Reels, 1:1, 16:9), música de fondo opcional, y si quieres narración con la voz sintética del navegador (Web Speech API).
4. Genera el video completo grabando el canvas en tiempo real y lo convierte a MP4 con ffmpeg.wasm.
5. Puedes subir varios MP4/WebM ya generados (por ejemplo, de distintos guiones) y unirlos en un solo archivo final.

## Limitaciones a tener en cuenta

- **Narración grabada**: para capturar la voz sintética dentro del MP4, el navegador debe darte la opción de "compartir esta pestaña con audio" (usa `getDisplayMedia`). Esto solo funciona de forma confiable en **Chrome/Edge de escritorio**. En otros navegadores, o si cancelas el permiso, el video se genera sin narración (queda la música y los subtítulos en pantalla).
- El render ocurre en tiempo real (el video de 90s tarda aproximadamente 90s en grabarse) y luego unos segundos más para la conversión a MP4.
- Todo el procesamiento es local: nada se sube a ningún servidor, así que los guiones y videos nunca salen del dispositivo del usuario.

## Desplegar en Vercel desde GitHub

1. Crea un repositorio nuevo en GitHub y sube estos archivos:
   ```bash
   git init
   git add .
   git commit -m "VIDEOIA STUDIO"
   git branch -M main
   git remote add origin https://github.com/TU_USUARIO/videoia-studio.git
   git push -u origin main
   ```
2. Entra a [vercel.com](https://vercel.com), inicia sesión con tu cuenta de GitHub.
3. **Add New → Project**, selecciona el repositorio `videoia-studio`.
4. Framework Preset: elige **Other** (o déjalo en "None" / "Static"). No necesita build command ni output directory especiales — Vercel detecta y sirve el `index.html` directamente.
5. Click en **Deploy**. En menos de un minuto tendrás una URL pública (`videoia-studio.vercel.app` o el nombre que le des).

Cada vez que hagas `git push` a `main`, Vercel vuelve a desplegar automáticamente.

## Desarrollo local

No requiere instalación de dependencias. Basta con servir el archivo:

```bash
npx serve .
# o simplemente abre index.html en el navegador
```

(Nota: la captura de audio de pestaña para narración requiere `https://` o `localhost` — funciona con `npx serve` o en Vercel, pero no abriendo el `.html` directamente con `file://`.)
