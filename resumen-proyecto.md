# Resumen del proyecto: Migración Ravina → WordPress (gymkanas.com.pe)

> Generado el 2026-07-18. Este documento resume todo el trabajo hecho hasta ahora para poder continuar el proyecto en otra sesión o cuenta.

---

## 1. CONTEXTO DEL PROYECTO

**Producción:**
- URL: `https://www.gymkanas.com.pe/`
- Admin: `https://www.gymkanas.com.pe/wp-admin/`
- Usuario admin de WordPress: `patricia` (rol Administrador)
- Estado: publicado, en vivo, **sin ningún cambio** durante todo este proyecto

**Staging (entorno de prueba):**
- URL: `https://www.gymkanas.com.pe/ravina-staging/`
- Admin: `https://www.gymkanas.com.pe/ravina-staging/wp-admin/`
- Cómo se creó: plugin **WP Staging (versión gratuita)**, instalado en producción. El staging es un **clon completo en subcarpeta del mismo dominio** (no un subdominio ni servidor aparte).
- Base de datos del staging: `mjrvqwtv_gymkanasperu`, prefijo de tablas `wpstg0_`
- Ruta en servidor: `/home/mjrvqwtv/public_html/ravina-staging/`
- Credenciales: **las mismas que producción** (usuario `patricia`), porque es un clon de la base de datos completa
- El staging se **restableció 3 veces** desde producción durante este proyecto (función "Restablecer" de WP Staging) tras los intentos fallidos de actualizar el núcleo — cada reset es seguro y no afecta producción

**Hosting:**
- Proveedor: **BanaHosting** (hosting compartido, cPanel)
- Servidor web: **LiteSpeed**
- cPanel: `https://www.gymkanas.com.pe:2083` (usuario del sistema: `mjrvqwtv`) — **credenciales aún no disponibles**, ver sección 6
- Nameservers: `ns8940.banahosting.com`, `ns8941.banahosting.com`
- IP: `216.246.46.152`
- Correo del dominio: Google Workspace (MX apuntan a Google)
- Base de datos: MariaDB 11.4.12

**Versiones y stack (producción Y staging, idénticas):**
- WordPress: **6.1.1** (hay una versión 7.0.2 disponible, pero **no se pudo actualizar** — ver sección 4)
- PHP: **7.4.33** (desactualizado; actualización planeada pero no ejecutada — ver sección 5)
- Tema activo: **Avantage Child** (tema padre: Avantage) — se mantiene como base neutra, NO se reemplaza por Kadence (ver sección 3)
- Builder original del tema viejo: **Bold Builder** (se está reemplazando por Gutenberg nativo)

**Plugins relevantes instalados en staging (estado actual):**
| Plugin | Estado en staging | Uso |
|---|---|---|
| Yoast SEO | Activo | SEO por página (ya estaba en producción) |
| Yoast Duplicate Post | Activo | Duplicar páginas (clave para plantilla de Experiencias) |
| UpdraftPlus | Activo | Backups (ya estaba en producción; se tomó 1 backup completo real en producción) |
| WP Staging (free) | Activo | Gestión del clon de staging |
| **LiteSpeed Cache** | Activo (nuevo) | Caché/rendimiento — aprovecha que el servidor ya es LiteSpeed |
| **Enable Media Replace** | Activo (nuevo) | Reemplazar fotos sin romper enlaces |
| **WPCode Lite** | Activo (nuevo) | Insertar el `<link>` de Google Fonts en el `<head>` global |
| Better Search Replace | Instalado, inactivo | Para la migración final (limpiar URLs de staging) |
| Contact Form 7 + Flamingo | Activos | Formulario de contacto (se reutilizará) |
| Meta Box, Really Simple SSL, Sidebar Manager | Activos | Sin decisión aún, no interfieren |
| Avantage Plugin, Bold Builder, BoldThemes WordPress Importer, Click to Chat, GTM4WP, Soundy Background Music, WP-Optimize | **Desactivados** (solo en staging) | Plugins del diseño viejo, ya no se usarán |

