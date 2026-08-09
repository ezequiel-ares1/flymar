# 08 · Propuesta definitiva — Flymar

**Revisión completa tras la segunda reunión del 2026-08-08.**
Sustituye al [documento 04](./04-propuesta-de-solucion.md), integra el [documento 07](./07-reinventar-la-captura.md) y aterriza la ambición del [documento 06](./06-propuesta-v2-plataforma.md) a lo que el negocio realmente necesita ahora.

---

# Parte I · Lo que cambió

La segunda conversación con Marcela cambió cuatro cosas. No son matices: **tres de ellas invalidan restricciones sobre las que estaba construida toda la propuesta anterior, y la cuarta introduce un requisito nuevo que ninguna propuesta —ni la mía ni las de terceros— había considerado.**

## ① Canva deja de ser un requisito

Era **la** restricción estructurante. El PDF original decía *"mantenga el resultado editable dentro de Canva"*, y sobre eso se descartaron plataformas de generación por API, se descalificó la propuesta de Andrés Escobedo y se construyó toda la investigación de la Design Editing API.

Ahora:

> **Marcela:** *"Pues, ¿qué es más sencillo? O sea, yo quiero que se parezca a mi diseño, sabes, lo más que se pueda, pero… si la solución es más sencilla en otro lado y se parece el diseño —como la propuesta que me envió Andrés, se parecía a algunos elementos, no eran como los que yo tenía en Canva, pero se parecían— entonces no se me hace tan mal, **porque ese diseño yo se lo puedo proponer a mi cliente**, porque está más difícil hacerlo en Canva."*

Tres cosas se desprenden de ahí, y las tres importan:

1. **El requisito real nunca fue Canva. Era "que se parezca a mi diseño".** Canva era el medio, no el fin.
2. **Marcela tiene autoridad para cambiar el diseño con su cliente.** Puede proponer una plantilla nueva. Eso significa que no hay que replicar píxel a píxel una plantilla heredada: hay que diseñar *una mejor* y venderla.
3. **Reconoce que Canva es la parte difícil.** Estaba defendiendo una herramienta que le cuesta, no una que le gusta.

**Consecuencia:** se desbloquea el motor de composición propio. Y con él, todo lo que la [propuesta V2](./06-propuesta-v2-plataforma.md) planteaba como aspiracional pasa a ser inmediato.

## ② Marcela pidió, con sus propias palabras, la solución del documento 07

Esto fue lo más llamativo de la conversación. Sin haber leído el documento, describió el Nivel 3 casi literalmente:

> *"Lo que también estoy pensando es que se pueda así como **llenar el formulario, que lo puedan hacer desde el celular, como cuando llenas una encuesta**."*
>
> *"Que **ellos me llenen**, así como que sea una encuesta y ellos me llenen. 'Limón va a estar a tanto', ¿me entiendes?"*
>
> *"Si yo les enviara la encuesta **semana tras semana** y en el celular está como **primero carnicería** y me pongan todos los productos de carnicería, y luego…"*

Cuatro requisitos concretos en tres frases: **móvil**, **tipo encuesta**, **recurrente semanal**, **agrupada por categoría en el orden del flyer**.

Y cerró con el argumento de negocio:

> *"En vez de que ellos lo hagan así y luego yo esa misma información la pase, **que ya sea de una**."*

Es exactamente el diagnóstico del documento 07: el dato se estructura, se degrada y se vuelve a estructurar. Marcela lo vio sola.

## ③ Aparece el riesgo de desintermediación

Este es el requisito nuevo, y es delicado:

> *"**Yo no quiero que ellos sepan que ellos lo están armando**, porque si no van a decir 'pues yo lo puedo hacer'."*
>
> *"Yo necesito que ellos sientan que me necesitan. Porque si no, ellos le van a decir al primo 'pues tú publícalo', ya no necesito la agencia porque yo lo estoy haciendo. **Como ya hay mucha competencia aquí**, yo necesito que ellos sientan que me necesitan, que no sepan que estoy usando una herramienta que me lo hace más rápido."*

No es paranoia. Es una lectura correcta de su mercado: el marketing para supermercados latinos independientes tiene barreras de entrada bajas, y el sobrino con Canva es competencia real.

**Pero la conclusión que saca es equivocada, y merece una respuesta honesta.** Se trata en la Parte II.

## ④ El crecimiento ya ocurrió

> *"Acaban de abrir **otras 2 tiendas**, entonces están creciendo muy rápido y nosotros necesitamos algo que **no voy a poner a todo mi equipo a hacer esto**, ¿sabes? O sea, no me conviene a mí, **no es negocio**."*

Dos datos en una frase:

- **De 7–8 sucursales pasa a 9–10.** El crecimiento no es una proyección: ya pasó.
- **Marcela identificó sola el problema estructural de su modelo:** hoy más tiendas significa más personas, y más personas significa que el margen se evapora. Textualmente: *"no es negocio"*.

Esto convierte el proyecto de "ahorro de tiempo" en **condición para poder crecer**. Es un cambio de categoría en la conversación de inversión.

---

## Tabla de restricciones: antes y ahora

| Restricción de la propuesta V1 | Estado | Consecuencia |
|---|---|---|
| El resultado debe quedar editable en Canva | ❌ **Levantada** | Motor de composición propio |
| Nada de plataforma para los managers | ❌ **Levantada por ella misma** | Formulario móvil recurrente |
| El manager solo usa email | ⚠️ **Matizada** | Email sigue como carril de respaldo, no como único |
| Presupuesto mínimo | ⚠️ **Matizada** | Ahora hay caso de negocio: sin esto no puede crecer |
| Mantenimiento mínimo | ✅ **Sigue** | Sigue sin equipo técnico |
| **NUEVA: el manager no debe percibir que "se arma solo"** | 🆕 | Requisito de diseño de producto |
| **NUEVA: escalar sin sumar personal** | 🆕 | Requisito de arquitectura |

---

# Parte II · La tensión central: "que sientan que me necesitan"

Esta sección no es técnica, pero es la más importante del documento. Si se resuelve mal, el producto puede ser perfecto y el negocio perderse igual.

## El miedo, entendido bien

Marcela teme que al facilitarle la vida al manager, este descubra que la parte difícil ya no es difícil. Y en su lectura, si el manager llena un formulario bonito y el flyer aparece, la conclusión natural del manager es: *"esto lo hace cualquiera".*

**El miedo es legítimo. La defensa que propone —esconder la herramienta— es frágil**, por tres razones:

1. **No escala con la calidad.** Cuanto mejor funcione el sistema, más rápido responderá Fresco, y más evidente será que hay una herramienta. El éxito del producto erosiona la estrategia de ocultarlo.
2. **Un secreto no es un foso.** Cualquier competidor puede montar un Google Form mañana. Si el diferencial es "yo tengo un formulario y tú no", el diferencial dura una semana.
3. **Compite en el eje equivocado.** Defiende la *producción del flyer*, que es precisamente la parte que se está volviendo commodity — y no solo para Fresco: para toda la industria.

## La respuesta que salió en la reunión, y por qué es la correcta

