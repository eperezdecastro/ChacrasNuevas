# Decisiones técnicas y funcionales · Chacras Nuevas de Matanzas

Documento vivo. Registra las decisiones de arquitectura, los flujos establecidos y las funcionalidades activas del sitio. Se actualiza con cada cambio significativo.

---

## Arquitectura general

### Decisión: sitio estático puro, sin build step

**Qué:** HTML, CSS e imágenes servidos directamente desde Netlify. Sin framework, sin npm, sin compilación.

**Por qué:** El sitio es un proyecto de ventas boutique con pocas páginas y contenido que cambia con poca frecuencia. Un build step añade complejidad sin beneficio real. La simplicidad garantiza que cualquier actualización sea un `git push`.

**Implicancia:** React y Babel corren en el cliente vía CDN (unpkg). Esto añade ~300ms de carga inicial al primer render, aceptable para el perfil de usuario del proyecto.

---

### Decisión: React + Babel Standalone en CDN

**Qué:** `react@18.3.1` y `@babel/standalone@7.29.0` cargados desde unpkg con integrity checks.

**Por qué:** Permite escribir JSX y componentes React sin build tooling. Adecuado para sitios de hasta ~5 páginas con lógica moderada.

**Limitación conocida:** No apto para sitios con decenas de componentes o requisitos de performance estrictos. Si el sitio crece significativamente, migrar a Vite + React.

---

## Páginas y su función

| Página | Propósito | Audiencia |
|---|---|---|
| `Editorial.html` | Homepage principal. Presenta el proyecto, galería, sitios disponibles, ubicación y contacto. | Compradores potenciales |
| `MasterPlan.html` | Plano interactivo con fichas detalladas de cada sitio (m², precio, estado, fotos). | Compradores en etapa de decisión |
| `Blog.html` | Blog "Vida en Matanzas" — artículos sobre restaurantes, playa, actividades. Doble función: SEO y generación de confianza. | Compradores + visitantes orgánicos |
| `index.html` | Solo redirect. No contiene contenido. | — |

---

## Flujo de contenido — Blog

### Fuente de verdad: `data/posts.json`

Todos los posts viven en `data/posts.json`. El archivo tiene esta estructura:

```json
[
  {
    "id": "uuid",
    "status": "published",
    "title": "Título del post",
    "category": "restaurante",
    "date": "2026-05-20",
    "excerpt": "Resumen corto...",
    "body": "Texto completo en HTML o texto plano...",
    "image": "assets/vida/nombre-imagen.jpg",
    "tags": ["gastronomía", "matanzas"]
  }
]
```

**Estados posibles:** `published` | `draft` | `pending`

### Flujo A — Panel admin (recomendado para contenido no técnico)
1. Abrir `Blog.html` en navegador
2. Pie de página → **Admin** → contraseña `chacras2026`
3. Crear / editar / aprobar posts
4. **↓ Exportar posts.json** → reemplazar `data/posts.json` en el repo
5. `git add data/posts.json && git commit -m "content: publicar post X" && git push`
6. Netlify redespliega en ~30 segundos

### Flujo B — Claude Code (más rápido para posts simples)
Indicar a Claude Code:
> "Agrega un post al blog: título '...', categoría '...', texto '...', fecha de hoy. Commitea y pushea."

Claude Code edita `posts.json` directamente y hace el push.

### Categorías de posts establecidas
- `restaurante` — gastronomía local
- `playa` — vida de costa, caleta
- `actividad` — deportes, bike, trekking
- `proyecto` — novedades del proyecto
- `entorno` — naturaleza, pueblos, vida rural

---

## Flujo de captura de leads

### Un solo form: `leads-chacras-nuevas`

Todos los puntos de contacto envían al mismo formulario de Netlify. El campo `origen` identifica de dónde viene cada lead.

| Punto de captura | Archivo | `origen` | Campos |
|---|---|---|---|
| Modal "Recibir brochure" | `Editorial.html` | `Modal Brochure` | nombre, email, teléfono, contactar (checkbox) |
| Sección #contacto | `Editorial.html` | `Editorial - Contacto` | nombre, email, teléfono, sitio, mensaje |
| Sección #contacto | `MasterPlan.html` | `MasterPlan - Contacto` | nombre, email, teléfono, sitio, mensaje |

### Dónde ver los leads
**Netlify → Forms → leads-chacras-nuevas**

### Notificaciones
Configurar en Netlify: **Forms → Form notifications → Email notification** con el email de destino.

### Comportamiento post-envío
Todos los formularios muestran un mensaje de confirmación inline ("Gracias, te contactamos pronto") con botón directo a WhatsApp. El formulario no se resetea ni redirige — la página permanece visible.

---

## Flujo de actualización de imágenes

Las imágenes son archivos estáticos en `assets/`. Para reemplazar o añadir:

1. Copiar la imagen nueva a la subcarpeta correspondiente:
   - `assets/proyecto/` — renders y fotos del proyecto
   - `assets/sitios/` — fotos de sitios individuales
   - `assets/vida/` — entorno, restaurantes, playa
2. Actualizar la referencia en el HTML o en `data/posts.json`
3. `git add assets/ && git commit -m "assets: ..." && git push`

**Caché de assets:** configurado en `netlify.toml` para 1 año. Si se reemplaza una imagen manteniendo el mismo nombre, el caché viejo puede persistir. Para forzar actualización: renombrar el archivo (ej: `foto-v2.jpg`) y actualizar la referencia.

---

## Información de contacto del proyecto

Estos valores están hardcodeados en `Editorial.html` y `MasterPlan.html`. Para cambiarlos, editar las constantes al inicio del script de cada página:

```javascript
const WA_URL = "https://wa.me/56974300513?text=Me%20interesa%20saber%20m%C3%A1s%20de%20Chacras%20Nuevas";
const EMAIL  = "eperezdecastro@gmail.com";
const PHONE  = "+56 9 7430 0513";
```

---

## Infraestructura y servicios

| Servicio | Plan | Uso |
|---|---|---|
| Netlify | Free (Starter) | Hosting, CDN, SSL, Forms, Deploy |
| GitHub | Free | Repositorio, versionado |
| Google Fonts | Free | Tipografías (Cormorant Garamond, Inter, JetBrains Mono) |
| unpkg CDN | Free | React 18, Babel Standalone |
| NIC.cl | — | Registro de dominio `chacrasnuevas.cl` |

**DNS:** Delegado a Netlify (nameservers de Netlify). Netlify gestiona registros A, CNAME y SSL automáticamente.

---

## Límites conocidos del plan actual

| Recurso | Límite Netlify Free | Uso estimado |
|---|---|---|
| Builds / mes | 300 | < 20 (actualizaciones manuales) |
| Bandwidth / mes | 100 GB | Bajo (sitio liviano, imágenes optimizadas) |
| Form submissions / mes | 100 | Depende de la demanda |
| Funciones serverless | 125.000 req | No se usan actualmente |

Si los leads superan los 100/mes: actualizar a Netlify Pro ($19/mes) o migrar forms a Formspree.

---

_Última actualización: 2026-05-22 — v1.1.0_
