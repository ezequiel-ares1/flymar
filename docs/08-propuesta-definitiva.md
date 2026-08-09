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

| Modelo | Requisito | Cuándo |
|---|---|---|
| Fee por pieza *(hoy)* | — | — |
| **Suscripción mensual por tienda** | El sistema propuesto | **Mes 4–6** |
| Base fija + variable por venta incremental | Atribución (§4) | Mes 7–12 |
| Retail media: cobrar al CPG | 30+ tiendas y medición | Mes 12+ |

**La suscripción por tienda es el siguiente paso realista:** deja de ser comparable pieza a pieza, hace los ingresos predecibles y **el margen crece con la escala**, porque el costo marginal por tienda cae.

**En una frase:** un formulario fácil no debilita a la agencia — debilita a la agencia que solo vendía llenar formularios.

---

## 3 · El flujo

### 3.1 El principio: proponer decisiones, no pedir datos

Marcela imaginó un formulario que el manager llena. **Un formulario traslada la transcripción en vez de eliminarla** — antes transcribía Fresco, ahora transcribiría el manager. Y 16 productos en un teléfono siguen siendo 10 minutos.

El trabajo real del manager no es escribir *"pierna de pollo, 0.89, LB"*. Es **decidir qué conviene ofertar**. El sistema debe proponer decisiones y recibir una elección.

Jerarquía de esfuerzo, que baja a medida que el sistema acumula histórico:

```
~25 min  Hoy: arma un Excel                         ← estimación, a confirmar
                                                       con la línea base (§13, paso 6)
 10 min  Formulario en blanco                       primeras semanas de cada tienda
 90 s    Revisar propuesta y ajustar 2–3 precios    cuando hay histórico  ← objetivo
 20 s    Elegir entre plan A, B o C                 mes 3+
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
│ 👉 ofertas.<dominio>/p/7Ka9dR  (válido 48h) │
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

1. **Un solo componente de edición.** El editor de listas hay que construirlo igual para el equipo de Fresco; el manager reutiliza **ese componente** en su propia superficie, con permisos recortados y sin acceso al tablero (§3.6). Los Flows serían una segunda interfaz que mantener.
2. **El manager ve el flyer.** Todo el argumento de §2 depende de que perciba trabajo hecho. Un formulario de campos no lo transmite; su arte con su logo, sí.
3. **Los Flows no pueden hacer lo que hace falta:** no admiten arrastrar para reordenar, ni vista previa en vivo, ni repetidores dinámicos.
4. **Iterar sin permiso.** Cada cambio en un Flow pasa por publicación y revisión de Meta. La web se despliega en segundos.
5. **El canal queda intercambiable de verdad.** El mismo link sale por SMS o email si Meta cambia las reglas.

**Detalles del link que importan:** vencimiento **por tiempo (48 h), nunca de un solo uso** — la vista previa de WhatsApp consumiría el token y el manager encontraría un enlace muerto. Sin datos sensibles en la URL. Y hay que probarlo en el navegador embebido de WhatsApp en iOS y Android reales, que se comporta distinto a Safari o Chrome.

### 3.3 Tres carriles, nadie queda fuera

```
A · Notificación + web                          ← el objetivo
B · Formato libre: Excel, PDF, foto o audio,    ← respaldo permanente,
    por email o por WhatsApp                       sin fecha de retiro
