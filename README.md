# Flymar

Automatización con IA de la producción de flyers de ofertas y de la publicidad en Meta para supermercados latinos.

**Cliente:** Fresco Marketing (Kansas City, MO) · **Contacto:** Ana Marcela

---

## El problema en una línea

Producir ~60 flyers al mes a mano (leer archivos desordenados, traducir, buscar imágenes, maquetar en Canva) y crear cada anuncio en Meta al día siguiente, perdiendo el 25 % de la vigencia de cada oferta.

## La propuesta en una línea

Un pipeline que lee los tres formatos de entrada con IA, valida y traduce con un glosario propio, propone imágenes del banco de la agencia, deja que una persona apruebe en 5 minutos, genera el diseño **editable en Canva** y lanza el post + anuncio en Meta a las 21:00 — por **menos de USD 65 al mes** de operación.

---

## Documentación

| # | Documento | Contenido |
|---|---|---|
| 01 | [Análisis del problema](./docs/01-analisis-del-problema.md) | Proceso actual, insumos reales, reglas de negocio, siete dolores priorizados, restricciones y criterios de éxito |
| 02 | [Benchmark: IA en marketing](./docs/02-benchmark-ia-en-marketing.md) | Cómo operan WPP/Publicis/Omnicom, cifras de ROI de automatización creativa, software de circulares, MCP, n8n, límites reales de la extracción con LLM, marco legal de imágenes |
| 03 | [Viabilidad técnica](./docs/03-viabilidad-tecnica.md) | Verificación contra documentación oficial: por qué la Autofill API de Canva no sirve, cuál sí, cómo recuperar las 12 horas en Meta, costos reales de IA, matriz de riesgos |
| 04 | [Propuesta de solución](./docs/04-propuesta-de-solucion.md) | Arquitectura, componentes clave, stack, plan por fases, costos, KPIs y lo que la propuesta *no* promete |
| 05 | [Revisión de las propuestas recibidas](./docs/05-revision-de-propuestas.md) | Evaluación honesta de viabilidad y factibilidad de las dos propuestas de terceros: qué acierta cada una, qué está bloqueado y qué queda fuera de alcance |

## Los tres hallazgos que cambian la conversación

1. **La Autofill API de Canva requiere plan Enterprise** (USD 2,000–30,000/año). El cliente tiene Canva Pro. Esto invalida el camino "obvio" y probablemente explica por qué las propuestas anteriores nunca mostraron un demo con más de dos productos. → [doc 03 §1](./docs/03-viabilidad-tecnica.md)

2. **La Design Editing API sí está disponible en todos los planes**, incluido Pro, vía transacciones de edición. Es la vía viable, y es nueva de 2026. → [doc 03 §2](./docs/03-viabilidad-tecnica.md)

3. **El límite de Meta para programar anuncios es de la interfaz, no de la API.** Publicando el post vía Graph API a las 21:00 y creando el anuncio con `object_story_id` en el mismo segundo, se recuperan ~12 h de vigencia por campaña de 48 h. → [doc 03 §3](./docs/03-viabilidad-tecnica.md)

## Costo operativo estimado

| | Inicial | Mensual | 3 años |
|---|---|---|---|
| Propuesta A (Escobedo) | USD 5,000 | USD 500 | USD 23,000 |
| Propuesta B (Hexio) | *sin cotizar* | *sin cotizar* | — |
| **Flymar (operación)** | *desarrollo aparte* | **USD 18–64** | **USD 650–2,300** |

## Estado

📄 Fase de propuesta. Sin implementación todavía.

Siguiente paso recomendado: contratar **Fase 0 + Fase 1** (fundaciones + extracción y revisión), que entrega el 60–70 % del ahorro de tiempo y deja la decisión sobre Canva informada por un spike técnico real.
