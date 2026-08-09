# 05 · Revisión crítica de las propuestas recibidas

**Documentos revisados:**
- `propuestas/2026-05-26-cotizacion-catalog-generator.pdf` — Andrés Escobedo Lara, 3 de junio de 2026
- `propuestas/url-propuesta-2.txt` → https://hexio-fresco-flyers.netlify.app/ — Hexio Cloud & AI, junio de 2026

**Fecha de la revisión:** 2026-08-08

> Esta revisión evalúa **viabilidad técnica y ajuste al problema**, no la calidad comercial de los proveedores. Ambas propuestas están hechas por gente competente y ambas contienen ideas correctas. Donde señalo un problema, es porque tiene consecuencia práctica para Fresco Marketing, no porque el enfoque sea "malo".

---

## Nota previa: una cifra que circulaba mal

El resumen automático de la reunión registra *"rechazó propuestas previas por costo (USD 5,000 + USD 500/mes; **otra de USD 15,000 + USD 1,500–2,000/mes**)"*.

Al revisar la transcripción, la segunda cifra **no corresponde a ninguna propuesta recibida**. Aparece en el minuto 47:05, dicha por el propio consultor en tono de broma al cerrar la reunión, comentando que el alcance había crecido durante la conversación:

> *"Ahora tu aplicación ya no va a costar 5,000 USD, sino 15,000 USD y … 1,500 USD mensuales."*

**La realidad documentada es:**

| Propuesta | Inversión declarada |
|---|---|
| Andrés Escobedo Lara | **USD 5,000 setup + USD 500/mes** |
| Hexio Cloud & AI | **Sin precio.** El sitio dice "Aún no disponible" y la Fase 01 es, literalmente, el análisis de costos. |

Esto cambia la conversación: **solo existe una cifra real sobre la mesa**, y la segunda propuesta todavía no es comparable porque no cotiza.

---

## Propuesta A · Andrés Escobedo Lara — "Generador de Catálogos Automático"

### Qué propone

Un dashboard web propio donde se elige el cliente, se sube el Excel, el sistema arma el catálogo, el equipo revisa y aprueba, y se exporta en JPG y PDF. Tres motores: lectura del Excel, matching de imágenes con memoria que aprende, y reproducción del template del cliente.

**Inversión:** USD 5,000 (50/50 firma y entrega) + USD 500/mes. Alcance del piloto: **un template**, **solo Excel**.

### Lo que está bien, y bien de verdad

1. **El diagnóstico es correcto.** "El catálogo se arma imagen por imagen" es exactamente el cuello de botella. No hay confusión sobre cuál es el problema.
2. **El motor de imágenes con memoria es la idea correcta.** Que cada aprobación o corrección se recuerde, y que el llenado mejore semana a semana porque los productos se repiten, es precisamente el mecanismo que hace que este tipo de sistema funcione a mediano plazo. Coincide con lo que propongo en el documento 04.
3. **El human-in-the-loop está bien planteado.** "El equipo solo supervisa y aprueba" es la postura honesta; no promete cero intervención.
4. **El sistema ya está construido y activo.** Esto reduce muchísimo el riesgo de ejecución y el plazo de entrega. Es una ventaja real y no debe subestimarse.
5. **El alcance de plataforma es sensato**: panel de clientes, biblioteca gestionable, recordatorios automáticos por correo, retomar borradores. Todo eso es pertinente.
6. **La presentación es excelente** y el precio, para lo que ofrece, no es abusivo.

### Los problemas, en orden de gravedad

#### 1. No entrega en Canva — y ese era el requisito número uno

La página 4 lo dice sin ambigüedad:

> *"ARMA EL FLYER IDÉNTICO A SU DISEÑO — Reproduce el template del cliente al detalle: mismo layout, misma tipografía, los círculos de precio, las franjas. […] Exporta en JPG y PDF."*

Esto es un **motor de renderizado propio que replica** el diseño de Canva. No es Canva. Canva no aparece mencionado como destino en ninguna de las nueve páginas.

Contrastar con el PDF que Fresco Marketing envió a los proveedores:

> *"Genere automáticamente el flyer utilizando mis plantillas. **Mantenga el resultado editable dentro de Canva.** Permita mover productos y realizar cambios manuales cuando sea necesario."*

Y con lo que se dijo en la reunión (minuto 04:05): *"Puedes vaciar toda la información aquí dentro de Canva… a veces me dicen: yo quería el melón acá en vez del cilantro, entonces lo tengo que cambiar."*

La propuesta sí ofrece corrección: *"si una imagen no convence, o si algún precio o nombre no quedó como debería, se corrige a mano con un clic antes de aprobar"*. Pero eso es un formulario de override sobre campos predefinidos — un subconjunto muy inferior a la libertad de un editor gráfico. Reordenar productos, cambiar el fondo por una imagen de festividad, mover un elemento tres píxeles, ajustar el encuadre de una foto: nada de eso está contemplado.