> **Ezequiel:** *"Lo que ellos necesitan de ti es que vendan, que ganen."*
>
> *"El problema real es que ellos deben vender contigo. Y si venden contigo, entonces estás creando esa necesidad que tú dices, pero paralelamente, sigilosamente, de forma secundaria."*
>
> *"¿Qué le interesa más a la sucursal? **Vender su stock.** No le interesa con quién sea, pero que venda su stock."*
>
> *"Yo tengo en stock una tonelada de carne y en todo un día Marce me la ha vendido; y tengo otro producto en stock y con esta publicidad no me lo ha vendido, solo he vendido 500 [kg]."*

Y el remate sobre por qué la métrica actual no defiende nada:

> **Marcela:** *"A ellos les gusta ver los likes en las páginas. Para ellos, eso piensan que les fue bien."*
> **Ezequiel:** *"Pero ahí me pasas nomás el link y yo le lleno de likes con bots."*

**Los likes no son un foso: son falsificables y no significan nada.** Mientras el entregable de Fresco sea "un flyer bonito con muchos likes", cualquiera puede igualarlo o simularlo.

## La reformulación

> **Lo que hoy vende Fresco:** *"Yo te armo el flyer y lo publico."*
> **Lo que debería vender:** *"Yo sé qué mover, cuándo publicarlo y cuánto vendiste con ello."*

Lo primero lo copia el sobrino con Canva. **Lo segundo requiere histórico, datos y sistema — y se vuelve más difícil de copiar cada semana que pasa.**

Al mes seis, Fresco puede decirle a una sucursal:

> *"La pierna de pollo a $0.89 en martes te movió tres veces más que a $0.99 en jueves. La papaya no rota si va en la última fila. Y las campañas que salieron a las 21:00 te costaron 40 % menos por interacción. Esto es lo que te propongo para la semana que viene."*

Ese párrafo es imposible de improvisar y **es la razón real por la que el manager no llama a su primo.**

## Cómo se resuelven las dos cosas a la vez

La buena noticia es que no hay que elegir. El diseño de producto puede honrar el miedo de Marcela *y* construir el foso verdadero:

| Principio | Cómo se implementa |
|---|---|
| **El sistema es invisible; Fresco es visible** | Todo lo que ve el manager lleva la marca de Fresco. No hay logo de Flymar, ni "powered by", ni nada que insinúe una plataforma de terceros. |
| **Lo que llega no es una herramienta: es un servicio** | El mensaje no dice *"entra a llenar el formulario"*. Dice *"Hola Luis, aquí está la propuesta de ofertas que preparamos para Metropolitan esta semana"*. **El formulario es la forma de responder, no un producto de autoservicio.** |
| **El manager confirma; no construye** | Ver Parte III. Si lo que recibe es una propuesta ya armada, la percepción no es *"lo estoy haciendo yo"* sino *"me lo están haciendo y yo apruebo"*. **Esto invierte por completo la sensación de autoría.** |
| **El entregable visible cambia de flyer a resultado** | El informe mensual de rotación y desempeño es lo que se pone al frente de la relación. El flyer pasa a ser el medio, no el producto. |
| **La inteligencia no se regala** | El manager ve la recomendación (*"te sugerimos estos 16 productos"*), nunca el porqué (*"porque el histórico dice que a este precio rotan 3×"*). El razonamiento se queda en la agencia. |

**El resultado neto:** al manager le resulta más fácil trabajar con Fresco que sin Fresco — no porque no sepa hacer un flyer, sino porque **sin Fresco no sabe qué ofertar ni qué funcionó.**

## La analogía que salió en la reunión, y que conviene tomar en serio

> **Ezequiel:** *"Como Uber, que creó el mapa para los conductores, ¿no?"*

Es la analogía correcta y vale la pena desarrollarla, porque contiene la respuesta a la pregunta de fondo.

**Un conductor de Uber no elige Uber por la aplicación.** La aplicación es fácil, intuitiva y cualquiera podría clonarla — de hecho, muchos la clonaron. El conductor elige Uber **porque le trae pasajeros**. La app no es el producto: es el canal por el que se entrega el producto.

Lo mismo aquí. **El formulario fácil e intuitivo no es lo que Fresco vende. Es cómo lo entrega.** Confundir las dos cosas es lo que produce el miedo de la Parte II — y también lo que produciría un producto mal diseñado.

La pregunta correcta, entonces, no es *"¿cómo evito que vean la herramienta?"* sino **"¿qué es lo que traigo yo que ellos no pueden conseguir solos?"**.

---

## Las cinco razones por las que un cliente elige a la agencia

Ordenadas de la más frágil a la más defendible. Fresco hoy está apoyada casi por completo en la primera.

### ① Sabe usar la herramienta *(frágil — es lo que se está evaporando)*

Es el diferencial actual: *"yo sé Canva y tú no"*. Es exactamente lo que el sobrino puede replicar en un fin de semana, y lo que la IA está volviendo commodity en toda la industria — no solo aquí. **Defender esto es defender el terreno que se está perdiendo.**

### ② Le ahorra tiempo *(débil — mejora, pero sigue siendo comparable)*

*"Yo lo hago y tú no tienes que hacerlo"*. Es real y tiene valor, pero se compara en precio: si el sobrino cobra la mitad, la conversación se vuelve una negociación de tarifa. **Todo servicio que se defiende por ahorro de tiempo termina compitiendo por precio.**

### ③ Tiene el acceso y la operación *(medio)*

Administra las páginas, el Business Portfolio, las campañas segmentadas, el presupuesto. Cambiar de agencia implica migrar accesos y perder el historial de las campañas. Es fricción real, pero es fricción de salida, no valor de entrada. **No hace que te quieran; hace que les dé pereza irse.** Y la pereza se vence cuando alguien cobra la mitad.

### ④ **Sabe qué funciona** *(fuerte — aquí empieza el foso)*

*"La pierna de pollo a $0.89 en martes te movió tres veces más que a $0.99 en jueves."*

Esto **no se puede improvisar**. Requiere haber operado esa tienda durante meses, con los datos guardados de forma estructurada y comparable. El sobrino, aunque haga un flyer idéntico, no tiene esa información y no puede conseguirla — no está a la venta.

Y crece solo: **cada semana que pasa, el foso es una semana más profundo.**

### ⑤ **Ve lo que una sola tienda no puede ver** *(el más fuerte — y hoy está desperdiciado)*

Este es el activo que Fresco ya posee y no está capitalizando.

El manager de Metropolitan ve **una** tienda. Fresco ve **diez** — diez supermercados latinos, en el mismo mercado, con el mismo público, compitiendo por los mismos clientes.

Eso permite decir cosas que **nadie más en Kansas City puede decir**:

> *"El aguacate se está ofertando en cinco de las tiendas que manejamos esta semana. Si lo pones tú también, compites contra todos. Te propongo mango ataulfo: nadie lo tiene y en mayo rota."*
>
> *"El precio promedio de la pierna de pollo en el mercado esta semana es $0.94. Tú estás a $1.09."*
>
> *"Los martes a las 9 pm rinden 40 % mejor que los jueves a las 7 pm. En todas las tiendas, sin excepción."*

**Esto es inteligencia de mercado, y solo la puede producir alguien que opera varias tiendas a la vez.** Es estructuralmente inaccesible para el sobrino, para el manager y para cualquier agencia con un solo cliente.

