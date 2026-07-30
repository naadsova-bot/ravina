# Mapa de keywords y SEO — Ravina (gymkanas.com.pe)

> **Paso 0 del montaje en WordPress.** De este mapa salen: `alt` y nombres de archivo de las imágenes, el título+meta de cada página (Yoast), los H1/encabezados y el schema. Todo tira de aquí para no hacerlo a ojo.
>
> **Fuentes combinadas:** (1) auditoría SEO `gymkanas-seo-report.pdf` (24 may 2026, score 37/100) → keywords de búsqueda, meta, schema, NAP; (2) **el copy real del mockup** (`index.html`, `quienes-somos.html`, `experiencias/*.html`) → voz de marca, value props y alt existentes. Este mapa **prioriza la voz del copy real** y le inyecta las keywords de la auditoría.

---

## Contexto de marca y decisión clave
- Dominio se queda en **gymkanas.com.pe** → **preservar la keyword "gymkanas"** aunque la marca visual pase a **Ravina**.
- Razón social: **Grupo Ravina Producciones S.A.** · Lima, Perú · **15 años**.
- Negocio: **eventos corporativos / servicios locales** → SEO local de **Lima** es crítico.

## Voz de marca (extraída del copy real — respetarla, no genérica)
El sitio nuevo NO vende "actividades divertidas"; vende **transformación de la cultura organizacional y resultados de negocio**. Palabras que definen la voz:
> cultura organizacional · engagement · retención de talento · clima organizacional · liderazgo · productividad · bienestar corporativo · dinámicas de integración · sentido de pertenencia · burnout · colaboradores felices y valorados

Frases ancla del copy (reutilizar en meta/encabezados):
- Home: *"Eventos corporativos que fortalecen la cultura de tu organización"* · *"Las mejores culturas organizacionales se construyen cuando las personas viven experiencias que fortalecen la confianza y el sentido de pertenencia."*
- Quiénes somos: *"Expertos en transformar la cultura organizacional"* · *"Creemos que colaboradores felices y valorados son el motor de toda empresa exitosa."*
- Los 5 "superpoderes organizacionales": **Liderazgo · Productividad · Engagement · Retención de talento · Calidez humana**.

## Activo E-E-A-T que YA tenemos (la auditoría lo pedía como faltante)
Logos de **clientes reales** ya en el mockup (Home) → usarlos con nombre en Home + schema, son prueba de autoridad:
**BCP · Interbank · Repsol · Yamaha · Mérieux NutriSciences · Unibanca · Abraham Lincoln School · Imaco · La Portuaria Cooperativa · Yichang**

## Datos NAP (footer, contacto, schema)
- **Tel:** +51 998-304-278 · **Email:** ravina.patty@gymkanas.com.pe · **Precio desde:** S/ 1,690 · **15 años**
- **Horarios reales de oficina:** 8:00–22:00 (8am–10pm), lunes a domingo. **Decisión (2026-07-18): NO mostrar horarios en la web** — ni en el schema `LocalBusiness` (sin `openingHours`), ni en footer, ni en contacto. Los horarios solo aplican, si acaso, a la ficha de **Google Business Profile** (propiedad aparte que gestiona Patricia).
- ❌ **PENDIENTE (pedir a Patricia):** dirección física + Google Maps embed → sin dirección no se compite por el "Local Pack". Tel y email ya están definidos.

## Keywords transversales
`gymkanas` · `team building` · `eventos corporativos` · `eventos empresariales` · `dinámicas de integración` · `Lima` / `Perú` · `empresas` / `corporativo`

---

## Fórmula de `alt` de imágenes
**`[Actividad concreta que muestra la foto] para [tipo de evento] en Lima`** — una keyword por imagen, natural, **accesibilidad primero** (lo leen lectores de pantalla), **sin amontonar keywords**.

Los alt del mockup ya son descriptivos; solo falta **inyectarles keyword + "Lima/integración/corporativo"**. Ejemplos de mejora (existente → optimizado):
- `Bubble soccer en evento corporativo` → **`Bubble soccer en evento corporativo de integración en Lima`**
- `Bingo Show en el jardín` → **`Bingo Show para evento empresarial al aire libre en Lima`**
- `Gymkana de globos — equipos comunicados` → **`Gymkana de integración con globos para empresas en Lima`**
- `Dinámica de disfraces gigantes en equipo` → **`Dinámica de team building con disfraces para empresas en Lima`**

