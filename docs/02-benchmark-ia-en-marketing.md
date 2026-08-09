# 02 · Benchmark — Cómo usan IA las agencias de marketing hoy (2026)

Investigación de mercado realizada el 2026-08-08. Todas las afirmaciones con fuente citada al final.

---

## 1. El cambio estructural: de asistente a operador

El titular del año, según eMarketer y AdAge, es que las grandes redes de agencias dejaron de vender *horas creativas* y pasaron a vender *plataformas operativas*. La industria describe 2026 como el fin de "IA como asistente creativo" y el inicio de **"IA como operador"**: software autónomo que planifica campañas, ejecuta compras de medios y optimiza sin intervención humana en cada paso.

| Holding | Plataforma | Apuesta |
|---|---|---|
| **WPP** | WPP Open / Open Pro | USD 400 M en 5 años con Google para integrar Google Cloud y Gemini. Modelo *self-serve* para marcas que gestionan sus propias campañas. |
| **Publicis** | CoreAI + Marcel | Marcel conecta a 90,000 empleados; CoreAI centraliza datos e identidad global. |
| **Omnicom** | Omni | Fusión Omnicom–IPG cerrada a fines de 2025: el mayor holding del mundo, con foco en *connected commerce intelligence*. |
| **Dentsu, Havas, Horizon, Stagwell** | Equivalentes propios | Todos construyeron su "sistema operativo" con IA generativa y agentes. |

El efecto colateral está documentado: **oleadas de despidos** en las redes mientras los clientes exigen automatización omnicanal y honorarios más bajos.

**Lectura para este proyecto:** lo que Fresco Marketing quiere construir no es una excentricidad — es exactamente lo que las grandes están haciendo, en versión proporcional a su escala. La diferencia es que ellas gastan USD 400 M y aquí el presupuesto es de tres cifras al mes. Eso es posible porque las piezas (LLMs con visión, APIs de diseño, MCP, orquestadores open source) hoy están disponibles a precio de commodity.

## 2. Automatización creativa: los números que se publican

Estas cifras vienen de proveedores y de un estudio Forrester TEI, así que hay que leerlas como *techo optimista*, no como promesa. Aun descontando la mitad, el orden de magnitud sostiene el caso de negocio.

| Métrica | Valor reportado | Fuente |
|---|---|---|
| ROI a 3 años (cliente compuesto empresarial) | **94 %**, USD 4.16 M en beneficios, *payback* en 6 meses | Forrester TEI / Superside |
| Reducción de rondas de revisión | **−60 %** | Forrester TEI / Superside |
| Ahorro de tiempo de producción | **60–80 %** | Storyteq |
| ROI típico de automatizar procesos creativos | **200–400 %** | Storyteq |
| Time-to-launch omnicanal | De 2–3 semanas a **< 2 días** | Luma Labs |
| Equipos que reportan ahorro significativo con IA | **73 %** | Thunderbit |
| Plazo para ver ROI medible | **3–6 meses** | Optiow |

**El indicador más relevante para Flymar es el de las rondas de revisión (−60 %)**, porque el dolor #4 del cliente es precisamente el ciclo de feedback por errores de ortografía y precios. Ese ahorro no viene del modelo generativo: viene de estandarizar la entrada y validar los datos antes de maquetar.

## 3. El sector específico: circulares de supermercado

Este es un nicho con software propio desde hace años, y conviene conocerlo para no reinventarlo — y para saber por qué **no** aplica aquí.

| Producto | Qué hace | Por qué no encaja |
|---|---|---|
| **Comosoft LAGO** | Estándar de la industria para producción de circulares retail impresas y digitales, con versionado por sucursal, PIM integrado y automatización de layout. | Software empresarial para cadenas grandes. Licenciamiento y despliegue fuera de escala (y de presupuesto) para una agencia con 7 tiendas. |
| **Swiftly SmartCircular** (lanzado 2026) | Convierte el PDF de la circular en experiencia digital interactiva con *tap targets* generados por IA; mide la circular como canal de performance. | Resuelve el problema *aguas abajo* (la circular ya existe). Aquí el problema es *aguas arriba*: producirla. |

**Conclusión:** existe una categoría madura para el retail grande, y un vacío para agencias pequeñas y medianas que atienden supermercados independientes. Ese vacío es la oportunidad de Flymar — y potencialmente un producto revendible a otras agencias del mismo perfil.

## 4. Plataformas de generación de creatividades por API

El mercado de "diseñas una plantilla maestra, la API genera N variantes con datos" está consolidado y es barato:

| Plataforma | Precio de entrada | Notas |
|---|---|---|
| **Templated** | USD 29/mes · 1,000 renders | El más económico del grupo |
| **Bannerbear** | USD 49/mes · 1,000 créditos (≈ USD 0.049/imagen) | El pionero; API madura, integra con Zapier/Make |
| **Creatomate** | ~USD 41/mes · 2,000 créditos | Imagen + video + PDF |
| **Imejis.io** | USD 14.99/mes (solo imagen) | El más barato |

**Por qué no son la respuesta aquí, y por qué igual importan:** todos producen un **PNG/JPG final**, no un diseño editable en Canva. El requisito #1 del cliente ("mantén el resultado editable dentro de Canva, quiero mover productos") los descalifica como solución principal. Pero son una **vía de contingencia perfectamente válida** si la integración con Canva se vuelve inviable: coste marginal por flyer de ~5 centavos.

## 5. MCP (Model Context Protocol): la capa nueva de 2026

Este es el cambio más relevante para el proyecto y no existía cuando se hicieron las propuestas anteriores que el cliente rechazó.

### 5.1 Canva MCP oficial

Canva publicó su propio servidor MCP (`canva.dev/docs/mcp`). Expone: generación de diseños, **edición de diseños**, búsqueda, carpetas, exportación, gestión de assets y comentarios.

Lo decisivo son los **requisitos de plan**, que se detallan en el documento 03:

| Grupo de herramientas | Plan requerido |
|---|---|
| Diseños, **edición**, assets, exportación, carpetas, comentarios | **Todos los planes** ✅ |
| Brand templates, resize | Pro o superior |
| **Autofill**, brand dataset | **Enterprise** ❌ |

Las operaciones de edición funcionan por **transacciones**: `start-editing-transaction` → `perform-editing-operations` → `commit-editing-transaction`, con límites de 20–50 peticiones/minuto. Están disponibles en **todos los planes**, incluido Pro.

### 5.2 Meta Ads MCP oficial

El **29 de abril de 2026** Meta abrió su stack publicitario a asistentes de IA con un servidor MCP oficial en `mcp.facebook.com/ads` y una CLI para desarrolladores. Cubre reportes, **creación y gestión de campañas**, catálogos, señales, tests A/B y logs de actividad. Lee **y escribe**.

Se conecta con el flujo OAuth estándar de Meta Business — sin registro de app de desarrollador, sin espera de app review, sin token que rotar. Hereda exactamente los permisos de quien autentica.

> Nota: existe información contradictoria en fuentes secundarias sobre si el MCP oficial existe (algunas de julio de 2026 afirman que no). Las fuentes que documentan el lanzamiento de abril son consistentes entre sí y describen la URL y el flujo concretos. **Debe verificarse en el primer sprint técnico** antes de comprometer arquitectura sobre esta pieza.

También existen implementaciones open source (`attainmentlabs/meta-ads-mcp`, `pipeboard-co/meta-ads-mcp`) y servicios gestionados de pago (Superscale desde USD 99/mes, Pipeboard con plan gratuito).

### 5.3 Dónde encaja MCP — y dónde no

Esta distinción evita un error de diseño frecuente:

| Capa | Herramienta | Por qué |
|---|---|---|
| **Pipeline productivo** (corre solo, todas las noches, sin humano) | **API directa** (Canva Connect / Graph API) | Debe ser idempotente, determinista, con reintentos y logs. Un protocolo pensado para conversación con un modelo no da esas garantías. |
| **Operación asistida** (consultas, ajustes puntuales, exploración) | **MCP** (Canva MCP + Meta Ads MCP) | Aquí brilla: "muéstrame el gasto de la tienda Blue Ridge este mes", "duplica este ad set con estas fechas". Ahorra construir UI. |

## 6. Orquestación: n8n vs. las alternativas

El cliente necesita un orquestador que dispare por email entrante, ejecute pasos condicionales, llame APIs y corra en cron a las 21:00.

| Plataforma | Modelo de cobro | Costo al volumen de Flymar | Veredicto |
|---|---|---|---|
| **n8n self-hosted (Community)** | Gratis, licencia fair-code. Workflows, ejecuciones y usuarios **ilimitados**. Solo pagas el VPS (USD 5–7/mes con Docker Compose). | **≈ USD 5–20/mes** | ✅ **Recomendado** |
| n8n Cloud Starter | USD 24/mes · 2,500 ejecuciones | ~USD 24/mes | Alternativa si no quiere administrar servidor |
| n8n Cloud Pro | USD 60/mes · 10,000 ejecuciones | Sobredimensionado | — |
| Make | Por *operación* | Se dispara al añadir pasos de IA | ⚠️ |
| Zapier | Por *tarea* | De 10K a 100K tareas puede costar USD 500+/mes | ❌ |

