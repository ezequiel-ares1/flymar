# 07 · Reinventar la captura — cómo entra la información y cómo debería entrar

Análisis del canal de entrada (email + Excel/PDF/foto) y de las alternativas para ordenarlo, acelerarlo o eliminarlo. Investigación del 2026-08-08.

---

## 1. El diagnóstico: el dato no nace desordenado, se desordena en el camino

Todo el proyecto asume que la información llega mal y hay que interpretarla. Es cierto, pero es un síntoma. La causa es otra, y se ve mirando la cadena completa:

```
┌─────────────────────────────────────────────────────────────────┐
│  ORIGEN — el POS del supermercado                               │
│  PLU 469 · "PIERNA DE POLLO" · $0.89/LB · vigencia 13–14 may   │
│  ✅ dato estructurado, con código de producto y precio real     │
└────────────────────────┬────────────────────────────────────────┘
                         │  ⚠️  alguien lo mira y lo copia a mano
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│  DEGRADACIÓN 1 — la maqueta                                     │
│  Excel con celdas fusionadas · PDF de 4 columnas · foto         │
│  ❌ el dato se vuelve imagen: pierde tipo, relación y código    │
└────────────────────────┬────────────────────────────────────────┘
                         │  email con adjunto
                         ▼
┌─────────────────────────────────────────────────────────────────┐
│  DEGRADACIÓN 2 — la re-lectura                                  │
│  El equipo de Fresco lo lee y lo vuelve a transcribir           │
│  ❌ se paga otra vez el costo de estructurar, con más errores   │
└────────────────────────┬────────────────────────────────────────┘
                         ▼
                       Flyer
```

**La información se estructura, se destruye y se vuelve a estructurar.** Todo el esfuerzo de extracción con IA del que hablan las propuestas —la mía incluida— es el costo de reparar un daño que ocurrió antes, río arriba.

### La prueba está en los propios anexos

Tres evidencias que no dejan lugar a dudas:

**① `Ejemplo 1.jpg` es la foto de un monitor.** Alguien tenía los datos en pantalla y, en lugar de exportarlos, le tomó una fotografía. Es la degradación en su forma más pura: de píxeles con estructura a píxeles sin ella.

**② El Excel real trae los PLU y nadie los usa.** En `TWO DAYS OFFERS 05_13-05_14-.xlsx`, la fila 15 contiene `469`, `573`, `PACKAGED MEAT`; más abajo `3454`, `4062`, `3202052201`, `7152402097`. Son códigos de producto —PLU y UPC—. **Es el identificador único que resolvería el 90 % del problema de matching de imágenes y de traducción**, y hoy se descarta porque el flyer no lo muestra.

**③ El manager escribe instrucciones dentro del archivo.** `"USE IMAGES OF COOKED MEAT FOR PACKAGED MEAT"`. Sabe lo que quiere, pero no tiene dónde ponerlo salvo un comentario al margen de una hoja de cálculo.

## 2. Seis niveles de intercepción, de menos a más ambicioso

La pregunta correcta no es "¿cómo leo mejor el email?" sino **"¿qué tan arriba puedo interceptar el dato?"**. Cada nivel es independiente: se pueden implementar varios a la vez, para tiendas distintas.

### Nivel 0 · Donde estamos

Correo a una bandeja general, adjunto de formato libre, sin acuse automático, sin estructura, sin trazabilidad de quién envió qué y cuándo.

---

### Nivel 1 · Ordenar el canal actual, sin pedirle nada al manager

El email se queda. Lo que cambia es todo lo demás. **Impacto alto, esfuerzo bajo, cero fricción con el cliente.**

| Medida | Qué resuelve |
|---|---|
| **Dirección dedicada por tienda** (`metropolitan@ofertas.fresco…`) | Elimina el paso de adivinar de qué sucursal es cada correo. La identidad viene del canal, no del contenido. |
| **Asunto estructurado sugerido** (`OFERTAS · Metropolitan · 13-14 may`) | Fecha y vigencia sin inferirlas del adjunto. |
| **Acuse de recibo automático en 30 segundos** | Hoy alguien responde a mano *"ya las recibimos"* (visible en la reunión, minuto 18:42). Automatizarlo es trivial y libera atención. |
| **Plantilla de hoja con validación** (Google Sheet vinculado a cada tienda) | Listas desplegables para unidad, validación de formato de precio, categoría obligatoria. **Elimina errores en origen, y no cuesta nada.** |
| **Campo de notas explícito** | El manager deja de escribir instrucciones al margen; hay un lugar para `"usa imágenes de carne cocida"`. |
| **Recordatorio escalonado** | El sistema sabe que los martes y jueves hay ofertas. A las 08:00 avisa; a las 14:00, si faltan tres tiendas, insiste solo a esas tres. |

