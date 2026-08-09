# 04 · Propuesta de solución — Flymar

**Para:** Fresco Marketing · Ana Marcela
**Alcance:** del correo del manager al anuncio publicado en Meta, con reportes
**Fecha:** 2026-08-08

---

## 1. La tesis en un párrafo

El trabajo que hoy toma 45–60 minutos por flyer no es trabajo creativo: es **transcribir datos de un archivo mal estructurado, traducirlos, buscar imágenes y colocarlos en el mismo lugar de siempre**. Eso es automatizable casi por completo. Lo que no se automatiza —y no debe automatizarse— es el criterio: qué imagen se ve mejor, si un precio es raro, si el manager quiso decir otra cosa. La propuesta separa esas dos cosas: la máquina hace el 90 % mecánico y le entrega a la persona una pantalla donde en **5 minutos** confirma o corrige lo dudoso. Y del otro lado del proceso, mueve la creación del anuncio en Meta de las 9 de la mañana a las 9 de la noche, recuperando **el 25 % de la vigencia** de cada oferta de dos días.

## 2. Principios de diseño

1. **El humano decide, la máquina prepara.** Nunca se publica algo que nadie miró. La revisión pasa de "armar" a "aprobar".
2. **Editable en Canva, siempre.** El entregable es un diseño de Canva abierto, no una imagen cerrada. Es un requisito del cliente y se respeta.
3. **El manager no cambia de herramienta.** Sigue mandando un correo. Toda la complejidad vive del lado de la agencia.
4. **Fases que entregan valor solas.** Cada fase debe ahorrar tiempo aunque las siguientes nunca se construyan. Nada de "cuando esté todo, funciona".
5. **Costo operativo por debajo de USD 100/mes.** El sistema debe seguir siendo barato de tener, no solo de construir.
6. **Cada corrección enseña.** Lo que la operadora corrige alimenta el glosario y el catálogo de imágenes: el sistema mejora solo, sin reentrenar nada.
7. **Sin dependencias que no se puedan reemplazar.** Cada integración externa vive detrás de un adaptador propio.

## 3. Arquitectura

```
┌──────────────────────────────────────────────────────────────────────┐
│  ENTRADA                                                             │
│  Gmail (etiqueta "ofertas") ──▶ n8n trigger                          │
│  + recordatorio automático a managers (mar/jue 08:00)                │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  EXTRACCIÓN                                                          │
│  Excel → imagen · PDF nativo · Foto                                  │
│         └──▶ Claude Sonnet 5 (visión + structured outputs)           │
│              └──▶ JSON canónico + confianza por campo                │
│  Validación determinista: precios, unidades, categorías, rangos      │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  ENRIQUECIMIENTO                                                     │
│  ① Traducción EN⇄ES con glosario propio (controlado, no libre)       │
│  ② Matching producto → imagen (embeddings sobre catálogo indexado)   │
│  ③ Reordenamiento: Meat → Produce → Grocery → Bakery                 │
│  ④ Detección de holiday → variante de fondo                          │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  ▓▓ REVISIÓN HUMANA ▓▓  ← el corazón del sistema                     │
│  Tablero web: producto | EN | ES | precio | unidad | 3 imágenes      │
│  Solo se resalta lo de baja confianza. Objetivo: < 5 min.            │
│  Cada corrección → glosario + catálogo (aprendizaje)                 │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  GENERACIÓN EN CANVA                                                 │
│  Copiar plantilla de la tienda → editing transaction →               │
│  reemplazar 16 textos + 16 image fills + fecha → commit              │
│  ▶ Salida: link de Canva EDITABLE                                    │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  ENTREGA Y APROBACIÓN                                                │
│  Export + recorte automático → email al manager (plantilla fija)     │
│  Respuesta del manager → clasificación (aprobado / feedback)         │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  PUBLICACIÓN + ANUNCIO   ⏰ 21:00                                    │
│  1. Publicar post en la página (Graph API)                           │
│  2. Duplicar ad set con fechas y lifetime budget                     │
│  3. Crear ad con object_story_id → a revisión de Meta ESA NOCHE      │
│  ▶ +12 h de vigencia efectiva                                        │
└────────────────────────────┬─────────────────────────────────────────┘
                             ▼
┌──────────────────────────────────────────────────────────────────────┐
│  ESTADO Y REPORTES                                                   │
│  Tablero único de estados ⇄ Slack (mensaje que se edita) ⇄ Asana     │
│  Reporte mensual automático: gasto, costo/interacción, por tienda,   │
│  comparativa por horario de lanzamiento → PDF al cliente             │
└──────────────────────────────────────────────────────────────────────┘
```

