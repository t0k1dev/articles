---
title: "La IA está redefiniendo lo que significa ser desarrollador"
slug: la-ia-esta-redefiniendo-lo-que-significa-ser-desarrollador
date: 2026-03-15
author: t0k1dev
tags: [ai, devtools, opinion, career, architecture]
status: draft
description: >
  El título del puesto sigue siendo el mismo, pero el trabajo
  real cambió. Una mirada honesta a lo que eso significa para
  los desarrolladores que aún no lo han asimilado del todo.
---

Hace unos meses publiqué una funcionalidad en una sola tarde
que le habría costado a mi equipo casi todo un sprint. No era
algo trivial: un pipeline completo de exportación de datos con
filtrado, paginación, procesamiento en segundo plano y pruebas.
Cerré la laptop y sentí inquietud en lugar de orgullo. Algo
había cambiado y todavía no había decidido qué pensar al
respecto.

Ya tuve tiempo para pensarlo. El cambio no está por llegar.
Ya ocurrió. La pregunta no es si la IA cambió cómo trabajamos
— lo hizo. La pregunta es si te diste cuenta, y si estás
trabajando con esa realidad o alrededor de ella. El título
del puesto es el mismo. El trabajo no lo es.

## El mayor cambio desde el código abierto

Recuerdo cuando npm me hacía sentir que estaba haciendo
trampa. Podías incorporar una librería que a alguien le tomó
años construir, agregarla a un proyecto en treinta segundos y
publicarla como si la hubieras escrito tú. Se sentía casi
injusto. Esa sensación desapareció rápido, porque todos tenían
el mismo acceso y la industria se adaptó a su alrededor.

Así es como funcionó cada cambio real en el software. El
código abierto no volvió obsoletos a los desarrolladores —
volvió obsoleto el control de acceso. Antes, las librerías de
calidad para producción requerían presupuesto, aprobación
legal, o equipos internos reconstruyendo las mismas cosas
desde cero. Después, ese costo se fue a casi cero. La
capacidad siempre estuvo ahí. El acceso era el problema.

La nube hizo lo mismo con la infraestructura. El rol de
administrador de sistemas no desapareció — se reorientó.
Surgieron DevOps y SRE. Quienes se adaptaron subieron en la
cadena de valor. Quienes no lo hicieron pasaron años
defendiendo un trabajo que se estaba automatizando por debajo
de ellos.

Las herramientas de IA están haciendo esto con la velocidad
de implementación. El trabajo de escribir código repetitivo,
conectar patrones conocidos, crear estructura base de
servicios — una parte significativa ya puede generarse. Los
desarrolladores que escribían esas funcionalidades tampoco
desaparecieron. Pero lo que hacen en un día se ve diferente,
y pretender lo contrario es cometer el mismo error que el
administrador de sistemas.

Eso no es una mejora incremental. Es un juego distinto.

## Escribir código ya no es el cuello de botella

Esta es la versión incómoda de lo que noté esa tarde: el
pipeline no era difícil. La parte difícil sucedió en las dos
horas antes de abrir el editor — decidir cómo debía funcionar,
cuáles eran los modos de falla, qué forma debían tener los
datos más adelante. La implementación tomó unas horas con IA.
El pensamiento tomó exactamente lo mismo de siempre.

GitHub realizó un estudio controlado sobre esto en 2022.
Los desarrolladores con Copilot completaron la misma tarea
un 55% más rápido que quienes trabajaron sin él. Ese número
es llamativo, pero resulta ligeramente engañoso si te quedas
ahí. Las investigaciones que rastrean el comportamiento real
de los desarrolladores han encontrado de forma consistente
que escribir código ya es una minoría de la jornada laboral
— la mayor parte del tiempo se va en entender código, en
comunicación y en tomar decisiones. Si eso ya era verdad, lo
que la IA aceleró fue una fracción del trabajo total.

El cuello de botella no desapareció. Se desplazó hacia arriba.
Ahora vive en las decisiones que tomas antes de la primera
línea, y en tu capacidad de evaluar lo que sale al final.

La IA no aceleró el trabajo. Hizo visible cuál parte del
trabajo siempre fue la difícil.

## Lo que un desarrollador senior hace ahora

He estado prestando atención a lo que hago realmente en un
día, y se ve diferente de lo que habría descrito hace dos
años.