> Hoy la falta de puntualidad del insumo es un cuello de botella real: *"esta sucursal no me envió el feedback a tiempo, entonces eso se hace hasta el día siguiente"* (minuto 33:07).

---

### Nivel 2 · El acuse inteligente — mover el feedback al principio

**Esta es, en mi opinión, la mejora de mayor retorno de todo el documento, y no requiere que el manager cambie absolutamente nada.**

Hoy el ciclo de correcciones ocurre **al final**, sobre el flyer ya maquetado:

```
correo → extracción → maquetación → envío → «este precio está mal» → corregir → reenviar → aprobar
                                              ↑
                                    el error se descubre aquí,
                                    después de todo el trabajo
```

Basta con moverlo al principio. Treinta segundos después de recibir el correo, un agente responde **en el mismo hilo** con lo que entendió:

```
Recibimos sus ofertas de Metropolitan para el 13–14 de mayo. Esto es lo que leímos:

CARNICERÍA
  1. Pierna de pollo ................ $0.89 / LB
  2. Discada de puerco .............. $2.89 / LB
  3. Caldo mixto de res ............. $2.99 / LB
  4. Chorizo en tripa ............... $2.99 / LB

PRODUCE
  1. Naranja bolsa 10 lb ............ $7.99
  2. Papaya ......................... $0.89 / LB
  3. Coco seco ...................... $2.29 c/u

⚠️  Dos cosas por confirmar:
    · "Gelatina circular 12″" — ¿el precio $14.99 es por pieza?
    · No tenemos foto de "Carlotas de vaso". ¿Nos manda una?

Si algo está mal, responda a este correo y lo corregimos.
Si no responde en 2 horas, seguimos con esto tal cual.
```

Por qué esto cambia la economía del proceso:

- **Corregir un precio en una tabla cuesta segundos; corregirlo en un flyer maquetado, aprobado y programado cuesta un ciclo completo.**
- Convierte el email en un formulario conversacional **sin pedirle al manager que abra nada**.
- Las dudas del extractor (baja confianza) se resuelven con quien tiene la respuesta, no adivinando.
- Genera un registro con marca de tiempo de qué se envió y qué se confirmó — útil cuando después hay discusión sobre un precio.
- El "sin respuesta en 2 horas = aprobado" evita que el acuse se convierta en un nuevo bloqueo.

Técnicamente es un patrón ya establecido: tratar el correo como **flujo de eventos estructurado**, no como buzón. Los proveedores de email para agentes (AgentMail, Postmark, Mailtrap) exponen parsing entrante, continuidad de hilo y respuesta sobre el mensaje recibido sin construir cabeceras a mano.

---

### Nivel 3 · Cambiar de canal: WhatsApp

Aquí hay un dato que obliga a replantear la premisa de que "todo debe ser por email".

| Métrica | Email | WhatsApp |
|---|---|---|
| Tasa de apertura | 20–25 % real | **98 %** |
| Tasa de respuesta en conversación | 1–5 % | **40–60 %** (hasta **12×** más) |
| Uso regular entre hispanos en EE. UU. | — | **46 %** |
| Adopción entre pequeñas empresas (LatAm, 2023→2025) | — | 22 % → **45 %** |

Y la pieza técnica que lo vuelve viable para captura de datos: **WhatsApp Flows**, formularios estructurados de varias pantallas **dentro del propio chat**. El manager no sale de WhatsApp, no instala nada, no crea cuenta — y del otro lado llega **JSON validado**, no un adjunto.

Un flujo realista:

```
[WhatsApp · martes 08:00]
  Flymar: Buenos días. ¿Enviamos las ofertas de Metropolitan
          para el 13–14 de mayo?
          [ Sí, empezar ]  [ Esta semana no ]

  → toca «Sí, empezar» → se abre el formulario dentro del chat
     ┌────────────────────────────────┐
     │ CARNICERÍA          (1 de 4)   │
     │ Producto  [Pierna de pollo ▾]  │  ← autocompleta del histórico
     │ Precio    [0.89            ]   │
     │ Unidad    [LB              ▾]  │  ← lista cerrada
     │ [ + otro ]        [Siguiente]  │
     └────────────────────────────────┘
```

**La objeción de Ana Marcela era razonable y hay que tomarla en serio:** *"En cada sucursal hay muchos managers y a veces los cambian. Me ha costado mucho trabajo, primero, que me lo envíen por email"*.

