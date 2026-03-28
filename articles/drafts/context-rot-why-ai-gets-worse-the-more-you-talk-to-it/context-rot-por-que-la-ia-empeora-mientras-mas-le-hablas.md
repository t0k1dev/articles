---
title: "Context rot: por que la IA empeora mientras mas le hablas"
slug: context-rot-por-que-la-ia-empeora-mientras-mas-le-hablas
date: 2026-03-28
author: t0k1dev
tags: [ai, architecture, opinion, devtools]
status: draft
description: >
  Mientras mas larga la conversacion con una IA, peor
  responde. Una investigacion sobre el context rot y lo que
  cambio en mi forma de trabajar.
---

El mes pasado estuve en una sesion larga de refactoring con
un asistente de IA. Los primeros veinte minutos fueron
excelentes. El modelo entendia mis restricciones, seguia los
patrones que le puse, escribia codigo limpio que encajaba con
el proyecto. Iba rapido.

Despues algo cambio. Alrededor del turno 20, introdujo un bug
en una funcion que el mismo habia escrito quince mensajes
antes. Lo corregi. Unos turnos despues, ignoro una convencion
de nombres que yo habia establecido al principio de la sesion.
Para el turno 30 estaba recomendando una arquitectura que
contradecia decisiones que habiamos tomado juntos una hora
antes. Sin reconocerlo. Sin darse cuenta.

Yo no habia cambiado. El modelo empeoro.

No tuve nombre para esto hasta que encontre el estudio de
Chroma. Investigadores le pidieron a los modelos algo que un
nino podria hacer: copiar la palabra "apple" cinco mil veces.
Qwen respondio *"Voy a tomar un descanso. No estoy de humor.
Necesito relajarme."* Gemini empezo a escupir texto sin
sentido. GPT simplemente se nego. Estos son modelos de miles
de millones de dolares fallando en copiar y pegar.

La comunidad empezo a llamar a esto "context rot" a mediados
de 2025. El nombre pego porque la experiencia es universal. Si
usaste un asistente de IA por mas de quince minutos, lo
sentiste.

## Que es lo que se pudre

Lo pienso como un pizarron. Cada mensaje que envias se escribe
en el. La IA lee todo el pizarron cada vez que responde. El
context rot es lo que pasa cuando el pizarron se llena tanto
que el modelo todavia puede *ver* todo pero deja de *prestarle
atencion* a la mayor parte.

Esta distincion importa. No se trata de quedarse sin espacio.
La calidad de la atencion se adelgaza con cada mensaje que
agregas. La ventana no esta llena. El enfoque se fue.

*"Una ventana de 1M de tokens igual se pudre a los 50K. El
problema es el ruido, no la capacidad."*

Eso me pego cuando lo lei por primera vez. El cuello de
botella no es cuanto puede retener el modelo. Es cuanto puede
realmente usar.

## Como se ve la degradacion en el trabajo real

Esto ya lo viste. Tal vez no le pusiste nombre, pero lo viste.

**Se olvida de tus reglas.** Le dijiste "siempre usa
interfaces de TypeScript, nunca type aliases" hace veinte
mensajes. Acaba de escribir un type alias.

**Se contradice.** Eligio el enfoque A antes. Ahora esta
reescribiendo todo con el enfoque B. Sin explicacion. Sin
conciencia de que cambio de rumbo.

**Empieza a inventar cosas.** Mientras mas larga la sesion,
mas seguro esta de cosas incorrectas. Las respuestas tempranas
citan APIs reales. Las tardias las inventan.

**Pierde el hilo.** Le pediste refactorizar un modulo. Treinta
mensajes despues, esta resolviendo un problema completamente
distinto.

**El codigo se degrada en silencio.** La funcion que escribio
en el turno 5 era limpia. La del turno 25 contradice los
patrones que el mismo establecio antes. Misma sesion, mismo
modelo, calidad diferente.

**Las instrucciones dejan de pegar.** Esta es sutil. El
contexto acumulado puede literalmente *anular* tu system
prompt. El comportamiento del modelo se desplaza hacia lo que
domina en la conversacion, no hacia lo que le dijiste al
principio. La investigacion de Anthropic sobre aprendizaje en
contexto muestra que esto sigue una ley de potencias: mientras
mas contenido en el contexto, mas fuerte la anulacion.

Empece a tratarlo como una senal. Cuando el modelo se siente
"raro," no insisto. Reviso la longitud del contexto. Nueve de
cada diez veces, esa es la respuesta.

## Fui a buscar datos. Es peor de lo que pensaba.

