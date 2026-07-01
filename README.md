# Paneles de Acreditación · Universidad de Antofagasta

Sitio estático con dos dashboards de acreditación CNA conectados en vivo a una planilla de Google Sheets publicada como CSV. No requiere backend: todo el cálculo (filtros de oferta vigente, porcentajes, semáforo de vigencias) se hace en el navegador.

## Contenido

- `index.html` — Portada que enlaza ambos paneles.
- `postgrado.html` — Panel de postgrado (Doctorado, Magíster, Especialidades).
- `pregrado.html` — Panel de pregrado obligatorio.
- `render.yaml` — Blueprint opcional para desplegar en Render.

## Fuente de datos

Los paneles leen la pestaña `dashboard_data` publicada como CSV. La URL está definida en la constante `CSV_URL`, al inicio del `<script>` de cada archivo `postgrado.html` y `pregrado.html`. Si cambias de planilla, edita esa línea en ambos.

La pestaña `dashboard_data` no contiene correos ni teléfonos, por lo que es seguro publicarla.

## Despliegue en Render (Static Site)

### Paso 1 — Subir a GitHub
1. Crea un repositorio nuevo (por ejemplo `paneles-acreditacion-ua`).
2. Sube estos archivos a la raíz del repo:
   ```
   git init
   git add .
   git commit -m "Paneles de acreditacion"
   git branch -M main
   git remote add origin https://github.com/Hans-Contreras/paneles-acreditacion-ua.git
   git push -u origin main
   ```

### Paso 2 — Crear el servicio en Render
1. En el dashboard de Render: **New +** → **Static Site**.
2. Conecta el repositorio de GitHub.
3. Configura:
   - **Name:** `paneles-acreditacion-ua` (define el subdominio final).
   - **Branch:** `main`
   - **Build Command:** *(déjalo vacío)*
   - **Publish Directory:** `.`
4. **Create Static Site**.

Queda publicado en `https://paneles-acreditacion-ua.onrender.com`. Los Static Sites de Render se sirven por CDN y **no hibernan**.

### Alternativa — Blueprint
Si prefieres infraestructura como código, usa **New +** → **Blueprint** y apunta al repo; Render leerá `render.yaml`.

## Actualización de datos

Editas la planilla (o el Excel importado) → los paneles reflejan los cambios. Google puede tardar unos minutos en propagar el CSV publicado. El botón «↻ Actualizar» de cada panel fuerza una relectura.

## Cambiar de fuente / diagnóstico

- Si un panel no carga por CORS al abrirlo localmente (`file://`), súbelo a Render/GitHub Pages: hospedado funciona.
- Alternativa de endpoint si el `/pub?output=csv` fallara: usar el endpoint `gviz`:
  `https://docs.google.com/spreadsheets/d/ID_INTERNO/gviz/tq?tqx=out:csv&sheet=dashboard_data`
