# Óptica — Seguimiento de Trabajos

PWA (aplicación web instalable) para el seguimiento de trabajos de laboratorio, búsqueda, mensajería a sucursales e informes. Sin backend, sin build — HTML/CSS/JS puros, lista para GitHub Pages.

## Publicar en GitHub Pages (sin configuración adicional)

1. Crea un repositorio nuevo en GitHub y sube todo el contenido de esta carpeta a la rama `main`.
   ```bash
   git init
   git add .
   git commit -m "Óptica PWA"
   git branch -M main
   git remote add origin https://github.com/TU-USUARIO/TU-REPOSITORIO.git
   git push -u origin main
   ```
2. En el repositorio en GitHub: **Settings → Pages**.
3. En "Build and deployment" → **Source: Deploy from a branch**.
4. Rama: **main**, carpeta: **/ (root)** → **Save**.
5. En 1-2 minutos, GitHub te da la URL pública: `https://TU-USUARIO.github.io/TU-REPOSITORIO/`.
6. Abre esa URL en Chrome/Edge — aparecerá el ícono para **instalar como app** en la barra de direcciones.

No hace falta ninguna GitHub Action ni paso de build: es un sitio estático, `Deploy from a branch` es suficiente.

## Estructura

```
index.html              página principal
css/styles.css           estilos
js/app.js                toda la lógica (búsqueda, trabajos+mensajería, informes)
data.json                base de datos real (6,199 trabajos)
manifest.webmanifest      metadatos de instalación (PWA)
sw.js                     service worker (uso sin conexión)
icons/                    íconos de la app
.nojekyll                 evita que GitHub procese el sitio con Jekyll
```

## Funciones incluidas

- **Panel principal** — indicadores generales (pendientes, retrasados, recibidos, enviados) + mensajería a sucursal.
- **Buscar** — por cliente, material, laboratorio o sucursal; parcial, sin distinguir mayúsculas ni tildes.
- **Trabajos y mensajería** (fuente única) — tabla completa con filtros (incluido filtro por estado de mensajería), orden por columna, exportar CSV, registrar trabajo nuevo con Material/Laboratorio/Sucursal como listas de selección (con opción "Otro" para valores nuevos). Cada trabajo tiene un panel de detalle con **línea de tiempo** de todos sus movimientos: registrado → enviado a laboratorio → recibido de laboratorio → enviado a sucursal → recibido en sucursal.
- **Informes** — dos pestañas:
  - **Crítico**: retrasados y próximos a vencer.
  - **Envíos del día**: informe diario de lo que se despachó a sucursal en una fecha específica, agrupado por sucursal.
  - Ambos con botón para enviar por correo (abre el cliente de correo con el mensaje redactado hacia **labnowsion@gmail.com**) y para descargar en .txt.

## Cómo se guardan los datos

`data.json` es de solo lectura (la base real). Cualquier edición que hagas en la app (fechas, mensajería, trabajos nuevos) se guarda en el `localStorage` del navegador de quien la usa — no se sincroniza automáticamente entre distintos dispositivos o usuarios. Para que varias personas trabajen sobre los mismos datos en tiempo real, el siguiente paso sería conectar esta interfaz a una base de datos real (Firebase, Supabase, Google Sheets vía API, etc.).

## Actualizar la base de datos

`data.json` es una foto fija del archivo compartido. Para refrescarlo con datos más recientes, genera un nuevo `data.json` con el mismo formato (arreglo de objetos con las llaves `id, marcaTemporal, cliente, material, laboratorio, fechaEnvio, fechaEstimada, fechaRecepcion, sucursal, estatus`) y reemplázalo en el repositorio.

## Desarrollo local

```bash
python3 -m http.server 8080
# abrir http://localhost:8080
```
(Necesario porque los navegadores bloquean `fetch()` a archivos locales abiertos con doble clic — no es un requisito de GitHub Pages, solo para probar en tu máquina.)
