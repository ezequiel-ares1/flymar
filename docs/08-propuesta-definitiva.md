# 08 · Propuesta definitiva — Flymar

Revisada tras la segunda reunión del 2026-08-08. Sustituye al [documento 04](./04-propuesta-de-solucion.md).

> **Nota de método.** Lo que Marcela dijo sobre *cómo* debería funcionar esto ("una encuesta en el celular", "que se parezca a mi diseño", "que no sepan que uso una herramienta") son ideas, no requisitos. Ella aporta el problema; el diseño nos toca a nosotros. Donde la propuesta se aparta de lo que sugirió, se dice por qué.

---

## 1 · Qué cambió

| Señal de la reunión | Qué es | Consecuencia |
|---|---|---|
| *"Si la solución es más sencilla en otro lado y se parece el diseño… ese diseño yo se lo puedo proponer a mi cliente"* | **Restricción levantada** | Se desbloquea el motor de composición propio. Canva deja de ser obligatorio. |
| *"Una encuesta en el celular… que ya sea de una"* | **Restricción levantada** | Deja de defender el email como canal único. Se puede intervenir en el origen. |
| *"Acaban de abrir otras 2 tiendas… no voy a poner a todo mi equipo a hacer esto, no es negocio"* | **Restricción dura y nueva** | Escalar sin sumar personal condiciona toda la arquitectura. |
| *"Necesito que ellos sientan que me necesitan… que no sepan que uso una herramienta"* | **Preocupación legítima, táctica equivocada** | Se resuelve, pero con atribución de ventas, no con secretismo (§2). |

**Sigue vigente:** sin equipo técnico para mantener, presupuesto acotado, y el email como carril de respaldo permanente.

---

## 2 · Qué se vende — la reinvención del modelo

Esta sección no es técnica y es la que sostiene todo lo demás.

**El miedo de Marcela es real:** si el manager ve que el flyer "se arma solo", puede pensar que no necesita la agencia. Pero **esconder la herramienta es una defensa frágil**: no escala (cuanto mejor funcione, más evidente será), un secreto no es un foso, y defiende la producción del flyer — justo lo que se está volviendo commodity en toda la industria.

Las cinco razones por las que un cliente elige una agencia, de la más frágil a la más defendible:

| | Razón | Defensa |
|---|---|---|
| ① | Sabe usar la herramienta | ❌ Es lo que se está evaporando |
| ② | Le ahorra tiempo | ❌ Se compara en precio |
| ③ | Tiene los accesos y la operación | ⚠️ Fricción de salida, no valor de entrada |
| ④ | **Sabe qué funciona** | ✅ Requiere histórico. No se improvisa ni se compra |
| ⑤ | **Ve diez tiendas donde el manager ve una** | ✅✅ Estructuralmente inaccesible para el competidor |

La quinta es el activo que Fresco ya tiene y no capitaliza. Permite decir lo que nadie más en su mercado puede:

> *"El aguacate ya está en cinco de las tiendas que manejamos esta semana. Te propongo mango ataulfo: nadie lo tiene y en mayo rota."*

Y mejora con cada tienda nueva: **el crecimiento que hoy le pesa se convierte en la fuente del diferencial.** (Siempre agregado y anonimizado — nunca revelar datos de una tienda a otra.)

### La escalera de cobro

Cambiar de herramienta sin cambiar el modelo de cobro solo abarata la misma cosa.

| Escalón | Modelo | Requisito | Cuándo |
|---|---|---|---|
| 0 | Fee por pieza *(hoy)* | — | — |
| 3 | **Suscripción mensual por tienda** | El sistema propuesto | **Mes 4–6** |
| 4 | Base fija + variable por venta incremental | Atribución (§4) | Mes 7–12 |
| 5 | Retail media: cobrar al CPG | 30+ tiendas y medición | Mes 12+ |

El escalón 3 es el siguiente paso realista: deja de ser comparable pieza a pieza, hace los ingresos predecibles y **el margen crece con la escala**, porque el costo marginal por tienda cae.

**En una frase:** un formulario fácil no debilita a la agencia — debilita a la agencia que solo vendía llenar formularios.

---

## 3 · El flujo

### 3.1 El principio: proponer decisiones, no pedir datos