## 4. Los cuatro componentes que hacen la diferencia

### 4.1 El glosario controlado (elimina el dolor #4)

El feedback más frecuente del manager son **faltas de ortografía**. La causa no es descuido del equipo: es que se transcriben a mano nombres en dos idiomas, 16 veces por flyer, 60 veces al mes — casi mil transcripciones mensuales.

La solución **no es pedirle al modelo que traduzca bien**. Es un **diccionario propio, versionado, de términos de supermercado latino**:

```jsonc
{
  "pierna y muslo de pollo marinada": {
    "en": "Marinated Split Chicken Leg Quarters",
    "categoria": "meat",
    "alias": ["pierna de pollo marinada", "cuarto de pollo marinado"],
    "imagenes": ["img_0421", "img_0788"]
  },
  "semita hondureña": {
    "en": "Honduran Semita",
    "categoria": "bakery",
    "alias": ["semita", "semita hondurena"]
  }
}
```

- Si el término está en el glosario → traducción **determinista**, imposible que falle.
- Si no está → el modelo propone, la operadora confirma, **y se guarda en el glosario**.
- A los 2–3 meses el glosario cubre la enorme mayoría del catálogo recurrente, porque el supermercado repite productos.

**Este componente por sí solo justifica la Fase 1.** Es barato de construir y ataca directamente el ciclo de correcciones.

### 4.2 El matching de imágenes

El paso que más tiempo consume hoy ("yo aquí tengo que buscar y si no me lo encuentro, a veces el manager me envía la foto o la busco por Google").

```
Catálogo indexado (una vez):
  cada imagen → { nombre_es, nombre_en, categoría, alias[], licencia,
                  origen, fecha, url_cdn, embedding }

En cada flyer:
  producto → embedding → búsqueda por similitud dentro de la categoría
          → top-3 candidatos con score
          → si score₁ > umbral: se preselecciona
          → si no: hueco marcado, la operadora elige o sube una nueva
```

Restringir la búsqueda **dentro de la categoría** (no buscar "pierna" en Bakery) sube muchísimo la precisión, y el catálogo es pequeño (unos cientos de imágenes), así que la búsqueda es instantánea y prácticamente gratis.

Cada selección manual refuerza el índice: el sistema aprende que "adobada" se resuelve con `img_0932`.

### 4.3 El adelanto del anuncio a las 21:00

El detalle técnico completo está en el [documento 03 §3](./03-viabilidad-tecnica.md). En términos de negocio:

| | Hoy | Con Flymar |
|---|---|---|
| Post publicado | 21:00 (programado en Meta) | 21:00 (publicado vía API) |
| Anuncio creado | ~09:00 del día siguiente | **21:00, misma noche** |
| Revisión de Meta | Ocurre durante la vigencia | Ocurre mientras nadie compra |
| Vigencia efectiva (oferta 48 h) | ~36 h | **~48 h** |
| Trabajo manual en Ads Manager | 9 pasos × 60 campañas/mes | **0** |

Se eliminan además los 9 pasos manuales que Ana Marcela describe uno por uno en la reunión — que no solo consumen tiempo, sino que son la fuente típica de errores (olvidar quitar el botón de Messenger, poner mal la fecha de fin, presupuesto equivocado).

### 4.4 El tablero de estados (reemplaza el copiar/pegar de Slack)

Hoy el estado de las 7 tiendas vive en un mensaje de Slack que se reescribe a mano varias veces al día. La propuesta: **una sola fuente de verdad**, y Slack como espejo.

```
recibido → extraído → revisado → generado → enviado →
  ├─ con feedback → corregido → enviado (ciclo)
  └─ aprobado → programado → publicado → anunciado ✓
```