Una vez que tuve el nombre, busque la investigacion. Lo que
encontre confirmo todo lo que venia sintiendo -- y despues lo
empeoro.

### El medio es zona muerta

Investigadores de Stanford encontraron un patron en forma de
U: la IA presta mas atencion a lo que esta al principio y al
final del contexto. Todo lo del medio cae hasta un 30%.

Eso me explico mucho. Mi system prompt esta al principio. Mi
ultimo mensaje esta al final. Pero las decisiones de
arquitectura, las restricciones que acordamos quince turnos
atras -- esas caen justo en la zona muerta. Es como leer un
hilo largo de email y solo recordar el primer mensaje y la
ultima respuesta.

### Los benchmarks son mentiras bonitas

Los tests estandar de "aguja en un pajar" -- escondes un dato
en un texto largo y le pides al modelo que lo encuentre -- se
ven geniales. Los modelos sacan arriba de 90%. Pero Adobe
hizo una version que requeria comprension real en lugar de
busqueda por palabras clave. Todo colapso:

- GPT-4o: de 99% bajo a 70%
- Claude 3.5: de 88% bajo a 30%
- Gemini 2.5 Flash: de 94% bajo a 48%
- Llama 4: de 82% bajo a 22%

Los tests que hacen quedar bien a la IA son los tests
equivocados. El trabajo real requiere razonamiento, no ctrl+F.
Y la mitad de los modelos que dicen soportar 32K+ de contexto
ni siquiera pudieron mantener el rendimiento a ese tamano en
tareas que no fueran triviales.

### Menos contexto le gana a mas contexto

Este hallazgo cambio como construyo cosas. Chroma probo 18
modelos grandes y el dato que mas me quedo: darle a un modelo
300 tokens de informacion relevante fue dramaticamente mejor
que darle 113,000 tokens de historial de conversacion
completo.

Menos no solo fue mejor. Fue *dramaticamente* mejor.

Otros hallazgos del mismo estudio: una sola oracion engañosa
en el contexto es suficiente para despistar a los modelos. El
texto bien organizado es mas dificil de navegar para los
modelos que el texto aleatorio -- todavia no puedo explicar
eso del todo. Y cuando no estan seguros, algunos modelos lo
admiten (Claude) mientras otros fabrican con confianza (GPT).
Ambos se degradan. Solo se degradan diferente.

Todo este cuerpo de investigacion es reciente. Stanford
publico "Lost in the Middle" en 2023. El benchmark RULER
demostro que los tamanos de contexto publicitados no se
sostienen en 2024. NoLiMa de Adobe destruyo la ilusion de los
benchmarks a principios de 2025. El termino "context rot" se
acuno en Hacker News en junio de 2025. El estudio de Chroma
con 18 modelos se viralizo un mes despues. Para septiembre,
Anthropic adopto el termino y propuso "context engineering"
como disciplina. El problema paso de frustracion sin nombre a
conocimiento generalizado en unos dos anos.

## Por que pasa (y por que ventanas mas grandes no lo arreglan)

Asi es como lo pienso. Cada vez que el modelo lee tu contexto,
decide cuanta atencion darle a cada parte. Esa atencion es un
presupuesto fijo. Agrega mas mensajes y a cada uno le toca una
porcion mas delgada. Cinco personas cenando, puedes seguir
todas las conversaciones. Cincuenta personas, capturas
fragmentos.

Ademas de eso, hay un sesgo estructural. Los primeros tokens
del contexto actuan como "sumideros de atencion" -- absorben
enfoque sin importar lo que realmente dicen. Los tokens
recientes se benefician de la recencia. Todo lo del medio se
queda sin recursos. No porque sea menos importante. Por donde
esta ubicado.

La mayoria de los modelos tambien fueron entrenados con textos
mas cortos que su ventana maxima. Lleva los al limite y estan
en territorio desconocido. Nunca vieron tanto contexto durante
el entrenamiento.

Nada de esto es un bug que alguien va a parchear. Asi funciona
la arquitectura.

Por eso las ventanas de contexto mas grandes no lo resuelven.
Cada pocos meses sale un modelo nuevo con un numero mas
grande: 128K, 1M, 10M tokens. La implicacion es mas memoria,
mejor IA. Me la crei por un tiempo.

Pero la atencion escala cuadraticamente. Duplica el contexto,
cuadruplica las relaciones que el modelo tiene que rastrear.
Algo tiene que ceder. Un pizarron mas grande no ayuda si solo
puedes enfocarte en una esquina a la vez.

Los numeros lo confirman. En Reddit, usuarios de modelos
locales reportan un contexto util de alrededor de 10K tokens
en modelos que anuncian 128K. Eso es menos del 8% de lo que
dice la caja.

