# Flow Berro & El Lashy — proyecto PWA para empaquetar como APK

## Incluye
- Tu HTML original con una puerta de PIN local añadida.
- `manifest.json`, `sw.js` y página de reserva sin conexión.
- Iconos PNG 192 y 512 px.
- Plantilla de `.well-known/assetlinks.json`.
- El contenido de la guía original se conserva.

## IMPORTANTE: seguridad del PIN
El PIN incluido es una barrera local para evitar acceso casual. Como la app web y su JavaScript se publican en GitHub Pages, NO es una autenticación fuerte frente a alguien técnico con acceso al navegador/dispositivo. El hash local no sustituye una autenticación en servidor. No guardes contraseñas importantes en esta app hasta reforzar ese apartado. El PIN se configura la primera vez en cada navegador/instalación de almacenamiento.

## Publicar en GitHub Pages
1. Descomprime este ZIP.
2. Sube el CONTENIDO de esta carpeta al repositorio (debe quedar `index.html` en la raíz de publicación).
3. En GitHub, Settings → Pages, publica desde `main` y `/ (root)`.
4. Abre la URL HTTPS de GitHub Pages y prueba la app.
5. Para primera instalación offline, abre la web online al menos una vez y espera a que el service worker instale la caché.

## Construir APK (Windows)
1. Instala Node.js LTS: https://nodejs.org/
2. Instala Android Studio: https://developer.android.com/studio
3. Abre PowerShell en la carpeta del proyecto.
4. Ejecuta: `npm install -g @bubblewrap/cli`
5. Cuando ya esté publicado, ejecuta: `bubblewrap init --manifest https://TU-USUARIO.github.io/TU-REPO/manifest.json`
6. Sigue las preguntas. Define un identificador único, por ejemplo `com.flowberro.ellashy` si no está usado.
7. Ejecuta: `bubblewrap build`
8. La herramienta te indicará dónde generó el APK. Instálalo en tu Android para probar.

## Asociar el APK con la web
Para una Trusted Web Activity, `assetlinks.json` debe contener el package ID exacto y la huella SHA-256 del certificado real de firma. Sustituye los dos marcadores y publica el archivo en:
`https://TU-USUARIO.github.io/TU-REPO/.well-known/assetlinks.json`
Si la herramienta de firma genera otro certificado, actualiza la huella. Sin esta asociación, Android puede mostrar la web en un navegador personalizado en lugar de una experiencia verificada a pantalla completa.

## Actualizaciones y offline
- Cambia `CACHE_NAME` en `sw.js` cuando hagas cambios importantes de los archivos precacheados, por ejemplo `flow-berro-el-lashy-v2`.
- Los cambios web se descargan cuando el dispositivo tiene conexión. Sin internet se utiliza la última copia guardada; no se pueden recibir actualizaciones sin conexión.
- El almacenamiento local (localStorage/IndexedDB) debe probarse al actualizar; exporta copias de seguridad regularmente.
- GitHub Pages es hosting estático: no implementa límites de solicitudes del lado servidor, protección contra bots en backend ni autenticación robusta. Si se necesita eso, hace falta un servicio de backend/proxy configurado.

## Archivos incluidos
- `index.html`: app original + PIN local + registro del service worker.
- `manifest.json`: identidad e iconos de la PWA.
- `sw.js`: caché offline y actualización básica.
- `offline.html`: mensaje de respaldo.
- `icons/`: iconos de la app.
- `.well-known/assetlinks.json`: plantilla, requiere configuración real.
