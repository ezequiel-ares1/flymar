# 06 · Propuesta V2 — Flymar Platform

**Una plataforma de *content supply chain* para retail independiente, construida por una persona dirigiendo agentes de IA.**

> Este documento **ignora deliberadamente las restricciones** que planteó Ana Marcela (Canva Pro, nada de portal para managers, presupuesto mínimo, mantenimiento cero). No las contradice por descuido: la [propuesta V1](./04-propuesta-de-solucion.md) sigue siendo la respuesta correcta *a lo que ella pidió*. Esta V2 responde a otra pregunta: **si partimos solo de los dolores y no de las restricciones, ¿cuál es la solución que además se convierte en un negocio?**
>
> Al final hay una sección honesta sobre cuándo esto es una buena idea y cuándo es sobre-ingeniería.

---

## 1. El replanteo: el flyer no es el producto

Todo el análisis anterior —y las dos propuestas de terceros— asume que el trabajo es *producir un flyer*. Ese encuadre es el que limita el techo de la solución.

Mira los dolores otra vez, sin el flyer en el centro:

| Lo que duele | Lo que realmente falta |
|---|---|
| Transcribir productos de un Excel maquetado | **No existe un catálogo estructurado.** Cada semana los datos se leen, se usan y se tiran. |
| Traducir 16 nombres, dos veces por semana, 60 veces al mes | **No existe un diccionario de producto.** Se retraduce lo mismo indefinidamente. |
| Buscar la foto de cada producto | **No existe un DAM.** Hay carpetas de Canva sin metadatos ni licencia. |
| Maquetar en Canva | **No existe una plantilla como dato.** El diseño vive como archivo, no como estructura. |
| Perder 12 h esperando la aprobación de Meta | **No existe orquestación.** Cada campaña se arma a mano, paso a paso. |
| No poder demostrar que la inversión funciona | **No existe medición.** Ni del contenido, ni del canal. |

Ninguno de esos vacíos es un problema de diseño gráfico. Son un problema de **datos**.

Y cuando existe el dato, el flyer estático deja de ser el entregable y pasa a ser **el primer render de muchos**:

```
                        ┌─────────────────────┐
                        │    OFFER GRAPH      │
   Excel / PDF ────────▶│  producto · precio  │────┬──▶ Flyer 4:5 (Facebook)
   Foto / WhatsApp      │  tienda · vigencia  │    ├──▶ Story 9:16
   API del POS          │  imagen · idioma    │    ├──▶ Carrusel de 5 tarjetas
                        │  marca (CPG)        │    ├──▶ Video 6s por producto
                        └─────────────────────┘    ├──▶ Circular digital interactiva
                              un solo dato          ├──▶ Catálogo de Meta (feed)
                                                    ├──▶ WhatsApp Business
                                                    ├──▶ Pantalla in-store
                                                    └──▶ Impresión / print
```

Ese es exactamente el marco que la industria llama **content supply chain**, y es lo que Adobe vende como GenStudio a empresas: *"una cadena de suministro de contenido agéntica que conecta contexto empresarial, inteligencia de marca y agentes de IA a través de planificación, creación, activación, entrega y reporte"*. Un cliente suyo reporta **35× más contenido, 65 % más rápido**.

**La tesis de la V2:** construir eso mismo, con alcance quirúrgico, para el nicho que Adobe y WPP nunca van a atender — agencias que sirven a supermercados independientes.

## 2. Por qué esto puede ser un negocio y no solo una herramienta

Tres datos de la investigación que, juntos, cambian la conversación:

**① El retail media es la categoría de más rápido crecimiento en publicidad digital.** De ~USD 45–50 mil millones en 2023 a **superar los USD 100 mil millones en 2026** en EE. UU.

**② El circular tradicional rinde mal comparado con el retail media.** Cuando un retailer mide, encuentra que *"retail media entregó USD 3.50 en ventas por cada dólar gastado, mientras el anuncio circular entregó USD 1.20"*.

