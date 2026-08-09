# 03 · Viabilidad técnica

Verificación de cada pieza contra la documentación oficial, 2026-08-08. Este documento existe para responder una sola pregunta: **¿qué de esto se puede construir realmente, con Canva Pro y sin presupuesto empresarial?**

---

## Resumen ejecutivo de hallazgos

| Pieza | Veredicto | Nota |
|---|---|---|
| Lectura de Excel / PDF / foto con IA | ✅ **Viable** | Costo despreciable (~USD 0.05/flyer) |
| Traducción EN⇄ES con glosario | ✅ **Viable** | Determinista + LLM como respaldo |
| Matching producto → imagen | ✅ **Viable** | Embeddings sobre catálogo propio |
| **Canva Autofill API** | ❌ **BLOQUEADO** | **Requiere Canva Enterprise. El cliente tiene Pro.** |
| **Canva Design Editing (Connect / MCP)** | ✅ **Viable** | **Disponible en todos los planes.** Esta es la vía. |
| Canva Apps SDK (app privada) | ✅ Viable | Alternativa/complemento |
| Publicar post en la página vía API | ✅ Viable | Graph API |
| Crear el anuncio inmediatamente tras publicar | ✅ **Viable — resuelve el dolor #2** | Marketing API con `object_story_id` |
| Reportes de Meta automáticos | ✅ Viable | Insights API |
| Orquestación en cron a las 21:00 | ✅ Viable | n8n self-hosted |

---

## 1. El hallazgo crítico: la Autofill API de Canva no sirve aquí

Esta es la conclusión más importante del análisis técnico, y probablemente **explica por qué las propuestas anteriores costaban USD 5,000 y USD 15,000 sin llegar a demostrar un demo funcional con más de 2 productos**.

La forma "obvia" de automatizar Canva es la **Autofill API**: le pasas un *brand template* y un JSON de datos, y Canva rellena el diseño. Es exactamente lo que este proyecto necesita.

**Pero la documentación oficial es inequívoca:**

