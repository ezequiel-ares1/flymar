# Flymar — contexto para agentes

Automatización con IA de la producción de flyers de ofertas y de la publicidad en Meta para supermercados latinos en EE. UU.

**Cliente:** Fresco Marketing (Kansas City, MO) · contacto: Marcela
**Estado:** fase de propuesta. **Sin implementación todavía** — el repositorio contiene documentación y los insumos originales.

---

## Cómo está organizado

```
docs/01-analisis-del-problema.md        proceso actual, insumos reales, 30 dolores
docs/02-benchmark-ia-en-marketing.md    estado del arte y fuentes
docs/03-viabilidad-tecnica.md           verificación contra documentación oficial
docs/04-propuesta-de-solucion.md        ⚠️ SUPERADA por la 08
docs/05-revision-de-propuestas.md       evaluación de las dos propuestas de terceros
docs/06-propuesta-v2-plataforma.md      alternativa de producto, sin restricciones
docs/07-reinventar-la-captura.md        seis niveles de intercepción del dato
docs/08-propuesta-definitiva.md         ⭐ LA PROPUESTA VIGENTE
docs/09-arquitectura-y-despliegue.md    sustento técnico y decisiones
info-inicial/                           insumos del cliente (no modificar)
```

**Al trabajar sobre la propuesta, el documento 08 es la fuente de verdad.** El 04 se conserva por su análisis técnico pero sus restricciones ya no aplican.

`info-inicial/` es material recibido del cliente: **no editarlo ni reorganizarlo.**

---

## Decisiones tomadas — no volver a abrirlas sin motivo nuevo

| Decisión | Razón corta |
|---|---|
| **Canva no es obligatorio** | La clienta levantó la restricción; se construye motor de composición propio |
| **La Autofill API de Canva no se usa** | Requiere plan Enterprise; el cliente tiene Pro |
| **WhatsApp avisa, la web trabaja** | Sin WhatsApp Flows: no admiten arrastrar ni vista previa |
| **Postgres, no MySQL** | RLS nativa, `pgvector` y `pg_trgm`; MySQL exigiría tres piezas más |
| **Monolito modular, no microservicios** | No hay equipos que separar y sí una persona que mantendría N despliegues |
| **Editor de datos, no de canvas** | Ninguna corrección real necesita mover píxeles; además impide descuadrar la plantilla |
| **Infraestructura gestionada, no self-hosted** | Sin equipo que parchee; la portabilidad se logra por diseño, no por hosting |
| **La captura va antes que el diseño** | Arreglar el origen hace más fácil todo lo demás |

Detalle y alternativas descartadas en el [documento 09](./docs/09-arquitectura-y-despliegue.md).

---

## Convenciones

**Idioma:** todo en **español**, con ortografía y acentuación completas. Los términos técnicos y los identificadores de código van en su forma original.

**Documentación:**
- Densa, sin redundancia. Si una decisión cambia, **borrar lo obsoleto** en vez de dejarlo como historia.
- Distinguir siempre **dato verificado** de **estimación** de **decisión pendiente**. Marcar las dos últimas.
- Toda afirmación técnica sobre capacidades de plataformas se **verifica contra documentación oficial** antes de sostenerla, y se cita la fuente.
- Tablas para comparativas, prosa para argumentos.
- **Tras comprimir un documento, revisar las referencias cruzadas** (`§N`, enlaces entre documentos, totales de tablas): es lo primero que se rompe.

**No incluir en la documentación:** estimaciones de horas de desarrollo, tarifas ni honorarios propios. Sí los costos de infraestructura y operación, que son argumento de venta.

**Commits:** mensaje en español, en imperativo, explicando **el porqué** y no solo el qué. Cuerpo con los cambios sustantivos cuando son varios.

---

## Cómo se trabaja aquí

- **Las sugerencias del cliente sobre *cómo* hacer algo son señales, no especificaciones.** Separar lo que dijo / la necesidad real que revela / la respuesta técnica, que puede ser distinta. Cuando la propuesta se aparte, decirlo y argumentarlo.
- **Preguntar en vez de asumir.** Los datos que solo conoce el cliente (tiempos reales, número de sucursales, accesos) se preguntan; no se rellenan con supuestos.
- **Honestidad sobre complacencia.** Señalar los puntos flojos de una idea aunque el resto sea bueno, reconocer los errores propios sin dramatizar, y ser explícito sobre lo que **no** es innovador en la propuesta.

---

## Lo que bloquea el arranque

Dos trámites que no dependen de escribir código y pueden retrasar todo:

1. **Categorización de la plantilla de WhatsApp.** Meta bloquea la categoría *marketing* hacia números de EE. UU. desde abril de 2025 (error `63049`). El caso de uso es *utility*, pero la categoría la decide Meta al aprobar. **Hay que probarlo antes de construir la captura.**
2. **Acceso al DNS de `frescomktg.com`** para crear subdominios y registros SPF/DKIM/DMARC. Bloquea la verificación de WhatsApp Business y la entregabilidad del correo.

Y lo de mayor severidad con menor costo de mitigar: **la auditoría del banco de imágenes**. Hay una demanda activa contra otra agencia por el mismo tipo de uso.