---

## 2. OBJETIVO DE LA ACTUALIZACIÓN

Esto **no es una actualización de plugin/tema tradicional** — es una **migración/rediseño completo** del sitio:

- **Origen:** sitio WordPress viejo con marca "Gymkanas", tema Avantage + Bold Builder, contenido y diseño de 2022-2024.
- **Destino:** nuevo diseño de marca **"Ravina"**, ya desarrollado como mockup HTML estático de alta fidelidad en el repo local `/Users/nat/ravina` (9 páginas: Home, Quiénes somos, Experiencias + 6 subpáginas).
- **El dominio se mantiene** (`gymkanas.com.pe`) para conservar el SEO/posicionamiento existente; solo cambia la marca visual y el contenido.
- **Regla innegociable:** el sitio viejo **sigue publicado sin cambios** hasta que el usuario dé el "go" explícito. Recién ahí se **despublica** (nunca se borra) y queda de backup.

### Decisiones de stack (y por qué)

| Decisión | Resultado |
|---|---|
| Constructor de páginas | **Gutenberg nativo** (se descartó Elementor) |
| Bloques adicionales (carrusel/tabs/contador) | **Descartado Kadence Blocks** — requiere WordPress 6.6+, incompatible con la versión actual (6.1.1). Se usa el bloque nativo **"HTML personalizado"** reutilizando el CSS/JS vanilla que ya trae el mockup |
| Tema | **Descartado Kadence theme** — requiere WordPress 6.3+. Se mantiene **Avantage** como base neutra; el diseño de marca va vía **CSS Adicional** |
| Actualización de núcleo de WordPress | **Descartada** — 3 intentos fallaron reproduciblemente (ver sección 4). Se sigue trabajando en WP 6.1.1 |
| Entorno de montaje | **Staging privado** (WP Staging), nunca se toca producción hasta el "go" |

**Nota clave:** el propio mockup HTML ya incluye, en un comentario al inicio de `index.html` (líneas 1-37), un **"mapa de implementación en WordPress"** que mapea cada clase CSS a un bloque de Gutenberg sugerido (`.wp-section` → Group block, `.wp-card` → Group repetido en Columns, etc.) y confirma que el CSS global "va UNA sola vez en Apariencia > Personalizar > CSS adicional". Esto validó exactamente el enfoque que se terminó usando.

---

## 3. TRABAJO REALIZADO HASTA AHORA (cronológico)

1. **Exploración del sitio viejo**: WP 6.1.1, tema Avantage Child, Bold Builder, 18 páginas (varias son backups viejos sin usar).
2. **Exploración del mockup nuevo**: repo `/Users/nat/ravina`, 9 páginas HTML autocontenidas con CSS/JS vanilla, 139 imágenes en base64, sistema de diseño de 17 variables de marca.
3. **Identificación de infraestructura**: hosting BanaHosting confirmado vía DNS/Site Health; cPanel existe pero sin credenciales.
4. **Backup completo real** tomado en producción con UpdraftPlus (base de datos + archivos + plugins + temas + subidas, ~364 MB) — `18/07/2026 ~14:20`.
5. **Instalación de WP Staging** en producción y creación del clon `ravina-staging`.
6. **Intento de instalar el tema Kadence y Kadence Blocks** → se descubrió que ambos requieren versiones de WordPress más nuevas que la actual (6.3+ y 6.6+ respectivamente).
7. **3 intentos de actualizar el núcleo de WordPress** (6.1.1 → 7.0.2, tanto en español como en inglés) en staging, incluso reduciendo los plugins activos al mínimo primero. **Los 3 intentos fallaron** dejando el sitio con error 503 (ver detalle en sección 4). Cada vez se restableció el staging con la función "Restablecer" de WP Staging, sin ningún impacto en producción.
8. **Decisión final de stack**: abandonar Kadence y la actualización de núcleo; usar Gutenberg nativo + bloques "HTML personalizado" con el CSS/JS propio del mockup.
9. **Instalación de LiteSpeed Cache y Enable Media Replace** en staging.
10. **18 páginas viejas movidas a la papelera** — solo dentro del staging (base de datos separada), producción intacta.
11. **Desactivación de 7 plugins legacy** en staging (Avantage Plugin, Bold Builder, BoldThemes WordPress Importer, Click to Chat, GTM4WP, Soundy Background Music, WP-Optimize) — ya no se usarán en el diseño nuevo.
12. **Extracción y consolidación del CSS de marca**: se extrajo el bloque `:root` + sistema de tipografía/tarjetas/grids/botones/carrusel/tabs desde `index.html` del mockup (17 variables de marca + ~280 líneas de sistema de diseño, 16 KB).
13. **CSS aplicado y publicado** en Personalizar → CSS Adicional del staging (vía la API de CodeMirror por JavaScript, reemplazando el CSS viejo del sitio Bold Builder que estaba ahí).
14. **Instalación de WPCode Lite** para insertar en el `<head>` global el `<link>` de Google Fonts (Bricolage Grotesque + Plus Jakarta Sans), reproduciendo exactamente lo que tenía el mockup.
15. **Verificado en consola del navegador**: las variables CSS (`--naranja`, `--container-max`, etc.) y la fuente de Google cargan correctamente en el staging.
16. **Identificados los patrones reutilizables** del sitio para eficiencia de construcción (ver sección 5).