C · API del POS de la tienda                    ← el futuro, elimina la captura
```

El carril B no es de segunda: quien quiera seguir mandando su Excel puede hacerlo indefinidamente. Su flyer sale más tarde porque hay que interpretarlo y confirmarlo. **La estandarización se incentiva, no se impone.**

El audio se transcribe y estructura, pero **siempre con confirmación explícita** — un `$0.89` mal oído como `$8.90` publicado en Facebook es un problema comercial y legal.

### 3.4 De dónde sale la propuesta pre-llenada

| Fuente | Disponible | Aporta |
|---|---|---|
| **Carga inicial del material pasado** | Fase 0, **parcial** | Arranque adelantado, no completo — ver abajo |
| Histórico propio del sistema | Semana 3–4 | Qué ofertó, cuándo, a qué precio |
| Patrones estacionales | Mes 2–3 | Chile en septiembre, flores en mayo |
| Rendimiento medido (§4) | Mes 4+ | Qué rotó de verdad |

> **Dato confirmado:** el material pasado de Fresco existe pero está **poco organizado y disperso**. La carga inicial cubrirá **parte** del catálogo, no todo. En consecuencia: **la propuesta pre-llenada arranca a medias y mejora con las semanas**, no funciona completa desde el día uno. Las primeras semanas de cada tienda el formulario irá vacío o parcialmente lleno, y el escalón de los tres planes se desplaza a **mes 3–4**.

**Esa curva es la que construye la dependencia:** el sistema se vuelve más útil cuanto más lleva operando, y ese valor no es transferible a un competidor. La contrapartida es que **el argumento de venta no está disponible el primer mes** — hay que gestionar esa expectativa con Marcela.

### 3.5 Reglas de operación

Decisiones tomadas, no supuestos. Condicionan la máquina de estados y el modelo de permisos.

| Regla | Definición |
|---|---|
| **Revisión interna de Fresco** | **Por excepción.** El flyer sale directo al manager salvo que el sistema marque algo: producto sin imagen, precio fuera del rango histórico de esa tienda, texto sin traducción en el glosario, o confianza baja en el carril B. Se espera que el 10–20 % caiga en revisión. *Es lo que hace que el ahorro de horas sea real: si el 100 % pasa por un humano, el cuello de botella se mantiene.* |
| **Manager sin respuesta** | **Escala a Fresco, no se publica solo.** Recordatorio automático al mediodía; si a la hora de corte sigue sin responder, se notifica al equipo y **una persona decide** (llamar, publicar igual o saltar la semana). Publicar sin confirmación es un riesgo comercial que no se automatiza. |
| **Destinatario del link** | **Un contacto principal por sucursal.** Sin edición concurrente ni conflictos. Fresco puede cambiarlo desde el tablero cuando rote el manager — que ocurre a menudo, así que el cambio debe tomar segundos, no un ticket. |
| **Idioma de la web** | **Español e inglés, seleccionable** y recordado por contacto. El mercado es hispano pero opera en EE. UU.; ambos casos existen. |
| **"Cambiar algo" tras ver el arte** | Reabre el mismo link mientras siga vigente. Si ya venció, el sistema emite uno nuevo automáticamente. |

**Umbrales que hay que fijar con datos, no de antemano:** qué desviación de precio dispara la revisión, cuál es la hora de corte de cada tienda, y cuánta confianza basta en el carril B. Se calibran en el piloto de la Fase 1; empezar con valores conservadores y aflojarlos con evidencia.

### 3.6 Seguimiento: tres superficies separadas

El estado del pipeline es **información interna**. El manager no debe verlo: si ve colas, estados y tiempos de proceso, está viendo **la operación**, y ahí empieza a pensar *"esto es un proceso, no un servicio"*. Ve su propuesta, su arte y su resultado — nada más.

| Superficie | Quién | Qué muestra | Naturaleza |
|---|---|---|---|
| **Tablero interno** | Marcela y sus colaboradoras | Las N tiendas, en qué va cada una, qué está bloqueado, qué espera revisión, histórico | **Fuente de verdad** |
| **Slack** | El mismo equipo | Lo de hoy, empujado | **Espejo del tablero** |
| **Web del manager** | Un manager, una tienda | Su propuesta, su flyer, su informe | **Externa y acotada** |

**La regla: lo interno muestra el *cómo*; lo externo muestra el *qué*.**

#### Slack y Asana se quedan — y dejan de ser trabajo

Este es el cambio que hace que el equipo lo adopte en vez de resistirlo. Hoy alguien **escribe** en Slack: copia el mensaje, actualiza qué tienda va en qué estado, lo vuelve a pegar, varias veces al día. Es la razón de que el estado esté siempre algo desactualizado.

Con el tablero como origen, **Slack se escribe solo**: un mensaje por día que el bot edita conforme avanza todo.

```
📋 Ofertas · martes 13 de mayo
✅ Metropolitan     aprobado · programado 21:00
✅ Blue Ridge       aprobado · programado 21:00
🟡 Independence     esperando confirmación del manager (desde 08:00)
🔴 Northtown        precio fuera de rango — necesita revisión
⚪ Shawnee          sin enviar · recordado 12:00
```

No se les quita la herramienta que ya usan y donde tienen notificaciones: se les quita el trabajo de mantenerla. Igual con **Asana**, que sigue siendo el recordatorio del día de Marcela — la tarea se cierra sola cuando la publicación se confirma.

#### Por qué además hace falta el tablero

Slack responde *"¿qué pasó hoy?"*. No responde *"¿cuántas veces Northtown envió tarde este trimestre?"*, *"¿qué precios publicamos el 13 de mayo?"* ni *"¿qué está bloqueado ahora mismo en las 30 tiendas?"*.

A 7 tiendas el chat alcanza. **A 20 o 30 no** — y ese es el escenario que justifica el proyecto.

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

**Render: Satori + resvg**, 50–200 ms sin navegador. Cuatro formatos por oferta: **4:5 para el feed, 9:16 para stories, 1:1 para el carrusel y una versión recortada sin borde para enviar al manager**. 10 tiendas × 2 días × 4 formatos = 80 renders semanales ≈ **12 segundos de cómputo**.

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

**Arquitectura: monolito modular** — una sola aplicación desplegable con módulos de dominio de fronteras explícitas. No microservicios: no hay equipos que separar y sí una persona que mantendría N despliegues. Sustento completo, alternativas descartadas y despliegue en el **[documento 09](./09-arquitectura-y-despliegue.md)**.

| Capa | Elección |
|---|---|
| Aplicación | **Next.js + TypeScript** — un lenguaje, un proyecto: web del manager, tablero interno, API y render |
| Base de datos | **Supabase** (Postgres + pgvector + auth + storage), con `tenant_id` y RLS desde el primer commit |
| Despliegue | **Vercel Pro** (USD 20 — Hobby prohíbe uso comercial) · alternativa: Railway USD 5 |
| Dominio | **Subdominio del dominio de Fresco** *(por confirmar cuál controla)* — marca blanca, verificación de WhatsApp y entregabilidad del correo |
| Notificación | **WhatsApp Business API**, detrás de un adaptador de canal |
| IA | Escalonada por tarea (ver §9). **No interviene en el carril A ni en la propuesta pre-llenada**, que es SQL sobre el histórico |
| Render | **Satori + resvg** |
| Editor | Listas reordenables + vista previa — sin librerías de canvas |
| Almacenamiento | **Cloudflare R2** (egreso gratuito) |
| Meta | **Marketing API** directa, con adaptador propio |

**Cinco controles de calidad**, porque el desarrollo lo dirige una persona con agentes de IA: typecheck y tests (segundos), *golden files* de render con diff de píxeles (~1 min), **evals de extracción sobre 50 documentos reales** (~2 min) y despliegue de vista previa por cada PR. Si la precisión de extracción baja del 95 %, el despliegue se detiene. Detalle en el [documento 09 §5](./09-arquitectura-y-despliegue.md).

---

## 7 · Escalar de 7 a 30 tiendas

| Tiendas | Horas/mes manual | Horas/mes con el sistema | Equivalente a tiempo completo |
|---:|---:|---:|---|
| **9–10** *(hoy)* | **~60** | ~13 | 0.4 → 0.08 |
| 20 | 120 | ~23 | 0.8 → 0.14 |
| 30 | 180 | ~33 | **1.1 → 0.2** |
| 40 | 240 | ~43 | 1.5 → 0.27 |

*Base del cálculo: 8 flyers/tienda/mes. Manual a 45 min = 6 h por tienda. Con el sistema, ~5 min por flyer más 3 h fijas de operación al mes. **Son estimaciones que la línea base de la Fase 0 debe confirmar.** El equivalente a tiempo completo se calcula sobre 160 h/mes; en la práctica ese trabajo hoy se reparte entre Marcela y 2–3 colaboradoras que además hacen otras cosas.*

El costo manual crece **linealmente**; con el sistema crece **sublinealmente**, porque catálogo, glosario e imágenes ya están construidos y los productos se repiten entre supermercados latinos. A 30 tiendas la diferencia son ~147 horas al mes: **casi un empleado a tiempo completo**, y creciendo.

**Qué se rompe y cuándo:** hasta 15 tiendas, nada. De 15 a 25, revisar tantas propuestas se vuelve trabajo → aprobación automática por excepción para tiendas con histórico limpio. De 25 a 40, varias personas revisando → roles. Más de 40, otras agencias quieren usarlo → es la [propuesta V2](./06-propuesta-v2-plataforma.md).

---

## 8 · Plan por fases

**La captura va primero.** Arreglar el origen hace más fácil todo lo demás.

**Fase 0 · Fundaciones (semanas 1–2)**
Auditoría y catalogación del banco de imágenes con licencia y origen. Línea base: cronometrar 10 flyers y **preguntar a dos managers cuánto les toma preparar el Excel**. Alta de WhatsApp Business API y **prueba de categorización de la plantilla** (§11, R1). Diseño de la plantilla base como dato. Preguntar a las sucursales si ya publican su circular en Flipp, Freshop o Mercatus.

**Fase 1 · Captura (semanas 3–7)**
Web responsive bilingüe con link de vencimiento por tiempo. Notificación por WhatsApp y escalado a Fresco si no hay respuesta. Carril B: extracción de Excel/PDF/foto/audio con evals desde el día uno. **Tablero interno con la máquina de estados, espejo en Slack y cierre automático de la tarea de Asana.** **Piloto en 2 sucursales**; las demás siguen por email sin cambios.
→ *En esta fase el flyer todavía lo arma el equipo en Canva — pero con el dato ya limpio, ordenado y traducido. El ahorro viene de eliminar la transcripción, no la maquetación.*

**Fase 2 · Composición (semanas 8–12)**
Plantilla como dato con las seis primitivas. Render de las 9–10 sucursales en los cuatro formatos. Editor de listas con vista previa. Revisión interna por excepción. Salida a Canva como transición.
→ *Aquí desaparece la maquetación manual: menos de 5 minutos de trabajo humano por flyer.*

**Fase 3 · Publicación (semanas 13–16)**
El arte vuelve al chat del manager para aprobación. Publicación a las 21:00 y creación automática del anuncio (+12 h de vigencia, 0 pasos en Ads Manager). **DCO: 10–15 variantes por campaña** — Meta necesita al menos 10 para que el algoritmo aprenda; hoy recibe una. **Instagram se activa aquí**: mismo Business Portfolio, mismo ad set, coste marginal casi nulo.

**Fase 4 · Medición (semanas 17–22)**
QR y códigos de oferta. Informe mensual de rotación. **Conversación con una sucursal para obtener su CSV de ventas por PLU.** Primer informe con atribución real.

**Fase 5 · Inteligencia y consulta (mes 6+)**
Propuesta basada en rendimiento medido. Alta de tienda en minutos. Aprobación automática por excepción. **Dashboard de consulta para el manager** — ver más abajo.

> **Sobre el dashboard para managers.** Es una decisión de negocio antes que técnica y **conviene consultarla con Marcela**, porque toca directamente lo de §2. Dos condiciones si se hace: (a) **muestra resultado, nunca operación** — cuánta gente vio sus ofertas, qué productos generaron más interés, comparativa con el mes anterior y la recomendación; nunca CPM, CTR, estructura de campañas ni segmentación, porque eso es enseñarle a prescindir de la agencia; y (b) **el informe empujado es el producto y el dashboard es el respaldo**, no al revés — la evidencia dice que cuesta que respondan un correo, así que un panel que hay que ir a visitar no lo visita nadie. Añade además carga de soporte: 30 managers con panel son 30 personas que preguntan.

---

## 9 · Costos

### Recurrente

| Concepto | Arranque | 10 tiendas | 30 tiendas |
|---|---:|---:|---:|
| Despliegue (Vercel Pro) | USD 20 | USD 20 | 20–40 |
| Base de datos (Supabase) | 0 *(free)* | 25 | 25–60 |
| Almacenamiento e imágenes | 0–2 | 2–5 | 8–15 |
| WhatsApp (solo el aviso) | ~1 | ~1 | ~3 |
| IA (solo carril B) | 1–4 | 1–4 | 3–10 |
| Dominio y monitoreo | ~1 | 1–11 | 10–20 |
| **Total** | **USD 23–28** | **USD 50–66** | **USD 69–148** |

*Cifras revisadas al alza tras verificar el despliegue: el plan gratuito de Vercel **prohíbe el uso comercial**, así que el mínimo real es Pro (USD 20). La estimación anterior de USD 29–45 asumía despliegue gratuito. Con Railway (USD 5) en lugar de Vercel, el arranque baja a ~USD 10. Desglose y alternativas en el [documento 09 §3](./09-arquitectura-y-despliegue.md).*

### Pago único de arranque

| Concepto | Costo |
|---|---:|
| Cargar el material pasado que se pueda recuperar | USD 10–25 |
| Etiquetar el banco de imágenes existente | USD 10–20 |
| **Total** | **USD 20–45** |

**Licencias: ninguna.** El editor no usa librerías de canvas ni SDK de pago.

### Dónde interviene la IA, y dónde no

Con el carril A funcionando, el uso de modelos cae mucho. Conviene ser explícito para no sobredimensionar el renglón:

| Tarea | Modelo | Costo unitario | Frecuencia |
|---|---|---:|---|
| Clasificar el correo entrante (tienda, fecha, tipo) | Haiku 4.5 | ~USD 0.002 | Solo carril B |
| Extraer un Excel, PDF o foto | Sonnet 5 | ~USD 0.035 | Solo carril B |
| Documento que el validador marca como dudoso | Opus 5 | ~USD 0.08 | Excepción |
| Transcribir y estructurar un audio | Sonnet 5 | ~USD 0.02 | Opcional |
| **Cargar el histórico al catálogo** | Sonnet 5 | ~USD 0.035/doc | **Una sola vez** |
| **Etiquetar el banco de imágenes** | Haiku 4.5 | ~USD 0.002/img | **Una sola vez** |

**No usan IA:** el carril A (el dato llega estructurado), la traducción EN⇄ES (sale del glosario determinista), la propuesta pre-llenada (es SQL sobre el histórico) ni el render.

El *prompt caching* baja más el costo: el prompt de extracción y el glosario son fijos, y las lecturas de caché cuestan ~0.1×.

**El gasto recurrente cae conforme las tiendas adoptan la web.** Si 7 de 10 migran, el renglón de IA baja un 70 %.

El de arranque conviene hacerlo igualmente, aunque **el material pasado esté disperso** (§3.4): cada documento que se recupere adelanta la curva de la propuesta pre-llenada, que es lo que sostiene el argumento del foso (§2). Con material incompleto el rango baja: **USD 15–40** en lugar de 30–60.

**Sobre WhatsApp:** Meta factura por mensaje desde julio de 2025. En EE. UU. una plantilla *utility* cuesta ~USD 0.006 (~0.011 con margen del proveedor) y **la ventana de servicio de 24 h es gratuita**. Como solo se paga el mensaje que abre la conversación: ~86 avisos al mes a 10 tiendas ≈ **USD 1**.

**La comparación relevante ya no son las propuestas rechazadas, es contratar:**

| | Mensual | 3 años |
|---|---:|---:|
| Propuesta A (Escobedo) | 500 + add-ons | USD 23,000+ |
| **Una persona más** | 2,500–3,500 | **USD 90,000–126,000** |
| **Flymar** | 50–148 | **USD 1,800–5,300** |

---

## 10 · Métricas

| | Hoy | Mes 3 | Mes 6 |
|---|---|---|---|
| Minutos de Fresco por flyer | 45–60 | < 15 | **< 5** |
| Minutos del manager por envío | por medir (paso 6, §13) | < 5 | **< 2** |
| Flyers sin feedback de error | por medir | > 85 % | **> 95 %** |
| Sucursales que responden antes del mediodía | por medir | 60 % | **> 90 %** |
| Vigencia efectiva (campaña de 48 h) | ~36 h | ~48 h | ~48 h |
| Variantes por campaña | 1 | 4 | **10–15** |
| **Horas del equipo al mes en producción** | ~60 *(9–10 tiendas)* | < 30 | **< 15** |
| **Tiendas atendibles sin contratar** | 9–10 | 20 | **> 30** |
| **Modelo de cobro** | por pieza | por pieza | **suscripción/tienda** |
| Sucursales con informe de rotación | 0 | 0 | **≥ 1** |
| Banco con licencia documentada | por medir | > 60 % | **100 %** |

*Con el equipo actual —Marcela más 2 o 3 colaboradoras— las 9–10 sucursales consumen del orden de 60 horas al mes solo en producción de flyers. Ese número y el techo de tiendas atendibles son los que responden a **"no es negocio"**; el modelo de cobro responde a **"que sientan que me necesitan"**. Los demás son medios.*

---

## 11 · Riesgos

| # | Riesgo | Mitigación |
|---|---|---|
| R1 | **Meta clasifica la plantilla como *marketing*** — categoría bloqueada para números de EE. UU. desde abril de 2025, sin fecha de reapertura (error `63049`) | **Prueba en la semana 1, antes de construir encima.** Redacción estrictamente utility. Si falla: ritual iniciado por el manager (ventana de servicio, gratis y sin restricción) o el mismo link por SMS/email vía el adaptador |
| **R1b** | **Marcela no cambia el modelo de cobro** — todo el §2 depende de ello. Si sigue cobrando por pieza, el sistema abarata la misma cosa y no cambia nada de fondo: el foso no se construye y la desintermediación sigue latente | Es **decisión de negocio, no técnica** (paso 9, §13). Conviene abordarla antes de la Fase 2, cuando ya hay resultados que enseñar pero todavía no se rediseñó el servicio entero. El sistema funciona igual con el modelo viejo — solo que rinde mucho menos |
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
5. Acceso de lectura a Canva para inventariar el banco de imágenes, **y rescatar todo el material pasado que se pueda** (flyers y Excels de semanas anteriores) para adelantar la curva de la propuesta pre-llenada.
5b. **Confirmar qué dominio controla Fresco** y si puede crear subdominios. Bloquea la verificación de WhatsApp y la configuración del correo.
6. **Preguntar a dos managers cuánto tiempo les toma preparar el Excel.** Es el número que justifica el cambio ante el cliente final.
7. Diseñar la plantilla base como dato y presentarle a Marcela la versión propia frente a la de Canva.
8. Definir con ella el guion hacia los managers: qué se les dice y qué no.
9. **Conversación aparte sobre el modelo de cobro.** Es la más importante y no depende del código: construir el sistema sin cambiar el modelo solo abarata la misma cosa.

---

## Anexo A · Trazabilidad: cada dolor de la primera reunión

Repaso exhaustivo de la transcripción del 8 de agosto (primera sesión), para verificar que nada se perdió al comprimir.

| # | Dolor, con su marca de tiempo | Cubierto por | Estado |
|---|---|---|---|
| 1 | Tres formatos de entrada: Excel, PDF, foto *(01:04)* | Carril B con extracción por visión | ✅ |
| 2 | Categorizar los productos a mano *(01:34)* | Carril A trae la categoría; carril B la infiere | ✅ |
| 3 | Escribir nombres en inglés y español *(01:34)* | Glosario determinista + catálogo canónico | ✅ |
| 4 | Buscar la imagen de cada producto en Canva *(01:52)* | Matching por embeddings sobre el banco indexado | ✅ |
| 5 | Si no la encuentra, la pide o la busca en Google *(01:52)* | Hueco marcado en la propuesta + solicitud al manager en el mismo flujo | ✅ |
| 6 | Cambiar fecha, nombres, cantidad, precio y orden *(02:26)* | Todo sale del dato; el orden es regla de la plantilla | ✅ |
| 7 | Plantilla distinta por tienda, con colores propios *(02:26)* | Plantilla como dato: colores, logo y rejilla por sucursal | ✅ |
| 8 | Variante de festividad: fondo y elemento inferior *(02:52)* | Zona de temporada, una de las seis primitivas | ✅ |
| 9 | Poder mover productos: *"el melón en vez del cilantro"* *(04:57)* | Listas reordenables en el editor | ✅ |
| 10 | Revisar antes de enviar al manager *(15:46)* | Revisión interna por excepción (§3.5) | ✅ |
| 11 | Encuadre y tamaño de las imágenes *(16:09)* | Normalización en el catálogo: recorte a proporción fija, fondo homogéneo y validación al ingresar | ✅ |
| 12 | *"Esta debería ir más grande"* — producto destacado *(16:09)* | Marca de producto héroe en la tarjeta | ✅ |
| 13 | Descargar y recortar el borde antes de enviar *(18:11)* | Cuarto formato de render: versión sin borde para el manager | ✅ |
| 14 | Feedback por precios, ortografía e imagen *(16:39)* | Glosario + validación + confirmación antes de maquetar | ✅ |
| 15 | Saber cuándo envían y cuánto tardan *(17:50)* | Marcas de tiempo automáticas y tablero de estados | ✅ |
| 16 | Meta ya no deja programar el anuncio *(21:46)* | Publicación por API a las 21:00 + anuncio inmediato | ✅ |
| 17 | Se pierde medio día de vigencia *(22:10)* | +12 h recuperadas por campaña | ✅ |
| 18 | Los 9 pasos manuales en Ads Manager *(24:39–26:24)* | Automatizados en la Fase 3 | ✅ |
| 19 | Publicar a las 21:00 rinde mejor *(27:39)* | Regla del scheduler; se valida con A/B en la Fase 4 | ✅ |
| 20 | No mide conversiones, compra en tienda física *(26:42)* | Cuatro vías de atribución (§4) | ✅ |
| 21 | Reporte mensual simple para el cliente *(37:26)* | Informe mensual de rotación y desempeño | ✅ |
| 22 | Justificar la inversión ante *"métele más dinero"* *(38:34)* | El informe, con datos de rotación | ✅ |
| 23 | **Seguimiento en Slack copiando y pegando estados** *(29:13–31:15)* | Tablero interno como fuente de verdad + mensaje único en Slack que el bot edita (§3.6) | ✅ |
| 24 | **Asana marca "publicado" al final del día** *(33:39)* | Cierre automático de la tarea al confirmarse la publicación (§3.6) | ✅ |
| 25 | Riesgo legal por imágenes de internet *(45:57)* | Auditoría en Fase 0 + licencia y origen por activo | ✅ |
| 26 | Los managers rotan y cuesta que envíen ordenado *(40:13)* | Contacto principal cambiable en segundos (§3.5) | ✅ |
| 27 | Una sucursal puede pedir dos tipos de oferta con dos plantillas *(32:45)* | El modelo admite varias propuestas por tienda y fecha, cada una con su plantilla | ✅ 🔑 |
| 28 | Hay tiendas que van solo en inglés *(41:07)* | `idioma` es atributo de la tienda, no una constante global | ✅ 🔑 |
| 29 | Si el feedback llega tarde, se pasa al día siguiente *(33:07)* | Escalado a Fresco y métrica de puntualidad | ✅ |
| 30 | Ella es la única que hace los anuncios *(34:21)* | Automatización de la pauta | ✅ |

**Los 30 dolores quedan cubiertos.** Cinco no estaban en la versión anterior de este documento y se incorporaron tras este repaso: encuadre de imágenes (11), Slack (23), Asana (24), múltiples ofertas por tienda (27) e idioma por sucursal (28).

🔑 **Los dos marcados son de modelo de datos y hay que resolverlos en el esquema desde el primer commit.** Retrofitearlos después obliga a migrar datos en producción.

---

## Anexo B · Qué aporta esta propuesta, honestamente

### Lo que NO es innovador

Conviene decirlo antes que lo otro:

- **Extraer datos de documentos con IA.** Lo hace todo el mundo. Ambas propuestas de terceros lo incluyen.
- **Generar imágenes desde plantillas.** Hay decenas de productos maduros: Bannerbear, Templated, Creatomate.
- **Publicar en Meta por API.** Es documentación pública.
- **Un tablero de revisión y aprobación.** Es exactamente lo que ofrecía Escobedo.
- **Matching de imágenes que aprende del uso.** Escobedo lo plantea explícitamente como *"motor de imágenes con memoria"*, y tiene razón.

**La tecnología de este proyecto es commodity.** Cualquiera con tiempo puede construir cada pieza.

### Lo que sí es diferencial

| | Aporte | Frente a los proveedores | Frente a nuestras versiones previas |
|---|---|---|---|
| ① | **Adelantar el anuncio a las 21:00** recuperando ~12 h por campaña | **Ninguno toca Meta.** Es el hallazgo con mayor impacto económico directo de todo el proyecto, y es técnicamente simple: publicar por API en vez de programar en la interfaz | Ya estaba en la V1 |
| ② | **Invertir la captura**: proponer en vez de pedir | Ambos parten de que el manager entrega datos. Ninguno cuestiona el origen | **Nuevo.** No estaba ni en la V1 ni en la V2 |
| ③ | **Atribución de venta como entregable** | Ambos terminan en el flyer. Ninguno mide nada | **Nuevo aquí**; la V2 lo insinuaba vía retail media |
| ④ | **Inteligencia cross-tienda** — ve 10 donde el manager ve 1 | Nadie lo menciona. Y Fresco **ya tiene ese activo**, solo que no lo capitaliza | **Nuevo** |
| ⑤ | **El modelo de cobro como parte del diseño** | Ambos venden una herramienta con retainer | **Nuevo.** La V1 y la V2 eran propuestas técnicas |
| ⑥ | **Verificar antes de proponer** | Escobedo prometió Canva editable y entrega JPG/PDF; Hexio construyó sobre una API que exige Enterprise | La V1 ya lo hacía; se mantiene |

### El diferencial real, en una frase

**No está en ninguna pieza tecnológica. Está en el encuadre.**

Las dos propuestas de terceros —y nuestra propia V1— tratan esto como **un problema de producción gráfica**: entra un archivo, sale un flyer, cobremos por la pieza. Esta propuesta lo trata como **un problema de datos con un modelo de negocio detrás**: el flyer es un subproducto, el activo es el catálogo, y lo que se vende es saber qué mover y cuánto se vendió.

De ahí se derivan todas las decisiones que las diferencian: por qué la captura va primero, por qué el catálogo es el activo, por qué la medición no es un extra, y por qué el modelo de cobro está en el documento técnico.

### Y lo que hay que reconocerle a los demás

- **Escobedo tenía razón** en que el motor de imágenes con memoria es el mecanismo correcto, y en que un sistema ya construido reduce el riesgo de ejecución. Su propuesta es la más ejecutable de las tres a corto plazo.
- **Hexio tenía la arquitectura correcta** —extracción multimodal, embeddings, `pgvector`, revisión por confianza— y metas honestas (90 %, no 100 %). Su error fue una verificación de diez minutos, no de criterio.

**Lo que ninguno tenía era el resto del negocio.** Ahí está la diferencia, y es más de estrategia que de ingeniería.

---

## Anexo C · Dónde nos apartamos de lo que sugirió Marcela

| Decisión | Lo que ella sugirió | Por qué |
|---|---|---|
| **Web responsive con link temporal** | Un formulario dentro del chat | Un solo frontend, el manager ve el flyer, y los Flows no admiten arrastrar ni vista previa |
| **Proponer decisiones (planes A/B/C)** | Que el manager llene una encuesta | Un formulario traslada la transcripción en vez de eliminarla. Él aporta criterio sobre *qué ofertar*, no sobre teclear |
| **Mejorar el diseño, no replicarlo** | *"Que se parezca lo más que se pueda"* | El diseño actual se heredó, nunca se validó. Optimizar por parecido congela una decisión no comprobada |
| **Atribución de ventas** | Ocultarle al manager que hay una herramienta | Un secreto no es un foso: no escala con la calidad y compite en el eje que se vuelve commodity |
| **Editor de datos con vista previa** | *(no lo planteó; yo había propuesto canvas)* | Ninguna corrección real necesita mover píxeles. Gratis, más rápido de construir, y no permite descuadrar la plantilla |

---

← [07 · Reinventar la captura](./07-reinventar-la-captura.md) · [Índice](../README.md)