Escribo menos código directamente. Paso más tiempo escribiendo
descripciones de lo que quiero — precisas, acotadas, lo
suficientemente explícitas para que el resultado sea útil y
no solo plausible. Paso más tiempo leyendo código que no
escribí y preguntándome si es correcto, no solo si funciona.
Paso más tiempo al inicio del proceso, pensando en las
restricciones antes de que exista algo que revisar.

El juicio se desplazó en dos direcciones a la vez. Hacia
arriba: ¿qué deberíamos construir y cómo debería encajar?
Hacia abajo: ¿este resultado es correcto y es seguro
publicarlo? La capa de implementación en el medio — que
solía ser donde ocurría la mayor parte del trabajo visible
— es más delgada ahora.

El cambio al que sigo volviendo es este: el trabajo solía
ser principalmente "¿cómo escribo esto?". Ahora es
principalmente "¿es esto lo correcto que hay que escribir,
y la IA lo hizo bien?".

Eso es un cambio más grande de lo que parece. "¿Cómo escribo
esto?" tiene una respuesta correcta que puedes buscar. "¿La
IA lo hizo bien?" requiere que tengas todo el sistema en la
cabeza, que conozcas los casos extremos, que entiendas lo
que se supone que debe hacer el código con la suficiente
profundidad para detectar algo que parece correcto pero no
lo es. Eso no se googlea. Requiere el tipo de juicio que
solo viene de haber construido suficientes cosas como para
ver los modos de falla antes de que ocurran.

Cuando trato de comprimirlo en una lista, se ve así:

1. Escribir descripciones precisas del problema antes de
   generar cualquier cosa
2. Evaluar el resultado por corrección, casos extremos y
   seguridad — no solo si funciona
3. Establecer restricciones de arquitectura que mantengan
   el resultado de la IA coherente en todo el código base
4. Saber cuándo anular, reescribir o descartar lo generado,
   y poder explicar por qué
5. Documentar las decisiones para que el siguiente contexto
   — humano o IA — entienda por qué las cosas son como son

El resultado más valioso de un desarrollador senior ya no es
un pull request. Es una decisión.

## Un solo desarrollador puede ahora superar a un equipo

El año pasado, un desarrollador que conozco publicó un
producto SaaS solo en unas seis semanas. Autenticación,
facturación, un panel de control, un sistema de trabajos en
segundo plano, una API. Le pregunté cómo. Dijo que la
respuesta honesta era que dejó de escribir lo que la IA
podía escribir y dedicó ese tiempo a lo que ella no podía.
Dos años antes, ese proyecto habría necesitado un equipo y
financiamiento. Él lo hizo en noches y fines de semana.

No era excepcional. Simplemente tenía contexto completo —
sin costo de transferencia, sin reuniones de alineación, sin
esperar el PR de alguien más. Los datos de GitHub de 2023
muestran que el 46% del código en archivos con Copilot
habilitado es completado por IA, y el 92% de los
desarrolladores ya usa estas herramientas en el trabajo o
de forma personal. El desarrollador individual que supera a
un equipo ya no es una historia sobre un prodigio. Es cada
vez más solo lo que parece el trabajo enfocado ahora.

Pero hay límites reales y no quiero ignorarlos.

Los proyectos con código heredado son más difíciles. La IA
se degrada sin contexto, y los sistemas antiguos están llenos
de supuestos no documentados incorporados en las formas de
los datos, convenciones implícitas que nadie recuerda del
todo, y décadas de decisiones que tenían sentido en su
momento. Cuanto más viejo y desordenado el código, menos
útil se vuelve la IA. Lo he visto personalmente — coloca
una IA en un monolito Rails de diez años y el resultado es
inmediatamente peor que en un proyecto nuevo.

Los requisitos ambiguos son peores, no mejores. La IA no
resuelve la ambigüedad — la rellena con suposiciones que
suenan seguras. Entra basura, sale basura, pero más rápido
y en mayor volumen.

La seguridad es el límite más agudo. Un estudio de 2022 de
investigadores de NYU encontró que un porcentaje significativo
de las sugerencias de Copilot contenía vulnerabilidades de
seguridad. La herramienta no sabe que está equivocada. Tú
tienes que detectarlo. Eso requiere un juicio que la
herramienta no puede proveer.

## Tu código base es una conversación con un agente de IA