- El tablero se actualiza solo con cada evento del pipeline.
- Un **único mensaje de Slack por día se edita** (no se reenvía) con el estado de las 7 tiendas — el bot lo mantiene.
- Asana sigue recibiendo el "publicado" al final del día, sin cambiar el hábito del equipo.

## 5. Stack técnico recomendado

| Capa | Elección | Por qué |
|---|---|---|
| Orquestación | **n8n self-hosted** (Docker, VPS) | Cobra por ejecución, no por operación: el costo no escala con las llamadas a IA. Community edition gratis. Nodos de Gmail, cron, HTTP, y AI Agent nativos. |
| Servicio propio | **Python + FastAPI** (o Node/TypeScript) | Extracción, matching por embeddings, adaptadores de Canva y Meta. La lógica delicada no vive en cajitas de n8n. |
| IA | **Claude Sonnet 5** (extracción), **Haiku 4.5** (clasificación de correos) | Structured outputs, visión nativa para PDF/imagen, costo < USD 5/mes total. |
| Base de datos | **PostgreSQL** (Supabase) | Estados, glosario, catálogo de imágenes, histórico. `pgvector` para los embeddings, sin servicio extra. |
| Almacenamiento de imágenes | **Cloudflare R2** o S3 + CDN | Canva exige URLs públicas con CORS para subir assets. |
| Tablero de revisión | **Next.js** (o Streamlit para el MVP) | Streamlit si se prioriza velocidad; Next.js si va a ser producto duradero. |
| Diseño | **Canva Connect API** (Design Editing) | Única vía compatible con Canva Pro. |
| Publicidad | **Meta Marketing API** + Meta Ads MCP | API para el pipeline; MCP para consultas y ajustes asistidos. |
| Operación asistida | **Canva MCP + Meta Ads MCP** | Para preguntar "¿cuánto gastó Blue Ridge este mes?" sin construir UI. |

## 6. Plan por fases

Cada fase entrega valor por sí sola. Si el proyecto se detiene en cualquier punto, lo construido sigue sirviendo.

### Fase 0 · Fundaciones (semanas 1–2) — *casi sin código*

| Entregable | Detalle |
|---|---|
| Plantilla de correo estandarizada | Instrucciones para managers: formato, bilingüismo, ejemplos. Reduce la variabilidad en origen. |
| Plantilla de hoja de ofertas | Un Excel/Sheet que los managers rellenan. No obligatorio (el sistema tolera los 3 formatos), pero baja los errores. |
| **Auditoría de la biblioteca de imágenes** | ⚠️ **Prioridad crítica.** Clasificar cada imagen: propia / licenciada / origen desconocido. Retirar y reemplazar las de origen dudoso. Ya hay demanda contra otra agencia. |
| Catalogación con metadatos | Nombre ES/EN, categoría, alias, licencia, origen, fecha, autor. Es el insumo del matching. |
| Medición de línea base | Cronometrar 10 flyers reales. Sin esto, no hay forma de demostrar el ahorro. |
| **Spike técnico de Canva** (2 días) | Probar Design Editing API contra **una plantilla real del cliente**. Criterio de aceptación: reemplazar 3 textos y 1 imagen, y que el diseño siga siendo editable a mano. Decide si sigue el Plan A, B o C. |
| Configuración de accesos Meta | System User en el Business Portfolio, token de larga duración, permisos. |

**Valor entregado sin escribir una línea:** menos errores en origen, riesgo legal mitigado, y datos para medir todo lo demás.

### Fase 1 · Extracción, traducción y revisión (semanas 3–6)

- Ingesta desde Gmail con clasificación automática (tienda, fecha, tipo de oferta).
- Extracción multi-formato con Claude + structured outputs.
- Validación determinista de precios, unidades y categorías.
- Glosario EN⇄ES con carga inicial de términos del histórico.
- Matching de imágenes con embeddings sobre el catálogo de Fase 0.
- **Tablero de revisión** con confianza por campo.
- Salida: JSON/CSV validado + hoja lista para pegar.

> **Ahorro estimado en esta fase: de ~45 min a ~10–15 min por flyer**, todavía pegando en Canva a mano. Ya es la mayor parte del beneficio.

### Fase 2 · Generación automática en Canva (semanas 7–10)