**③ Los supermercados independientes están fuera de la fiesta.** No tienen la plataforma de Walmart o Kroger, así que **pierden los fondos de trade marketing de los CPG** — pero controlan volumen real en muchas categorías y enfrentan mucha menos competencia por la atención del comprador. Los márgenes brutos de una red de retail media para un grocer independiente se estiman en **70–90 %**.

Ahora mira los anexos reales de Fresco Marketing con esos ojos:

> `SALSA VALENTINA` · `BIMBO HOT DOG BUNS` · `MALTIN POLAR` · `KELLOGGS CORN FLAKES` · `PEPSI 12PK` · `MAC'S PORK CRACKLINS` · `JENIN EXTRA VIRGIN OLIVE OIL` · `TORTILLAS EL MILAGRO`

**Cada flyer ya es inventario publicitario. Hoy se regala.**

Fresco Marketing produce ~60 piezas al mes, en 7–10 tiendas, dirigidas a un público hispano en EE. UU. que esas marcas quieren alcanzar y que casi ningún retail media network segmenta bien. Lo único que falta para monetizarlo es **medir** — saber cuántas impresiones e interacciones recibió el bloque de `SALSA VALENTINA` y poder facturárselo a la marca.

Eso convierte el modelo de negocio de **cobrar honorarios por armar flyers** a **cobrar un porcentaje de la inversión de los CPG**, que es una curva de ingresos completamente distinta.

## 3. Arquitectura

```
╔══════════════════════════════════════════════════════════════════════════╗
║  ① CAPTURA — la entrada deja de ser un problema                          ║
║  Email · WhatsApp Business · Portal opcional · API del POS · Foto        ║
║  Agente de ingesta: identifica tienda, vigencia, formato e intención     ║
╚═══════════════════════════════╤══════════════════════════════════════════╝
                                ▼
╔══════════════════════════════════════════════════════════════════════════╗
║  ② OFFER GRAPH — el activo real, y el foso defensivo                     ║
║                                                                          ║
║   Producto canónico ─── GTIN/UPC/PLU · nombre ES/EN · categoría          ║
║        │                marca CPG · alias · embedding                    ║
║        ├── Oferta ───── precio · unidad · sintaxis · vigencia · tienda   ║
║        ├── Activo ───── imagen · licencia · origen · derechos · hash     ║
║        └── Histórico ── qué se ofertó, cuándo, a qué precio, con qué     ║
║                         resultado                                        ║
║                                                                          ║
║   Postgres + pgvector · multi-tenant con Row-Level Security              ║
╚═══════════════════════════════╤══════════════════════════════════════════╝
                                ▼
╔══════════════════════════════════════════════════════════════════════════╗
║  ③ MOTOR DE COMPOSICIÓN — "el mini Canva", pero de dominio específico    ║
║                                                                          ║
║   Plantilla = JSON versionado (no un archivo)                            ║
║   ├─ Render headless   → Satori + resvg · 50–200 ms · corre en el edge   ║
║   └─ Editor web propio → ajuste fino, arrastrar, swap de imagen          ║
╚═══════════════════════════════╤══════════════════════════════════════════╝
                                ▼
╔══════════════════════════════════════════════════════════════════════════╗
║  ④ FAN-OUT — un dato, N formatos, generados en paralelo                  ║
║  Flyer · Story · Carrusel · Video 6s · Circular interactiva · Feed       ║
╚═══════════════════════════════╤══════════════════════════════════════════╝
                                ▼
╔══════════════════════════════════════════════════════════════════════════╗
║  ⑤ ACTIVACIÓN — publicación y pauta como código                          ║
║  Meta: post + ad set + DCO con 10–15 variantes · WhatsApp · TikTok       ║
║  Scheduler propio: el anuncio entra a revisión a las 21:00               ║
╚═══════════════════════════════╤══════════════════════════════════════════╝
                                ▼
╔══════════════════════════════════════════════════════════════════════════╗
║  ⑥ MEDICIÓN — lo que nadie más tiene en este nicho                       ║
║  Por producto y por marca · QR/cupón para atribución offline             ║
║  Dashboard para la tienda · Dashboard para el CPG  ← aquí está el dinero ║
╚══════════════════════════════════════════════════════════════════════════╝

        ⑦ CAPA DE AGENTES — atraviesa todas las anteriores
        Ingesta · Catálogo · Creativo · Campaña · Reporte
        Con herramientas acotadas, políticas de aprobación y evals en CI
```