Piensa en lo que hace un nuevo empleado el primer día. Abre
el repositorio, lee lo que hay, mira cómo está estructurado
el código existente e intenta inferir las reglas a partir
de la evidencia. Lo que no puede descifrar lo pregunta.
Al principio es lento porque le falta el contexto, y mejora
a medida que lo va acumulando.

Un agente de IA hace lo mismo, excepto que no puede hacer
preguntas. Lee la estructura de tus archivos, tus nombres,
tu código existente, tu documentación — o la ausencia de
todo eso — e infiere qué hacer a partir de lo que encuentra.
No hay seguimiento. No hay "oye, ¿cuál es la convención
para el manejo de errores aquí?". Trabaja con lo que tiene.

Esto significa que un proyecto desordenado produce resultados
de IA desordenados. No porque el modelo sea malo. Porque la
entrada es mala. Los nombres vagos generan código vago. La
falta de convenciones genera código inconsistente. La ausencia
de documentación sobre la intención arquitectónica genera
código que es localmente plausible y globalmente incorrecto.

Archivos como AGENTS.md y las reglas de Cursor existen porque
la gente lo aprendió por las malas. Describen las convenciones,
las restricciones, los patrones — y sirven a dos audiencias
simultáneamente: el humano que los lee para entender el código
base, y la IA que los usa como contexto para la generación.
Escribir para ambos a la vez es una habilidad nueva. La
mayoría de los proyectos no fueron diseñados para eso.

Lo más práctico que puedes hacer hoy: abre uno de tus
proyectos y pregúntate si una IA que llegara a él sin contexto
previo entendería las reglas. Si la respuesta es no, el nuevo
desarrollador que se incorporó el mes pasado probablemente
tampoco las entiende.

Tu código base es documentación. Siempre lo fue. Ahora también
son instrucciones.

## La parte incómoda

Quiero decir algo en lo que he estado pensando durante un
tiempo y que resulta incómodo decir directamente.

Pasé años orgulloso de código que, en retrospectiva, era
principalmente ensamblaje de partes conocidas. Flujos de
autenticación. Endpoints CRUD. Pipelines de transformación
de datos. Trabajos en segundo plano. Lógica de paginación.
Escribí todo eso cuidadosamente, y la mayor parte seguía
patrones que ya había escrito antes, o visto en otros
proyectos, o que podría haber descrito en un párrafo. No era
sin creatividad — pero tampoco era donde ocurría el pensamiento
real.

Una encuesta de Stripe a más de 11,000 desarrolladores
encontró que aproximadamente el 70% del tiempo de desarrollo
se va en mantenimiento, depuración y deuda técnica, en lugar
de construir cosas nuevas. Eso no es pereza ni mal proceso.
Es simplemente lo que es el trabajo la mayor parte del tiempo.

En una encuesta de 2022, el 87% de los desarrolladores dijo
que las herramientas de IA les ayudan a preservar el esfuerzo
mental en tareas repetitivas. No a eliminar las tareas —
a preservar el esfuerzo mental en ellas. Los propios
desarrolladores ya saben qué partes del trabajo son mecánicas.
La IA solo hizo que la proporción sea imposible de ignorar.

La implicación incómoda es que una parte significativa de lo
que la industria pagó — y sobre lo que muchos desarrolladores
construyeron sus carreras — fue la capa de implementación.
El costo de hacer negocios. Esa capa se está abaratando.

La mayor parte del código que la mayoría de nosotros escribimos
era ensamblaje de partes conocidas. La IA solo hace eso más
rápido ahora.

La parte que siempre importó — las decisiones, el diseño, la
depuración de cosas que no deberían ser posibles — no cambió.
Simplemente hay más espacio para ello ahora, lo que es
emocionante o amenazador dependiendo de qué tan honesto estés
dispuesto a ser sobre dónde pasaste la mayor parte de tu tiempo.

## Las habilidades que realmente importan ahora

Las habilidades que te hicieron bueno en este trabajo no van
a desaparecer. Simplemente son finalmente todo el trabajo.

El diseño de sistemas siempre fue la habilidad senior, pero
solía compartir el escenario con la implementación. Si podías
escribir código complejo rápido, eras valioso aunque tus
instintos arquitectónicos fueran ordinarios. Ese intercambio
desapareció. La IA puede completar las piezas. No puede decidir
dónde van. Saber cómo debe encajar un sistema — dónde están
los límites, cuáles serán los modos de falla, qué
compromisos son realmente compromisos — es ahora lo
principal.