Marcela imaginó un formulario que el manager llena. **Un formulario traslada la transcripción en vez de eliminarla** — antes transcribía Fresco, ahora transcribiría el manager. Y 16 productos en un teléfono siguen siendo 10 minutos.

El trabajo real del manager no es escribir *"pierna de pollo, 0.89, LB"*. Es **decidir qué conviene ofertar**. El sistema debe proponer decisiones y recibir una elección.

Jerarquía de esfuerzo, que baja a medida que el sistema acumula histórico:

```
25 min   Hoy: arma un Excel                              —
10 min   Formulario en blanco                       semanas 1–2 de cada tienda
 90 s    Revisar propuesta y ajustar 2–3 precios    semana 3+     ← objetivo
 20 s    Elegir entre plan A, B o C                 mes 2+
 10 s    Aprobar de un toque                        mes 4+ (con datos del POS)
```

El escalón de los tres planes respeta lo que Marcela dijo —*"ellos deciden sus ofertas"*— y sube el valor del servicio: el manager decide **entre opciones fundamentadas** en vez de improvisar.

### 3.2 El canal: WhatsApp avisa, la web trabaja

```
Martes 08:00 · WhatsApp del manager
┌─────────────────────────────────────────────┐
│ Fresco Marketing                            │
│ Su propuesta de ofertas de Metropolitan     │
│ para el 13–14 de mayo está lista.           │
│ 👉 fresco.mx/p/7Ka9dR   (válido 48 h)       │
└─────────────────────────────────────────────┘
                    ↓ un toque
┌─────────────────────────────────────────────┐
│ WEB RESPONSIVE · sin cuenta ni contraseña   │
│                                             │
│  CARNICERÍA          │  [ vista previa del  │
│  ⠿ Pierna pollo $0.89│    flyer, con su     │
│  ⠿ Discada     $2.89 │    logo, en vivo ]   │
│  ⠿ Caldo mixto $2.99 │                      │
│  ⠿ Chorizo     $____ │                      │
│    ↑ arrastrar para reordenar               │
│                                             │
│  PRODUCE  · GROCERY · BAKERY                │
│  Temporada: [ Ninguna ▾ ]                   │
│                                             │
│           [ Confirmar ]                     │
└─────────────────────────────────────────────┘
                    ↓ ~20 min
┌─────────────────────────────────────────────┐
│ ¡Listo! Aquí está su arte.                  │
│ [🖼 flyer]                                  │
│ ¿Lo publicamos hoy a las 9 pm?              │
│      [ Aprobar ]   [ Cambiar algo ]         │
└─────────────────────────────────────────────┘
```

**Regla:** si hay algo que mirar, se mira en la web. Si no hay nada que mirar, se responde en el chat.

| Acción | Dónde | Por qué |
|---|---|---|
| Revisar y confirmar la propuesta | **Web** | Debe ver lo que aprueba |
| Editar precios, reordenar, cambiar imagen | **Web** | Necesita interfaz |
| Aprobar el flyer terminado | **Chat** | El arte va como imagen — ya lo está viendo |
| *"Esta semana no"* · *"Recuérdame luego"* | **Chat** | No hay nada que revisar |

**Por qué la web y no un formulario dentro del chat (WhatsApp Flows):**

1. **Un solo frontend.** El editor hay que construirlo igual para el equipo de Fresco; el manager usa el mismo con permisos recortados. Los Flows serían una segunda interfaz que mantener.
2. **El manager ve el flyer.** Todo el argumento de §2 depende de que perciba trabajo hecho. Un formulario de campos no lo transmite; su arte con su logo, sí.
3. **Los Flows no pueden hacer lo que hace falta:** no admiten arrastrar para reordenar, ni vista previa en vivo, ni repetidores dinámicos.
4. **Iterar sin permiso.** Cada cambio en un Flow pasa por publicación y revisión de Meta. La web se despliega en segundos.
5. **El canal queda intercambiable de verdad.** El mismo link sale por SMS o email si Meta cambia las reglas.

**Detalles del link que importan:** vencimiento **por tiempo (48 h), nunca de un solo uso** — la vista previa de WhatsApp consumiría el token y el manager encontraría un enlace muerto. Sin datos sensibles en la URL. Y hay que probarlo en el navegador embebido de WhatsApp en iOS y Android reales, que se comporta distinto a Safari o Chrome.

