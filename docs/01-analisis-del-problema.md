# 01 · Análisis del problema

**Proyecto:** Flymar — Automatización de flyers de ofertas y anuncios en Meta
**Cliente:** Fresco Marketing (Kansas City, MO) · contacto: Ana Marcela
**Fuentes:** PDF "Automatización de Flyers de Ofertas", transcripción de la reunión del 2026-08-08, anexos (Excel real, PDF de ofertas, foto de ofertas, captura de biblioteca de Canva)
**Fecha:** 2026-08-08

---

## 1. Qué hace el negocio

Fresco Marketing gestiona contenido y publicidad en Meta para **supermercados latinos en Estados Unidos**. El producto recurrente es el **flyer de ofertas**: una pieza gráfica bilingüe (inglés / español) que anuncia los productos en promoción de cada sucursal.

| Dimensión | Dato | Fuente |
|---|---|---|
| Sucursales atendidas | 5–10 (habitualmente 7, a veces 8) | Transcripción 30:25 |
| Días de ofertas | Martes y jueves | Transcripción 30:00 |
| Duración típica | 2 días (también hay de 4 días) | Transcripción 22:10, Anexo "Ejemplo 1" |
| Plantillas distintas | ~5 para ofertas de 2 días; ~7 para fin de semana | Transcripción 33:15 |
| Productos por flyer | 12–16 (3–4 por categoría × 4 categorías) | Anexos Excel / PDF |
| Canal de publicación | **Solo Meta** (Facebook) | Transcripción 21:04 |
| Objetivo de campaña | Engagement (sin píxel, sin conversiones) | Transcripción 26:26 |

**Volumen estimado:** ~14 flyers/semana ≈ **56–60 flyers al mes**, cada uno con su post y su anuncio.

## 2. El proceso actual, paso a paso

```
[1] Manager de la sucursal envía ofertas por email (Excel | PDF | foto)
        │
[2] Se leen y se categorizan los productos manualmente
        │
[3] Se traducen los nombres EN ⇄ ES, se escriben precios y unidades
        │
[4] Se busca la imagen de cada producto en las carpetas de Canva
    (si no está: la pide al manager o la busca en Google  ← riesgo legal)
        │
[5] Se arma el flyer manualmente sobre la plantilla de la tienda en Canva
        │
[6] Se descarga, se recorta el borde y se envía por email al manager
        │
[7] Feedback ("cambia este precio", "esta palabra está mal escrita",
    "no es esa imagen", "mueve el melón donde está el cilantro")
        │
[8] Corrección → reenvío → aprobación
        │
[9] Se programa el post en Facebook para las 21:00
        │
[10] AL DÍA SIGUIENTE: se entra a Ads Manager, se duplica el ad set,
     se cambian fechas y presupuesto, se selecciona el post y se publica
```

**Coordinación interna:** el seguimiento del día vive en un canal de **Slack** (mensaje que se copia y reescribe con el estado de cada sucursal); **Asana** solo marca "publicado" al final del día, con tareas que se generan automáticamente cada semana.

## 3. Los insumos reales (lo que muestran los anexos)

Este es el punto que más condiciona el diseño técnico: **los tres formatos de entrada no son tabulares**, son *maquetas visuales*.

### 3.1 Excel — `TWO DAYS OFFERS 05_13-05_14-.xlsx`

Al inspeccionar el archivo (una sola hoja, `0513-0514`), la estructura no es una tabla sino una **cuadrícula de tarjetas con celdas fusionadas**:

```
Fila 10  →  [encabezado de categoría]  "FRESH MEAT & PACKAGED MEAT"
Fila 14  →  numeración de posición       1        2        3
Fila 16  →  unidad                       LB       LB       LB
Fila 17  →  producto           PIERNA DE POLLO / DISCADA DE PUERCO / CALDO MIXTO DE RES
Fila 20  →  precio                       0.89     2.89     2.99
Fila 29  →  siguiente categoría          "PRODUCE"
...
```

Además contiene **5 imágenes incrustadas** (sellos USDA Choice, Halal, Family Pack, IBP…) y notas al margen del propio manager (`"USE IMAGES OF COOKED MEAT FOR PACKAGED MEAT"`).