**Consecuencia operativa:** cada vez que el diseño necesite un cambio que el formulario no cubra —y en la reunión se mencionan varios como rutinarios— hay que pedírselo al proveedor. Eso convierte el retainer de USD 500/mes en una dependencia estructural, no en un servicio opcional.

#### 2. No traduce — y la traducción es la fuente del dolor de las correcciones

Página 5:

> *"BILINGÜE RESPETADO — Los nombres salen tal como vienen — español, inglés o ambos. **El sistema no traduce ni inventa**: respeta lo que el cliente manda."*

Presentado como virtud, es en realidad una limitación. Los insumos reales lo demuestran: en `Ejemplo 2.pdf` la mayoría de productos vienen **en un solo idioma** (`RAMBUTAN`, `CHAYOTE`, `PEPSI 12PK`, `KELLOGGS CORN FLAKES`). El flyer necesita ambos.

Y en la reunión (minuto 40:46) se dijo lo contrario de lo que la propuesta ofrece:

> *"Me gustaría que me lo enviaran en inglés y en español, los nombres ya traducidos, pero **por ese entonces la inteligencia artificial lo puede hacer**."*

**Consecuencia:** con esta propuesta, escribir la segunda mitad de cada nombre sigue siendo trabajo manual. Y como el dolor #4 documentado son las faltas de ortografía en el feedback del manager, ese dolor queda **intacto**.

#### 3. El piloto solo lee Excel — pero el problema real son tres formatos

Página 8, letra chica: *"ENTRADA DE DATOS: Excel (foto/PDF en fase 2)"*.

El PDF original de Fresco abre diciendo: *"Los clientes envían ofertas en Excel, Google Sheets, PDF o imágenes."* De los tres anexos compartidos, **dos no son Excel**.

**Consecuencia:** el piloto no cubre el caso de uso real. Las sucursales que mandan PDF o foto siguen procesándose a mano hasta una fase 2 que no está cotizada.

#### 4. Un template incluido; los otros son add-on sin precio

Página 8: *"ALCANCE: 1 template en el piloto"* · *"CLIENTES EXTRA: Add-on por template"*.

En la reunión se describen **5 plantillas** para ofertas de dos días y **7** para las de fin de semana, y se aclara que *"en otras sí cambia ya un poco más la plantilla"*.

**Consecuencia:** el costo real de cubrir la cartera no son USD 5,000. Son USD 5,000 más N add-ons cuyo precio no aparece. Sin ese número, la propuesta no se puede evaluar económicamente.

#### 5. Meta queda completamente fuera

Ni publicación, ni anuncios, ni reportes. Nada en las nueve páginas.

Esto es entendible —la propuesta se cotizó el 3 de junio, y el tema de Meta se desarrolla en la reunión de agosto— pero hay que decirlo con claridad: **el dolor #2, que es el único con impacto medible en dinero (el 25 % de vigencia perdida en cada campaña de dos días), no se toca.**

#### 6. La "fase 2" apunta en una dirección que ya fue rechazada

Entre lo que sigue después figura: *"Aprobación del supermercado por link compartible"*.

En la reunión (minuto 40:13) eso se descartó con un argumento sólido:

> *"No, pero eso siento que va a ser más difícil. En cada sucursal hay muchos managers y a veces los cambian. Me ha costado mucho trabajo, primero, que me lo envíen por email."*

#### 7. Los USD 500/mes no son costo de infraestructura

Página 8: *"OPERACIÓN — MENSUAL: Cubre infraestructura y hosting, soporte continuo, mejoras del motor de imágenes y mantenimiento."*

A este volumen —60 catálogos al mes, un dashboard, una biblioteca de imágenes— la infraestructura real cuesta entre USD 20 y 50 mensuales. Los otros ~USD 450 son **retainer de soporte y evolución**, que es un modelo de negocio perfectamente legítimo. Pero conviene nombrarlo como lo que es: no se está pagando servidores, se está pagando disponibilidad del proveedor. Sobre tres años son **USD 23,000**.

### Veredicto sobre la propuesta A

**Técnicamente viable y con bajo riesgo de ejecución** — el sistema existe, funciona, y el proveedor entiende el problema.

**Pero resuelve un problema adyacente al que se planteó.** Es un *generador de catálogos*: sube Excel, obtén JPG. Lo que Fresco Marketing pidió es *automatizar su flujo actual sin salir de Canva*. Son cosas distintas, y la diferencia no es cosmética: define quién controla el diseño y quién depende de quién.

