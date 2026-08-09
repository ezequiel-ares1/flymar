# 09 · Arquitectura, despliegue y decisiones técnicas

Complemento del [documento 08](./08-propuesta-definitiva.md). Aquí está el sustento de cada elección: qué se descartó, por qué, dónde se despliega y qué queda sin definir.

**Criterio rector, del que se derivan casi todas las decisiones:** lo construye y lo mantiene **una persona dirigiendo agentes de IA**, sin equipo de operaciones. Cada pieza que se añade hay que parchearla, monitorearla y entenderla a las 2 de la mañana cuando falle. **La complejidad operativa es el costo real, no la licencia.**

---

## 1 · Tipo de arquitectura: monolito modular

**Decisión: una sola aplicación desplegable, dividida internamente en módulos de dominio con fronteras explícitas.** No microservicios, no serverless disperso, no repositorios separados.

```
apps/web                    ← Next.js: web del manager + tablero interno + API
  └── módulos de dominio (fronteras explícitas, sin importaciones cruzadas)
      ├── capture/          Notificación, link temporal, formulario, carril B
      ├── catalog/          Producto canónico, glosario, imágenes, histórico
      ├── compose/          Plantilla como dato, render, editor
      ├── publish/          Meta: post, ad set, ad, scheduler
      ├── measure/          Insights, atribución, informes
      └── shared/           Tipos, esquemas Zod, utilidades
```

**Por qué no microservicios.** El argumento a favor es escalar equipos y desplegar por separado. Aquí no hay equipos y no hace falta desplegar por separado. A cambio se pagaría: N despliegues, N conjuntos de secretos, trazas distribuidas, contratos entre servicios que se rompen en runtime en vez de en el typecheck, y una superficie de fallo mucho mayor. **Es coste puro sin beneficio a esta escala.**

**Por qué módulos con fronteras y no un monolito informe.** Tres razones concretas:

1. **Agentes en paralelo.** Un agente puede trabajar en `compose/` mientras otro toca `capture/` sin pisarse, porque las fronteras son explícitas y verificables con una regla de lint que prohíbe importaciones cruzadas.
2. **El contexto cabe.** Un agente que trabaja en un módulo carga ese módulo, no el sistema entero.
3. **Extraer después es barato.** Si algún día `compose/` necesita escalar aparte, ya está aislado. **Empezar distribuido y volver atrás sí es caro.**

**Estilo interno:** capas finas. `route handler → caso de uso → repositorio`. Sin ORM mágico ni abstracciones especulativas. Los agentes de IA escriben mejor código sobre patrones directos que sobre arquitecturas hexagonales con cinco capas de indirección.

---

## 2 · Stack, decisión por decisión

### 2.1 Lenguaje: TypeScript de punta a punta

**Por qué:** el tipo `Offer` es literalmente el mismo objeto en captura, catálogo, render y publicación. Un agente que rompe el contrato lo descubre en `tsc` en segundos, no en producción tres semanas después.

| Descartado | Por qué |
|---|---|
| Python + FastAPI para el backend | Dos lenguajes, dos ecosistemas, y el tipo compartido se pierde en la frontera. La ventaja de Python está en ML, y aquí no entrenamos nada — solo llamamos APIs. |
| Go | Excelente para servicios, pero pierde el tipo compartido con el frontend y tiene menos masa de entrenamiento para los agentes en este tipo de app. |
| Rails / Laravel | Productividad alta con equipo humano; menor con agentes, y el tipado dinámico elimina el bucle de retroalimentación más rápido que tenemos. |

### 2.2 Framework: Next.js (App Router)

**Por qué:**
- **Un solo proyecto cubre tres superficies:** la web pública del manager, el tablero interno autenticado y la API. Sin CORS, sin dos despliegues, sin duplicar tipos.
- **Server Components reducen el JavaScript enviado al móvil.** No es un detalle: el manager abre el link **en el navegador embebido de WhatsApp, en el pasillo de la tienda, con señal irregular**. Cada kilobyte cuenta.
- **Route handlers** para los webhooks de WhatsApp y Meta.
- **Satori** (el motor de render) es de Vercel y se integra de forma natural.
- **Masa de entrenamiento.** Es donde los agentes de IA se equivocan menos, y eso es un criterio legítimo cuando el equipo son agentes.