### Archivos modificados
- **Ninguno en el repo local `/Users/nat/ravina`** — todo el trabajo de WordPress se hizo vía el admin (Customizer, WPCode, plugins), no se tocó el código fuente del mockup salvo lectura/extracción de su CSS.
- Archivo temporal de trabajo (fuera del repo, en scratchpad de la sesión): CSS de marca extraído y consolidado (`ravina-additional-css.css`, 16 KB) — su contenido ya está pegado en WordPress, no es necesario para continuar salvo referencia.

### Configuraciones cambiadas (solo en staging)
- `wp-config.php` de staging: `WP_CACHE` puesto en `FALSE` (hecho automáticamente por WP Staging, no manual).
- Enlaces permanentes desactivados en staging (hecho automáticamente por WP Staging, "por razones técnicas").
- Customizer → CSS Adicional (staging): reemplazado completo con el sistema de diseño de Ravina.
- WPCode → Cabecera y pie de página globales (staging): agregado el snippet de Google Fonts en "Cabecera".

### Comandos ejecutados
Solo comandos de shell locales (bash) para leer/extraer el CSS del mockup (`grep`, `sed`, `awk`, `python3` para dedupe seguro). **No se usó wp-cli ni git** — no hay acceso SSH al hosting; todo el trabajo en WordPress fue vía navegador (admin de WP).

---

## 4. ESTADO ACTUAL

### ✅ Qué funciona
- Producción: **100% intacta, publicada, sin ningún cambio**.
- Staging: saludable, en WordPress 6.1.1, accesible en `https://www.gymkanas.com.pe/ravina-staging/`.
- Sistema de diseño de marca (CSS + Google Fonts) cargando correctamente en staging.
- Plugins base para el nuevo enfoque instalados y activos (LiteSpeed Cache, Enable Media Replace, WPCode).
- Staging limpio: 18 páginas viejas en papelera, plugins legacy desactivados.
- Backup real de producción disponible (UpdraftPlus).

### 🔄 Re-verificado el 18/07/2026
Se confirmó directamente en producción (wp-admin → Salud del sitio → pestaña Información → sección Servidor) que **nada cambió**: WordPress sigue en **6.1.1** y PHP sigue en **7.4.33**. No hubo ninguna actualización de versión desde la última sesión.