Pero esa objeción apunta a un **portal web** —crear cuenta, recordar contraseña, aprender una interfaz—. WhatsApp es lo contrario: **es la app que el manager ya tiene abierta**. No hay onboarding, no hay contraseña, y cuando cambian de manager basta con agregar el número nuevo. Si algo, la barrera es más baja que el email.

**Recomendación:** no reemplazar el email. **Ofrecer WhatsApp como vía alternativa a una sola tienda como piloto** y medir puntualidad y errores contra las demás. El dato decidirá.

---

### Nivel 4 · Aceptar el caos, pero estructurarlo al entrar

Hay managers que no van a llenar un formulario nunca. El de carnicería a las 6 de la mañana no abre un Sheet — pero **manda un audio de WhatsApp en treinta segundos**.

> *"Oye, para el martes ponme pierna de pollo a ochenta y nueve la libra, discada de puerco a dos ochenta y nueve, y el caldo mixto a dos noventa y nueve."*

Eso hoy es intransformable. Con transcripción + extracción estructurada, es un `OfferSheet` completo. El estado del arte en 2026 lo confirma: la transcripción cruda es un problema resuelto; **el diferenciador es lo que ocurre después — extracción de entidades, estructuración e integración**.

Lo mismo aplica a la foto del cartel escrito a mano, al mensaje de texto suelto y al reenvío de un correo del mayorista.

**Advertencia técnica seria:** cuando la transcripción alimenta agentes autónomos, **un error se propaga por toda la cadena**. Un `$0.89` mal oído como `$8.90` publicado en Facebook es un problema comercial y potencialmente legal. Por eso el audio **siempre** debe pasar por el acuse del Nivel 2 con confirmación explícita — nunca autoaprobado.

---

### Nivel 5 · Ir al origen: el POS de la tienda

Aquí desaparece el problema en lugar de resolverse.

Los sistemas de punto de venta que usan los supermercados independientes en EE. UU. **tienen API**:

| Sistema | Notas |
|---|---|
| **ECRS Catapult** | Integra POS, inventario y precios en un solo sistema, con actualización de precios en tiempo real en todos los puntos de contacto |
| **IT Retail** | Construido específicamente para entornos de grocery independiente |
| **LOC Software** | Integraciones vía API unificada, sin gestionar integraciones individuales |
| **Vori** | Conecta checkout, precios, pedidos, lealtad y reportes |
| **RORC POS** | Sistema central de operaciones para supermercados independientes |

El precio promocional, su vigencia y el PLU **ya existen ahí antes de que el manager escriba nada**. Un *pull* semanal elimina el paso humano por completo: el flyer se genera de la fuente de verdad.

**Realismo:** requiere que el dueño del supermercado autorice el acceso, que el POS tenga API en su versión contratada, y una integración por sistema. No es para el mes uno. **Pero para una tienda grande y estable es la jugada de mayor retorno de todas**, y es el tipo de integración que hace que un proveedor sea imposible de reemplazar.

---

### Nivel 6 · El estándar que ya existe en la industria

Dos hallazgos que conviene conocer aunque no se usen de inmediato:

**① EDI 889 — Promotion Announcement.** Es el conjunto de transacciones X12 estandarizado para comunicar información promocional entre fabricantes, distribuidores y minoristas: montos de descuento, fechas y condiciones de desempeño. **Kroger lo exige a sus socios comerciales.** Puede automatizarse por completo para notificar a muchos socios sin errores de tecleo.

Si el supermercado ya recibe 889 de sus CPG, **las promociones existen en formato estructurado antes de que el manager las escriba a mano**.

**② La plataforma de circular digital que quizá ya usan.** Freshop —muy extendido entre grocers independientes— incluye *"integración de circular digital semanal que evita que tu personal capture manualmente cada artículo en oferta"*. Y **Flipp se asoció con Independent Grocers Alliance (IGA)** para circulares digitales localizadas por tienda.

**Vale la pena preguntar a cada sucursal si ya publica su circular en alguna de estas plataformas.** Si la respuesta es sí, el dato estructurado ya existe y alguien lo está capturando dos veces.

---

## 3. Tres reinversiones de proceso, independientes del canal

### 3.1 Invertir el flujo: que el manager confirme en vez de escribir

El cambio conceptual más potente y el más barato de implementar una vez que existe el histórico.

Hoy: *"mándame tus ofertas"* → el manager escribe 16 productos desde cero.
Propuesto: *"esto es lo que creemos que va esta semana, confirma o ajusta"*.