Y tiene una propiedad notable: **mejora con cada tienda nueva.** A 10 tiendas es útil; a 30 es imbatible. **El crecimiento que a Marcela hoy le pesa —"no es negocio"— se convierte en la fuente del diferencial.**

> **Nota de manejo:** esta inteligencia debe entregarse **agregada y anonimizada**, nunca revelando datos de una tienda a otra. *"El precio promedio del mercado"*, no *"Blue Ridge lo tiene a $0.89"*. Es lo correcto éticamente y además es lo que la hace sostenible: si las tiendas sospechan que se filtran sus datos, se acaba.

---

## Reinventar el modelo de negocio

Aquí está el fondo del asunto. **Un cambio de herramienta sin un cambio de modelo de cobro solo hace más barata la misma cosa** — y termina bajando el precio, no subiéndolo.

### Dónde está Fresco hoy

**Cobra por producir piezas.** Ese modelo tiene tres problemas estructurales:

1. **Está atado al insumo, no al resultado.** Se cobra por flyer, no por venta. El cliente compra actividad.
2. **Es directamente comparable.** *"¿Cuánto cobras por flyer?"* es una pregunta que el sobrino puede responder más barato.
3. **Sus ingresos crecen solo si crecen los costos.** Más tiendas = más piezas = más personas. Es el *"no es negocio"* de Marcela, expresado como modelo de ingresos.

### La escalera de modelos

Cada escalón vende algo distinto, se defiende mejor y depende menos de las horas del equipo.

| # | Modelo | Qué se cobra | Defensa frente al sobrino | Requisitos |
|---|---|---|---|---|
| **0** | **Fee por pieza** *(hoy)* | Producción | ❌ Ninguna | — |
| **1** | **Retainer de gestión** | Disponibilidad y operación | ⚠️ Baja | Nada nuevo |
| **2** | **% de inversión publicitaria** | Gestión del gasto en medios | ⚠️ Media | Reportes creíbles |
| **3** | **Suscripción por tienda con SLA** | Un servicio, no un entregable | ✅ Media-alta | El sistema propuesto |
| **4** | **Variable por desempeño** | Resultado de venta | ✅✅ Alta | Atribución (Parte IV) |
| **5** | **Retail media: cobrar al CPG** | Acceso a audiencia | ✅✅✅ Muy alta | Volumen + medición |
| **6** | **Licenciar el sistema a otras agencias** | El software | ✅✅✅ Otra industria | [Propuesta V2](./06-propuesta-v2-plataforma.md) |

#### Escalón 3 · Suscripción por tienda — *el siguiente paso realista*

En lugar de cobrar por flyer, cobrar **una cuota mensual por sucursal** que incluye: todas las ofertas de la semana, publicación, gestión de anuncios, y el informe mensual de rotación.

Por qué es mejor aunque el monto fuera parecido:

- **Deja de ser comparable pieza a pieza.** No se compra un flyer; se contrata un servicio.
- **Los ingresos se vuelven predecibles y el margen crece con la escala**, porque el costo marginal por tienda cae (Parte VI).
- **Se puede empaquetar por niveles:** básico (ofertas y publicación) / avanzado (+ variantes creativas y optimización de horarios) / *performance* (+ informe de rotación y recomendación de surtido).
- **Alinea el incentivo con el sistema:** cada tienda nueva es margen casi puro.

#### Escalón 4 · Variable por desempeño

Una vez que existe atribución (Parte IV), se puede añadir un componente variable: una base fija más un porcentaje sobre la venta incremental de los productos ofertados.

**Este es el escalón que cambia la conversación por completo**, porque convierte a Fresco de proveedor en socio. Y tiene una consecuencia práctica: **el cliente deja de preguntar cuánto cobras y empieza a preguntar cuánto más puedes vender.**

*Precaución:* exige medición confiable. No se propone hasta tener al menos una sucursal con datos del POS y tres meses de histórico. Prometerlo antes es venderse un problema.

#### Escalón 5 · Retail media — cambiar de pagador

El salto conceptual más grande, y ya está documentado en la [propuesta V2](./06-propuesta-v2-plataforma.md): **el supermercado deja de ser el único que paga.**

En los propios anexos de Fresco aparecen `SALSA VALENTINA`, `BIMBO`, `PEPSI 12PK`, `KELLOGGS`, `MALTIN POLAR`, `TORTILLAS EL MILAGRO`. Esas marcas destinan presupuesto de *trade marketing* para aparecer donde compran los hispanos, y los supermercados independientes **quedan fuera de ese reparto** porque no tienen cómo medir ni facturar.

Cada flyer ya es inventario publicitario. Hoy se regala.

Aquí el sobrino con Canva no compite: **no se trata de hacer un flyer, se trata de tener una audiencia medida y un sistema de facturación.**

*Realismo:* requiere volumen (30+ tiendas) y medición sólida. Es horizonte de 12–18 meses, no de este trimestre. Pero condiciona decisiones de hoy — sobre todo guardar los datos correctamente desde el primer día.

### La transición recomendada

No hay que saltar al escalón 5. Hay que subir en orden, y cada escalón financia el siguiente:

```
Mes 1–3    Escalón 0 → 1     Mismo cobro, pero se empieza a hablar de
                              servicio en vez de piezas. Sin fricción.

Mes 4–6    Escalón 1 → 3     Aparece el informe de rotación. Se propone
                              suscripción por tienda con niveles.
                              ← el sistema ya lo permite

Mes 7–12   Escalón 3 → 4     Con atribución real, se ofrece variable
                              por desempeño a la sucursal más grande.

Mes 12+    Escalón 5         Con volumen y medición, conversación con
                              el primer CPG.
```

---

## La conclusión, en una frase

**Un formulario fácil e intuitivo no debilita a la agencia: debilita a la agencia que solo vendía llenar formularios.**

Si Fresco sigue vendiendo *"yo te armo el flyer"*, entonces sí — hacerlo fácil acelera su propia sustitución. Si pasa a vender *"yo sé qué mover, cuándo publicarlo y cuánto vendiste"*, la herramienta deja de ser una amenaza y se vuelve la infraestructura que hace posible el nuevo producto.

**La herramienta no reemplaza a la agencia. Reemplaza a la parte de la agencia que ya no vale dinero, y libera al equipo para vender la parte que sí.**

Y hay un detalle final que conviene decirle a Marcela con claridad: **este cambio va a ocurrir con o sin ella.** La producción de flyers se está volviendo commodity en toda la industria. La pregunta no es si conviene automatizar, sino si Fresco quiere llegar primero a lo que viene después —los datos, la atribución, el retail media— o llegar cuando alguien más ya esté ahí.

---

# Parte III · La nueva captura, en detalle

Este es el corazón de la propuesta revisada, y responde punto por punto a lo que Marcela pidió.

## 3.1 El principio: invertir el flujo

El cambio conceptual, en una imagen:

```
ANTES                                    AHORA
─────                                    ─────
"Mándame tus ofertas"                    "Esto preparamos para ti,
        ↓                                 confirma o ajusta"
El manager abre Excel                            ↓
        ↓                                 El manager abre WhatsApp
Escribe 16 productos                             ↓
        ↓                                 Ve 16 productos ya propuestos
Maqueta las columnas                             ↓
        ↓                                 Ajusta 3 precios
Adjunta y manda                                  ↓
        ↓                                 Toca «Confirmar»
~20–30 minutos                            ~90 segundos
```