| Descartado | Por qué |
|---|---|
| SPA (React/Vite) + API separada | Dos despliegues, dos repos o un monorepo con fronteras artificiales, y peor rendimiento en móvil de gama baja. |
| Remix | Muy competente, ecosistema y masa de entrenamiento menores. |
| Astro | Excelente para contenido, menos natural para una app con estado y formularios. |

### 2.3 Base de datos: PostgreSQL

**Por qué Postgres y no otra cosa:** el dominio **es relacional** (tienda → oferta → producto → campaña → resultado). Además resuelve tres necesidades sin añadir servicios:

- **`pgvector`** para el matching semántico de imágenes → no hace falta una base vectorial aparte.
- **`JSONB`** para la plantilla como dato → sin un almacén de documentos aparte.
- **Row-Level Security** para el aislamiento multi-tenant **en el motor, no en el código de aplicación**. Un agente que olvide un `WHERE tenant_id` no puede filtrar datos entre clientes. Es una red de seguridad estructural, y es la razón principal de esta elección.

### 2.4 Proveedor de datos: Supabase

| | Supabase | Neon |
|---|---|---|
| Qué es | Postgres **+ auth + storage + realtime** | Postgres puro, con *branching* excelente |
| Auth del tablero interno | ✅ Incluido | ❌ Hay que integrar otro proveedor |
| Almacenamiento de archivos | ✅ Incluido | ❌ Aparte |
| RLS | ✅ Integrado en el flujo | ✅ Nativo de Postgres, se implementa a mano |
| *Branching* por rama de agente | ⚠️ Existe | ✅ Su punto fuerte |

**Decisión: Supabase.** El criterio es *menos piezas que mantener*, y trae resueltos auth y storage — dos servicios que de otro modo habría que elegir, integrar y parchear. El *branching* de Neon es tentador para trabajar con agentes, pero no compensa integrar auth y storage por separado.

**Puerta de salida:** son datos Postgres estándar. Migrar a Neon o a Postgres autogestionado es directo si algún día el branching pesa más que la comodidad.

### 2.5 Render: Satori + resvg

Convierte una descripción declarativa a SVG y luego a PNG en **50–200 ms**, con ~10 MB de dependencias y sin levantar un navegador. Limitación: **solo Flexbox** (sin Grid, sin z-index, sin JS). Para una rejilla de tarjetas con bandas de categoría, Flexbox sobra.

| Descartado | Por qué |
|---|---|
| Puppeteer / Chrome headless | Levantar un navegador por render: cientos de MB, arranque lento, y una fuente de fallos intermitentes que a las 2 de la mañana no quieres depurar. |
| ImageMagick / canvas nativo | Componer tipografía y layout a mano es reinventar un motor de maquetación. |
| API de terceros (Bannerbear, Templated) | USD 29–49/mes y una dependencia externa en el camino crítico, para algo que aquí cuesta 12 segundos de cómputo al mes. |

### 2.6 Modelos de IA

Escalonados por tarea, según el [documento 08 §9](./08-propuesta-definitiva.md). Lo relevante en clave de arquitectura: **la IA vive detrás de una interfaz propia** (`extract(document): OfferSheet`), no esparcida por el código. Cambiar de modelo o de proveedor es tocar un archivo.

### 2.7 Orquestación: dentro de la aplicación

Cron y colas en el mismo proyecto, no n8n como núcleo.

**Por qué:** lo del cron de las 21:00 y el pipeline de publicación es **lógica de negocio con dinero de por medio**. Necesita tests, control de versiones, reintentos con idempotencia y trazas. Un flujo visual no da nada de eso.

**Dónde sí n8n:** integraciones sueltas y experimentos — conectar una hoja de cálculo, probar un webhook. No en el camino crítico.

---

## 3 · Despliegue

### 3.1 El dato que cambia el cálculo

> **El plan Hobby de Vercel es gratuito solo para proyectos personales no comerciales.** Cualquier proyecto que genere ingresos requiere Pro (USD 20 por usuario/mes). Además, Hobby **no tiene excedentes**: al llegar al límite la aplicación se detiene hasta el siguiente ciclo.

Esto es comercial. **Hobby no es una opción**, y presentarlo como gratis habría sido un error de presupuesto.

### 3.2 Opciones evaluadas

