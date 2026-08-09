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
| 04 | [Propuesta de solución](./docs/04-propuesta-de-solucion.md) ⚠️ | *Superada por el doc 08.* Arquitectura y plan bajo las restricciones originales (Canva obligatorio, sin plataforma para managers) |
| 05 | [Revisión de las propuestas recibidas](./docs/05-revision-de-propuestas.md) | Evaluación honesta de viabilidad y factibilidad de las dos propuestas de terceros: qué acierta cada una, qué está bloqueado y qué queda fuera de alcance |
| 06 | [Propuesta V2 · Flymar Platform](./docs/06-propuesta-v2-plataforma.md) | Alternativa sin restricciones: plataforma multi-tenant de *content supply chain*, editor propio, DCO, retail media y stack elegido para construirse dirigiendo agentes de IA |
| 07 | [Reinventar la captura](./docs/07-reinventar-la-captura.md) | Cómo entra hoy la información (email + Excel/PDF/foto) y seis niveles para interceptar el dato antes de que se degrade: acuse inteligente, WhatsApp Flows, audio, API del POS, EDI 889 y feeds de circular digital |
| **08** | **[Propuesta definitiva](./docs/08-propuesta-definitiva.md)** ⭐ | **La propuesta vigente.** Revisada tras la segunda reunión: captura por WhatsApp con propuesta pre-llenada, motor de composición propio, atribución de ventas como foso competitivo y arquitectura para llegar a 30 tiendas sin sumar personal |

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

## Lo que cambió en la segunda reunión

Cuatro giros que reescribieron la propuesta:

1. **Canva dejó de ser un requisito.** *"Si la solución es más sencilla en otro lado y se parece el diseño… ese diseño yo se lo puedo proponer a mi cliente."* Se desbloquea el motor de composición propio.
2. **Marcela pidió el formulario móvil** con sus propias palabras: *"que lo puedan hacer desde el celular, como cuando llenas una encuesta… y en el celular está primero carnicería"*.
3. **Apareció el miedo a la desintermediación:** *"yo necesito que ellos sientan que me necesitan"*. Es un requisito de producto, y la respuesta correcta no es esconder la herramienta sino cambiar lo que se vende.
4. **El crecimiento ya ocurrió:** *"acaban de abrir otras 2 tiendas… no voy a poner a todo mi equipo a hacer esto, no es negocio."*

## La idea central de la propuesta vigente

> **Lo que hoy vende Fresco:** *"Yo te armo el flyer y lo publico."*
> **Lo que debería vender:** *"Yo sé qué mover, cuándo publicarlo y cuánto vendiste con ello."*

Lo primero lo copia el sobrino con Canva. Lo segundo requiere histórico, datos y sistema, y se vuelve más difícil de copiar cada semana.

## Estado

📄 Fase de propuesta. Sin implementación todavía.

**Siguientes pasos:** elegir las 2 sucursales piloto para el formulario, iniciar la verificación de WhatsApp Business API (es lo que más tarda), auditar el banco de imágenes, y preguntar a dos managers cuánto tiempo les toma preparar el Excel — ese número es el que justifica todo el cambio ante el cliente final.