El argumento decisivo lo documenta un caso comparativo publicado: un cliente que procesaba 1,000 registros/día necesitaba el plan Teams de Make (USD 29/mes + excedentes), mientras que 30,000 ejecuciones mensuales entraban cómodas en n8n Pro. **Al añadir una capa de agente IA con 8 llamadas LLM por registro, el costo en Make se disparó y el de n8n se mantuvo plano** — porque n8n cobra por ejecución de workflow, no por paso.

n8n 2.0 además trae integración nativa con LangChain y 70+ nodos de IA, memoria persistente entre ejecuciones y patrones de *human-in-the-loop* — exactamente lo que este proyecto necesita.

## 7. Extracción de datos con LLM: qué esperar realmente

Aquí conviene ser honesto, porque es donde las propuestas infladas suelen prometer de más.

Los benchmarks públicos de 2026 sobre extracción estructurada muestran:

- **Precisión a nivel de campo: 65–80 %**, con un agregado de ~72.9 % en extracciones válidas.
- **Precisión a nivel de documento completo (todos los campos correctos): 4.6 %** en uno de los benchmarks.
- En dominios acotados con esquema estricto (extracción de propiedades de materiales científicos), la precisión supera el **90 %**.
- **La estrategia de prompting importa más que el tamaño del modelo.**
- Varios benchmarks populares tienen errores en su propio *ground truth*, lo que infla o desinfla resultados.

### Qué significa esto para Flymar

1. **La revisión humana no es opcional, es parte del diseño.** Cualquier propuesta que prometa "cero intervención" desde el día uno está vendiendo humo. El objetivo correcto es reducir 45 minutos a 5, no a 0.
2. **Pero este caso está del lado fácil del espectro.** El dominio es cerradísimo: 4 categorías, ~15 productos, 5 sintaxis de precio, un catálogo finito de unidades. Con *structured outputs* (esquema JSON forzado), validación determinista posterior (regex de precios, catálogo de unidades) y un glosario de traducción controlado, es razonable apuntar a **> 95 % de campos correctos** — muy por encima del benchmark genérico.
3. **El diseño debe exponer confianza por campo**, no un blob que hay que revisar entero. La operadora revisa lo dudoso, no todo.

## 8. Imágenes generadas por IA: el terreno legal en 2026

Relevante porque el cliente tiene un incidente activo y está construyendo su banco propio.

- **`Thaler v. Perlmutter`**: en marzo de 2026 la Corte Suprema de EE. UU. declinó conocer el caso, dejando en pie los fallos inferiores: **un sistema de IA no puede ser titular de copyright**. Una obra que combina material generado por IA con aportación creativa humana sustancial sí recibe protección, limitada a la porción humana.
- **Los riesgos operativos reales no son de copyright, sino de disclosure y likeness**: FTC, EU AI Act, regulación de Nueva York, políticas de Etsy y de **Meta** exigen declarar contenido generado por IA; el derecho de imagen y GDPR aplican a personas reconocibles.
- **Para fotos de producto alimenticio en particular**, los riesgos concretos son: marcas y logotipos, mascotas de marca, parecidos con celebridades, personas privadas reconocibles y personajes con copyright.
- **Si un modelo se entrenó con imágenes con copyright y la salida resulta sustancialmente similar a una de ellas**, el titular original puede reclamar.
- **Adobe Firefly** se entrenó sobre la biblioteca licenciada de Adobe Stock y **Adobe ofrece indemnización** — es la opción más segura para uso comercial.
- **La edición humana del resultado cumple doble función:** mejora la calidad y refuerza la posición legal.

---

## Conclusiones del benchmark

1. **La dirección es correcta y está validada por la industria.** Fresco Marketing quiere en pequeño lo que WPP hace en grande.
2. **Existe un vacío de producto** entre las herramientas empresariales de circulares (LAGO, Swiftly) y las agencias pequeñas. Lo que se construya aquí es potencialmente revendible.
3. **Las piezas nuevas de 2026 (Canva MCP con edición en todos los planes, Meta Ads MCP oficial) cambian el cálculo** frente a la propuesta de USD 5,000 + 500/mes que el cliente rechazó. Buena parte de lo que antes había que construir, hoy se conecta.
4. **n8n self-hosted es la elección de orquestación correcta** por modelo de cobro: el costo no escala con la cantidad de llamadas a IA.
5. **La revisión humana debe estar en el diseño**, no como parche. Los benchmarks de extracción lo exigen, y además es lo que el propio cliente pidió ("me gustaría revisarlo primero").
6. **El riesgo legal de las imágenes es el punto más urgente y el más barato de mitigar.** Auditar la biblioteca actual cuesta horas, no dinero, y el precedente ya existe.

---

## Fuentes