## 4. Las cinco decisiones que definen la plataforma

### 4.1 El Offer Graph es el producto, no el flyer

Todo lo demás es reemplazable. El catálogo canónico —con nombres bilingües curados, imágenes con licencia trazada, alias que la operación fue corrigiendo, histórico de precios por tienda— **no lo tiene nadie más y no se puede comprar**. Se acumula operando.

A los seis meses ese grafo permite cosas que hoy son imposibles: detectar que una tienda ofertó aguacate a USD 0.99 hace tres semanas y ahora a 1.49, avisar que `MANGO ATAULFO` sale en cuatro tiendas la misma semana, o proponer la oferta de la semana siguiente a partir de lo que funcionó.

**Es también la razón por la que este proyecto se defiende de que Canva o Meta lancen algo parecido:** ellos tienen el editor y el canal, pero no el catálogo del supermercado latino.

### 4.2 El "mini Canva": construir el 5 % que importa

La tentación es construir Canva. Es la trampa clásica de este tipo de proyectos.

Un flyer de ofertas no necesita 1,000 tipografías, filtros, animaciones ni colaboración en tiempo real. Necesita **seis primitivas**:

```
1. Rejilla de productos      (3×4, 4×4, 3×3 según plantilla)
2. Tarjeta de producto       (imagen + nombre ES + nombre EN + precio + unidad)
3. Chip de precio            (las 5 sintaxis: 0.99 · $3.49/LB · 2/$7.00 · 2X1.00)
4. Banda de categoría        (MEAT · PRODUCE · GROCERY · BAKERY)
5. Encabezado                (logo tienda + vigencia + dirección)
6. Zona de temporada         (fondo y franja intercambiables por festividad)
```

Eso es un editor de dominio, no un editor de diseño. Y tiene una propiedad valiosa: **como las primitivas son semánticas, la IA puede operarlas.** "Pon el melón donde está el cilantro" es un intercambio de dos nodos en un JSON, no una manipulación de píxeles.

**Opciones de implementación, evaluadas:**

| Opción | Costo | A favor | En contra |
|---|---|---|---|
| **Fabric.js** | Gratis (MIT) | El estándar de facto para editores de diseño en 2026; sin licencia; control total | Construyes toolbar, capas, exportación y flujos desde cero |
| **Konva** | Gratis | Excelente para UI interactiva | Más bajo nivel: todo el editor es trabajo tuyo |
| **Polotno SDK** | **USD 899** self-serve | Trae el editor completo, sistema de plantillas, API programática y renderer. **Todo elemento se expone en JSON** → hecho para automatización. Construido sobre Konva por el mismo autor | Licencia comercial; acopla al modelo de Polotno; enterprise "low five figures" |

**Recomendación: Polotno para el editor del MVP, con el modelo de plantilla propio.** Los USD 899 compran meses de trabajo, y su enfoque JSON-first encaja exactamente con la automatización. La condición es no dejar que el formato de Polotno *sea* tu formato: la plantilla canónica vive en tu esquema y se traduce a Polotno para editar y a Satori para renderizar. Así el renderer es reemplazable.

### 4.3 Render headless: 50–200 ms y corre en el edge

Para producción masiva no se abre un editor. **Satori + resvg** convierte JSX/HTML a SVG y luego a PNG en **50–200 ms**, con una dependencia de ~10 MB, y funciona en Cloudflare Workers, Vercel Edge y Lambda@Edge.

La comparación con la alternativa obvia es contundente: Puppeteer con Chrome headless implica levantar un navegador entero por render — mucha complejidad y sobrecarga.

**La limitación de Satori es que solo soporta Flexbox** (sin Grid, sin z-index, sin JS). Para un flyer de ofertas —rejilla de tarjetas, bandas de categoría, encabezado— Flexbox sobra. Si una plantilla llegara a necesitar más, ese caso concreto cae a un renderer Chromium.