> *"To use the Autofill APIs, your integration must act on behalf of a user who is a member of a Canva Enterprise organization."*
> — [Canva Connect APIs · Autofill](https://www.canva.dev/docs/connect/api-reference/autofills/)

Los planes Free, Pro y Teams **no tienen acceso** al endpoint de autofill. Existe una cuota de prueba limitada mientras la integración está en desarrollo, pero no para producción.

**Canva Enterprise** está diseñado para equipos de 30+, se vende con precio a medida vía comercial, y las estimaciones públicas van de **USD 2,000 a 30,000+ al año**.

La misma restricción aparece en el MCP oficial de Canva:

| Grupo de herramientas MCP | Plan requerido |
|---|---|
| Assets, Comments, **Designs**, **Exports**, **Editing**, Folders | **Todos los planes** |
| Brand templates, Resizes | Pro y superior |
| **Autofill**, Brand dataset | **Enterprise** |

> **Consecuencia:** cualquier propuesta que prometa "rellenar tu plantilla de Canva automáticamente vía Autofill API" con una cuenta Pro **no es ejecutable**. Verificarlo cuesta 10 minutos de lectura de documentación; no verificarlo cuesta USD 5,000.

## 2. La vía que sí funciona: Design Editing API

En 2026 Canva lanzó **12 APIs nuevas**, entre ellas **Design Editing API** (en beta) y Data Connectors. Permiten *"leer y actualizar programáticamente el layout y el contenido de un diseño de Canva, incluyendo tamaño, posición y estructura de los elementos individuales"*.

Y críticamente, en el MCP oficial las operaciones de edición figuran como **disponibles en todos los planes**:

```
start-editing-transaction     (20 req/min)   ← todos los planes
perform-editing-operations    (50 req/min)   ← todos los planes
commit-editing-transaction    (20 req/min)   ← todos los planes
cancel-editing-transaction    (20 req/min)   ← todos los planes
```

### El flujo propuesto

```
1. Copiar el diseño-plantilla de la tienda          → nuevo design_id
2. start-editing-transaction(design_id)
3. perform-editing-operations:
     · reemplazar texto  "PRODUCTO_1_EN"  → "MARINATED CHICKEN LEG QUARTERS"
     · reemplazar texto  "PRODUCTO_1_ES"  → "PIERNA Y MUSLO DE POLLO MARINADA"
     · reemplazar texto  "PRECIO_1"       → "$0.99"
     · reemplazar texto  "UNIDAD_1"       → "LB"
     · reemplazar fill de imagen del rect 1 → asset_ref de la foto
     · ... × 12–16 productos
     · reemplazar texto  "FECHAS"         → "4 DIAS DEL 22 AL 25 DE MAYO"
4. commit-editing-transaction
5. Devolver el link de edición → el equipo abre, revisa y ajusta
```

### Detalles técnicos que condicionan la implementación

Del Apps SDK, que comparte modelo con la Connect API:

- **No existe el concepto de "elemento imagen".** Las imágenes y videos se manejan como **rects con fills**. Reemplazar una foto = cambiar el `fill` de un rect.
- Se puede leer y modificar: posición (`top`, `left`), rotación, transparencia, contenido de texto con formato, colores y fills, agrupar/desagrupar, crear y eliminar elementos.
- **No soporta elementos de tabla.**
- Solo funciona sobre páginas "absolute" (no Canva Docs).
- Las sesiones de edición **expiran en 1 minuto** → hay que agrupar las operaciones y hacer un solo commit al final, nunca sincronizar por cada cambio.
- Multi-página está en preview; una sola página se carga a la vez.

**Riesgo declarado:** Design Editing API está en **beta**. Debe validarse con un *spike* técnico de 1–2 días antes de comprometer la Fase 2 completa (ver plan de contingencia en el punto 5).

### 2.1 Subida de imágenes a Canva

```js
const image = await upload({
  type: "image",
  mimeType: "image/jpeg",
  url: "https://cdn.flymar.example/productos/pierna-pollo.jpg",
  thumbnailUrl: "https://cdn.flymar.example/thumbs/pierna-pollo.jpg",
  aiDisclosure: "none"
});
// → image.ref se usa como fill del rect
```

Requisitos: la URL debe ser **públicamente accesible** (no `localhost`), y las URLs de miniatura deben soportar **CORS**. Esto obliga a tener el banco de imágenes en un CDN/bucket público (S3 + CloudFront, Cloudflare R2, o similar) — un requisito de infraestructura que hay que presupuestar (es barato: céntimos al mes a este volumen).

### 2.2 Alternativa: Canva App privada (Apps SDK)

En lugar de operar desde fuera, se construye una **app de Canva** que vive en el panel lateral del editor. El equipo abre su plantilla, hace clic en "Rellenar flyer", elige la tienda y la fecha, y la app inyecta todo.

- Se puede distribuir de forma privada al equipo, o usar en modo desarrollo/preview.
- Usa `addElementAtPoint`, la Design Editing API y `upload()` del paquete `@canva/asset`.
- **Ventaja:** control total sobre la UX, y el humano está literalmente delante del diseño cuando ocurre el llenado.
- **Desventaja:** requiere que alguien abra Canva — no sirve para un pipeline 100 % nocturno.

**Recomendación:** construir la vía Connect API como principal, y considerar la app privada en Fase 2b si se quiere una experiencia de "un clic dentro de Canva".

## 3. Meta: cómo recuperar las 12 horas perdidas

### 3.1 El problema, entendido correctamente

Meta ya no permite crear el anuncio sobre un post *programado* desde la interfaz. Por eso Ana Marcela programa el post a las 21:00 y **vuelve al día siguiente** a crear el anuncio, perdiendo la ventana nocturna de revisión.

Pero la limitación es **de la interfaz, no de la API**.

### 3.2 La solución

**No usar el programador de Meta.** En su lugar:

```
21:00:00  cron dispara
21:00:01  POST /{page_id}/feed  (published=true)  → post_id
21:00:03  POST /act_{account}/adsets  (copia del ad set plantilla,
                                       lifetime_budget, start_time, end_time)
21:00:05  POST /act_{account}/adcreatives
             object_story_id = "{page_id}_{post_id}"
21:00:07  POST /act_{account}/ads  (status = ACTIVE)
21:00:08  → el anuncio entra a revisión de Meta a las 21:00, no a las 09:00
```

El punto clave es `object_story_id`: la Marketing API permite crear anuncios que referencian **posts de página existentes** por su ID, lo que además **preserva la prueba social** (likes, comentarios y compartidos se acumulan en un único post aunque tenga varias variantes de anuncio).

### 3.3 El impacto medible

Datos públicos sobre tiempos de revisión de Meta:

- La mayoría de anuncios se revisan **dentro de 24 h**; cuentas establecidas con buen historial suelen ver aprobaciones en **2–6 h**.
- Los anuncios publicados de noche pueden empezar a entregar **4–11 h más tarde** que los publicados en tarde de día hábil, porque las colas de revisores están más delgadas — **pero eso es exactamente lo que se busca aquí**: que la revisión ocurra mientras nadie compra, no durante la vigencia de la oferta.
- **La subasta de Meta no penaliza los lanzamientos nocturnos.** Lo que importa es el momento en que Meta empieza a gastar el presupuesto.

**Ganancia esperada:** de crear el anuncio a las ~09:00 del día siguiente a crearlo a las 21:00 del día anterior son **~12 horas recuperadas** sobre una vigencia de 48 h → **+25 % de tiempo efectivo de campaña**. Esto además es consistente con la observación empírica que ya tiene el cliente ("cuando lo hago a las 9 de la noche me cuesta menos la interacción").

> **Nota metodológica honesta:** la mejora del costo por interacción que Ana Marcela ya observó es una correlación, no un experimento controlado. La Fase 4 debe incluir un **test A/B explícito de horario de lanzamiento** para tener un dato defendible ante el cliente final, no una anécdota.

### 3.4 Programación a nivel de ad set

Para ad sets con `lifetime_budget` (que es justo lo que usa el cliente), Meta soporta `campaign_schedule` — bloques de `start_time` / `end_time` en minutos desde medianoche, con array de días. **No se puede aplicar un schedule a un ad set ya activo**, debe configurarse al crearlo. Como aquí siempre se crea uno nuevo por campaña, la restricción no aplica.

### 3.5 Autenticación y permisos

- Se requiere una **app de Meta** con permisos `ads_management` y `pages_manage_posts`.
- Recomendación: usar un **System User** dentro del Business Portfolio del que ya es administradora, con un token de larga duración. Evita la fricción del app review para uso interno.
- El **Meta Ads MCP oficial** (`mcp.facebook.com/ads`, abierto en beta el 29-abr-2026) usa el flujo OAuth estándar de Meta Business, sin registro de app ni espera de revisión — buena opción para la capa de consulta y ajustes, y para acelerar el prototipo.

## 4. Extracción de datos: costo y estrategia

### 4.1 Los tres formatos, un solo camino

| Formato | Tratamiento |
|---|---|
| **Excel `.xlsx`** | ⚠️ El parseo tabular clásico falla (ver doc 01 §3.1). Dos opciones: (a) convertir la hoja a imagen/PDF y procesarla con visión, o (b) reconstruir bloques por celdas fusionadas. **Recomendado: (a)**, es más robusto ante cambios de maquetación del manager. |
| **PDF** | Entrada nativa de documento al modelo (hasta 32 MB / 600 páginas). |
| **Foto JPG/PNG** | Entrada de imagen nativa. |

En los tres casos la salida es el mismo esquema JSON, forzado con **structured outputs** (`output_config.format` con JSON Schema), lo que garantiza que la respuesta valide contra el esquema — no hay que parsear texto libre ni reintentar.

### 4.2 Esquema canónico propuesto

```jsonc
{
  "tienda": "Mercado Fresco Metropolitan",
  "direccion": "5117 Independence Ave",
  "vigencia": { "inicio": "2026-05-13", "fin": "2026-05-14", "dias": 2 },
  "idioma": "bilingue",              // "bilingue" | "solo_ingles"
  "notas_manager": ["USE IMAGES OF COOKED MEAT FOR PACKAGED MEAT"],
  "productos": [
    {
      "categoria": "meat",           // meat | produce | grocery | bakery
      "orden": 1,
      "nombre_es": "PIERNA DE POLLO",
      "nombre_en": "CHICKEN LEG QUARTERS",
      "precio": { "valor": 0.89, "moneda": "USD", "formato": "simple" },
      "unidad": "LB",
      "plu": "469",
      "confianza": { "nombre": 0.98, "precio": 0.99, "unidad": 0.97 }
    }
  ]
}
```

Los formatos de precio se normalizan a un tipo cerrado: `simple` (`0.89`), `por_unidad` (`$3.49/LB`), `multiple` (`2/$7.00`), `multiple_x` (`2 X 1.00`).

### 4.3 Costo real de la IA

Precios vigentes de la API de Claude (por millón de tokens):

| Modelo | Input | Output | Contexto |
|---|---|---|---|
| Claude Opus 5 | USD 5.00 | USD 25.00 | 1M |
| Claude Sonnet 5 | USD 3.00 (intro USD 2.00 hasta 2026-08-31) | USD 15.00 (intro USD 10.00) | 1M |
| Claude Haiku 4.5 | USD 1.00 | USD 5.00 | 200K |

**Estimación por flyer** (1 documento/imagen ≈ 2–5 K tokens + prompt ~2 K + salida ~1.5 K):

| Modelo | Costo por flyer | Costo mensual (60 flyers) |
|---|---|---|
| Claude Sonnet 5 | ~USD 0.045 | **~USD 2.70** |
| Claude Opus 5 | ~USD 0.075 | ~USD 4.50 |
| Claude Haiku 4.5 | ~USD 0.015 | ~USD 0.90 |

Además hay dos palancas que bajan más el costo:
- **Batch API: 50 % de descuento** para trabajo no urgente (ej. los reportes mensuales).
- **Prompt caching:** las lecturas de caché cuestan ~0.1× el precio de input. Como el prompt de extracción y el glosario son fijos, el ahorro es sustancial.

> **Conclusión de costos:** el gasto variable de IA en este proyecto es de **menos de USD 5 al mes**. Cualquier propuesta que justifique USD 500–2,000/mes de "mantenimiento" por costos de IA está inflando el precio. El costo real está en el desarrollo inicial, no en la operación.

**Recomendación de modelo:** Sonnet 5 para la extracción (el mejor equilibrio para un dominio acotado), con escalado a Opus 5 solo en documentos que el validador marque como dudosos.

### 4.4 Precisión esperada y por qué hace falta revisión humana

Los benchmarks públicos de 2026 sitúan la extracción estructurada genérica en **65–80 % de precisión por campo**. Este caso está mucho mejor posicionado (dominio cerrado, esquema estricto, validación determinista posterior), pero **la revisión humana debe estar en el diseño desde la Fase 1**, no añadirse después.

La validación determinista que va *después* del modelo es la que sube la precisión real:

```
✓ precio parseable con las 4 sintaxis conocidas → si no, marcar
✓ unidad ∈ catálogo conocido → si no, marcar y añadir al catálogo
✓ categoría ∈ {meat, produce, grocery, bakery} → obligatorio
✓ nombre_es y nombre_en no vacíos si idioma == bilingue
✓ ortografía contra glosario propio de términos de supermercado
✓ 3–4 productos por categoría (fuera de rango → marcar)
```

## 5. Matriz de riesgos técnicos y contingencias

| # | Riesgo | Probabilidad | Impacto | Mitigación / Plan B |
|---|---|---|---|---|
| R1 | Design Editing API está en **beta** y cambia o se restringe | Media | Alto | **Spike de 2 días en Fase 0** antes de comprometer Fase 2. Plan B: app privada de Canva (Apps SDK). Plan C: render propio HTML/SVG → PNG e importación a Canva. |
| R2 | El resultado en Canva no queda tan editable como el cliente espera | Media | Alto | Validar con **el diseño real del cliente**, no con una maqueta, en el spike. Criterio de aceptación explícito: "puedo mover el melón donde estaba el cilantro". |
| R3 | Meta cambia políticas o endpoints | Media | Medio | Aislar toda la lógica de Meta en un adaptador con interfaz propia. Cambios se absorben en un archivo. |
| R4 | Permisos `ads_management` requieren app review | Media | Medio | Usar System User dentro del Business Portfolio existente + Meta Ads MCP oficial (sin app review). |
| R5 | Extracción con errores en producción | Alta | Medio | Human-in-the-loop obligatorio en Fases 1–2. Métrica de precisión visible. Toda corrección alimenta el glosario. |
| R6 | El manager cambia el formato de su archivo | Alta | Bajo | La extracción con visión es tolerante al formato (esa es su ventaja frente a un parser rígido). Además, Fase 0 estandariza la plantilla de entrada. |
| R7 | **Reclamación legal por una imagen de la biblioteca actual** | **Media** | **Crítico** | **Auditoría de la biblioteca en Fase 0.** Ya existe demanda contra otra agencia por el mismo tipo de uso. |
| R8 | Rate limits de Canva (20–50 req/min) | Baja | Bajo | Agrupar operaciones en una sola transacción por diseño; cola con backoff. |
| R9 | Sesión de edición de Canva expira (1 min) | Media | Bajo | Preparar todo el payload antes de abrir la transacción; un solo commit. |

---

**Siguiente:** [04 · Propuesta de solución](./04-propuesta-de-solucion.md)
