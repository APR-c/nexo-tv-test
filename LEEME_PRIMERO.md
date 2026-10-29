# Nexo TV para Hisense VIDAA 9 — versión 0.1 experimental

Este paquete es un **prototipo original** para intentar un launcher que no necesite el ordenador encendido. Es distinto de CleanFrame y NO modifica ni sustituye su instalación.

## Qué funciona y qué se está investigando

- Interfaz HTML, CSS y JavaScript en español, sin dependencias de terceros.
- Flechas, OK y Atrás del mando (según el mapeo que proporcione VIDAA).
- Favoritos y apariencia sencilla; favoritos guardados en `localStorage` cuando esté disponible.
- Service Worker con caché de los archivos del launcher, **solo si tu versión de VIDAA lo soporta**.
- Diagnóstico de capacidades de VIDAA sin ejecutar operaciones peligrosas.
- Apertura de Netflix/YouTube/Plex, etc.: **experimental y sin confirmar**. Se intenta llamar a `Hisense_launchAppViaName(nombre)` únicamente cuando el usuario pulsa la tarjeta y si dicha función está presente. No modifica archivos ni aplica técnicas de root.
- No sustituye el botón Home de VIDAA: eso sigue requiriendo otro mecanismo, como el observador MQTT de CleanFrame.
- No desinstala aplicaciones preinstaladas ni acelera los componentes internos de VIDAA.

## PRUEBA A — Ver la interfaz en el PC

Abre `index.html` en Chrome/Edge desde tu ordenador. El diseño y navegación deberían funcionar. La caché offline NO funciona abriendo archivos con `file://`.

## PRUEBA B — Publicar sin ordenador doméstico encendido

Necesitamos que tu TV pueda abrir el launcher con una URL HTTPS. La ruta más fácil para esta primera prueba es **GitHub Pages** (gratis, pero público):

1. Crea un repositorio público llamado `nexo-tv-test` en tu cuenta GitHub (no lo mezcles con CleanFrame).
2. Descomprime este ZIP y sube a la raíz del repositorio los archivos `index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png` e `icon-512.png`.
3. En GitHub: `Settings > Pages > Build and deployment > Deploy from a branch > main > /(root) > Save`.
4. Copia la URL HTTPS que te muestre GitHub Pages; normalmente tendrá la estructura `https://TU_USUARIO.github.io/nexo-tv-test/`.
5. Abre esa URL en el navegador de la Hisense (por ahora **no** borres ni sustituyas CleanFrame).
6. Entra en **Diagnóstico** en la tele, vuelve a la página y recárgala. Mira si `SW controla la página` pasa a `true`.

GitHub Pages permite usar el launcher sin tu PC encendido **si el televisor tiene Internet**. Para no depender ni de GitHub una vez cargado, el firmware deberá conservar y utilizar la caché offline; eso hay que demostrarlo.

## PRUEBA C — Comprobar si la interfaz queda en el televisor

1. Con la interfaz funcionando y, preferiblemente, `SW controla la página: true`, desactiva temporalmente la conexión a Internet de la TV.
2. Intenta volver a abrir la misma URL mediante un marcador o el historial. Si ya has registrado un icono propio, pruébalo también.
3. Si abre completa, la caché está disponible. Prueba además después de apagar y volver a encender el televisor.
4. Si no abre, reactiva la conexión. No significa que haya fallado la instalación: probablemente VIDAA no está utilizando la caché como un navegador moderno.

**Nota:** incluso si la interfaz funciona offline, Netflix/YouTube seguirán necesitando Internet para reproducir contenidos.

## PRUEBA D — Registrar un icono propio, sin tocar CleanFrame

Los iconos de apps web de VIDAA suelen guardar una dirección URL. Para registrar un acceso separado que apunte a la URL HTTPS de Nexo TV, el proyecto [VIDAA Edge](https://github.com/weinzii/vidaa-edge) documenta dos métodos de instalación que dependen del firmware: `Hisense_installApp` o su mecanismo experimental basado en `Appinfo.json`.

**No ejecutaremos modificaciones manuales de `Appinfo.json`** ni cambiaremos la entrada `cleanframehome` antes de confirmar el diagnóstico. VIDAA Edge requiere un instalador web temporal y configuración de DNS para acceder a funciones internas; no es necesario instalarlo para la prueba B. Su documentación está en el repositorio.

El panel de Sidee actual no admite URLs arbitrarias, de modo que no lo uses para instalar esta app personalizada sin modificarlo.

## Recuperación

Este paquete no escribe ni borra archivos del sistema. Puedes cerrar la página y regresar a tu CleanFrame habitual. Mientras tengas activado el retorno automático de CleanFrame y Docker siga encendido, ese observador podría abrir CleanFrame por encima de Nexo TV: desactiva temporalmente `Return to CleanFrame` para hacer pruebas controladas y reactívalo después si quieres.

## Qué compartir para la siguiente fase

Haz una foto de la sección **Diagnóstico** que aparece al abrir Nexo TV **en la Hisense**. Necesitamos saber si están disponibles `Hisense_launchAppViaName`, `Hisense_launchAppViaUri`, Service Worker, CacheStorage y almacenamiento local. Esto no expone tokens ni contraseñas.

## Fuentes técnicas

- CleanFrame: https://github.com/mcncl/cleanframe-hisense
- VIDAA Edge: https://github.com/weinzii/vidaa-edge
- Investigación de APIs Hisense: https://bananamafia.dev/post/hisensehax/
- Caché offline en PWA: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Guides/Caching