**Y aquí está la elegancia del diseño:** esta inversión resuelve simultáneamente los cuatro problemas.

| Problema | Cómo lo resuelve la inversión del flujo |
|---|---|
| El manager tarda y a veces no envía | Pasa de 25 minutos a 90 segundos. La puntualidad deja de ser un problema de voluntad. |
| El dato llega degradado | Nunca se degrada: nace estructurado y así se queda. |
| Fresco transcribe | No hay nada que transcribir. |
| **El manager podría sentir que "lo hace él"** | **Al contrario: recibe una propuesta ya hecha. La percepción es que Fresco trabajó para él.** |

> El último punto merece énfasis. Un formulario en blanco dice *"llénalo tú"*. Una propuesta pre-llenada dice *"te lo preparamos"*. **Es el mismo mecanismo técnico y la percepción opuesta.**

## 3.2 Cómo se ve, concretamente

**Martes, 08:00. WhatsApp del manager de Metropolitan:**

```
┌──────────────────────────────────────────┐
│  Fresco Marketing                        │
│                                          │
│  Buenos días, Luis 👋                    │
│                                          │
│  Preparamos la propuesta de ofertas de   │
│  Metropolitan para el martes 13 y        │
│  miércoles 14 de mayo.                   │
│                                          │
│  Son 16 productos. Revísala y            │
│  confírmanos, o dinos qué cambiar.       │
│                                          │
│      [ Ver propuesta ]    [ Llamar ]     │
└──────────────────────────────────────────┘
```

**Toca "Ver propuesta" — se abre dentro de WhatsApp, sin salir de la app:**

```
┌──────────────────────────────────────────┐
│  CARNICERÍA                     1 de 4   │
│  ────────────────────────────────────    │
│                                          │
│  🥩 Pierna de pollo                      │
│     $ [0.89]  por [LB ▾]         ✓      │
│                                          │
│  🥩 Discada de puerco                    │
│     $ [2.89]  por [LB ▾]         ✓      │
│                                          │
│  🥩 Caldo mixto de res                   │
│     $ [2.99]  por [LB ▾]         ✓      │
│                                          │
│  🥩 Chorizo en tripa                     │
│     $ [____]  por [LB ▾]        ⚠ falta │
│                                          │
│  [ + Agregar otro ]                      │
│                                          │
│  ──────────────────────────────────────  │
│         [ Quitar ]      [ Siguiente ]    │
└──────────────────────────────────────────┘
```

Cuatro pantallas —carnicería, produce, abarrotes, panadería, **en el orden del flyer**—, y una final:

```
┌──────────────────────────────────────────┐
│  ¿Algo especial esta semana?             │
│                                          │
│  [ ] Es Día de las Madres                │
│  [ ] Quiero destacar un producto         │
│  [ ] Tengo una foto que subir            │
│                                          │
│  Notas para el equipo:                   │
│  ┌────────────────────────────────────┐  │
│  │ Usen fotos de carne cocida para   │  │
│  │ el paquete de res                 │  │
│  └────────────────────────────────────┘  │
│                                          │
│           [ Confirmar y enviar ]         │
└──────────────────────────────────────────┘
```

**Y de vuelta en el chat, 20 minutos después:**

```
┌──────────────────────────────────────────┐
│  Fresco Marketing                        │
│                                          │
│  ¡Listo, Luis! Aquí está el arte de      │
│  Metropolitan para el 13–14 de mayo.     │
│                                          │
│  [🖼 imagen del flyer]                   │
│                                          │
│  Si está bien lo programamos hoy a las   │
│  9 pm. ¿Lo aprobamos?                    │
│                                          │
│      [ Aprobar ]     [ Cambiar algo ]    │
└──────────────────────────────────────────┘
```

**El ciclo completo —solicitud, captura, producción, aprobación— ocurre dentro de una conversación de WhatsApp que dura 20 minutos, y el manager tocó cinco botones.** Hoy ese ciclo toma entre medio día y día y medio.

## 3.3 Por qué WhatsApp y no un portal ni email

La objeción original de Marcela era contra un **portal**, y era correcta:

> *"En cada sucursal hay muchos managers y a veces los cambian. Me ha costado mucho trabajo, primero, que me lo envíen por email."*

Un portal exige cuenta, contraseña, capacitación y recuperar accesos cuando alguien rota. WhatsApp no exige nada de eso:

| | Portal web | Email | **WhatsApp Flows** |
|---|---|---|---|
| Requiere crear cuenta | Sí | No | **No** |
| Requiere instalar algo | No, pero sí aprender | No | **No — ya lo tiene abierto** |
| Cambio de manager | Alta, permisos, capacitación | Cambiar dirección | **Agregar un número** |
| Tasa de apertura | — | 20–25 % real | **98 %** |
| Tasa de respuesta | Baja | 1–5 % | **40–60 %** (hasta 12×) |
| Uso entre hispanos en EE. UU. | — | — | **46 % regularmente** |
| Devuelve dato estructurado | Sí | No | **Sí (JSON validado)** |
| Se puede usar desde el celular en el pasillo de la tienda | Mal | Mal | **Es su caso natural** |

Ese último punto no es menor: el manager de carnicería no está frente a una computadora. Está en la tienda, con el teléfono en la mano.

## 3.4 Tres carriles: nadie se queda fuera

Obligar a un solo canal es cómo se rompen estos sistemas cuando entra un manager nuevo. La arquitectura acepta tres entradas y todas terminan en el mismo lugar:

```
CARRIL A · Propuesta + confirmación (el objetivo)
   WhatsApp Flow pre-llenado ──────────┐
                                       │
CARRIL B · Formato libre (el respaldo) │
   Email con Excel/PDF/foto ───────────┼──▶ Mismo esquema
   WhatsApp con foto o audio ──────────┤    OfferSheet validado
                                       │
CARRIL C · Origen (el futuro)          │
   API del POS · feed de circular ─────┘
```

**Carril B es importante y no es un fallback de segunda:** el manager que quiera seguir mandando su Excel puede hacerlo indefinidamente. Simplemente su flyer sale más tarde, porque hay que interpretarlo y confirmarlo. **La estandarización se incentiva, no se impone.**

Y ese carril incorpora el audio, que en el contexto real de una carnicería a las 6 de la mañana es la vía más natural que existe:

> *"Oye, para el martes ponme pierna de pollo a ochenta y nueve la libra, discada de puerco a dos ochenta y nueve, y el caldo mixto a dos noventa y nueve."*

Se transcribe, se estructura y **se devuelve confirmación explícita** —nunca se autoaprueba, porque un `$0.89` mal oído como `$8.90` publicado en Facebook es un problema comercial y legal.

## 3.5 De dónde sale la propuesta pre-llenada

Es la pregunta obvia: para proponer 16 productos hay que saber cuáles. Tres fuentes, en orden de disponibilidad:

| Fuente | Cuándo está disponible | Qué aporta |
|---|---|---|
| **Histórico de la propia tienda** | Semana 3 en adelante | Qué ofertó, cuándo, a qué precio, con qué frecuencia. Es la base. |
| **Patrones estacionales** | Mes 2 | Chile en septiembre, pavo en noviembre, flores en mayo. |
| **Rendimiento medido** | Mes 4 (ver Parte IV) | Qué rotó de verdad. Aquí la propuesta deja de ser estadística y empieza a ser inteligente. |