### 3.3 Tres carriles, nadie queda fuera

```
A · Notificación + web         ← el objetivo
B · Email con Excel/PDF/foto   ← respaldo permanente, sin fecha de retiro
B'· Audio o foto por WhatsApp  ← el carnicero a las 6 am
C · API del POS de la tienda   ← el futuro, elimina la captura
```

El carril B no es de segunda: quien quiera seguir mandando su Excel puede hacerlo indefinidamente. Su flyer sale más tarde porque hay que interpretarlo y confirmarlo. **La estandarización se incentiva, no se impone.**

El audio se transcribe y estructura, pero **siempre con confirmación explícita** — un `$0.89` mal oído como `$8.90` publicado en Facebook es un problema comercial y legal.

### 3.4 De dónde sale la propuesta pre-llenada

| Fuente | Disponible | Aporta |
|---|---|---|
| Histórico de la tienda | Semana 3 | Qué ofertó, cuándo, a qué precio |
| Patrones estacionales | Mes 2 | Chile en septiembre, flores en mayo |
| Rendimiento medido (§4) | Mes 4 | Qué rotó de verdad |

Las dos primeras semanas de cada tienda el formulario va vacío. **Esa curva es la que construye la dependencia:** el sistema se vuelve más útil cuanto más lleva operando, y ese valor no es transferible a un competidor.

---

## 4 · De los likes a la venta

Hoy Fresco reporta engagement porque es lo único que Meta da sin píxel. Y el engagement **es falsificable con bots** — no defiende nada. Cuatro vías de atribución, de menor a mayor precisión:

| Vía | Cuándo | Qué da |
|---|---|---|
| Código de oferta en el arte | Inmediato | Señal direccional |
| **QR o cupón por producto destacado** | Mes 2 | Interés por producto, no por publicación |
| Pregunta en caja | Mes 2 | Tasa de conversión estimada |
| **Reporte de ventas por PLU del POS** | Mes 4+ | **Atribución real** |

La cuarta cierra la ecuación y no requiere ingeniería: requiere pedirle a **una** sucursal un CSV semanal.

```
Producto ofertado + campaña ejecutada + unidades vendidas esa semana
  = "Esta campaña movió 1,240 lb de pierna de pollo, 214 % más que la
     semana previa, con $38 de inversión."
```

El informe mensual con eso —lo que rotó, lo que no, y la recomendación para el mes siguiente— **es el entregable que sustituye al flyer como producto de la relación.** Y le muestra al manager el *qué*, nunca el *cómo*.

---

## 5 · El diseño

**Canva deja de ser obligatorio.** Tres caminos: seguir en Canva (ahora el más frágil: API en beta y ya no lo exige nadie), motor propio, o híbrido. **Recomendado: híbrido tres meses, motor propio como destino** — el sistema produce el flyer y quien quiera retocar a mano lo exporta a Canva, hasta que deje de hacer falta.

**No replicar el diseño actual.** Marcela dijo *"que se parezca lo más que se pueda"* y ambos proveedores organizaron su oferta alrededor de eso. Es la meta equivocada: **ese diseño se heredó, nunca se validó.** Nadie ha comprobado si ese orden o ese tamaño de foto venden más. Propuesta: respetar la identidad visual, corregir desde el inicio lo que la plantilla de Canva no permite (legibilidad del precio en miniatura, jerarquía entre producto héroe y relleno, formatos nativos 4:5 y 9:16), y **a partir del mes 4 optimizar con datos**. Ese es el argumento con el que Marcela le vende el cambio a su cliente: no *"cambiamos de herramienta"* sino *"esta versión vende más y te lo demuestro"*.

**La plantilla es un dato**, no un archivo: JSON versionado con colores, logo, rejilla por sección y zona de temporada. Habilita dar de alta una tienda nueva en minutos —con dos sucursales recién abiertas ya importa—, generar variantes en masa y que la IA opere el diseño (*"el melón donde está el cilantro"* es intercambiar dos nodos).

**Seis primitivas, alcance congelado:** encabezado, banda de categoría, rejilla, tarjeta de producto, chip de precio (5 sintaxis) y zona de temporada. Nada entra si no aparece en un flyer real.