Nombres de archivo: minúsculas, guiones, sin tildes/ñ. Ej: `bingo-show-evento-corporativo-lima.jpg`. (Nota: convertir a **WebP** en la carga a WordPress.)

---

## Mapa por página (9 páginas)
Cada bloque: **H1 real (mockup)** → **H1 recomendado** (conserva la voz + mete keyword) · **title tag** (Yoast) · **keywords** · **meta desc** · **schema**.
Regla H1 experiencias: el mockup pone solo el nombre ("Bingo Show") — **demasiado delgado para SEO** (es justo lo que la auditoría critica). Se expande apenas para meter la keyword, sin perder el look limpio; el "en Lima" va en el title tag.

### 1. Home (`/`)
- **H1 real:** "Eventos corporativos que fortalecen la cultura de tu organización." → **ya es bueno, mantener** (tiene la keyword primaria + value prop; reemplaza al viejo "No hay nada imposible").
- **Title:** `Gymkanas, Team Building y Eventos Corporativos en Lima | Ravina`
- **Keywords:** gymkanas y team building para empresas en Lima · eventos corporativos Lima · cultura organizacional · actividades de integración empresarial
- **Meta desc:** "Gymkanas, team building y eventos corporativos que fortalecen la cultura de tu empresa en Lima. 15 años de experiencia. Cotiza gratis."
- **Schema:** `LocalBusiness` + `Organization` (CRÍTICO). Logos de clientes reales como señal de autoridad.

### 2. Quiénes somos (`/quienes-somos/`)
- **H1 real:** "Expertos en transformar la cultura organizacional" → **mantener** (voz E-E-A-T fuerte).
- **Title:** `Quiénes somos — Gymkanas y Eventos Corporativos en Lima | Ravina`
- **Keywords:** transformar la cultura organizacional · Grupo Ravina Producciones · 15 años · engagement · retención de talento · liderazgo
- **E-E-A-T:** ya hay 5 superpoderes (Liderazgo/Productividad/Engagement/Retención/Calidez). Sumar casos reales (empresa, fecha, nº participantes) y perfiles de equipo.

### 3. Experiencias — listado (`/experiencias/`)
- **H1 real:** "Experiencias para cada necesidad de tu organización" → **mantener**.
- **Title:** `Servicios de Eventos Corporativos e Integración en Lima | Ravina`
- **Keywords:** servicios de eventos corporativos e integración en Lima · gymkanas · team building · full day · bingo show · juegos de feria · eventos privados
- **Schema:** `Offer`/`PriceSpecification` (desde S/ 1,690).

### 4. Bingo Show (`/experiencias/bingo-show/`)
- **H1 real:** "Bingo Show" → **recomendado: "Bingo Show para Empresas"**
- **Title:** `Bingo Show para Eventos Corporativos en Lima | Ravina`
- **Keywords primaria:** bingo show para eventos corporativos en Lima · **secundarias (del copy):** entretenimiento corporativo · pausas activas de alto engagement · fidelizar colaboradores · clima organizacional · reconocimiento
- **Meta desc:** "Bingo Show para eventos empresariales en Lima: pausas activas de alto engagement, animación y premios que fidelizan a tus colaboradores. Cotiza por WhatsApp."
- **Schema:** `Service`

### 5. Eventos privados (`/experiencias/eventos-privados/`)
- **H1 real:** "Eventos Privados" → **recomendado: "Eventos Privados Corporativos"**
- **Title:** `Eventos Privados y Corporativos en Lima | Ravina`
- **Keywords primaria:** eventos privados corporativos en Lima · **secundarias:** aniversarios de empresa · fiestas de fin de año · fiestas temáticas corporativas · reconocimiento al equipo · fidelización de talento
- **Meta desc:** "Producción de eventos privados corporativos en Lima: aniversarios, fin de año, inauguraciones y logro de metas. Logística impecable, cero estrés para RR.HH."
- **Schema:** `Service`