**Las primeras dos semanas el formulario va vacío** —es simplemente una encuesta ordenada, que ya es mejor que el Excel—. A partir de la tercera empieza a proponer. **Y esa curva es exactamente la que construye la dependencia:** el sistema se vuelve más útil cuanto más tiempo lleva operando, y ese valor no es transferible al primo con Canva.

## 3.6 Puntualidad como dato

Cada evento queda con marca de tiempo, sin que nadie lo registre a mano:

```
Metropolitan     propuesta 08:00 · confirmó 08:14 · aprobó 11:20   ✓
Blue Ridge       propuesta 08:00 · confirmó 14:47 · aprobó 17:30   ⚠ tarde 2 sem.
Independence     propuesta 08:00 · recordado 12:00 · sin respuesta ✗
```

Sirve para tres cosas: saber a quién insistir, tener evidencia en la conversación con el cliente, y decidir qué sucursal merece la integración directa con su POS.

---

# Parte IV · El foso: de los likes a la venta

Esta parte no existía en la propuesta anterior. Es la que responde al miedo de la Parte II.

## 4.1 El problema con la métrica actual

Hoy Fresco reporta engagement porque es lo único que Meta le da sin píxel. Y como observó Ezequiel en la reunión, **eso es falsificable con bots**. Peor: es un lenguaje que no le sirve al dueño del supermercado, que no piensa en likes sino en rotación de inventario.

## 4.2 Cómo medir venta sin comercio electrónico

No hay píxel porque no hay tienda en línea. Pero hay cuatro vías, de menor a mayor precisión:

### ① Código de oferta en el flyer *(inmediato, casi gratis)*

Un identificador discreto en el arte (`MF-0513-A`). Cuando el cliente lo menciona o el cajero lo teclea, se atribuye. Requiere colaboración mínima de la tienda y da una señal direccional.

### ② Cupón / QR por producto destacado *(mes 2)*

El flyer lleva un QR por producto héroe. Escanearlo abre una página con el cupón. **Se mide interés real por producto, no por publicación.** Con eso ya se puede decir *"la pierna de pollo generó 340 escaneos, la papaya 12"*, que es una conversación distinta a *"tuvimos 89 likes"*.

### ③ Pregunta en caja *(mes 2, cero tecnología)*

*"¿Vio nuestra oferta en Facebook?"*. Una muestra pequeña y constante da una tasa de conversión estimable. Es rudimentario y funciona.

### ④ **Datos del POS — el premio mayor** *(mes 4+)*

Si una sola sucursal comparte su reporte de ventas por PLU y semana —ni siquiera en tiempo real, un CSV semanal basta—, la ecuación se cierra por completo:

```
   Producto ofertado (PLU 469, $0.89/LB, 13–14 may)
        +
   Campaña ejecutada (gasto, alcance, interacciones)
        +
   Unidades vendidas esa semana vs. la semana anterior
        =
   "Esta campaña movió 1,240 lb de pierna de pollo,
    un 214 % más que la semana previa, con $38 de inversión."
```

**Ese informe es el producto que nadie más puede entregar**, y no requiere que Marcela construya nada complejo: requiere pedirle a una sucursal un archivo semanal. Es una conversación, no un proyecto de ingeniería.

## 4.3 El informe que cambia la relación

```
╔══════════════════════════════════════════════════════════╗
║  MERCADO FRESCO METROPOLITAN · MAYO 2026                 ║
╠══════════════════════════════════════════════════════════╣
║  Inversión publicitaria           $ 412                  ║
║  Alcance                          38,400 personas        ║
║  Costo por interacción            $ 0.043  ↓ 38 % vs abr ║
╠══════════════════════════════════════════════════════════╣
║  LO QUE MÁS ROTÓ                                         ║
║  1. Pierna de pollo   $0.89/LB    ↑ 214 %   ⭐           ║
║  2. Chorizo en tripa  $2.99/LB    ↑ 156 %                ║
║  3. Discada de puerco $2.89/LB    ↑  88 %                ║
╠══════════════════════════════════════════════════════════╣
║  LO QUE NO FUNCIONÓ                                      ║
║  · Papaya $0.89/LB — sin movimiento. Fue en última       ║
║    fila las 3 veces. Sugerimos subirla o retirarla.      ║
╠══════════════════════════════════════════════════════════╣
║  RECOMENDACIÓN PARA JUNIO                                ║
║  · Mantener pierna de pollo los martes                   ║
║  · Probar costilla de res — no se ha ofertado en 6 sem.  ║
║  · Publicar 21:00: costó 40 % menos por interacción      ║
╚══════════════════════════════════════════════════════════╝
```

Compárese con lo que hoy se entrega: un flyer y, si acaso, una captura de Ads Manager.

**Ese informe es el que hace que el manager no llame a su primo.** Y nótese que no oculta nada al manager: le muestra el *qué*, nunca el *cómo*.

---

# Parte V · El diseño, ahora que Canva no obliga

## 5.1 La decisión

Con la restricción levantada, se abren tres caminos:

| Camino | Descripción | Veredicto |
|---|---|---|
| **A · Seguir en Canva** | Design Editing API sobre las plantillas actuales | Sigue siendo viable, pero ahora es **la opción más frágil**: API en beta, dependencia externa, y ya no la exige nadie |
| **B · Motor propio** | La plantilla es un dato; se renderiza en el servidor | **Recomendado.** Control total, sin dependencias, sin límites de plan, render en milisegundos |
| **C · Híbrido** | Motor propio para producir; exportación a Canva solo cuando alguien quiera retocar a mano | **Recomendado como transición** |

**Recomendación: C durante los primeros tres meses, B como destino.**

La transición importa porque el equipo de Marcela sabe usar Canva y la confianza se construye gradualmente. El sistema produce el flyer terminado; si alguien quiere retocarlo, se exporta y se abre en Canva como siempre. **A medida que la confianza sube, esa salida se usa cada vez menos, hasta que deja de usarse.**

## 5.2 La plantilla como dato

Hoy la plantilla vive como archivo en Canva. Nadie puede consultarla, versionarla ni generarla en masa. Propuesto:

```jsonc
{
  "plantilla": "metropolitan-v3",
  "formato": { "ancho": 1080, "alto": 1350 },
  "marca": {
    "primario": "#C8102E", "secundario": "#FFD100",
    "logo": "asset://metropolitan/logo",
    "tipografia": "Anton / Open Sans"
  },
  "estructura": {
    "encabezado": { "logo": true, "vigencia": true, "direccion": true },
    "secciones": [
      { "categoria": "meat",    "titulo_es": "CARNICERÍA", "titulo_en": "MEAT",
        "rejilla": "3x2", "color_banda": "#C8102E" },
      { "categoria": "produce", "titulo_es": "FRUTAS Y VERDURAS", "titulo_en": "PRODUCE",
        "rejilla": "3x1", "color_banda": "#4C9F38" },
      { "categoria": "grocery", "titulo_es": "ABARROTES", "titulo_en": "GROCERY",
        "rejilla": "3x1", "color_banda": "#0057B7" },
      { "categoria": "bakery",  "titulo_es": "PANADERÍA", "titulo_en": "BAKERY",
        "rejilla": "3x1", "color_banda": "#F2A900" }
    ],
    "pie": { "zona_temporada": true }
  }
}
```