**Consecuencia práctica:** 60 flyers × 8 formatos = 480 renders. A 150 ms cada uno, son **72 segundos de cómputo al mes**. El costo de render es esencialmente cero.

### 4.4 Agentes de verdad, no un chatbot

Cinco agentes con herramientas acotadas, no un asistente conversacional:

| Agente | Herramientas | Autonomía |
|---|---|---|
| **Ingesta** | Leer correo, clasificar adjunto, extraer con visión, validar esquema | Autónomo · escala a humano bajo umbral de confianza |
| **Catálogo** | Buscar producto canónico, proponer traducción, sugerir imagen, crear alias | Autónomo para coincidencias fuertes · propone el resto |
| **Creativo** | Componer plantilla, generar N variantes, verificar contraste y desbordes | Autónomo hasta el borrador · **nunca publica** |
| **Campaña** | Crear post, duplicar ad set, generar ad, ajustar presupuesto | **Requiere aprobación** para gasto |
| **Reporte** | Consultar Insights, detectar anomalías, redactar el informe | Autónomo |

Tres reglas que separan esto de una demo:

1. **Política de aprobación por acción, no por agente.** Gastar dinero y enviar al cliente siempre pasan por un humano; extraer y componer no.
2. **Evals en CI.** Un dataset de 50 archivos reales con su JSON esperado corre en cada cambio. Si la precisión baja del 95 %, el build falla. Sin esto, nadie sabe si el sistema empeoró.
3. **Cada corrección humana es dato de entrenamiento del catálogo**, no un parche perdido.

Esto se alinea con hacia dónde va la industria: el rol del anunciante pasa de *"el equipo de performance elige los assets a mano"* a *"la arquitectura de datos determina qué assets se producen"*.

### 4.5 DCO: producir 15 variantes en vez de 1

Meta recomienda **mínimo 10 variantes creativas por ad set**; por debajo de 10 el algoritmo se queda sin señal, y entre **15 y 30 aprende más rápido**.

Hoy Fresco produce **una** pieza por tienda. Con el Offer Graph y el motor de render, producir 15 variantes —distinto producto héroe, distinto orden, distinto encabezado— cuesta **segundos de cómputo**.

La economía ya cambió: generar 30 variantes de video para una campaña DCO cuesta **USD 50–300**, frente a **USD 5,000–30,000** por producción tradicional.

**Este es probablemente el mayor salto de rendimiento de toda la propuesta, y no requiere construir nada nuevo** una vez que existen las capas ② y ③.

## 5. El stack — elegido para que lo construya una persona con agentes

Aquí el criterio de selección no es "qué es mejor en abstracto" sino **"con qué se equivocan menos los agentes de IA y qué les permite auto-corregirse"**. Los benchmarks independientes reportan un **67 % de acierto en refactors que cruzan archivos en monorepos TypeScript cuando hay especificaciones y tests e2e como bucle de retroalimentación** — la clave no es el modelo, es el bucle.

| Capa | Elección | Por qué, en clave de "lo construyen agentes" |
|---|---|---|
| **Lenguaje** | **TypeScript de punta a punta** | Un solo lenguaje en backend, render y editor. El tipo `Offer` es literalmente el mismo objeto en las tres capas: un agente que lo rompe lo descubre en el typecheck, no en producción. |
| **Contratos** | **Zod → tipos → validación runtime** | El esquema es la fuente de verdad. Genera tipos y valida en los bordes. Un agente que inventa un campo falla en 10 segundos. |
| **Repo** | **Monorepo (pnpm + Turborepo)** | Límites de módulo explícitos → varios agentes trabajando en paralelo sin pisarse, uno por paquete. |
| **App** | **Next.js** (App Router) | Server components, rutas API y el editor en el mismo proyecto. Enorme masa de entrenamiento: es donde los agentes cometen menos errores. |
| **Base de datos** | **Postgres + pgvector**, en **Neon** | Un motor para relacional y vectorial. Neon escala a cero y su *branching* hace que **cada rama de agente tenga su propia base** — es lo que vuelve seguro dejar a un agente correr migraciones. |
| **Multi-tenant** | **Row-Level Security en Postgres** | El aislamiento vive en la base, no en el código de aplicación. Un agente que olvide un `WHERE tenant_id` no puede filtrar datos entre clientes. Es una red de seguridad estructural. |
| **Cómputo y colas** | **Cloudflare Workers + Queues + R2** | ~0 ms de arranque en frío, **egreso gratis** (1 TB de salida cuesta USD 0 en R2 frente a ~USD 90 en S3 — y esto es un producto que sirve imágenes). Stack completo típico: **USD 15–50/mes**. |
| **Render** | **Satori + resvg** | Corre en el mismo Worker. 50–200 ms. |
| **Editor** | **Polotno SDK** (o Fabric.js) | Ver §4.2 |
| **IA** | **Claude Sonnet 5** extracción · **Opus 5** casos difíciles · **Haiku 4.5** clasificación | Structured outputs, visión nativa, caching. |
| **Orquestación** | **n8n** para integraciones · **código** para el pipeline crítico | n8n donde la lógica es pegamento; código donde hay que testear. |

