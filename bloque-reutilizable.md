# Bloque reutilizable sincronizado — "Propuesta + Cierre + Contacto"

> **Paso 2 del montaje.** Se construye **1 vez** como patrón sincronizado de Gutenberg y se inserta en las **9 páginas**. Va justo antes del footer global (que es el paso 3, elemento aparte).
> Copy extraído tal cual del mockup (`experiencias/*.html`, sección compartida). El CSS de estas clases ya está en Customizer → CSS Adicional.

## Estructura: 2 secciones

### Parte A — Propuesta + Contacto
- **H2:** "Cuéntanos sobre tu próximo evento"
- Subtítulo: "Completa el formulario y nos contactaremos contigo a la brevedad."
- **3 pasos del proceso** (numerados 1-2-3):
  1. Conversamos brevemente sobre lo que necesitas.
  2. Diseñamos una propuesta a la medida de tu equipo.
  3. La ejecutamos juntos, del inicio al cierre.
- Nota: "Te respondemos en menos de 48 horas."
- **Formulario** (H3 "Solicita tu propuesta"):
  | Campo | Tipo | Placeholder |
  |---|---|---|
  | Nombre completo | text (req) | Tu nombre |
  | Empresa | text | Nombre de tu empresa |
  | Teléfono / WhatsApp | tel (req) | +51 999 999 999 |
  | Email corporativo | email (req) | tucorreo@empresa.com |
  | ¿Qué servicio te interesa? | select | (Gymkanas productivas, Team building, Bingo Show, Juegos de feria, Full Day, Eventos privados) |
  | Cuéntanos más | textarea | Número de personas, fecha aproximada, objetivo del evento... |
  - Botón: **"Enviar solicitud"**

### Parte B — Cierre (CTA)
- **H2:** "¿Diseñamos juntos una experiencia extraordinaria?"
- Texto: "Confía en nuestros profesionales para crear experiencias que conectan emociones, retienen el talento y multiplican tus resultados."
- **Badges:** "Propuesta en menos de 48 horas" · "Propuesta sin costo" · "+500 empresas han confiado en nosotros"
- Botón: **"Conversemos"** (ancla al formulario de arriba, o a WhatsApp)

## Estado de implementación
- ✅ **Formulario CF7 creado** en staging (2026-07-18): "Ravina — Solicitud de propuesta", **id=4773**. Shortcode:
  `[contact-form-7 id="4773" title="Ravina — Solicitud de propuesta"]`
  Campos: nombre*, empresa, telefono*, email*, servicio (select 6 opciones), mensaje. Correo → ravina.patty@gymkanas.com.pe, Reply-To: [email], asunto "Nueva solicitud de propuesta — [servicio] (de [nombre])".
- ✅ **JS global "reveal on scroll" agregado** (WPCode snippet id 4774, activo, footer sitewide). Sin esto, todo lo que tiene `.reveal` en el mockup queda invisible (`opacity:0`). Verificado que el CSS `.reveal{opacity:0}` + `.reveal.in-view{opacity:1}` está en el Customizer.
- ✅ **Verificado:** todo el CSS del sistema de diseño (form-input, form-select, wp-card, eyebrow, wp-cta, btn-white, etc.) SÍ está en el Customizer → CSS Adicional de staging (`wp-custom-css`, ~14 KB). El form se estilará bien.
- ⏳ **Pendiente:** 
  1. Reescribir la plantilla del CF7 (id 4773) para que contenga **toda la sección Parte A** con el markup del mockup (grid 2 col + copy + 3 pasos con íconos + form con clases `form-input`/`form-select`/`wp-card`), reemplazando los `<input>` por tags CF7 con `class:form-input` etc. Así el form calza pixel-perfect. Markup exacto extraído en scratchpad (`parteA.html`, `parteB.html`).
     - **OJO técnico:** CF7 aplica `wpautop` a la plantilla por defecto → puede insertar `<p>`/`<br>` y romper el layout. Solución: snippet PHP `add_filter('wpcf7_autop_or_not','__return_false');` (WPCode) antes de meter HTML complejo en la plantilla. Verificar tras aplicar.
  2. ✅ **HECHO.** Snippet PHP "CF7 sin autop" creado (WPCode id 4775). Plantilla CF7 reescrita con la sección Parte A completa (pixel-perfect). Bloque reutilizable **sincronizado** creado: **"Ravina — Propuesta + Cierre"** (post type wp_block, publicado 2026-07-18). Contiene: bloque Shortcode `[contact-form-7 id=4773]` (Parte A) + bloque HTML personalizado (Parte B cierre CTA), agrupados en un "Pila" (Stack). Los shortcodes NO se ejecutan en bloques "HTML personalizado" → por eso Parte A va en Shortcode.
  3. ⏳ **Pendiente:** insertar el bloque reutilizable en las 9 páginas cuando se construyan.

## ✅ Verificado (2026-07-18)
Se creó una página de prueba (id 4776, "TEST — bloque reutilizable") con el bloque y se confirmó en el frontend de staging: el formulario se ve **pixel-perfect** (grid 2 col, copy + 3 pasos con íconos a la izquierda, card blanco con form a la derecha, botón naranja) y el cierre CTA naranja con "Conversemos"/"Ver servicios" + 3 badges. El reveal anima bien.

## ⚠️ Pendiente estructural (afecta a todas las páginas)
Las secciones `.wp-section` se ven **acotadas al ancho del contenido** del tema (no full-bleed edge-to-edge) porque el tema Avantage constriñe `.entry-content`. Para que los fondos (crema/naranja) lleguen de borde a borde en las páginas reales, hay que usar una **plantilla de página full-width** (Avantage suele traer una) o alignfull. Decidir al construir las 9 páginas.

## Snippets/IDs creados en staging (resumen)
- CF7 form: **id 4773** ("Ravina — Solicitud de propuesta")
- WPCode JS reveal: **id 4774** (footer sitewide, activo)
- WPCode PHP CF7-sin-autop: **id 4775** (activo)
- Bloque reutilizable: **"Ravina — Propuesta + Cierre"** (wp_block)
- Página de prueba: **id 4776** (se puede borrar cuando ya no se necesite)

## Decisiones de implementación
- **Formulario → Contact Form 7** (ya instalado): form CF7 con esos 6 campos, shortcode dentro del patrón. El estilo se hereda del CSS de marca (ajustar clases si CF7 genera markup propio).
- **Resto (copy, pasos, badges, CTA) → bloque "HTML personalizado"** de Gutenberg con el markup del mockup, o bloques nativos (Group/Columns/Botón). Reutiliza el CSS ya cargado.
- **Sincronizado:** al ser patrón sincronizado, editarlo una vez actualiza las 9 páginas.
- **Destino del formulario:** email a ravina.patty@gymkanas.com.pe (via CF7 + Flamingo, ya instalados). WhatsApp: +51 998 304 278.
- El botón "Conversemos" (Parte B) puede apuntar al WhatsApp (+51 998 304 278) o hacer scroll al formulario.