Lo que esto habilita, y que con un archivo de Canva es imposible:

- **Una tienda nueva se configura en minutos** (colores, logo, rejilla), no rediseñando en Canva. Con dos sucursales recién abiertas, esto ya importa.
- **Se generan variantes en masa:** el mismo dato en 4:5, 9:16, carrusel y video.
- **La IA puede operar la plantilla** porque las primitivas son semánticas: *"pon el melón donde está el cilantro"* es intercambiar dos nodos, no mover píxeles.
- **Se versiona.** Se sabe qué plantilla produjo qué flyer y se puede volver atrás.

## 5.3 Las seis primitivas

No hay que construir Canva. Un flyer de ofertas necesita seis piezas, y ninguna más:

```
1. Encabezado         logo + vigencia + dirección
2. Banda de categoría CARNICERÍA / MEAT, con color de la plantilla
3. Rejilla            3×2, 3×1, 4×3 según sección
4. Tarjeta de producto imagen + nombre ES + nombre EN + precio + unidad
5. Chip de precio     las 5 sintaxis: 0.99 · $3.49/LB · 2/$7.00 · 2X1.00 · 2 EA/$3.00
6. Zona de temporada  fondo y franja intercambiables por festividad
```

Ese alcance está **congelado**: nada entra al editor si no aparece en un flyer real. Es la disciplina que evita que el proyecto se convierta en un clon de Canva a medio hacer.

## 5.4 Render

**Satori + resvg**: convierte una descripción declarativa a SVG y luego a PNG en **50–200 ms**, con ~10 MB de dependencias, y corre en el edge. La alternativa —Chrome headless— implica levantar un navegador por render.

Números concretos: **10 tiendas × 2 días de oferta × 4 formatos = 80 renders por semana ≈ 12 segundos de cómputo.** El costo de render es esencialmente cero, y no cambia si mañana son 30 tiendas.

Para retoque manual, un editor web ligero sobre las seis primitivas (Fabric.js, gratis) — o Polotno SDK (USD 899 una vez) si se prefiere comprar tiempo.

---

# Parte VI · Escalar de 7 a 30 tiendas

Esta parte responde directamente a *"acaban de abrir otras 2 tiendas… no voy a poner a todo mi equipo a hacer esto, no es negocio"*.

## 6.1 La matemática del modelo actual

Con producción manual, el costo crece **linealmente** con las tiendas:

| Tiendas | Flyers/mes | Horas/mes (a 45 min) | Personas necesarias |
|---:|---:|---:|---:|
| 7 | 56 | 42 h | ~1 (más ciclos de feedback) |
| 10 | 80 | 60 h | ~1.5 |
| 20 | 160 | 120 h | **~3** |
| 30 | 240 | 360 h | **~4.5** |

*(No incluye feedback, publicación ni gestión de anuncios, que suman al menos otro tanto.)*

**Cada tienda nueva cuesta lo mismo que la anterior.** El margen no mejora con la escala — que es exactamente lo que Marcela detectó cuando dijo *"no es negocio"*.

## 6.2 La matemática del modelo propuesto

| Tiendas | Trabajo humano/mes | Costo de infraestructura | Personas |
|---:|---:|---:|---:|
| 7 | ~7 h | USD 35 | 0.2 |
| 10 | ~10 h | USD 45 | 0.3 |
| 20 | ~18 h | USD 70 | 0.5 |
| 30 | ~25 h | USD 95 | **0.7** |

El trabajo humano crece **sublinealmente**: cada tienda nueva es más barata que la anterior, porque el catálogo, el glosario y el banco de imágenes ya están construidos. Los productos se repiten entre supermercados latinos.

```
   Horas/mes
   360 │                                          ● manual
       │                                    ●
   240 │                            ●
       │                    ●
   120 │            ●
       │      ●
    60 │  ●                    ○ ─ ─ ─ ─ ─ ─ ○ automatizado
    25 │  ○ ─ ─ ─ ○ ─ ─ ─ ○
       └────┬────┬────┬────┬────┬────┬────┬───
            7   10   15   20   25   30
```

**El cruce ocurre inmediatamente y la brecha se abre con cada tienda.** A 30 sucursales, la diferencia son ~335 horas al mes: entre dos y tres empleados a tiempo completo.

## 6.3 Qué se rompe, y cuándo

Ser honesto sobre los límites es parte de la propuesta:

| Escala | Qué aparece | Qué hay que hacer |
|---|---|---|
| **10–15 tiendas** | Nada. La arquitectura lo aguanta sin cambios. | — |
| **15–25** | Revisar 25 propuestas al día se vuelve trabajo. | Aprobación automática para tiendas con histórico limpio; revisión solo por excepción. |
| **25–40** | Varias personas revisando a la vez; conflictos. | Asignación y roles. Ya existe multi-usuario desde el diseño. |
| **40+** | Otras agencias quieren usarlo. | Multi-tenant en serio → es la [propuesta V2](./06-propuesta-v2-plataforma.md). |

**Decisión de diseño importante:** aunque hoy hay un solo cliente, la base de datos lleva `tenant_id` y aislamiento por fila desde el primer commit. **No cuesta nada hacerlo al empezar y es carísimo agregarlo después.**

---

# Parte VII · Arquitectura

```
┌─────────────────────────────────────────────────────────────────────┐
│ ① CAPTURA                                                           │
│                                                                     │
│   Martes/jueves 08:00 · el sistema propone                          │
│        │                                                            │
│        ├─ A · WhatsApp Flow pre-llenado ──┐  ← el objetivo          │
│        ├─ B · Email con Excel/PDF/foto ───┤  ← respaldo permanente  │
│        ├─ B'· Audio o foto de WhatsApp ───┤  ← el carnicero         │
│        └─ C · API del POS ────────────────┘  ← el futuro            │
└──────────────────────────────┬──────────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│ ② NORMALIZACIÓN                                                     │
│   Carril A → ya viene estructurado, solo se valida                  │
│   Carril B → extracción con visión + validación determinista        │
│   Acuse inteligente: confirmar lo dudoso ANTES de maquetar          │
└──────────────────────────────┬──────────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│ ③ CATÁLOGO  ← el activo que se acumula                              │
│   Producto canónico · PLU/UPC · nombre ES/EN · alias · categoría    │
│   Imagen con licencia y origen · histórico de precios y rotación    │
│   Postgres + pgvector · tenant_id desde el primer commit            │
└──────────────────────────────┬──────────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│ ④ COMPOSICIÓN                                                       │
│   Plantilla = JSON versionado por tienda                            │
│   Render Satori+resvg (50–200 ms) · Editor web para retoque         │
│   Salida a Canva solo si alguien la pide (transición)               │
└──────────────────────────────┬──────────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│ ⑤ APROBACIÓN                                                        │
│   El flyer vuelve al mismo hilo de WhatsApp · [Aprobar] [Cambiar]   │
└──────────────────────────────┬──────────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│ ⑥ PUBLICACIÓN · 21:00                                               │
│   Post en la página → ad set duplicado → ad con object_story_id     │
│   → a revisión de Meta esa noche  (+12 h de vigencia)               │
└──────────────────────────────┬──────────────────────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────────────┐
│ ⑦ MEDICIÓN  ← el foso                                               │
│   Meta Insights + QR/cupón por producto + (mes 4) datos del POS     │
│   Informe mensual de rotación y recomendación                       │
└─────────────────────────────────────────────────────────────────────┘
```