### Los cuatro bucles de retroalimentación

Esto es lo que hace que una persona sola pueda sostener el proyecto. Sin ellos, dirigir agentes es escribir código a ciegas:

```
1. TYPECHECK      tsc --noEmit          segundos    ← contratos rotos
2. TESTS          vitest                segundos    ← lógica rota
3. GOLDEN FILES   render → diff píxel   ~1 min      ← layout roto
4. EVALS DE IA    50 casos reales       ~2 min      ← extracción degradada
```

El bucle **3** merece atención especial: cada plantilla tiene un conjunto de renders de referencia. Si un agente toca el motor de composición y el flyer se desalinea tres píxeles, el diff de imagen lo detecta. **Es la única forma de dejar que un agente toque código de layout sin revisarlo visualmente cada vez.**

El bucle **4** es el que casi nadie implementa y el que separa un sistema de IA serio de una demo: sin evals, no sabes si el cambio de ayer mejoró o empeoró la extracción.

## 6. Hoja de ruta

Semanas, no horas de desarrollador — porque el cuello de botella pasa a ser tu criterio y tu revisión, no tu tecleo.

### Etapa I · Núcleo (semanas 1–6) — *herramienta interna*

Offer Graph, ingesta multiformato con evals, catálogo con embeddings, glosario, DAM con licencias, render headless y tablero de revisión. Multi-tenant desde el primer commit (RLS), aunque el único inquilino sea Fresco.

**Al final de la etapa:** flyer generado en menos de 5 minutos de trabajo humano. Esto **ya es la V1 completa**, con mejor arquitectura debajo.

### Etapa II · Composición y activación (semanas 7–12)

Editor visual sobre el modelo de plantilla propio. Fan-out a 4 formatos. Publicación y anuncios en Meta a las 21:00. **DCO: 15 variantes por campaña** en vez de una.

**Al final:** el retorno de Meta debería moverse de forma medible. Es el primer resultado defendible ante el cliente final.

### Etapa III · Medición (semanas 13–18)

Atribución por producto. QR y cupón por oferta para medir impacto en tienda física. Dashboard para la tienda. **Primer informe de rendimiento por marca CPG** — el artefacto que abre la conversación de retail media.

### Etapa IV · Producto (semanas 19–26)

Onboarding autoservicio, facturación, panel de agencia. **Segundo cliente: otra agencia del mismo nicho.** Es la validación real de que esto es producto y no una herramienta a medida.

### Etapa V · Retail media (mes 7 en adelante)

Inventario publicitario dentro de la circular, tarifario por marca, dashboard para el CPG. Es donde el modelo de ingresos cambia de naturaleza.

**Cada etapa se sostiene sola.** Si el proyecto se detiene en la I, Fresco tiene la mejor herramienta interna de su categoría. Si llega a la V, es una empresa distinta.

## 7. Costos

### Infraestructura

| Etapa | Componentes | USD/mes |
|---|---|---|
| I — un inquilino | Workers + R2 + Neon (escala a cero) + IA | **20 – 60** |
| II–III — producción | + Queues, más render, más almacenamiento | **60 – 150** |
| IV — multi-cliente | + Workers for Platforms (USD 25), Neon con ramas | **150 – 400** |
| V — retail media | + analítica, retención de datos | **400 – 900** |