**Render: Satori + resvg**, 50–200 ms sin navegador. 10 tiendas × 2 días × 4 formatos = 80 renders semanales ≈ **12 segundos de cómputo**.

**Editor: sin librerías de canvas ni licencias.** Ninguna corrección real del equipo necesita mover píxeles —cambiar un precio es editar un campo, *"el melón donde el cilantro"* es reordenar una lista, cambiar la foto es elegir de un selector—. Listas reordenables con vista previa en vivo, HTML corriente. Ventaja adicional sobre un editor de lienzo: **no permite descuadrar la plantilla.** Para el 2 % de casos raros, la salida es Canva, que ya existe.

---

## 6 · Arquitectura y stack

```
CAPTURA        WhatsApp (aviso) → web responsive        ← carril A
               Email · audio · foto                     ← carril B
               API del POS                              ← carril C
     ↓
NORMALIZACIÓN  Carril A: ya viene estructurado
               Carril B: extracción con visión + validación + confirmación
     ↓
CATÁLOGO       Producto canónico · PLU/UPC · ES/EN · alias
               Imagen con licencia · histórico de precios y rotación
               Postgres + pgvector · tenant_id desde el primer commit
     ↓
COMPOSICIÓN    Plantilla JSON → Satori+resvg → editor web
     ↓
APROBACIÓN     Flyer al chat · [Aprobar] [Cambiar]
     ↓
PUBLICACIÓN    21:00 · post → ad set → ad con object_story_id
               → a revisión de Meta esa noche  (+12 h de vigencia)
     ↓
MEDICIÓN       Insights + QR/cupón + (mes 4) datos del POS
```

| Capa | Elección |
|---|---|
| Aplicación | **Next.js + TypeScript** — un lenguaje, un proyecto: web del manager, tablero interno, API y render |
| Base de datos | **Postgres + pgvector** (Neon o Supabase), con `tenant_id` y aislamiento por fila desde el inicio |
| Notificación | **WhatsApp Business API**, detrás de un adaptador de canal |
| Extracción | **Claude Sonnet 5** (visión + structured outputs), ~USD 0.05/documento |
| Render | **Satori + resvg** |
| Editor | Listas reordenables + vista previa — sin librerías de canvas |
| Almacenamiento | **Cloudflare R2** (egreso gratuito) |
| Meta | **Marketing API** directa, con adaptador propio |

**Cuatro controles de calidad**, porque el desarrollo lo dirige una persona con agentes de IA: typecheck y tests (segundos), *golden files* de render con diff de píxeles (~1 min) y **evals de extracción sobre 50 documentos reales** (~2 min). Si la precisión baja del 95 %, el despliegue se detiene.

---

## 7 · Escalar de 7 a 30 tiendas

| Tiendas | Horas/mes manual | Horas/mes con el sistema | Personas |
|---:|---:|---:|---:|
| 7 | 42 | ~7 | 0.2 |
| 10 | 60 | ~10 | 0.3 |
| 20 | 120 | ~18 | 0.5 |
| 30 | 360 | ~25 | **0.7** |

El costo manual crece **linealmente**; con el sistema crece **sublinealmente**, porque catálogo, glosario e imágenes ya están construidos y los productos se repiten entre supermercados latinos. A 30 tiendas la diferencia son ~335 horas al mes: dos o tres empleados.

**Qué se rompe y cuándo:** hasta 15 tiendas, nada. De 15 a 25, revisar tantas propuestas se vuelve trabajo → aprobación automática por excepción para tiendas con histórico limpio. De 25 a 40, varias personas revisando → roles. Más de 40, otras agencias quieren usarlo → es la [propuesta V2](./06-propuesta-v2-plataforma.md).

---

## 8 · Plan por fases

**La captura va primero.** Arreglar el origen hace más fácil todo lo demás.

**Fase 0 · Fundaciones (semanas 1–2)**
Auditoría y catalogación del banco de imágenes con licencia y origen. Línea base: cronometrar 10 flyers y **preguntar a dos managers cuánto les toma preparar el Excel**. Alta de WhatsApp Business API y **prueba de categorización de la plantilla** (§10, R2). Diseño de la plantilla base como dato. Preguntar a las sucursales si ya publican su circular en Flipp, Freshop o Mercatus.