Critica valida: hay quienes argumentan que "context rot" es
solo un nombre elegante para algo que ya entendiamos. Tal vez.
Pero "tu base de datos es lenta" y "no tienes un indice"
describen el mismo problema a diferentes niveles de utilidad.
El nombre ayuda a que la gente construya alrededor de el.

## Lo que cambie

Una vez que entendi el problema, cambie como trabajo. Algunas
cosas las ajuste de un dia para otro. Otras tomaron mas
tiempo.

### Cambios inmediatos

**Empiezo de cero mas seguido.** Cuando una sesion se desvía,
no peleo. Resumo donde estoy, abro un chat nuevo, pego el
resumen y sigo. Se siente como desperdicio. No lo es.

**Las instrucciones importantes van al principio.** Siempre.
Y las repito cada diez o quince mensajes. El modelo no se
molesta. Necesita el recordatorio.

**Un problema por sesion.** Antes le tiraba todo a un solo
hilo. Ahora lo mantengo enfocado. Trabajo de base de datos en
una sesion. Frontend en otra. La diferencia en calidad es
obvia.

**Le echo ojo al conteo de tokens.** No de manera obsesiva.
Pero noto cuando las cosas pasan la mitad. Ahi es cuando me
pongo mas cuidadoso con lo que pregunto y lo que mantengo en
el contexto.

### Cambios como builder

**Resumir, no acumular.** En las herramientas que construyo,
comprimo el historial de conversacion viejo en resumenes. El
historial crudo es un acelerador de context rot.

**Recuperar, no rellenar.** En vez de meter todo en el prompt,
traigo contexto relevante bajo demanda. El hallazgo de Chroma
lo demostro: 300 tokens enfocados le ganan a 113K tokens de
todo. Construye para eso.

**Las sub-tareas tienen su propio contexto.** Divido el
trabajo complejo en sub-tareas enfocadas, cada una con un
contexto limpio. Los resultados vuelven resumidos. El contexto
padre se mantiene delgado.

**Las notas viven fuera del chat.** La IA escribe hallazgos
importantes en un archivo. Cuando necesita esa informacion
despues, lee el archivo en vez de buscar entre cincuenta
mensajes de historial. Anthropic llama a este patron
"structured note-taking." Yo le digo la unica forma cuerda de
correr una sesion larga.

## El presupuesto de atencion

La IA no tiene memoria. Tiene un presupuesto de atencion.
Cada mensaje que envias cuesta un poco de ese presupuesto. El
context rot es lo que pasa cuando te sobrepagas.

Este problema no se va a arreglar pronto. Es arquitectural.
Pero se esta empezando a *gestionar*. Anthropic llama a la
disciplina "context engineering." Las mejores herramientas de
IA no van a ser las que tengan las ventanas mas grandes. Van a
ser las que manejen el contexto tan bien que nunca notes la
degradacion.

Ahora mismo, la mayoria de las herramientas simplemente dejan
que la ventana se llene y esperan lo mejor. Eso va a cambiar.
En un ano, la gestion de contexto va a ser requisito minimo.

Mientras tanto: la proxima vez que una sesion se sienta rara
-- el modelo olvidando cosas, contradiciendose, equivocandose
con seguridad -- revisa que tan larga es la conversacion. Esa
es probablemente tu respuesta.

Empieza de cero. Es la solucion mas barata en IA.

---

## Referencias

- Liu et al., "Lost in the Middle: How Language Models Use
  Long Contexts," Stanford/Meta, TACL 2023.
  [arXiv:2307.03172](https://arxiv.org/abs/2307.03172)
- Xiao et al., "Efficient Streaming Language Models with
  Attention Sinks," ICLR 2024.
  [arXiv:2309.17453](https://arxiv.org/abs/2309.17453)
- Anthropic, "Many-shot Jailbreaking," 2024.
  [anthropic.com](https://www.anthropic.com/research/many-shot-jailbreaking)
- Hsieh et al., "RULER: What's the Real Context Size of
  Your Long-Context Language Models?," COLM 2024.
  [arXiv:2404.06654](https://arxiv.org/abs/2404.06654)
- Adobe, "NoLiMa: Long-Context Evaluation Beyond Literal
  Matching," 2025.
- Hong, Troynikov, Huber, "Context Rot," Chroma, 2025.
  [research.trychroma.com](https://research.trychroma.com/context-rot)
- Anthropic, "Effective Context Engineering for AI Agents,"
  2025.
  [anthropic.com](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- Timothy B. Lee, "Context Rot: The Emerging Challenge,"
  Understanding AI, 2025.