### 📸 Inventario de fotos — consolidado en un solo archivo (18/07/2026)
Había 3 versiones desincronizadas (Sheet oficial viejo, copia local `.xlsx`, y una copia reconciliada). Se descubrió que la copia local estaba desactualizada (79 filas/8 pendientes) frente al Sheet real (65 filas/**15 pendientes** = las 5 subpáginas de experiencias completas: Gymkanas productivas, Bingo Show, Juegos de feria, Full Day, Eventos privados, cada una con hero + card + galería).

Por decisión de Natalia quedó **un solo inventario oficial**: el Sheet **[inventario-fotos-web](https://docs.google.com/spreadsheets/d/1cke1US-HjlP7mIuQi1ISkcnOKBj22Rsk/edit?gid=1414441350)** (la copia reconciliada, ya renombrada limpio). Sus 15 pendientes están completas: estado "Stock temporal", descripción, observación, y **link a Drive** (15 placeholders de fondo blanco "Foto 1"–"Foto 15" en la carpeta de Drive "STOCK TEMPORAL (placeholders)", subidos por Natalia; imágenes locales en `fotos-web/stock-temporal/`).

Consolidación ejecutada: el **Sheet viejo** (`1R15vYr...`) se mandó a **papelera** de Drive; la **copia local** `inventario-fotos-web.xlsx` se **borró del repo** (queda como `D` en git, sin commitear). Compartido del Sheet oficial verificado: Natalia (Owner) + ravina.patty@gymkanas.com.pe (Editor). Pendiente futuro: subir las 15 imágenes a la Biblioteca de medios de WordPress cuando se haga la carga general.

### ⏳ Qué está pendiente
- **Ninguna página nueva de Ravina construida todavía en WordPress** — el proyecto está en fase de preparación/infraestructura, no de contenido.
- Bloque reutilizable compartido (propuesta+cierre+contacto) — no creado.
- Menú de navegación y footer del sitio nuevo — no armados.
- Fotos — no subidas a la Biblioteca de medios (inventario: 72 cargadas / 8 con stock temporal en el Sheet local, pero nada subido a WordPress aún). Las 8 pendientes ya tienen su placeholder generado (ver abajo), falta subir el Sheet oficial y la Biblioteca de medios.
- SEO/schema por página — no aplicado.
- Rol "Editor" para Patricia — no creado.
- Medición de Core Web Vitals — no hecha.
- WordPress sigue en versión **6.1.1** (no se pudo actualizar — ver error abajo).
- PHP sigue en versión **7.4.33** (actualización planeada para el momento del "go" final, bloqueada por falta de acceso a cPanel).

### ❌ Error encontrado (ya no bloqueante — se abandonó ese camino)
Al intentar actualizar WordPress núcleo de 6.1.1 a 7.0.2 vía `wp-admin/update-core.php` (probado con el paquete en español y en inglés, 3 veces en total, incluso con plugins reducidos al mínimo):

> **"503 Service Unavailable — The server is temporarily busy, try again later!"**

Esto ocurre en **cualquier página con ejecución PHP** (wp-admin, wp-login, home) después de la actualización. Se confirmó que los **archivos del núcleo sí se reemplazan** (el archivo estático `readme.html` pasó a reflejar los requisitos de la versión nueva, "PHP version 7.4 or greater"), pero algo rompe la ejecución de PHP inmediatamente después — muy probablemente un **límite de memoria/recursos PHP** del hosting compartido (BanaHosting) al arrancar el núcleo más nuevo. Se descartó que fuera un conflicto de plugins (se probó con la mayoría desactivados, mismo resultado).

**Decisión:** no se sigue intentando este camino. Se construye todo el sitio nuevo sobre WordPress 6.1.1 usando bloques nativos (compatibles con cualquier versión desde WP 5.0), sin depender de un tema o plugin que requiera una versión más nueva.

---

## 5. PRÓXIMOS PASOS PLANEADOS

### Patrones reutilizables identificados (para construir de forma eficiente, no página por página)
| Elemento | Se construye | Se usa en |
|---|---|---|
| Header/menú de navegación | 1 vez | Las 9 páginas (automático, parte del tema) |
| Footer | 1 vez | Las 9 páginas (automático, parte del tema) |
| Bloque "propuesta + cierre + formulario de contacto" | 1 vez (bloque reutilizable **sincronizado** de Gutenberg) | Las 9 páginas |
| Plantilla de página de Experiencia | 1 vez, duplicada 5 veces (Yoast Duplicate Post) | Las 6 subpáginas de Experiencias |
| Video de YouTube (patrón "facade": miniatura + botón play) | 1 vez (patrón de código) | Las 9 páginas (cambia solo el ID del video) |
| Carrusel del hero con flechas (`hero-cslide` + `hero-arrow` + función `heroGo()`) | 1 vez (patrón de código) | **Las 9 páginas** (incluye Home — corregido: se creía que Home usaba un carrusel automático sin flechas, pero el HTML real de `index.html` usa el mismo mecanismo de flechas que las demás páginas; la clase `.hero-slide` de auto-rotación existe en el CSS pero no se usa en ningún HTML) |
| Tabs (pestañas) | 1 vez (patrón de código) | 2 páginas (Quiénes Somos y Experiencias listado) |

### ✅ AVANCE AL 2026-07-19 — LAS 9 PÁGINAS construidas y publicadas en staging. Menú 100% cableado (0 enlaces vacíos).
| Página | id | Estado |
|---|---|---|
| Home ("Inicio") | 4823 | ✅ Publicada y **fijada como portada** — fotos finales, logos de clientes activos, alts SEO |
| Quiénes somos | 4798 | ✅ Publicada — carrusel, tabs, responsive, alts SEO |
| Bingo Show | 4863 | ✅ Publicada (fotos temporales; las reales extraídas en `fotos-web/bingo-show/`) |
| Gymkanas productivas | 4865 | ✅ Publicada (mockup casi sin fotos; placeholders diseñados) |
| Juegos de Feria | 4866 | ✅ Publicada (mockup sin fotos; placeholders diseñados) |
| Full Day | 4867 | ✅ Publicada (ídem) |
| Team Building | 4868 | ✅ Publicada (fotos temporales; reales en `fotos-web/team-building/`) |
| Eventos Privados | 4869 | ✅ Publicada (fotos temporales; reales en `fotos-web/eventos-privados/`) |
| **Experiencias (índice)** | 4874 | ✅ Publicada — tabs de 6 experiencias funcionando, todo cableado |

Menú del header cableado a las 8 páginas (staging `?page_id=`). Biblioteca de medios: 48/49 archivos con alt+título SEO. Metadata de imágenes de la Home optimizada con la fórmula del mapa de keywords.

### Pasos pendientes, en orden
1. **Natalia sube las fotos extraídas** (30 archivos: `fotos-web/bingo-show/` 7, `team-building/` 10, `eventos-privados/` 12, `gymkanas-productivas/` 1) → Claude hace el swap (cada carpeta tiene `_map.json` con el mapeo alt→archivo).
3. Pasada SEO final: titles/metas de Yoast por página + schema LocalBusiness/Service (todo definido en `mapa-keywords-seo.md`).
4. Fotos reales para Juegos de Feria y Full Day vía flujo del inventario (el mockup nunca las tuvo — placeholders diseñados a propósito).
5. Borrar página TEST 4776. Optimizar a WebP (opcional). Pedir dirección física a Patricia (Local SEO).
8. Aplicar SEO/schema por página (Yoast + datos estructurados).
9. Crear el rol **Editor** para Patricia (no Administrador).
10. Medir Core Web Vitals (PageSpeed Insights) sobre el staging y ajustar si hace falta.
11. **Entregar la URL de staging para revisión del usuario — no avanzar sin su aprobación.**
12. Solo con el **"go" explícito**: migrar el contenido (exportar staging → importar producción + Better Search Replace para limpiar URLs + reconstruir menú), **actualizar PHP** (con backup fresco, en este mismo momento), instalar/activar lo necesario en producción, **despublicar** (nunca borrar) las páginas viejas, configurar redirects 301 si cambian los slugs.

### Riesgos y consideraciones importantes
- **No reintentar la actualización de núcleo de WordPress** sin antes resolver con soporte de BanaHosting la causa raíz (probable límite de memoria PHP).
- **La actualización de PHP afecta todo el dominio a la vez** (producción y staging juntos, porque el staging es una subcarpeta, no un subdominio) — a diferencia del núcleo de WordPress, no se puede probar en aislamiento. Por eso se planeó para el momento del "go" final, con backup fresco tomado justo antes.
- **Falta el acceso a cPanel** — bloquea la actualización de PHP (ver sección 6). Patricia tiene el correo de recuperación de la cuenta de hosting, así que el acceso **es recuperable**; se gestionará junto con el envío del link de staging para revisión (no antes), dejando tiempo de margen porque el proceso de recuperación puede tardar días.
- **Regla innegociable:** nunca despublicar ni borrar nada de producción sin el "go" explícito del usuario.

### Riesgo aceptado: lanzar sobre WordPress 6.1.1 / PHP 7.4.33
El proyecto **sí se puede construir y publicar completo** sobre las versiones actuales — no hay ningún bloqueo técnico para eso. Si el acceso a cPanel tardara en recuperarse o nunca se lograra, el sitio seguiría funcionando, pero quedaría esta deuda técnica pendiente:
- **Seguridad:** PHP 7.4 llegó a su fin de soporte (EOL) en noviembre de 2022 — ya no recibe parches de seguridad. Cuanto más tiempo pase sin actualizar, más crece el riesgo.
- **Rendimiento:** PHP 7.4 tiene un techo de rendimiento más bajo que 8.x, lo cual choca con la prioridad #1 del proyecto (Core Web Vitals). Se compensa parcialmente con LiteSpeed Cache, pero no se elimina.
- **Control operativo:** sin cPanel no hay forma de ver logs de servidor, diagnosticar incidentes (como el error 503 de la sección 4), ni reaccionar si BanaHosting fuerza un cambio de versión de PHP de su lado.
- **Decisión:** se acepta el riesgo por ahora; se resuelve cuando Patricia recupere el acceso a cPanel (ver arriba).

---

## 6. ACCESOS NECESARIOS

Para continuar este proyecto, quien lo retome necesita:

| Acceso | Nivel | Estado |
|---|---|---|
| **wp-admin de producción** (`gymkanas.com.pe/wp-admin/`) | Administrador | ✅ Disponible (usuario `patricia`) |
| **wp-admin de staging** (`gymkanas.com.pe/ravina-staging/wp-admin/`) | Administrador | ✅ Disponible (mismas credenciales que producción, es un clon de la base de datos) |
| **cPanel de BanaHosting** (`gymkanas.com.pe:2083`) | Panel de hosting | ❌ **No disponible aún, pero recuperable** — Patricia tiene el correo de recuperación de la cuenta. Se gestionará junto con el envío del link de staging para revisión (no antes), dejando margen porque el proceso puede tardar días. Necesario para actualizar PHP. Vías: "Reset Password" en el login de cPanel (usuario del sistema: `mjrvqwtv`), contacto con "sergio" (autor histórico del sitio), o soporte de BanaHosting directamente |
| FTP / SSH | — | No ha sido necesario hasta ahora; todo el trabajo se hizo vía navegador (wp-admin) |
| Base de datos directa (phpMyAdmin, etc.) | — | No ha sido necesario; no se ha tocado la BD directamente, todo vía la interfaz de WordPress |
| Repo local del mockup | Lectura de archivos | `/Users/nat/ravina/` (index.html, quienes-somos.html, experiencias.html, carpeta `experiencias/`, carpeta `fotos-web/`, `inventario-fotos-web.xlsx`) |
| Inventario de fotos (Google Sheets) | Lectura/edición | Ver memoria del proyecto para el link; también hay copia local en `.xlsx` |

**No se necesitan contraseñas para continuar la lectura de este documento** — las credenciales reales de WordPress/hosting deben pedirse directamente a Patricia o Natalia, no están expuestas aquí.