- Adaptador de Canva Connect API con editing transactions.
- Mapeo de plantilla por tienda (qué elemento corresponde a qué campo).
- Subida de imágenes desde el CDN a Canva.
- Manejo de variantes: holiday, ofertas de 2 vs. 4 días, tienda solo-inglés.
- Salida: **link de Canva editable** listo para revisar.
- *(Opcional 2b: app privada de Canva para llenado desde el panel lateral.)*

> **Ahorro acumulado: de ~45 min a < 5 min por flyer.**

### Fase 3 · Publicación y anuncios en Meta (semanas 11–13)

- Export automático + recorte del borde + envío al manager con plantilla de correo.
- Clasificación de la respuesta del manager (aprobado / feedback / cambios puntuales).
- Scheduler de las 21:00: publicar post → duplicar ad set → crear ad con `object_story_id`.
- Registro completo de cada campaña creada (para el reporte).
- Alertas si algo falla (anuncio rechazado, post no publicado).

> **Ganancia: +25 % de vigencia efectiva por campaña y 0 trabajo manual en Ads Manager.**

### Fase 4 · Reportes, estados y recordatorios (semanas 14–16)

- Tablero de estados unificado + espejo en Slack + sincronización con Asana.
- Recordatorios automáticos a managers los martes y jueves a las 08:00.
- Reporte mensual automático: gasto, impresiones, interacciones, costo por resultado, por tienda y por periodo.
- **Test A/B de horario de lanzamiento** para documentar con datos la hipótesis de las 21:00.
- Generación de PDF y envío al cliente final.

### Fase 5 · Transferencia (semana 17) — *sesiones de capacitación*

Ana Marcela pidió explícitamente aprender a construirlo. Esto no es un extra: es parte del entregable y es lo que hace que el mantenimiento sea barato.

- Cómo funciona cada pieza y dónde tocar qué.
- Cómo agregar una tienda nueva (plantilla + mapeo).
- Cómo mantener el glosario y el catálogo de imágenes.
- Cómo leer los logs y qué hacer cuando algo falla.
- Documentación de operación.

## 7. Costos

### 7.1 Operación mensual

| Concepto | Costo |
|---|---|
| VPS para n8n + servicio (2 vCPU / 4 GB) | USD 12–24 |
| PostgreSQL gestionado (Supabase) | USD 0–25 |
| Almacenamiento + CDN de imágenes (R2/S3) | USD 1–5 |
| API de Claude (extracción, ~60 flyers) | USD 3–5 |
| Dominio y correo transaccional | USD 2–5 |
| Canva Pro | *ya lo tiene* |
| **Total** | **USD 18–64 / mes** |

Comparativa con las propuestas rechazadas:

| Propuesta | Inicial | Mensual | 3 años |
|---|---|---|---|
| Proveedor A | USD 5,000 | USD 500 | **USD 23,000** |
| Proveedor B | USD 15,000 | USD 1,500–2,000 | **USD 69,000–87,000** |
| **Flymar (operación)** | *desarrollo aparte* | **USD 18–64** | **USD 650–2,300** |

La diferencia no es negociación de precio: es que **el costo variable real de este sistema es de decenas de dólares al mes**, y las propuestas anteriores cobraban mantenimiento sobre un sistema que, como bien dijo Ana Marcela, *"una vez armado, es siempre lo mismo"*.

### 7.2 Esfuerzo de desarrollo

Estimación en horas, para que el precio final lo fije quien ejecute:

| Fase | Horas | Notas |
|---|---|---|
| Fase 0 · Fundaciones + spike | 12–18 | Mayormente proceso y auditoría, poco código |
| Fase 1 · Extracción + revisión | 45–65 | El grueso del valor |
| Fase 2 · Canva | 30–50 | Rango amplio: depende del resultado del spike |
| Fase 3 · Meta | 25–40 | |
| Fase 4 · Reportes y estados | 20–30 | |
| Fase 5 · Capacitación | 8–12 | |
| **Total** | **140–215 h** | |

**Recomendación de contratación:** cerrar **Fase 0 + Fase 1 como primer contrato**. Entrega el 60–70 % del ahorro de tiempo, valida la extracción con datos reales, y deja la decisión sobre Canva informada por el spike en lugar de por una promesa.

## 8. Indicadores de éxito