**Sería una buena compra si —y solo si—** Fresco Marketing estuviera dispuesta a abandonar Canva como herramienta de producción y aceptar que el diseño vive dentro del sistema del proveedor. Esa es una decisión de negocio legítima, pero es una decisión distinta a la que se creyó estar tomando.

Dato menor pero práctico: **la propuesta se emitió el 3 de junio con validez de 30 días. Ya está vencida.**

---

## Propuesta B · Hexio Cloud & AI

### Qué propone

Una plataforma multicliente que combina automatización estructurada, IA y revisión humana, para transformar las ofertas semanales en **diseños editables en Canva**. Arquitectura en seis capas, seis módulos funcionales y un plan de cuatro fases.

| Capa | Tecnología declarada |
|---|---|
| Frontend | Angular, UI responsive |
| Backend | API modular con autenticación |
| IA | Gemini, modelos multimodales, embeddings |
| Datos | PostgreSQL, pgvector, almacenamiento S3 |
| Diseño | **Canva Autofill** · "Canva App como evolución" |
| Automatización | Cola de trabajos, scheduler, email |

**Metas declaradas:** +60 % de reducción de tiempo, +90 % de precisión en productos y precios, +80 % de matching automático de imágenes.

### Lo que está bien — y es notablemente bueno

1. **La arquitectura es la correcta.** Ingesta multi-formato, extracción con modelos multimodales, embeddings para el matching, `pgvector`, bandeja de revisión para casos ambiguos, validación por confianza. Coincide casi punto por punto con lo que propongo en el documento 04. **Esta gente sabe lo que hace.**
2. **Sí apunta a Canva como destino**, que es el requisito que la propuesta A no cumple.
3. **Las metas son honestas.** 90 % de precisión, no 100 %. 80 % de matching, no "todo automático". Quien pone 90 % en un documento comercial entiende el problema real de la extracción con modelos de lenguaje.
4. **Contemplaron el riesgo de Canva.** La Fase 02 incluye explícitamente *"validación técnica de Canva"*. No lo dieron por sentado — es un riesgo gestionado, no ignorado. Es más de lo que suele verse.
5. **El portafolio respalda la capacidad.** LumIA —analítica conversacional sobre 200,000 puntos de luz de alumbrado público, con lenguaje natural a SQL auditable, visualización geográfica y control de acceso— no es un proyecto trivial.

### Los problemas

#### 1. La pieza central está bloqueada por el plan de Canva del cliente

La integración de diseño se declara así:

> *"Vamos a crear el documento final mediante campos de texto e imagen configurados en cada plantilla. **Canva Autofill** · Canva App como evolución."*

**La Autofill API de Canva requiere plan Enterprise.** Los planes Free, Pro y Teams no tienen acceso al endpoint. Fresco Marketing tiene **Canva Pro** — confirmado en la reunión, minuto 44:56.

Ver el [documento 03 §1](./03-viabilidad-tecnica.md) para la cita textual de la documentación oficial de Canva.

Dos matices, en favor de la propuesta:

- **"Canva App como evolución" es la vía correcta.** El Apps SDK sí funciona en todos los planes. Solo que está planteado como evolución posterior, no como plan base.
- **La Fase 02 iba a validarlo.** No es que lo ignoraran.

Pero el orden es incómodo: se contrata, se paga la Fase 01 de análisis, se entra en Fase 02 y **ahí** se descubre que la pieza central no está disponible. En ese punto las salidas son rediseñar sobre el Apps SDK (trabajo no previsto) o pedirle a Fresco que contrate Canva Enterprise (USD 2,000–30,000 al año). Ninguna de las dos es una conversación agradable a mitad de proyecto.

**Esto es verificable en diez minutos de documentación oficial, antes de escribir la propuesta.** Es la observación más importante de esta revisión.

#### 2. No es una propuesta: es una invitación a una fase de descubrimiento

- Sin precio: *"Aún no disponible"*, *"Consultar inversión"*.
- Sin plazos ni cronograma.
- Sin composición de equipo.
- La Fase 01 **es** el análisis de costos.

Con esto, Fresco Marketing no puede decidir. Y coincide exactamente con lo que se dijo en la reunión: *"Le dije: hay que seguir en esto para ver qué onda. Ella nunca me contestó"* (minuto 11:57).

#### 3. Meta queda fuera, igual que en la propuesta A

Ninguna mención a publicación, anuncios, campañas o reportes.

#### 4. No hubo demo

De la reunión (minuto 09:24): *"Pero nunca me dio como el demo para yo ver… él me lo mostró que sí servía, pero con 2 productos."*

Una arquitectura bien documentada es valiosa, pero no demuestra que el matching de imágenes funcione con el catálogo real ni que la extracción soporte el Excel maquetado que manda el manager.

#### 5. Está sobredimensionada para el problema