**Fase 1 · Captura (semanas 3–7)**
Web responsive con link de vencimiento por tiempo. Notificación por WhatsApp. Carril B: extracción de Excel/PDF/foto con evals desde el día uno, más audio. Recordatorio escalonado y marcas de tiempo. **Piloto en 2 sucursales**; las demás siguen por email sin cambios.
→ *El dato llega estructurado, el manager tarda 90 segundos y Fresco deja de transcribir. Es la mitad del ahorro con menos de la mitad del trabajo.*

**Fase 2 · Composición (semanas 8–12)**
Plantilla como dato con las seis primitivas. Render de las 9–10 sucursales. Editor de listas con vista previa. Salida a Canva como transición. Fan-out a 4:5 y 9:16.
→ *Menos de 5 minutos de trabajo humano por flyer.*

**Fase 3 · Publicación (semanas 13–16)**
Aprobación por WhatsApp. Publicación a las 21:00 y creación automática del anuncio (+12 h de vigencia, 0 pasos en Ads Manager). **DCO: 10–15 variantes por campaña** — Meta necesita al menos 10 para que el algoritmo aprenda; hoy recibe una.

**Fase 4 · Medición (semanas 17–22)**
QR y códigos de oferta. Informe mensual de rotación. **Conversación con una sucursal para obtener su CSV de ventas por PLU.** Primer informe con atribución real.

**Fase 5 · Inteligencia (mes 6+)**
Propuesta basada en rendimiento medido. Alta de tienda en minutos. Aprobación automática por excepción.

---

## 9 · Costos

| Concepto | 10 tiendas | 30 tiendas |
|---|---:|---:|
| Aplicación y base de datos | USD 20–30 | USD 40–60 |
| Almacenamiento e imágenes | 2–5 | 8–15 |
| WhatsApp (solo el aviso) | ~1 | ~3 |
| IA (extracción, carril B) | 3–6 | 8–15 |
| Dominio, correo, monitoreo | 5 | 10 |
| **Total** | **USD 31–47** | **USD 69–103** |

**Licencias: ninguna.** El editor no usa librerías de canvas ni SDK de pago.

**Sobre WhatsApp:** Meta factura por mensaje desde julio de 2025. En EE. UU. una plantilla *utility* cuesta ~USD 0.006 (~0.011 con margen del proveedor) y **la ventana de servicio de 24 h es gratuita**. Como solo se paga el mensaje que abre la conversación: ~86 avisos al mes a 10 tiendas ≈ **USD 1**.

**La comparación relevante ya no son las propuestas rechazadas, es contratar:**

| | Mensual | 3 años |
|---|---:|---:|
| Propuesta A (Escobedo) | 500 + add-ons | USD 23,000+ |
| **Una persona más** | 2,500–3,500 | **USD 90,000–126,000** |
| **Flymar** | 31–103 | **USD 1,300–4,000** |

---

## 10 · Métricas

| | Hoy | Mes 3 | Mes 6 |
|---|---|---|---|
| Minutos de Fresco por flyer | 45–60 | < 15 | **< 5** |
| Minutos del manager por envío | 20–30 | < 5 | **< 2** |
| Flyers sin feedback de error | por medir | > 85 % | **> 95 %** |
| Sucursales que responden antes del mediodía | por medir | 60 % | **> 90 %** |
| Vigencia efectiva (campaña de 48 h) | ~36 h | ~48 h | ~48 h |
| Variantes por campaña | 1 | 4 | **10–15** |
| **Tiendas por persona** | ~7 | 15 | **> 30** |
| **Escalón del modelo de cobro** | 0 | 0 | **3** |
| Sucursales con informe de rotación | 0 | 0 | **≥ 1** |
| Banco con licencia documentada | por medir | > 60 % | **100 %** |

*"Tiendas por persona"* responde a **"no es negocio"**. *"Escalón del modelo"* responde a **"que sientan que me necesitan"**. Los demás son medios.

---

## 11 · Riesgos