| KPI | Línea base | Meta mes 3 | Meta mes 6 |
|---|---|---|---|
| Minutos de trabajo humano por flyer | ~45–60 | < 15 | **< 5** |
| Flyers aprobados sin feedback de ortografía/precio | por medir | > 85 % | **> 95 %** |
| Productos con imagen asignada automáticamente | 0 % | > 70 % | **> 85 %** |
| Vigencia efectiva por campaña de 48 h | ~36 h | ~48 h | ~48 h |
| Trabajo manual en Ads Manager | 9 pasos × 60/mes | — | **0** |
| Costo operativo mensual | USD 0 (todo es tiempo) | < USD 65 | < USD 65 |
| Biblioteca con licencia documentada | por medir | > 60 % | **100 %** |
| Reporte mensual al cliente | manual / inexistente | automático | automático |

## 9. Lo que esta propuesta NO promete

Por transparencia, y porque las propuestas anteriores probablemente fallaron aquí:

1. **No promete cero intervención humana.** Los benchmarks de extracción estructurada no lo sostienen, y el propio cliente pidió revisar antes de enviar. El objetivo es 5 minutos, no cero.
2. **No promete que la Autofill API de Canva funcione.** No funciona con Canva Pro — está verificado. Lo que se propone es la vía de edición, que sí está disponible, con dos planes de contingencia documentados.
3. **No promete medir conversiones en tienda física.** Sin píxel ni compras online, eso no existe. Si más adelante interesa, la vía es un *proxy* (cupón con código, QR, pregunta en caja) — es un proyecto aparte.
4. **No promete que el costo por interacción baje solo por publicar a las 21:00.** Es una hipótesis con evidencia anecdótica del propio cliente. La Fase 4 la convierte en dato con un test A/B; hasta entonces, se comunica como hipótesis.
5. **No promete resolver la auditoría legal de las imágenes.** Eso requiere una decisión del negocio (retirar imágenes, producir reemplazos, y posiblemente asesoría legal). El sistema aporta el inventario y el control; la decisión es de la agencia.

## 10. Primeros pasos concretos

**Esta semana:**
1. Confirmar el alcance de la Fase 0 + Fase 1.
2. Acceso de lectura a la carpeta de Canva para inventariar la biblioteca.
3. Compartir 5–10 correos reales de managers (con los tres formatos) para calibrar el extractor.
4. Compartir una plantilla de Canva real de una tienda para el spike técnico.
5. Cronometrar 10 flyers reales → línea base.

**Semana 2:**
6. Spike de Canva Design Editing API → decisión Plan A / B / C.
7. Configurar System User de Meta con permisos.
8. Entregar la plantilla de correo estandarizada a los managers.

---

## Anexo · Decisiones clave y su justificación

| Decisión | Alternativa descartada | Razón |
|---|---|---|
| Design Editing API | Autofill API | Autofill requiere Canva Enterprise (USD 2K–30K/año). Verificado en la doc oficial. |
| n8n self-hosted | Make / Zapier | Cobro por ejecución vs. por operación: con capas de IA, Make y Zapier se disparan. |
| API directa para el pipeline | MCP para todo | MCP es interactivo; un pipeline nocturno necesita idempotencia, reintentos y logs. MCP sí para operación asistida. |
| Claude Sonnet 5 | Opus 5 / Haiku 4.5 | Mejor equilibrio para dominio acotado. Escalar a Opus solo en documentos dudosos. |
| Excel → imagen → visión | Parser de celdas | El Excel real es una maqueta visual con celdas fusionadas; el parser tabular falla y es frágil ante cambios del manager. |
| Glosario determinista | Traducción libre por LLM | El dolor #4 son faltas de ortografía. Un diccionario controlado no falla; un modelo puede variar. |
| Email (sin portal para managers) | Portal de autoservicio | Requisito explícito del cliente, con buen argumento: los managers rotan y costó mucho conseguir el correo ordenado. |
| Fases con valor independiente | Entrega big-bang | Riesgo de proyecto. Si se detiene en Fase 1, ya ahorró el 60–70 % del tiempo. |

---

← [03 · Viabilidad técnica](./03-viabilidad-tecnica.md) · [Índice](../README.md)