El sistema conoce el histórico de esa tienda: qué se ofertó, cuándo, a qué precio, con qué rotación. Puede pre-llenar un borrador con los 16 productos más probables y pedir solo confirmación de precios.

```
Metropolitan · martes 13 de mayo · borrador sugerido

  ✓ Pierna de pollo ......... $0.89/LB   (igual que hace 3 semanas)
  ✓ Discada de puerco ....... $2.89/LB
  ⚠ Papaya .................. $0.89/LB   (subió de $0.69, ¿confirmas?)
  ? Chorizo en tripa ........ ____       (falta precio)

  [ Confirmar todo ]   [ Ajustar ]
```

**El esfuerzo del manager pasa de veinte minutos a treinta segundos.** Y eso ataca directamente el problema de puntualidad, que es lo que hace que las ofertas se terminen al día siguiente.

### 3.2 Registrar la puntualidad como métrica

Hoy nadie mide cuánto tarda cada sucursal en enviar y en aprobar; se percibe. Con marcas de tiempo automáticas:

```
Tienda          Envió        Confirmó     Aprobó flyer   Estado
Metropolitan    mar 07:40    07:42        11:15          ✓ a tiempo
Blue Ridge      mar 13:20    —            —              ⚠ tarde 2 sem. seguidas
Independence    —            —            —              ✗ sin enviar · recordado 2×
```

Es un dato para conversar con el cliente con evidencia, y para saber a qué sucursal conviene ofrecerle WhatsApp o la integración con POS.

### 3.3 Un solo hilo por semana y por tienda

Hoy la conversación se fragmenta entre el correo del manager, el hilo de Slack del equipo y la tarea de Asana. Un identificador estable (`METRO-2026-W20`) que atraviese correo, tablero y campaña permite reconstruir cualquier semana completa en un clic: qué se pidió, qué se interpretó, qué se corrigió, qué se publicó y cuánto rindió.

---

## 4. Matriz de decisión

| Nivel / medida | Esfuerzo | Impacto | Fricción para el manager | Cuándo |
|---|---|---|---|---|
| **Acuse inteligente (N2)** | Bajo | **Muy alto** | **Ninguna** | 🟢 Ya |
| Buzón por tienda + asunto (N1) | Muy bajo | Alto | Ninguna | 🟢 Ya |
| Plantilla de hoja con validación (N1) | Muy bajo | Alto | Baja | 🟢 Ya |
| Recordatorio escalonado (N1) | Bajo | Medio | Ninguna | 🟢 Ya |
| Métrica de puntualidad (3.2) | Bajo | Medio | Ninguna | 🟢 Ya |
| **Audio y foto libres (N4)** | Medio | Alto | **Negativa** (les facilita) | 🟡 Fase 2 |
| **WhatsApp Flows (N3)** | Medio | **Muy alto** | Baja | 🟡 Piloto en 1 tienda |
| **Confirmar en vez de escribir (3.1)** | Medio | **Muy alto** | **Negativa** | 🟡 Requiere histórico (~2 meses) |
| Integración con POS (N5) | Alto | **Máximo** | Ninguna | 🔵 Fase 3, tienda ancla |
| Feed de circular digital (N6) | Bajo–Medio | Alto | Ninguna | 🔵 Preguntar primero |
| EDI 889 (N6) | Alto | Medio | Ninguna | ⚪ Solo si ya existe |

> **"Fricción negativa"** significa que el manager trabaja *menos* que hoy. Son las medidas que se adoptan solas, sin negociación.

## 5. Lo que NO recomiendo

**Un portal web para managers.** La objeción de Ana Marcela es correcta y está bien fundada: los managers rotan, y conseguir siquiera el correo ordenado costó trabajo. Un portal añade cuenta, contraseña y aprendizaje a cambio de nada que WhatsApp Flows no dé con menos fricción.

**Obligar a un único formato.** Aceptar los tres formatos actuales *y* audio *y* foto no es debilidad arquitectónica: es lo que evita que el sistema se rompa cuando entra un manager nuevo que hace las cosas a su manera. La estandarización debe ser un **incentivo** (quien usa la plantilla recibe su flyer antes), nunca un requisito.

**Automatizar la aprobación de audio.** Un error de transcripción en un precio se propaga hasta Facebook. El audio siempre pasa por confirmación explícita.

## 6. Recomendación

**Ahora (semanas 1–2), sin tocar nada del lado del cliente:**
1. Buzón dedicado por tienda + convención de asunto.
2. **Acuse inteligente con la tabla interpretada y confirmación en 2 horas.** ← la de mayor retorno
3. Plantilla de hoja con validación, ofrecida sin obligar.
4. Recordatorio escalonado los martes y jueves.
5. Registrar marcas de tiempo desde el primer día — sin el histórico, nada de lo demás se puede medir.