| Opción | Costo/mes | A favor | En contra |
|---|---:|---|---|
| **Vercel Pro** | USD 20 | Integración nativa con Next.js, **preview por cada PR** (clave trabajando con agentes), cero configuración | El más caro del grupo |
| **Railway Hobby** | USD 5 | Incluye USD 5 de uso; app y base de datos juntas; entornos por PR | Menos afinado para Next.js que Vercel |
| **Coolify en VPS (Hetzner)** | ~USD 6 | 10–20 % del costo de Vercel, control total | **Se descarta:** hay que parchear el servidor y la plataforma. En enero de 2026 se divulgaron **11 vulnerabilidades críticas en Coolify**, incluidas evasión de autenticación y ejecución remota de código. Sin equipo que aplique parches, es un pasivo. |
| Cloudflare Workers | ~USD 5 | Barato, arranque en frío nulo | Next.js sobre Workers todavía tiene fricciones; se cambia coste por depuración |

### 3.3 Recomendación

**Vercel Pro (USD 20) + Supabase + Cloudflare R2 + Cloudflare DNS.**

El sobrecosto frente a Railway son USD 15 al mes, y compra una cosa que aquí vale más que eso: **un despliegue de vista previa por cada pull request**. Cuando el código lo escriben agentes, poder abrir la URL del PR y ver el flyer renderizado en un teléfono real **es el quinto bucle de retroalimentación**, junto al typecheck, los tests, los *golden files* y las evals.

**Si el presupuesto aprieta, Railway a USD 5 es una opción legítima** y no compromete la arquitectura: la aplicación es la misma.

### 3.4 Costo de infraestructura, corregido

| Concepto | Arranque | Maduro (10 tiendas) |
|---|---:|---:|
| Vercel Pro | USD 20 | USD 20 |
| Supabase | 0 *(free)* | 25 *(Pro)* |
| Cloudflare R2 + DNS | 0–2 | 2–5 |
| Dominio | ~1.25 | ~1.25 |
| WhatsApp | ~1 | ~1 |
| IA | 1–4 | 1–4 |
| Sentry, monitoreo | 0 *(free)* | 0–10 |
| **Total** | **USD 23–28** | **USD 50–66** |

> **Corrección respecto al documento 08:** la tabla de allí estimaba USD 29–45 asumiendo un despliegue gratuito. **Con el uso comercial correctamente contemplado, el rango realista es USD 23–28 al arrancar y USD 50–66 en régimen.** Sigue siendo un orden de magnitud por debajo de cualquier alternativa, pero la cifra anterior era optimista.

---

## 4 · Dominio: sí, y no es opcional

Hacen falta **tres** razones para justificarlo, y las tres se cumplen:

1. **Confianza y marca blanca.** El manager recibe un link por WhatsApp. `ofertas.frescomktg.com/p/7Ka9dR` es de Fresco; `flymar-git-main-abc.vercel.app` parece phishing. Todo el argumento de la §2 del documento 08 —que el manager perciba un servicio de la agencia— se cae con una URL de aspecto técnico.
2. **Verificación de WhatsApp Business.** El proceso de Meta se apoya en un dominio del negocio.
3. **Entregabilidad del correo.** El acuse del carril B necesita SPF, DKIM y DMARC sobre un dominio propio. Sin eso va a spam, y el carril B deja de funcionar en silencio.

**Recomendación: un subdominio del dominio que Fresco ya tiene** (`frescomktg.com`, visible en el PDF original). Refuerza la marca blanca y no añade un dominio que mantener. Si se prefiere separar, un dominio propio cuesta ~USD 12–15 al año.

**Hay que decidir:** ¿`ofertas.frescomktg.com` o un dominio aparte? Es decisión de Marcela y afecta a la percepción del manager.

---

## 5 · Entornos, CI/CD y calidad

```
local        →  preview (por PR)  →  producción
Supabase dev    Supabase branch      Supabase prod
```

**Pipeline en cada pull request:**

```
1. typecheck        contratos rotos                segundos
2. tests            lógica rota                    segundos
3. golden files     layout roto (diff de píxeles)  ~1 min
4. evals            extracción degradada           ~2 min
5. preview deploy   verlo en un teléfono real      ~1 min
```

Los puntos 3 y 4 son los que casi nadie implementa y los que permiten que un agente toque código de layout o de extracción **sin revisión visual manual en cada cambio**. Si la precisión de extracción baja del 95 %, el despliegue se detiene.

**Migraciones:** versionadas en el repositorio, aplicadas en el pipeline. Nunca a mano contra producción.

