# Chacras Nuevas de Matanzas — Sitio Web

Sitio web oficial de **Chacras Nuevas de Matanzas**, proyecto inmobiliario boutique de diez sitios sobre el mar en Centinela, Matanzas, Región de O'Higgins.

**Sitio en producción:** [chacrasnuevas.cl](https://chacrasnuevas.cl)  
**Deploy:** Netlify (auto-deploy desde `main`)  
**Stack:** HTML estático + React 18 (CDN) + Babel Standalone — sin build step

---

## Estructura del repositorio

```
/
├── index.html                    ← Redirect 301 a Editorial.html (homepage)
├── Editorial.html                ← Página principal del sitio
├── MasterPlan.html               ← Plano interactivo de los 10 sitios
├── Blog.html                     ← Blog "Vida en Matanzas" + panel admin
│
├── netlify.toml                  ← Configuración Netlify (redirects, headers, caché)
├── robots.txt                    ← SEO: permite rastreo, apunta al sitemap
├── sitemap.xml                   ← Mapa del sitio para motores de búsqueda
│
├── data/
│   └── posts.json                ← Fuente de verdad del blog (contenido editorial)
│
└── assets/
    ├── logo-chacras.png
    ├── logo-mark.png
    ├── master-plan.pdf
    ├── proyecto/                 ← Imágenes del proyecto (renders, accesos, pradera)
    ├── sitios/                   ← Fotografías de sitios individuales
    └── vida/                     ← Imágenes del entorno (restaurantes, playa, hotel)
```

> **Nota:** Los archivos `.jsx`, `.css` de prototipado y la carpeta `uploads/` están en `.gitignore` — son artefactos de desarrollo de Cowork, no van a producción.

---

## Páginas

| Página | URL | Descripción |
|---|---|---|
| Home | `/` → `/Editorial.html` | Hero, galería, sitios, ubicación, contacto |
| Master Plan | `/MasterPlan.html` | Plano interactivo con fichas de cada sitio |
| Blog | `/Blog.html` | Artículos de vida en Matanzas + admin panel |

---

## Cómo actualizar el sitio

Cualquier cambio sigue el mismo flujo:

```bash
# 1. Editar los archivos correspondientes
# 2. Hacer commit y push
git add <archivos>
git commit -m "descripción del cambio"
git push
# → Netlify detecta el push y redespliega en ~30 segundos
```

---

## Blog — flujo de publicación

El contenido del blog vive en `data/posts.json`. Hay dos formas de publicar:

### A) Panel admin en el navegador
1. Abrir `Blog.html` → pie de página → **Admin** (contraseña: `chacras2026`)
2. Crear o editar posts; aprobar los que están en revisión
3. **↓ Exportar posts.json** → reemplazar `data/posts.json` en el repo
4. `git add data/posts.json && git commit -m "content: ..." && git push`

### B) Vía Claude Code (posts simples)
Pedir directamente:
> "Agrega un post al blog: título '...', categoría '...', texto '...', fecha de hoy."

Claude Code actualiza `posts.json` y hace el push.

---

## Captura de leads — Netlify Forms

Los formularios de contacto envían a **un único form** `leads-chacras-nuevas` en Netlify.

| Formulario | Origen registrado |
|---|---|
| Modal "Recibir brochure" | `Modal Brochure` |
| Sección contacto — Editorial | `Editorial - Contacto` |
| Sección contacto — MasterPlan | `MasterPlan - Contacto` |

Ver submissions: **Netlify → Forms → leads-chacras-nuevas**

---

## Archivos de referencia

| Archivo | Contenido |
|---|---|
| [`DEPLOY.md`](DEPLOY.md) | Guía paso a paso para deploy y dominio |
| [`CHANGELOG.md`](CHANGELOG.md) | Historial de cambios del sitio |
| [`DECISIONS.md`](DECISIONS.md) | Decisiones técnicas, flujos y arquitectura |

---

## Contacto del proyecto

- **WhatsApp:** +56 9 7430 0513
- **Email:** eperezdecastro@gmail.com
- **Propietario del repo:** [@eperezdecastro](https://github.com/eperezdecastro)