Referencia: un stack completo de Cloudflare (Workers + KV + D1 + R2) para los primeros 100,000 usuarios cuesta **USD 20–100/mes**, y el egreso gratuito de R2 importa mucho en un producto que sirve imágenes.

### Licencias

| Concepto | Costo |
|---|---|
| Polotno SDK self-serve | **USD 899** una vez (opcional; Fabric.js es gratis) |
| Dominio, correo transaccional, monitoreo | ~USD 30/mes |

### Lo que no aparece en la tabla

No hay línea de "desarrollador". Ese es el cambio estructural de esta propuesta: **la construcción se dirige con agentes de IA**, y el consumo de tokens a este volumen es ruido frente a cualquier nómina.

## 8. Riesgos — la parte honesta

| # | Riesgo | Por qué es real | Mitigación |
|---|---|---|---|
| R1 | **Construir un editor es un pozo sin fondo** | Es *el* error clásico. Siempre falta una función más, y Canva tiene 500 ingenieros. | Alcance congelado en las 6 primitivas de §4.2. Nada entra al editor si no aparece en un flyer real. Polotno para no escribirlo desde cero. |
| R2 | **Multi-tenant desde el día 1 sin un segundo cliente** | Añade complejidad que puede no rentabilizarse nunca. | El costo real es bajo si se paga temprano (RLS y `tenant_id` desde el primer commit); retrofitear aislamiento después es carísimo. Se acepta el riesgo, se paga barato. |
| R3 | **Retail media exige volumen que hoy no existe** | Un CPG no compra inventario con 60 piezas al mes en 7 tiendas. | Es la Etapa V, no la I. Requiere Etapa IV (más agencias) antes de tener sentido. No se promete a nadie todavía. |
| R4 | **Deriva de calidad de los agentes** | Un pipeline de IA sin evals se degrada en silencio. | Los cuatro bucles de §5, con evals bloqueando el CI. |
| R5 | **Una persona sola es un punto único de fallo** | Todo el conocimiento en una cabeza; sin equipo, una enfermedad para el producto. | Documentación como código (ADRs, `CLAUDE.md`), infra declarativa, cero pasos manuales. Que un agente pueda reconstruir el contexto es parte del entregable. |
| R6 | **Sobre-construir para un cliente de 7 tiendas** | **El riesgo más probable de todos.** | Ver §9. |
| R7 | Dependencia de las APIs de Meta | Cambios de política y de endpoints. | Adaptador propio; toda la lógica de Meta detrás de una interfaz. |
| R8 | Los derechos de imagen siguen siendo un pasivo | El pleito del que se habló en la reunión existe. | El DAM con licencia y origen por activo es **capa ② de la Etapa I**, no un extra. |

## 9. Cuándo esta propuesta es una mala idea

Sería deshonesto presentar la V2 sin esto.

**La V2 no es una versión mejor de la V1. Es una apuesta distinta.**

| | V1 (documento 04) | V2 (este documento) |
|---|---|---|
| Qué es | Automatizar el flujo de Fresco | Construir un producto |
| Cliente | Fresco Marketing | Fresco + N agencias + CPGs |
| Tiempo al primer valor | ~4 semanas | ~6 semanas |
| Tiempo al modelo completo | ~17 semanas | ~26 semanas + |
| Costo operativo | USD 18–64/mes | USD 20–900/mes según etapa |
| Riesgo | Bajo | **Medio-alto** |
| Techo | Ahorro de horas | **Empresa** |

**Elige la V1 si:** el objetivo es resolver el dolor de Fresco Marketing y cobrar por ello. Es más rápida, más barata, menos riesgosa, y hace exactamente lo que se pidió.

**Elige la V2 si:** el objetivo es construir un activo propio, y Fresco es el primer cliente y el campo de pruebas — no el destino final.

**Y hay una tercera opción, que es la que yo tomaría:**

> **Construir la Etapa I de la V2 y venderla como la V1.**

