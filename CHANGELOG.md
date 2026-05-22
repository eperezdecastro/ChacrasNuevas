# Changelog · Chacras Nuevas de Matanzas

Registro cronológico de todos los cambios, mejoras y correcciones del sitio.  
Formato: `[versión] — fecha · descripción`

---

## [1.1.0] — 2026-05-22

### Añadido
- **Netlify Forms** conectado en los 3 puntos de captura de leads, consolidados bajo el form `leads-chacras-nuevas`
  - Modal "Recibir brochure" (`Editorial.html`) — campo `origen: Modal Brochure`
  - Sección `#contacto` (`Editorial.html`) — campo `origen: Editorial - Contacto`
  - Sección `#contacto` (`MasterPlan.html`) — campo `origen: MasterPlan - Contacto`
- Estado de confirmación visual post-envío en los 3 formularios ("Gracias, te contactamos pronto")
- Función `submitLead()` como helper centralizado de envío en cada página
- Formularios HTML ocultos en cada página para detección de Netlify en build time

### Cambiado
- `BrochureModal`: ahora envía datos reales antes de cerrar; muestra confirmación con botón WhatsApp
- Formularios de contacto: campos con atributo `name` y `data-netlify="true"` correctamente configurados

---

## [1.0.0] — 2026-05-22

### Lanzamiento inicial
- Sitio estático desplegado en **Netlify** conectado a **GitHub** (`eperezdecastro/ChacrasNuevas`)
- Páginas publicadas: `Editorial.html` (home), `MasterPlan.html`, `Blog.html`
- `index.html` con redirect 301 a `Editorial.html`
- `netlify.toml` configurado con:
  - Redirect raíz `/` → `/Editorial.html`
  - Headers de seguridad (X-Frame-Options, CSP básico, Referrer-Policy)
  - Caché de 1 año para assets, sin caché para HTML y `data/`
- Blog con panel admin local (contraseña `chacras2026`) y flujo de publicación vía `data/posts.json`
- Dominio `chacrasnuevas.cl` conectado vía Netlify DNS (nameservers delegados a Netlify)
- SSL automático activado (Let's Encrypt)
- Archivos de documentación: `DEPLOY.md`, `README.md`, `CHANGELOG.md`, `DECISIONS.md`

---

_Última actualización: 2026-05-22_