La revisión de código también cambió. Siempre fue importante.
Ahora es el núcleo del trabajo. Evaluar resultados que no
escribiste, entenderlos lo suficientemente bien como para
detectar lo que está sutilmente mal en lugar de solo lo que
falla en una prueba — esa es la actividad principal. La
mayoría de los desarrolladores practicaba esto como una tarea
secundaria. Ya no es una tarea secundaria.

Luego está algo sobre lo que no veo suficiente conversación:
la articulación del problema. Describir lo que quieres con
suficiente precisión para que la IA produzca algo útil es
genuinamente difícil y requiere práctica. Se parece más a
escribir una especificación que a escribir código. Los
desarrolladores que lo tratan como algo obvio están
produciendo resultados mediocres y culpando al modelo.

Lo que está perdiendo valor es la contraparte de todo esto:
memorizar sintaxis, escribir código repetitivo desde cero,
configurar herramientas estándar a mano. Estas cosas llenaban
muchas horas. Nunca fueron el punto. El punto siempre fue el
juicio debajo de ellas, y ahora no queda nada que lo oculte.

## Por dónde seguir

El cambio ya ocurrió. Probablemente ya estás haciendo partes
de esto sin nombrarlo. La pregunta es si lo haces
deliberadamente.

Lo más útil que hice fue elegir un proyecto real — no un
tutorial, algo con apuestas reales — y usar IA en él todos
los días durante un mes. No para generar todo. Para descubrir
dónde ayudaba y dónde empeoraba las cosas, y para desarrollar
un sentido de cuándo confiar en el resultado y cuándo
descartarlo. Esa calibración no viene de leer sobre IA. Viene
de equivocarse suficientes veces en cosas que importaban.

Lo segundo es más difícil de practicar pero más importante:
sé preciso sobre lo que quieres antes de pedirlo. La mayoría
del mal resultado de la IA no es culpa del modelo. Es culpa
del prompt — vago, poco especificado, sin las restricciones
que habrían hecho útil el resultado. Trata escribir una
descripción del problema como escribir una especificación. La
disciplina es la misma.

Más allá de eso: lee más código del que escribes. Revisa
deliberadamente — resultados de IA, código abierto, tu propio
código antiguo. La habilidad de leer críticamente es lo que
separa a los desarrolladores que usan la IA bien de los que
simplemente confían en ella. Estudia el diseño de sistemas
explícitamente. El juicio arquitectónico que antes era
opcional para los desarrolladores de nivel medio ahora es
lo fundamental. Y documenta tus proyectos como si una IA
fuera a trabajar en ellos — porque lo hará.

Nada de esto requiere una herramienta en particular. Las
herramientas cambian cada seis meses. El cambio de fondo no.

El desarrollador que publicó en una tarde no tuvo suerte.
Solo notó lo que cambió.

---

## Referencias

Kalliamvakou, E. (2022). "Research: quantifying GitHub
Copilot's impact on developer productivity and happiness."
GitHub Blog, septiembre de 2022.
https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/

Meyer, A. N., Barton, L. E., Murphy, G. C., Zimmermann,
T., & Fritz, T. (2017). "The Work Life of Developers:
Activities, Switches and Perceived Productivity." IEEE
Transactions on Software Engineering, 43(12), 1178–1193.
https://doi.org/10.1109/TSE.2017.2656886

Kapser, C. J., & Godfrey, M. W. (2008). "'Cloning
Considered Harmful' Considered Harmful: Patterns of
Cloning in Software." Empirical Software Engineering,
13(6), 645–692.
https://doi.org/10.1007/s10664-007-9064-x

Stripe. (2018). "The Developer Coefficient." Harris Poll
survey of ~11,000 developers and business leaders.
(La página original ya no está disponible; los hallazgos
fueron ampliamente reportados en la prensa tecnológica
de 2018.)

GitHub. (2023). "Octoverse: The state of open source and
AI." GitHub, Inc.
https://github.blog/news-insights/octoverse/octoverse-2023/

Pearce, H., Ahmad, B., Tan, B., Dolan-Gavitt, B., & Karri,
R. (2022). "Asleep at the Keyboard? Assessing the Security
of GitHub Copilot's Code Contributions." IEEE Symposium on
Security and Privacy (S&P 2022).
https://doi.org/10.1109/SP46214.2022.9833571
