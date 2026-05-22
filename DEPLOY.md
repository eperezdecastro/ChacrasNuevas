# Guía de Deploy · Chacras Nuevas de Matanzas

> Sitio estático puro · GitHub + Netlify · Sin build step

---

## Estructura del repo (lo que se publica)

```
/
├── index.html          ← redirect a Editorial.html
├── Editorial.html      ← home principal
├── MasterPlan.html
├── Blog.html
├── netlify.toml        ← configuración de Netlify
├── robots.txt
├── sitemap.xml         ← actualizar BASE_URL antes de publicar
├── data/
│   └── posts.json      ← contenido del blog (fuente de verdad)
└── assets/
    ├── logo-chacras.png
    ├── vida/           ← imágenes del blog
    ├── proyecto/       ← imágenes del proyecto
    └── sitios/         ← renders de sitios
```

---

## PASO 1 · Crear el repositorio en GitHub

1. Ve a [github.com/new](https://github.com/new)
2. **Repository name:** `chacras-nuevas-web`
3. **Visibility:** Private (recomendado)
4. **No inicialices** con README ni .gitignore (lo haremos nosotros)
5. Clic en **Create repository**
6. Copia la URL del repo: `https://github.com/TU_USUARIO/chacras-nuevas-web.git`

---

## PASO 2 · Inicializar git y subir el código (Claude Code)

Abre Claude Code apuntando a la carpeta `Chacrasnuevas Web Docs` y ejecuta:

```bash
# Desde la carpeta del proyecto
git init
git add .
git commit -m "feat: launch Chacras Nuevas de Matanzas website"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/chacras-nuevas-web.git
git push -u origin main
```

Verifica que el repositorio en GitHub tenga todos los archivos correctamente.

---

## PASO 3 · Conectar Netlify

1. Ve a [app.netlify.com](https://app.netlify.com) → **Add new site** → **Import an existing project**
2. Selecciona **GitHub** y autoriza acceso
3. Elige el repo `chacras-nuevas-web`
4. Configuración de deploy:
   - **Branch:** `main`
   - **Build command:** *(dejar vacío)*
   - **Publish directory:** `.`
5. Clic en **Deploy site**

Netlify desplegará en ~30 segundos. La URL inicial será algo como `chacras-nuevas.netlify.app`.

---

## PASO 4 · Dominio personalizado (opcional)

1. En Netlify → **Domain settings** → **Add custom domain**
2. Ingresa `chacrasnuevas.cl` (o el dominio que tengas)
3. Netlify entrega los registros DNS (A o CNAME) para configurar en tu registrador
4. SSL/HTTPS se activa automáticamente (Let's Encrypt)

Una vez activo el dominio, actualiza `sitemap.xml` y `robots.txt` reemplazando `chacrasnuevas.cl` con tu dominio real y haz un nuevo commit.

---

## FLUJO EDITORIAL: Publicar posts del blog

El blog se alimenta de `data/posts.json`. El flujo para publicar nuevos posts es:

### Opción A — Desde el panel admin (recomendado)

1. Abre `Blog.html` localmente en el navegador
2. Haz clic en **Admin** (pie de página) → ingresa contraseña `chacras2026`
3. Crea o edita posts; aprueba los que están en revisión
4. Clic en **↓ Exportar posts.json** → se descarga el archivo
5. Reemplaza `data/posts.json` en el repo con el archivo descargado
6. Commitea y haz push:
   ```bash
   git add data/posts.json
   git commit -m "content: publish new blog post"
   git push
   ```
7. Netlify redespliega automáticamente en ~30 segundos

### Opción B — Vía Claude Code (más rápido para posts simples)

Dile a Claude Code:
> "Agrega un nuevo post al blog: título 'X', categoría 'restaurante', texto '...', fecha de hoy. Commitea y pushea."

Claude Code actualiza `data/posts.json` directamente y hace el push.

---

## Variables a actualizar antes del lanzamiento

| Archivo | Cambio |
|---|---|
| `sitemap.xml` | Reemplazar `BASE_URL` con `https://chacrasnuevas.cl` |
| `robots.txt` | Verificar que el dominio en `Sitemap:` sea el correcto |
| `Blog.html` | Cambiar email de contacto si es necesario |
| `Editorial.html` | Verificar número de WhatsApp y email |
| `MasterPlan.html` | Verificar número de WhatsApp y email |

---

## Actualizar el sitio (cualquier cambio)

```bash
# Edita los archivos que correspondan, luego:
git add .
git commit -m "descripción del cambio"
git push
# → Netlify detecta el push y redespliega en ~30 segundos
```

---

## Arquitectura técnica

- **Hosting:** Netlify (CDN global, SSL automático)
- **Repositorio:** GitHub (control de versiones, backup)
- **Framework:** React 18 + Babel Standalone (sin build step, carga en cliente)
- **Blog CMS:** `data/posts.json` (fuente), localStorage (drafts/admin local)
- **Fuentes:** Google Fonts CDN (Cormorant Garamond, Inter, JetBrains Mono)
- **No hay base de datos** — todo el contenido vive en archivos estáticos

---

*Última actualización: mayo 2026*