### 6. Full Day (`/experiencias/full-day/`)
- **H1 real:** "Full Day" → **recomendado: "Full Day Corporativo"**
- **Title:** `Full Day Corporativo para Empresas en Lima | Ravina`
- **Keywords primaria:** full day corporativo en Lima · **secundarias:** jornada de integración al aire libre · identidad corporativa · prevención de burnout · bienestar del equipo · desconexión de la rutina
- **Meta desc:** "Full Day corporativo al aire libre en Lima: jornadas de integración que previenen el burnout y fortalecen la identidad de tu equipo. Cotiza gratis."
- **Schema:** `Service`

### 7. Gymkanas productivas (`/experiencias/gymkanas-productivas/`)
- **H1 real:** "Gymkanas productivas" → **recomendado: "Gymkanas Productivas para Empresas"**
- **Title:** `Gymkanas Productivas para Empresas en Lima | Ravina`
- **Keywords primaria:** gymkanas para empresas en Lima · **secundarias:** dinámicas de integración · retención y fidelización del talento · reducción del estrés laboral · colaboración estratégica · valores de la empresa
- **Meta desc:** "Gymkanas productivas para empresas en Lima: dinámicas de integración que reducen el estrés, fidelizan el talento y fortalecen la colaboración entre áreas."
- **Schema:** `Service`

### 8. Juegos de feria (`/experiencias/juegos-de-feria/`)
- **H1 real:** "Juegos de Feria" → **recomendado: "Juegos de Feria para Empresas"**
- **Title:** `Juegos de Feria para Eventos Corporativos en Lima | Ravina`
- **Keywords primaria:** juegos de feria para empresas en Lima · **secundarias:** kermesse corporativa · actividades recreativas · competencia sana · bienestar corporativo · ruptura del hielo
- **Meta desc:** "Juegos de feria y kermesse para eventos corporativos en Lima: puestos temáticos y competencia sana que integran áreas y elevan el bienestar laboral."
- **Schema:** `Service`

### 9. Team building (`/experiencias/team-building/`)
- **H1 real:** "Team building" → **recomendado: "Team Building para Empresas"**
- **Title:** `Team Building para Empresas en Lima | Ravina`
- **Keywords primaria:** team building para empresas en Lima · **secundarias:** equipos de alto rendimiento · liderazgo · resolución de conflictos · retención de talento · productividad (B2B)
- **Meta desc:** "Team building B2B para empresas en Lima: desafíos que desarrollan liderazgo, mejoran la comunicación y elevan la productividad, con impacto medible en retención."
- **Schema:** `Service`

---

## Schema a implementar (auditoría: 4/100 — la mayor oportunidad)
| Schema | Dónde | Prioridad |
|---|---|---|
| `LocalBusiness` + `Organization` | Home | CRÍTICO |
| `Service` | Cada experiencia | ALTO |
| `Offer` / `PriceSpecification` (S/ 1,690+) | Listado / experiencias con precio | ALTO |
| `BreadcrumbList` | Todo el sitio | MEDIO |
| `AggregateRating` | Home | MEDIO (requiere reseñas) |

## Pendientes SEO heredados (de la auditoría) que resolvemos al construir
- **Meta descriptions:** faltaban en todas → arriba están las base por página.
- **Open Graph:** activar en Yoast → Social (compartir en LinkedIn/WhatsApp B2B).
- **Canonicals:** activar en Yoast → Advanced.
- **Contenido 600+ palabras** por experiencia (el copy real ya es sustancioso; verificar el mínimo).
- **Imágenes:** WebP + `loading="lazy"` (excepto hero) + width/height (anti-CLS).
- **Bloque reutilizable** propuesta+cierre+contacto: el copy ya está unificado ("Conversamos / Diseñamos propuesta / Ejecutamos juntos" + "¿Diseñamos juntos una experiencia extraordinaria?").
- **Blog (post-sitemap vacío = gran oportunidad):** keywords informacionales → "qué es una gymkana", "ideas team building Lima", "actividades día del trabajo empresas Lima", "full day corporativo Lima", "juegos de feria para empresas", "eventos de integración empresarial Lima".
- **`/llms.txt`** (AI/GEO, 12/100): crear con descripción + URLs clave para citabilidad en ChatGPT/Perplexity/AI Overviews.