## Stack

Deliberadamente más simple que el del [documento 06](./06-propuesta-v2-plataforma.md) — en la reunión la reacción a ese stack fue *"eso está muy difícil"* y *"100,000 usuarios, eso ya es demasiado"*. Tenían razón: no hay que construir para 100,000 usuarios, hay que construir para 30 tiendas y no cerrarse puertas.

| Capa | Elección | Por qué |
|---|---|---|
| **Aplicación** | **Next.js + TypeScript** | Un solo lenguaje y un solo proyecto: tablero, API y render. El tipo `Offer` es el mismo objeto en todas partes. |
| **Base de datos** | **Postgres + pgvector** (Neon o Supabase) | Relacional y vectorial en un motor. `tenant_id` y aislamiento por fila desde el inicio. |
| **Captura** | **WhatsApp Business API** (Flows) | Lo que pidió Marcela. Meta Cloud API directa o vía proveedor. |
| **Extracción** | **Claude Sonnet 5** (visión + structured outputs) | Carril B. ~USD 0.05 por documento. |
| **Render** | **Satori + resvg** | 50–200 ms, sin navegador. |
| **Editor** | **Fabric.js** (o Polotno, USD 899) | Retoque sobre las seis primitivas. |
| **Almacenamiento** | **Cloudflare R2** | Egreso gratuito — es un producto que sirve imágenes. |
| **Orquestación** | **Cron + colas** en la propia app; n8n solo para integraciones sueltas | Menos piezas móviles = menos mantenimiento. |
| **Meta** | **Marketing API** directa | Con adaptador propio para absorber cambios. |

### Los cuatro controles de calidad

Como el desarrollo lo dirige una persona con agentes de IA, la disciplina no está en revisar cada línea sino en que el sistema se revise solo:

```
1. TYPECHECK      contratos rotos            segundos
2. TESTS          lógica rota                segundos
3. GOLDEN FILES   layout roto (diff píxel)   ~1 minuto
4. EVALS          extracción degradada       ~2 minutos
                  (50 documentos reales con su resultado esperado)
```

El punto 4 es el que casi nadie implementa y el que evita que la calidad se degrade en silencio: si la precisión de extracción baja del 95 %, el despliegue se detiene.

---

# Parte VIII · Plan por fases

Cada fase entrega valor sola. **Y el orden cambió respecto a la propuesta anterior: la captura va primero**, porque arreglarla hace más fácil todo lo demás.

## Fase 0 · Fundaciones (semanas 1–2)

- **Auditoría del banco de imágenes.** Clasificar origen y licencia; retirar lo dudoso. Sigue siendo lo más urgente: hay una demanda activa contra otra agencia por el mismo tipo de uso.
- Catalogación con metadatos (nombre ES/EN, categoría, alias, licencia).
- **Medición de línea base:** cronometrar 10 flyers reales, y preguntar a dos managers cuánto les toma a ellos preparar el Excel. Sin ese número no se puede demostrar nada.
- Diseño de la plantilla base como dato, y **conversación con Marcela sobre proponerle a su cliente un diseño nuevo** —ya dijo que puede.
- Alta de WhatsApp Business API y verificación del negocio.
- Preguntar a las sucursales si ya publican su circular en Flipp, Freshop, Mercatus o similar. Cinco minutos que pueden ahorrar meses.

## Fase 1 · La captura (semanas 3–7) ← *el cambio de orden*

- WhatsApp Flow con las cuatro pantallas por categoría.
- Carril B: extracción de Excel/PDF/foto con evals desde el día uno.
- Audio como entrada de primera clase.
- **Acuse inteligente** con confirmación antes de maquetar.
- Recordatorio escalonado martes y jueves.
- Registro de marcas de tiempo.
- **Piloto en 2 sucursales**, las demás siguen por email sin cambios.

> **Al final de esta fase**, aunque no se haya tocado el diseño: el dato llega estructurado, el manager tarda 90 segundos, y Fresco deja de transcribir. **Es la mitad del ahorro con menos de la mitad del trabajo.**

## Fase 2 · Composición (semanas 8–12)

- Motor de plantilla como dato, con las seis primitivas.
- Render de las plantillas de las 7 sucursales + las 2 nuevas.
- Editor web para retoque.
- Exportación a Canva como salida de transición.
- Fan-out a 4:5 y 9:16.

> **Al final:** menos de 5 minutos de trabajo humano por flyer.

## Fase 3 · Publicación y anuncios (semanas 13–16)

- Aprobación por WhatsApp en el mismo hilo.
- Publicación a las 21:00 + creación automática del anuncio (+12 h de vigencia).
- Cero pasos manuales en Ads Manager.
- **DCO: 10–15 variantes por campaña.** Meta necesita al menos 10 para que el algoritmo aprenda; hoy recibe una.

## Fase 4 · Medición y el foso (semanas 17–22)

- Códigos de oferta y QR por producto destacado.
- Informe mensual automático de rotación y recomendación.
- **Conversación con la sucursal más cercana para obtener su reporte de ventas por PLU.** Un CSV semanal basta.
- Primer informe con atribución real.

## Fase 5 · Inteligencia y escala (mes 6+)

- Propuesta pre-llenada basada en rendimiento medido, no solo en histórico.
- Alta de tienda nueva en minutos.
- Aprobación automática por excepción para sucursales con histórico limpio.

---

# Parte IX · Costos

## Operación mensual

| Concepto | 10 tiendas | 30 tiendas |
|---|---:|---:|
| Aplicación y base de datos | USD 20–30 | USD 40–60 |
| Almacenamiento e imágenes (R2) | 2–5 | 8–15 |
| **WhatsApp Business API** | 5–15 | 15–40 |
| IA (extracción, carril B) | 3–6 | 8–15 |
| Dominio, correo, monitoreo | 5 | 10 |
| **Total** | **USD 35–60** | **USD 80–140** |

*Nota sobre WhatsApp:* Meta cobra por conversación iniciada por el negocio. A 10 tiendas × 2 veces por semana son ~80 conversaciones al mes; a 30 tiendas, ~240. Sigue siendo marginal frente a cualquier alternativa.

## Licencias opcionales

| Concepto | Costo |
|---|---|
| Polotno SDK (si se prefiere no construir el editor) | USD 899, una vez |
| Canva Pro | Ya lo tiene; se puede mantener durante la transición |

## Comparación a tres años

| | Inicial | Mensual | 3 años |
|---|---:|---:|---:|
| Propuesta A (Escobedo) | USD 5,000 | 500 + add-ons por template | **USD 23,000+** |
| Contratar una persona más | — | ~2,500–3,500 | **USD 90,000–126,000** |
| **Flymar (operación)** | *desarrollo con agentes* | **35–140** | **USD 1,300–5,000** |

La comparación relevante ya no es contra las propuestas rechazadas. **Es contra contratar.** Marcela lo dijo: a 20 o 30 tiendas hacen falta dos o tres personas más. Esa es la alternativa real, y cuesta entre 18 y 25 veces más.

---