Es casi el mismo trabajo y el mismo plazo. La diferencia está debajo: Offer Graph en vez de tablas ad hoc, multi-tenant desde el primer commit, plantilla como dato en vez de como archivo, evals desde el principio. **Nada de eso cuesta tiempo extra si se decide al empezar, y todo es carísimo de retrofitear.**

Fresco recibe exactamente lo que pidió, al precio que puede pagar. Y tú te quedas con los cimientos de algo más grande, sin haber apostado nada. La decisión de seguir a la Etapa II se toma con datos reales, no con una proyección.

Dicho de otro modo: **el mini Canva, el DCO y el retail media no hay que decidirlos hoy. Solo hay que no cerrarse las puertas hoy.**

---

## Fuentes

**Content supply chain y agencias**
- [Adobe — Introduces Brand Intelligence and Expands GenStudio Content Supply Chain (abril 2026)](https://news.adobe.com/news/2026/04/adobe-introduces-brand-intelligence)
- [Adobe GenStudio — plataforma de contenido generativo](https://business.adobe.com/products/genstudio.html)

**Retail media**
- [Local Express — Retail Media Networks for Grocers: High-Margin CPG Advertising Revenue](https://www.localexpress.io/post/how-regional-grocers-can-build-a-high-margin-retail-media-revenue-engine)
- [eMarketer — FAQ on digital grocery: AI, retail media and omnichannel in 2026](https://www.emarketer.com/content/faq-on-digital-grocery-how-ai-retail-media-omnichannel-fulfillment-reshaping-2026)
- [Improvado — Top 15 Retail Media Networks 2026](https://improvado.io/blog/top-retail-media-networks)

**Editor de diseño**
- [Polotno SDK vs Fabric.js](https://polotno.com/sdk/product/compare/polotno-sdk-vs-fabricjs) · [Polotno SDK vs Konva.js](https://polotno.com/sdk/product/compare/polotno-sdk-vs-konvajs)
- [PkgPulse — Fabric.js vs Konva vs PixiJS 2026](https://www.pkgpulse.com/guides/fabricjs-vs-konva-vs-pixijs-canvas-2d-graphics-2026)
- [IMG.LY — Open-Source Design SDKs: comparación](https://img.ly/blog/open-source-design-editor-sdks-a-developers-guide-to-choosing-the-right-solution/)

**Render**
- [Rendex — Best HTML to Image APIs in 2026](https://rendex.dev/compare/best-html-to-image-apis-2026)
- [DuneTools — HTML to Image: A Developer's Guide (2026)](https://www.dunetools.com/guides/html-to-image-developers/)

**DCO y Meta**
- [AllAspect — DCO in 2026: Dynamic Creative Optimization That Actually Scales](https://allaspect.com/insights/dynamic-creative-optimization-dco-guide-2026/)
- [AdLibrary — Meta Advantage+ Creative Guide 2026](https://adlibrary.com/posts/meta-advantage-creative-guide)
- [Marketing Brew — How Meta's AI push is changing ad creation](https://www.marketingbrew.com/stories/2026/04/07/meta-ai-ad-creation)

**Infraestructura**
- [Cloudflare Workers — pricing oficial](https://developers.cloudflare.com/workers/platform/pricing/)
- [Rajpoot — Cloudflare vs AWS vs Vercel for Backend in 2026](https://blog.rajpoot.dev/posts/devops/cloudflare-vs-aws-vs-vercel-2026/)
- [Neon — Best managed Postgres for multi-tenant SaaS](https://neon.com/faqs/best-managed-postgres-databases-multi-tenant-saas)
- [Klymentiev — Supabase vs Neon 2026](https://klymentiev.com/blog/supabase-vs-neon)

**Desarrollo con agentes**
- [Macroscope — AI Code Review for Monorepos: Complete Guide (2026)](https://macroscope.com/content/ai-code-review-monorepos-complete-guide)
- [Sourcegraph — Automated Code Review Tools](https://sourcegraph.com/blog/automated-code-review-tools)

---

← [05 · Revisión de propuestas](./05-revision-de-propuestas.md) · [Índice](../README.md)