---

## 6 · Datos, seguridad y continuidad

| Tema | Decisión |
|---|---|
| **Backups** | El catálogo **es el activo** — perderlo es perder el foso. Backup diario a R2, además del que traiga el proveedor. Probar la restauración una vez, no asumir que funciona. |
| **Multi-tenant** | `tenant_id` y RLS desde el primer commit. Cuesta cero al empezar y es carísimo después. |
| **Tokens del link** | Firmados, con vencimiento **por tiempo** (48 h), sin datos en la URL más que un identificador opaco. Nunca de un solo uso (la vista previa de WhatsApp lo consumiría). |
| **Secretos** | Variables de entorno del proveedor. Nunca en el repositorio, ni siquiera en un `.env` de ejemplo con valores reales. |
| **Datos personales** | Solo contactos de negocio (nombre y teléfono del manager). **No hay datos de consumidores finales**, lo que simplifica mucho el panorama regulatorio. |
| **Acceso a Meta** | System User dentro del Business Portfolio existente, con token de larga duración y permisos mínimos. |
| **Idempotencia** | Toda operación con dinero de por medio (publicar, crear anuncio) lleva clave de idempotencia. **Si el cron se dispara dos veces, no se publica dos veces.** |

---

## 7 · Observabilidad

Lo mínimo que hace falta para no depurar a ciegas, sin montar un stack de monitoreo:

- **Logs estructurados** con el identificador de la semana y la tienda (`METRO-2026-W20`) atravesando todo el pipeline. Cualquier incidente se reconstruye filtrando por esa clave.
- **Sentry** (plan gratuito) para excepciones no controladas.
- **El tablero de estados ya es parte del producto** — sirve de monitoreo funcional gratis: si una tienda se quedó en "extraído" y no avanzó, se ve.
- **Alerta al equipo** cuando algo del camino crítico falla: publicación fallida, anuncio rechazado por Meta, extracción por debajo del umbral.

---

## 8 · Lo que sigue sin definir

Honestamente, esto es lo que falta y **no debería asumirse**:

| # | Pendiente | Por qué importa |
|---|---|---|
| 1 | **Zona horaria por tienda** | Las 21:00 son Central en Kansas City. Si alguna sucursal está en otra zona, el scheduler necesita horario por tienda, no global. |
| 2 | **Qué pasa si Meta rechaza el anuncio** | Hoy Marcela lo ve y reacciona. Automatizado, hace falta un flujo: ¿reintenta con otro creativo, avisa, publica sin pauta? |
| 3 | **Alta de una tienda nueva** | ¿Quién carga logo, colores y rejilla? ¿Marcela desde el tablero, o es trabajo de desarrollo? Con dos sucursales recién abiertas, esto es inmediato. |
| 4 | **Propiedad del catálogo al terminar una relación** | Si Fresco pierde un cliente, ¿los datos de esa tienda se exportan, se borran, se conservan? Conviene decidirlo **antes** de que haya datos valiosos dentro. |
| 5 | **Accesibilidad de la web del manager** | Se usa en un pasillo, con una mano, posiblemente con guantes y mala luz. Objetivos de contraste y tamaño de toque, no como refinamiento posterior. |
| 6 | **Retención de imágenes y de artes generados** | Cuántos meses se guardan los flyers viejos. Afecta al costo de almacenamiento y a poder reconstruir un histórico. |
| 7 | **Tu tiempo y tu tarifa** | Todo el documento habla de costos de operación. **No dice cuánto cuesta construirlo ni cómo se cobra.** Es la pieza que falta para que esto sea una propuesta comercial y no solo un plan técnico. |

---

← [08 · Propuesta definitiva](./08-propuesta-definitiva.md) · [Índice](../README.md)

**Fuentes:** [Vercel — pricing y límites 2026](https://temps.sh/blog/vercel-pricing-complete-guide-2026) · [Vercel: uso comercial requiere Pro](https://schematichq.com/blog/vercel-pricing) · [Railway pricing 2026](https://www.srvrlss.io/provider/railway/) · [Coolify: alternativas y superficie de mantenimiento](https://northflank.com/blog/coolify-alternatives-in-2026) · [Supabase vs Neon para desarrollador individual](https://solodevstack.com/blog/supabase-vs-neon-solo-developers) · [Neon vs Supabase 2026](https://designrevision.com/blog/supabase-vs-neon)