# Parte X · Indicadores

| Indicador | Hoy | Mes 3 | Mes 6 |
|---|---|---|---|
| Minutos de Fresco por flyer | 45–60 | < 15 | **< 5** |
| **Minutos del manager por envío** | 20–30 | < 5 | **< 2** |
| Flyers sin feedback de ortografía o precio | por medir | > 85 % | **> 95 %** |
| Sucursales que responden antes del mediodía | por medir | 60 % | **> 90 %** |
| Vigencia efectiva en campaña de 48 h | ~36 h | ~48 h | ~48 h |
| Pasos manuales en Ads Manager | 9 × 80/mes | — | **0** |
| Variantes creativas por campaña | 1 | 4 | **10–15** |
| **Tiendas por persona del equipo** | ~7 | 15 | **> 30** |
| Sucursales con informe de rotación | 0 | 0 | **≥ 1** |
| Banco de imágenes con licencia documentada | por medir | > 60 % | **100 %** |

**Indicadores de modelo de negocio** — los que miden si la reinvención está ocurriendo:

| Indicador | Hoy | Mes 6 | Mes 12 |
|---|---|---|---|
| Escalón del modelo de cobro (0–6) | **0** · fee por pieza | **3** · suscripción por tienda | **4** · con componente variable |
| Ingreso recurrente mensual predecible | ~0 % | > 70 % | **> 90 %** |
| Margen por tienda | plano | creciente | **creciente** |
| Clientes que reciben inteligencia de mercado | 0 | ≥ 2 | **todos** |
| Conversaciones abiertas con marcas CPG | 0 | 0 | **≥ 1** |

El indicador **"tiendas por persona"** es el que responde a *"no es negocio"*. El **"escalón del modelo"** es el que responde a *"que sientan que me necesitan"*. Los demás son medios; esos dos son el fin.

---

# Parte XI · Riesgos

| # | Riesgo | Mitigación |
|---|---|---|
| R1 | **El manager no adopta el formulario** | Tres carriles: nadie está obligado. Piloto en 2 sucursales y se mide. La propuesta pre-llenada hace que trabajen *menos*, no más — es la adopción por conveniencia, no por imposición. |
| R2 | **Se percibe que "el manager lo hace todo"** | Marca blanca de Fresco, lenguaje de servicio (*"preparamos tu propuesta"*), y el informe de rotación al frente de la relación. Ver Parte II. |
| R3 | **El diseño propio no convence al cliente final** | Salida a Canva durante la transición. Y Marcela ya dijo que puede proponer un diseño nuevo. Se valida con una sucursal antes de extender. |
| R4 | **La aprobación de WhatsApp Business tarda** | Se inicia en Fase 0. Mientras tanto, el carril B (email) funciona igual. |
| R5 | **No se consigue el dato del POS** | Las vías ①②③ de la Parte IV no dependen de la tienda. El POS mejora la precisión, no habilita la medición. |
| R6 | **Deriva de calidad de la extracción** | Evals sobre 50 documentos reales bloqueando el despliegue. |
| R7 | **Reclamación legal por una imagen** | Auditoría en Fase 0. Es el riesgo con mayor severidad y el más barato de mitigar. |
| R8 | **Sobre-construir** | Alcance del editor congelado en seis primitivas. Multi-tenant es una columna, no un producto. |
| R9 | Cambios en las APIs de Meta | Adaptador propio; toda la lógica de Meta detrás de una interfaz. |

---

# Parte XII · Lo que esta propuesta no promete

1. **No promete cero intervención humana.** Con el carril A (formulario) el dato llega limpio y la revisión es mínima; con el carril B siempre habrá que confirmar. La meta son 5 minutos, no cero.
2. **No promete que el manager no se dé cuenta de que hay un sistema.** Promete que **no le importe**, porque lo que recibe de vuelta —el informe de qué vendió— no lo puede conseguir en otro lado. Ocultar la herramienta es táctica; la atribución es estrategia.
3. **No promete atribución exacta de ventas.** Sin píxel ni comercio electrónico, lo que se logra es una señal fuerte y direccional. Con datos del POS, se acerca mucho. Nunca será una atribución de e-commerce, y no hay que venderla como tal.
4. **No promete que las 10 sucursales adopten WhatsApp.** Probablemente algunas nunca lo hagan. Por eso hay tres carriles.
5. **No promete resolver la auditoría legal de las imágenes.** Aporta el inventario y el control; retirar imágenes y producir reemplazos son decisiones del negocio.

---

# Parte XIII · Los primeros pasos

**Esta semana:**
1. Confirmar el cambio de alcance: se levanta la restricción de Canva y la captura pasa a ser la Fase 1.
2. Elegir las **2 sucursales piloto** para el formulario — idealmente una de las dos recién abiertas, que no tiene costumbres que romper.
3. Iniciar la verificación de WhatsApp Business API (es lo que más tarda).
4. Acceso de lectura a la carpeta de Canva para inventariar el banco de imágenes.
5. **Preguntar a dos managers cuánto tiempo les toma preparar el Excel.** Es el número que justifica todo el cambio ante el cliente final.

**Semanas 2–3:**
6. Diseñar la plantilla base como dato y presentarle a Marcela la versión propia frente a la de Canva.
7. Preguntar a las sucursales si ya publican su circular en Flipp, Freshop o Mercatus.
8. Definir con Marcela el guion de comunicación hacia los managers: **qué se les dice y qué no.** Es una decisión de negocio, no técnica, y conviene tomarla antes de escribir el primer mensaje.
9. **Conversación aparte sobre el modelo de cobro.** Es la más importante de todas y no depende del código: revisar la escalera de la Parte II y decidir cuándo se propone la suscripción por tienda. Construir el sistema sin cambiar el modelo solo abarata la misma cosa.

---

## Anexo · Decisiones y su justificación

| Decisión | Alternativa descartada | Razón |
|---|---|---|
| Captura antes que diseño | Diseño primero (propuesta V1) | Arreglar el origen hace más fácil todo lo demás. Y es lo que Marcela pidió. |
| WhatsApp Flows | Portal web · Google Forms | Cero fricción, 12× más respuesta, y devuelve dato estructurado. Un portal exige cuenta y capacitación. |
| Propuesta pre-llenada | Formulario en blanco | Mismo mecanismo, percepción opuesta: *"me lo preparan"* en vez de *"lo hago yo"*. Y baja el esfuerzo del manager de 25 min a 90 s. |
| Motor de composición propio | Canva Design Editing API | La restricción se levantó. El motor propio quita dependencias, límites de plan y una API en beta. |
| Salida a Canva en la transición | Corte seco | El equipo sabe Canva; la confianza se construye gradual. |
| Satori + resvg | Chrome headless | 50–200 ms sin navegador. 80 renders semanales = 12 segundos de cómputo. |
| Medición de venta desde Fase 4 | Seguir con engagement | Los likes son falsificables y no defienden el negocio. La atribución sí. |
| `tenant_id` desde el primer commit | Añadirlo cuando haga falta | No cuesta nada al empezar; es carísimo después. |
| Stack simple | El stack del documento 06 | La reacción en la reunión fue correcta: no hay que construir para 100,000 usuarios. |

---

← [07 · Reinventar la captura](./07-reinventar-la-captura.md) · [Índice](../README.md)