Angular, API modular con autenticación, seis módulos, gestión de usuarios, plataforma multicliente. Es arquitectura de producto SaaS.

El problema real: 7 tiendas, ~60 flyers al mes, dos o tres personas usándolo. Esa distancia entre la solución planteada y el tamaño del problema es exactamente lo que produce presupuestos que asustan — y es probablemente la razón por la que la Fase 01 empieza por "análisis de costos" en lugar de por un número.

### Veredicto sobre la propuesta B

**El equipo más fuerte técnicamente de los dos**, con la arquitectura correcta y el destino correcto (Canva editable).

**Pero la propuesta no es accionable**: no cotiza, no fecha, no demuestra. Y su pieza central depende de una API que el plan de Canva del cliente no habilita — un bloqueo que se detecta leyendo una página de documentación.

Si Hexio replanteara sobre el **Apps SDK / Design Editing API** y pusiera un número y un plazo, sería un competidor serio.

---

## Lo que ninguna de las dos resuelve

Esta es la observación transversal, y quizá la más útil:

| Dolor documentado | Propuesta A | Propuesta B |
|---|:---:|:---:|
| Producción manual del flyer | ✅ | ✅ |
| Resultado editable en Canva | ❌ (render propio) | ⚠️ (vía bloqueada) |
| Traducción bilingüe automática | ❌ (explícitamente no) | — (no se menciona) |
| Lectura de PDF y foto | ⚠️ (fase 2, sin cotizar) | ✅ |
| Múltiples plantillas por tienda | ⚠️ (add-on sin precio) | ✅ |
| **Retraso del anuncio en Meta** | ❌ | ❌ |
| Reportes mensuales de campañas | ❌ | ❌ |
| Trazabilidad de estados (Slack/Asana) | ⚠️ (parcial) | ⚠️ (parcial) |
| Riesgo legal del banco de imágenes | ❌ | ❌ |

**Ambas atacan la mitad visible del problema.** La producción del flyer es lo que duele todos los martes y jueves, y es natural que sea lo primero que se cotiza. Pero **el retraso del anuncio en Meta es el único dolor con impacto medible en dinero**: 25 % de la vigencia de cada campaña de dos días, sobre ~60 campañas al mes, con presupuesto que se gasta fuera de la ventana en que la oferta existe. Ninguna de las dos lo menciona siquiera.

### Un patrón común de fondo

Las dos son **propuestas de producto**, no de automatización del flujo existente:

- Ambas construyen una plataforma nueva donde vive el trabajo.
- Ambas implican que el proveedor mantiene esa plataforma indefinidamente.
- Ninguna deja a Fresco Marketing con capacidad de operar o modificar el sistema.

Eso explica el retainer mensual y la dependencia. Es un modelo válido y muy común, pero choca con dos cosas que se dijeron en la reunión: *"una vez que tenga armado esto, prácticamente mis ofertas son lo mismo, no necesito tantos cambios"* y *"tú me dijiste que me podías enseñar"*.

---

## Recomendación

1. **Ninguna de las dos propuestas debería aceptarse tal como está.** La A no cumple el requisito de Canva y deja fuera Meta y la traducción; la B no tiene precio y su vía técnica principal está bloqueada.

2. **Si se quisiera reconsiderar la propuesta A**, hay tres preguntas que deberían responderse por escrito antes de firmar:
   - ¿El flyer terminado se puede abrir y editar en Canva, sí o no? Si es no, ¿qué pasa cuando hace falta un cambio de diseño que el formulario no contempla?
   - ¿Cuánto cuesta cada template adicional? Con 5–7 plantillas, ¿cuál es el total real?
   - ¿Cuánto cuesta añadir lectura de PDF y foto, y traducción automática EN⇄ES?

3. **Si se quisiera reconsiderar la propuesta B**, la pregunta es una sola: *¿Cómo se resuelve que la Autofill API requiere Canva Enterprise y nosotros tenemos Pro?* La respuesta correcta existe (Apps SDK / Design Editing API), y la calidad de esa respuesta dirá mucho sobre el proveedor.

4. **El precio de referencia real es USD 5,000 + 500/mes**, no los 15,000 que quedaron registrados por error en el resumen de la reunión. Sobre tres años son USD 23,000.

5. **Lo más valioso de ambas propuestas es gratis:** la idea del motor de imágenes con memoria (A) y la arquitectura de extracción con embeddings y validación por confianza (B) son correctas, y están incorporadas en la propuesta del [documento 04](./04-propuesta-de-solucion.md). No hay que pagar USD 5,000 por tener esas ideas; hay que ejecutarlas sobre la vía técnica que sí está disponible con Canva Pro, y extender el alcance a Meta, que es donde está el dinero medible.

---

← [04 · Propuesta de solución](./04-propuesta-de-solucion.md) · [Índice](../README.md)