| # | Riesgo | Mitigación |
|---|---|---|
| R1 | **Meta clasifica la plantilla como *marketing*** — categoría bloqueada para números de EE. UU. desde abril de 2025, sin fecha de reapertura (error `63049`) | **Prueba en la semana 1, antes de construir encima.** Redacción estrictamente utility. Si falla: ritual iniciado por el manager (ventana de servicio, gratis y sin restricción) o el mismo link por SMS/email vía el adaptador |
| R2 | El manager no adopta la web | Tres carriles: nadie está obligado. Piloto en 2 sucursales y se mide. La propuesta pre-llenada hace que trabaje *menos*, no más |
| R3 | Se percibe que "el manager lo hace todo" | Marca blanca de Fresco, lenguaje de servicio, y el informe de rotación al frente de la relación (§2) |
| R4 | El navegador embebido de WhatsApp se comporta distinto | Probar en iOS y Android reales en la Fase 1, no en emulador |
| R5 | El diseño propio no convence al cliente final | Salida a Canva durante la transición; se valida con una sucursal antes de extender |
| R6 | No se consigue el dato del POS | Las otras tres vías de atribución no dependen de la tienda |
| R7 | Deriva de calidad de la extracción | Evals sobre 50 documentos reales bloqueando el despliegue |
| R8 | **Reclamación legal por una imagen** | Auditoría en Fase 0. Mayor severidad, menor costo de mitigar |
| R9 | Cambios en las APIs de Meta | Adaptador propio |

---

## 12 · Lo que no promete

1. **Cero intervención humana.** Con el carril A el dato llega limpio; con el B siempre habrá que confirmar. La meta son 5 minutos, no cero.
2. **Que el manager no note que hay un sistema.** Promete que **no le importe**, porque el informe de qué vendió no lo consigue en otro lado.
3. **Atribución exacta de ventas.** Sin píxel ni comercio electrónico se logra una señal fuerte y direccional; con datos del POS se acerca mucho. Nunca será atribución de e-commerce.
4. **Que las 10 sucursales adopten la web.** Por eso hay tres carriles.
5. **Resolver la situación legal de las imágenes.** Aporta el inventario y el control; retirar y reemplazar son decisiones del negocio.

---

## 13 · Primeros pasos

1. Confirmar el alcance: se levanta la restricción de Canva y la captura pasa a ser la Fase 1.
2. **Enviar la plantilla de WhatsApp a aprobación y verificar que Meta la clasifique como *utility*.** Cuesta un día y determina si el canal principal es viable. **No se construye la captura hasta tener la respuesta.**
3. Iniciar la verificación del negocio en WhatsApp Business (es lo que más tarda).
4. Elegir las **2 sucursales piloto** — idealmente una de las dos recién abiertas, sin costumbres que romper.
5. Acceso de lectura a Canva para inventariar el banco de imágenes.
6. **Preguntar a dos managers cuánto tiempo les toma preparar el Excel.** Es el número que justifica el cambio ante el cliente final.
7. Diseñar la plantilla base como dato y presentarle a Marcela la versión propia frente a la de Canva.
8. Definir con ella el guion hacia los managers: qué se les dice y qué no.
9. **Conversación aparte sobre el modelo de cobro.** Es la más importante y no depende del código: construir el sistema sin cambiar el modelo solo abarata la misma cosa.

---

## Anexo · Dónde nos apartamos de lo que sugirió Marcela

| Decisión | Lo que ella sugirió | Por qué |
|---|---|---|
| **Web responsive con link temporal** | Un formulario dentro del chat | Un solo frontend, el manager ve el flyer, y los Flows no admiten arrastrar ni vista previa |
| **Proponer decisiones (planes A/B/C)** | Que el manager llene una encuesta | Un formulario traslada la transcripción en vez de eliminarla. Él aporta criterio sobre *qué ofertar*, no sobre teclear |
| **Mejorar el diseño, no replicarlo** | *"Que se parezca lo más que se pueda"* | El diseño actual se heredó, nunca se validó. Optimizar por parecido congela una decisión no comprobada |
| **Atribución de ventas** | Ocultarle al manager que hay una herramienta | Un secreto no es un foso: no escala con la calidad y compite en el eje que se vuelve commodity |
| **Editor de datos con vista previa** | *(no lo planteó; yo había propuesto canvas)* | Ninguna corrección real necesita mover píxeles. Gratis, más rápido de construir, y no permite descuadrar la plantilla |

---

← [07 · Reinventar la captura](./07-reinventar-la-captura.md) · [Índice](../README.md)