**Industria y holdings**
- [eMarketer — FAQ on ad agencies: Consolidation, AI disruption, and what's changing in 2026](https://www.emarketer.com/content/faq-on-ad-agencies--consolidation--ai-disruption--what-s-changing-2026)
- [AdAge — How AI is transforming agencies from creative partners into tech-fueled vendors](https://adage.com/technology/ai/aa-ad-agencies-operating-systems/)
- [Aragil — Agentic AI in Advertising: WPP, Omnicom & The 2026 Shift](https://www.aragil.com/blog/agentic-ai-the-new-operating-system-for-ad-spend)
- [Klover.ai — WPP AI Strategy](https://www.klover.ai/wpp_ai_strategy_analysis_of_dominance_in_marketing_communications_ai/)

**ROI de automatización creativa**
- [Storyteq — What is the ROI of automating creative processes?](https://storyteq.com/blog/what-is-the-roi-of-automating-creative-processes)
- [Superside — Automated Creative Production for Faster, On-Brand Growth in 2026](https://www.superside.com/blog/automated-creative-production)
- [Luma Labs — 33 Creative Production Time Statistics](https://lumalabs.ai/news/creative-production-time-statistics)
- [Thunderbit — Marketing Automation in 2026: 45 Stats and Insights That Drive ROI](https://thunderbit.com/blog/marketing-automation-stats-insights-roi)

**Circulares de supermercado**
- [Comosoft LAGO — Circular Ad Production Software](https://www.comosoft.us/lago/production/circular-ad-production-software/)
- [SalesTechStar — Swiftly Launches SmartCircular](https://salestechstar.com/predictive-ai-artificial-intelligence/swiftly-launches-smartcircular-transforming-the-weekly-grocery-circular-into-an-ai-powered-precision-revenue-engine/)

**Plataformas de creative automation**
- [Templated — Best Bannerbear Alternatives](https://templated.io/blog/best-bannerbear-alternatives/)
- [Imejis.io — 10 Best Bannerbear Alternatives in 2026](https://www.imejis.io/blogs/comparisons/best-bannerbear-alternatives)
- [Creatomate — Bannerbear Alternative](https://creatomate.com/compare/bannerbear-alternative)

**MCP**
- [Canva — Model Context Protocol (MCP) Documentation](https://www.canva.dev/docs/mcp/)
- [Soku — Meta Ads AI Connectors: Official MCP Setup for Ad Teams](https://soku.ai/blog/meta-official-mcp-ai-ad-teams)
- [AdAdvisor — mcp.facebook.com/ads: Set Up the Official Meta MCP (2026)](https://adadvisor.ai/blog/mcp-facebook-com-ads-official-meta-setup)
- [GitHub — attainmentlabs/meta-ads-mcp](https://github.com/attainmentlabs/meta-ads-mcp)
- [Superscale — Meta Ads MCP: manage Meta ads with Claude (2026)](https://superscale.ai/learn/meta-ads-mcp/)

**Orquestación**
- [No Code MBA — n8n Pricing 2026: Cloud Plans, Self-Hosting Costs](https://www.nocode.mba/articles/n8n-pricing)
- [Northflank — How to self-host n8n: Setup, architecture, and pricing guide](https://northflank.com/blog/how-to-self-host-n8n-setup-architecture-and-pricing-guide)
- [Digital Applied — Marketing Automation AI Agents: Make vs Zapier vs n8n](https://www.digitalapplied.com/blog/marketing-automation-ai-agents-make-zapier-n8n-2026)
- [AdLibrary — n8n Meta Ads Automation Recipes 2026](https://adlibrary.com/posts/n8n-meta-ads-automation-recipes)

**Extracción con LLM**
- [The Structured Output Benchmark (arXiv 2604.25359)](https://arxiv.org/html/2604.25359v1)
- [ExtractBench: A Benchmark and Evaluation Methodology for Complex Structured Extraction (arXiv 2602.12247)](https://arxiv.org/html/2602.12247v2)
- [Cleanlab — LLM Structured Output Benchmarks are Riddled with Mistakes](https://cleanlab.ai/blog/structured-output-benchmark/)

**Legal / imágenes**
- [Nightjar — The Legal Guide to AI Product Photography in 2026](https://nightjar.so/blog/ai-product-photography-legal-guide)
- [P20V — AI-Generated Images: Copyright, Licensing & Commercial Use Guide (2026)](https://p20v.com/blog/ai-generated-images-copyright-licensing-commercial-guide)
- [Kaboompics — Can You Use AI and ChatGPT Images Commercially in 2026](https://blog.kaboompics.com/can-you-use-ai-generated-images-for-commercial-use/)

---

**Siguiente:** [03 · Viabilidad técnica](./03-viabilidad-tecnica.md)