> **Implicación:** un parser de Excel convencional (leer columnas A, B, C) **falla**. La lectura correcta requiere interpretación visual o reconstrucción por bloques. Este es exactamente el caso de uso de un modelo con visión.

### 3.2 PDF — `Ejemplo 2.pdf`

Cuatro columnas, una por categoría, con tres celdas apiladas por producto (precio / nombre / unidad). El orden de columnas del cliente es `PRODUCE | BAKERY | MEAT | GROCERY`, es decir, **distinto** al orden que necesita el flyer.

### 3.3 Foto — `Ejemplo 1.jpg`

Fotografía de una pantalla con la tabla de ofertas de "Mercado Fresco Metropolitan", con orden `BAKERY → MEAT → PRODUCE → GROCERY`. Los nombres ya vienen bilingües en algunos casos (`CARNE DE RES PARA HAMBURGESA / HAMBURGER PATTIES` — nótese la falta de ortografía en el original) y en otros no.

### 3.4 Reglas de negocio que se deducen de los tres

| Regla | Detalle |
|---|---|
| **Orden de salida fijo** | Carnicería (Meat) → Produce → Grocery → Bakery. El orden de entrada es irrelevante y siempre distinto. |
| **Bilingüismo** | Inglés + español, salvo tiendas de producto internacional que van solo en inglés. |
| **Unidades** | `LB`, `EA`, `BOX`, `6 PK`, `12 PK`, `18 OZ`, `3 L`, `72 CT`, `12"`… — texto libre, no enumerado cerrado. |
| **Formatos de precio** | `0.99` · `$3.49 / LB` · `2 X 1.00` · `2/$7.00` · `2 EA / $3.00`. Cinco sintaxis para el mismo concepto. |
| **Notas del manager** | Instrucciones libres dentro del archivo ("usa imágenes de carne cocida"). Deben capturarse, no descartarse. |

## 4. La biblioteca de imágenes

Vive en **carpetas de Canva**, organizada por departamento:

```
Carnicería/
├── Chicken/Pollo Fotos oficiales   (44 items)
├── Res/Beef Fotos oficiales        (78 items)
├── Puerco/Pork Fotos oficiales     (40 items)
└── Comida Cocinada / Cooked food    (7 items)
Produce/    Grocery/    Bakery/
```

Dos observaciones críticas:

1. **Procedencia mixta.** Parte son fotos propias de la tienda ("Fotos oficiales"); otra parte proviene de internet.
2. **Ya hay un precedente legal.** Otra agencia que trabaja con el mismo cliente usó una foto de carne encontrada en internet y **está siendo demandada** por el autor, que la localizó con una herramienta de IA (transcripción 45:57–46:37). Fresco Marketing ya empezó a producir su propio banco, priorizando carnicería.

## 5. La operación en Meta

El flujo del anuncio, textualmente descrito por Ana Marcela (transcripción 24:39–26:24):

1. Publicar el post en la página desde Meta Business Suite (programado a las 21:00).
2. Al día siguiente entrar a **Ads Manager → Campañas** y elegir la campaña de esa publicación.
3. Duplicar el **ad set** (la campaña ya está preconfigurada y segmentada).
4. Quitar el botón "Enviar mensaje por Messenger".
5. Renombrar el ad set con la fecha.
6. Verificar que el presupuesto sea **lifetime budget** y el monto correcto.
7. Ajustar `start date` y `end date`.
8. En el ad → `Change post` → seleccionar la publicación.
9. Publicar.

### El cuello de botella real

> *"Meta ya no me deja programar el anuncio. Nomás puedo programar la publicación… entonces hasta el día siguiente es cuando empiezo a hacer el anuncio, y todo lo que tarda Meta en recibirlo y aprobarlo me quita tiempo. Si esas ofertas duran dos días y Meta tarda medio día en aprobar, ya perdí medio día."* — transcripción 21:46–22:30

Y la observación empírica que ya tiene medida:

> *"Si publico a las 9:00 de la noche y dejo que corra toda la noche para que Facebook me lo apruebe, mi resultado va mejor, me cuesta menos la interacción."* — transcripción 27:39–27:54

**Traducción a números:** una oferta de 48 h que pierde 12 h de aprobación pierde el **25 % de su vigencia**. Sobre ~60 campañas al mes, eso es presupuesto que se gasta fuera de la ventana en que la oferta existe.

## 6. Los siete dolores, priorizados por impacto

| # | Dolor | Impacto | Frecuencia |
|---|---|---|---|
| 1 | **Producción manual del flyer** (leer, categorizar, traducir, buscar imagen, maquetar) | Alto — es el consumo principal de horas del equipo | 60×/mes |
| 2 | **Retraso del anuncio en Meta** (no se puede programar) | Alto — pierde ~25 % de la vigencia y encarece el costo por interacción | 60×/mes |
| 3 | **Insumos no estandarizados** (3 formatos, ningún esquema) | Alto — multiplica el trabajo de lectura y es fuente de errores | 60×/mes |
| 4 | **Ciclos de feedback por errores** (ortografía, precio, imagen) | Medio — alarga producción y desgasta la relación con el manager | Frecuente |
| 5 | **Riesgo legal por imágenes de terceros** | **Crítico si se materializa** — hay demanda activa contra otra agencia | Latente |
| 6 | **Sin reportes de desempeño** | Medio — no puede justificar la inversión ante el cliente | Mensual |
| 7 | **Trazabilidad fragmentada** (Slack + Asana + email) | Medio — duplicación y poca visibilidad del estado real | Diario |

## 7. Restricciones que acota la solución

Estas condiciones no son negociables y **eliminan varias arquitecturas de entrada**:

1. **El resultado debe quedar editable dentro de Canva.** No sirve un PNG o un PDF plano: el equipo debe poder mover el melón donde estaba el cilantro y cambiar la imagen de fondo cuando hay *holiday*.
2. **Canva Pro / Premium, no Enterprise.** Confirmado en transcripción 44:56–45:03. Esto tiene consecuencias técnicas mayores (ver documento 03).
3. **Los clientes siguen usando email.** *"El cliente no quiero que tenga acceso a mi plataforma."* (41:44–42:30). Rechaza explícitamente un portal para managers, con buen argumento: los managers rotan y ya costó mucho conseguir que envíen por correo con plantilla.
4. **Presupuesto acotado.** La única propuesta cotizada que recibió pedía **USD 5,000 + 500/mes**; una segunda propuesta nunca llegó a cotizar. El razonamiento para rechazarla es sólido: *"una vez que tenga armado esto, mis ofertas son siempre lo mismo, no necesito tantos cambios"*. (Ver [documento 05](./05-revision-de-propuestas.md) — el resumen automático de la reunión registró por error una segunda cifra de USD 15,000 que en realidad fue una broma del consultor al cerrar.)
5. **Mantenimiento mínimo.** Debe operar sin un equipo técnico permanente.
6. **Quiere aprender a construirlo.** No busca solo comprar un entregable; busca capacitación (transcripción 12:59–13:18).

## 8. Lo que la solución debe lograr (criterios de éxito)

| Criterio | Métrica | Objetivo |
|---|---|---|
| Reducir tiempo de producción | Minutos por flyer | De ~45–60 min a **< 10 min** de intervención humana |
| Recuperar vigencia del anuncio | Horas de anuncio activo por campaña de 48 h | **+10 a 12 h** (anuncio enviado a revisión la misma noche) |
| Eliminar errores de texto | % de flyers aprobados sin feedback ortográfico | **> 95 %** |
| Cobertura de imágenes | % de productos con imagen asignada automáticamente | **> 85 %** al mes 3 |
| Costo operativo | USD/mes de infraestructura + IA | **< USD 100/mes** |
| Trazabilidad | Estados visibles sin copiar/pegar en Slack | Tablero único |
| Riesgo legal | % de biblioteca con licencia documentada | **100 %** al mes 6 |

---

**Siguiente:** [02 · Benchmark: cómo usan IA las agencias hoy](./02-benchmark-ia-en-marketing.md)