**Fase 2 (mes 2–3):**
6. Audio y foto libres como entradas de primera clase.
7. Piloto de WhatsApp Flows en **una** tienda, midiendo puntualidad y errores contra las demás.
8. Preguntar a las siete sucursales si ya publican su circular en Flipp, Freshop, Mercatus o similar. Es una pregunta de cinco minutos que puede ahorrar meses.

**Fase 3 (mes 4+):**
9. Con dos meses de histórico, activar el borrador anticipado: confirmar en vez de escribir.
10. Evaluar integración con el POS de la sucursal más grande y estable.

---

## Cierre

La conclusión más útil de esta investigación es que **el email nunca fue el problema**. El problema es que el dato nace estructurado en el POS, alguien lo convierte en una imagen, y en Fresco alguien lo vuelve a convertir en dato.

Se puede atacar por los dos extremos: **hacia abajo**, extrayendo mejor lo que llega —que es lo que hace la propuesta V1—; y **hacia arriba**, interceptando el dato antes de que se degrade.

Ambos suman, pero **el segundo tiene un techo mucho más alto**: la extracción perfecta te deja donde el POS ya estaba.

---

## Fuentes

**Email como flujo estructurado**
- [AgentMail — Best Inbound Email APIs for AI Agents (2026)](https://www.agentmail.to/blog/best-inbound-email-apis-ai-agents)
- [Mailtrap — 7 Best Email APIs for AI Agents (2026)](https://mailtrap.io/blog/best-email-api-for-ai-agents/)
- [Mastra — The 8 Best AI Agent Email Providers (julio 2026)](https://mastra.ai/blog/best-ai-agent-email-providers)

**WhatsApp**
- [Zargham Labs — WhatsApp Flows: Interactive In-Chat Forms (2026)](https://www.zarghamlabs.com/whatsapp-flows-interactive-forms-guide-2026/)
- [Infobip — WhatsApp Flows](https://www.infobip.com/blog/whatsapp-flows)
- [NimbleBiz — WhatsApp vs Email vs SMS Open Rates: 2026 Benchmarks](https://nimblebiz.ai/blog/whatsapp-open-rate-vs-email-vs-sms-benchmarks)
- [Aurora Inbox — Adopción de WhatsApp Business en LatAm por país](https://www.aurorainbox.com/en/2026/03/05/whatsapp-business-latam-adoption/)
- [YCloud — 100+ WhatsApp Statistics of 2026](https://www.ycloud.com/blog/whatsapp-statistics-for-businesses)

**Voz y captura no estructurada**
- [UMEVO — From Raw Audio to Actionable Intelligence (2026)](https://www.umevo.ai/blogs/ume-all-posts/the-2026-guide-to-ai-voice-recorder-features-from-raw-audio-to-actionable-intelligence)
- [Galileo — Top Enterprise Speech-to-Text Solutions 2026](https://galileo.ai/blog/top-enterprise-speech-to-text-solutions-for-enterprises)

**POS de grocery independiente**
- [IT Retail — Grocery Store API 101](https://www.itretail.com/blog/grocery-store-api)
- [Local Express — Grocery POS Integration Guide](https://www.localexpress.io/post/grocery-pos-integration-guide-ncr-toshiba-it-retail-more)
- [LOC Software — Integraciones](https://locsoftware.com/solutions/integrations/)
- [Vori — Grocery POS](https://www.vori.com/)

**Estándares y plataformas de circular**
- [SPS Commerce — EDI 889 Promotion Announcement](https://www.spscommerce.com/edi-document/edi-889-promotion-announcement/)
- [Stedi — X12 889 Promotion Announcement](https://www.stedi.com/edi/x12/transaction-set/889)
- [Kroger — EDI Programs and Requirements: Promotions](https://edi.kroger.com/EDIPortal/ProgramAndRequirements/Kroger/PR_Promotions_BO.html)
- [Flipp — Partnership with Independent Grocers Alliance](https://corp.flipp.com/resources/flipp-partners-with-independent-grocers-alliance-to-modernize-and-support-how-independent-grocers-compete-and-grow/)
- [AppIntent — Omnichannel Grocery Software 2026 (Freshop, Mercatus, Local Express)](https://www.appintent.com/software/grocery/omnichannel/)

---

← [06 · Propuesta V2](./06-propuesta-v2-plataforma.md) · [Índice](../README.md)
