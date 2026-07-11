# I+D sobre sí mismo: agent teams, enjambres y render de vídeo HTML

*2026-07-10 — cómo este sistema investiga una capacidad nueva antes de adoptarla.*

Antes de tocar una sola pieza de producción, el sistema corre una sesión de I+D con reglas
duras: **cada afirmación con fuente y fecha, experimentos en vivo como evidencia, y cero
adopción sin un piloto medible**. Esta es la radiografía de la última sesión, tal cual
ocurrió — incluida la parte donde se quedó sin gasolina.

## El método

Tres agentes de investigación **en paralelo** (uno por tema), mientras la sesión principal
hacía la línea base interna y los experimentos locales. Todo lo afirmado abajo tiene fuente
verificada el 2026-07-10; lo que no se pudo re-verificar quedó marcado como tal en el
dossier interno — y no se publica aquí.

## Tema 1 — Agent Teams (Claude Code): sí existe, y se midió en vivo

Los [*agent teams*](https://code.claude.com/docs/en/agent-teams) son sesiones
independientes de Claude Code («teammates») con contexto propio persistente, mensajería
directa entre ellas y una lista de tareas compartida con bloqueo de ficheros. Estado real:
**experimental** (flag de entorno, v2.1.178+), coste documentado de **~7× tokens**, y en
Windows solo modo in-process.

El experimento en vivo que lo demuestra mejor cabe en tres líneas: se lanzó un agente con
la instrucción de inventarse un nombre en clave y **no revelarlo**; minutos después, un
mensaje de seguimiento — sin repetirle nada — le pidió el nombre. Respondió «Xuixo» y
recordó hasta la multiplicación que se le había pedido (17×23=391). Contexto persistente
verificado de primera mano, no leído en una doc.

**Veredicto del sistema**: todavía no. 7× de coste contra una suscripción plana convierte
la cuota en el recurso escaso — de hecho, a media sesión el límite de uso tumbó a dos de
los tres agentes de investigación, y la sesión principal terminó el trabajo sola con
llamadas mínimas. Esa es exactamente la disciplina que un equipo de agentes necesita:
saber cuándo NO usar la herramienta cara.

## Tema 2 — «Agent swarms»: el nombre es marketing, el patrón es real

Nadie en Anthropic llama «swarm» a nada: el patrón canónico es **orquestador-trabajadores**,
documentado en [su blog de ingeniería](https://www.anthropic.com/engineering/multi-agent-research-system)
sobre el sistema multi-agente de investigación. Las cifras que importan, citadas de ahí: un sistema multi-agente consume **~15× los tokens** de
un chat; a cambio, un orquestador Opus con trabajadores Sonnet superó a un Opus solo en un
**+90,2 %** en su eval interna de investigación — y el uso de tokens explica por sí solo el
**80 %** de la varianza de rendimiento. Traducción: los enjambres pagan en tareas valiosas
y paralelizables en amplitud (investigación, auditorías en lote), y son un despilfarro en
trabajo secuencial con dependencias densas.

Lo que este sistema ya tiene de serie: la herramienta nativa de *workflows* de Claude Code
(fan-out programático con verificación adversaria, paneles de jueces y loop-until-dry).
El siguiente piloto interno: el lote matinal de auditorías como pipeline paralelo donde
**dos agentes escépticos intentan refutar cada cifra de oportunidad en €** antes de que
llegue a la cola de aprobación humana.

## Tema 3 — HyperFrames: cuando el open source te valida el stack

[HyperFrames](https://github.com/heygen-com/hyperframes) (HeyGen, Apache 2.0, 34k estrellas)
es «Write HTML. Render video. Built for agents»: composiciones HTML+CSS que un renderer
convierte en MP4 determinista con Chrome headless + FFmpeg — **exactamente el stack que la
fábrica de contenido de este sistema ya usaba a mano** para sus reels de marca a coste
marginal 0 €. La diferencia: animación frame-accurate (GSAP/Lottie/Three.js), pistas de
audio, lint/preview/render y **20 skills nativas para agentes de código**, incluido un
workflow *faceless* que encaja con las restricciones de contenido de la casa. Sin coste
por render, sin dependencia de APIs de pago.

**Veredicto**: piloto inmediato. Un mismo reel real, producido por el motor actual y por
HyperFrames, juzgado **a ciegas por el crítico visual del sistema con la misma rúbrica
(umbral 80/100)**. Solo si gana con números entra en producción — y el camino actual queda
de fallback. Así se adopta tecnología aquí: el productor nunca se aprueba a sí mismo.

## Los resultados, medidos — actualización 2026-07-11

Los tres pilotos se ejecutaron al día siguiente. Números reales, no promesas:

**HyperFrames — ADOPTADO tras ganar el A/B ciego.** El mismo reel real (un pub de la Costa
Daurada, guion y voz idénticos) producido por los dos motores y puntuado a ciegas por el crítico
visual del sistema con su rúbrica de siempre: **92/100 el piloto HyperFrames · 31/100 el motor
anterior**. La clave del 92: una escena de *metraje real* — la web viva del cliente scrolleando
dentro de un móvil — que el crítico verificó con `curl` contra el dominio en producción antes de
puntuarla. El A/B ciego además destapó un bug real del camino antiguo (una plantilla degradaba en
silencio a la marca equivocada): **el juez que no sabe qué está juzgando encuentra lo que nadie
buscaba**. El motor anterior queda de *fallback*; nada se borra.

**El lote de auditorías con verificación adversaria — funciona, y muerde.** Primera tanda real:
3 negocios locales auditados en paralelo (~30 minutos de reloj, 16 agentes, 5 etapas solapadas).
Los dos escépticos independientes **refutaron y obligaron a recalcular 2 de las 3 cifras de
oportunidad** (una bajó de 4.140 a 2.898 €/año; otra de 3.105 a 1.725). La cifra que llega a la
bandeja de aprobación humana ya no es la del agente entusiasta: es la que sobrevivió a dos
intentos serios de tumbarla. Bonus operativo: un agente se colgó a mitad de tanda y el lote se
reanudó desde caché re-ejecutando solo lo pendiente — y cuando el límite de uso cortó la sesión,
todo lo verificado era recuperable del registro. Los fallos también estaban en el diseño.

**Agent teams — medido, y adoptado solo a medias, a propósito.** Una tanda real con 3
especialistas en paralelo sobre 3 encargos independientes: **2,5× más rápido en reloj**
(4 m 46 s contra ~11 m 47 s en serie), ~69k tokens por agente, 2 de 2 materiales de venta
pasando la puerta de conversión. Veredicto: el patrón de equipo ligero se adopta para tandas de
3-5 encargos independientes; la beta experimental (7× de coste) sigue fuera hasta que aporte algo
que este patrón no dé. Saber cuándo no, otra vez.

## Lo que esto enseña del sistema

- **Investiga antes de adoptar**, con fuentes fechadas y experimentos reproducibles.
- **Mide el coste en tokens como un CFO**: la cifra de 7× o 15× decide más que el hype.
- **Toda adopción pasa por un piloto A/B con juez automático** y umbral numérico.
- **La honestidad es parte del método**: lo no verificado se marca, lo que falló se cuenta.

A mediados de 2026 la pregunta interesante ya no es «¿sabes lanzar agentes?» — es
**«¿sabes cuándo no?»**.

## ¿Quieres verlo por dentro?

El sistema entero está dibujado en el **[mapa interactivo](https://arekusumt.github.io/gradian-sistema/)**
(91 nodos, 115 conexiones), y el resto del ecosistema se explica solo: el
[caso real en producción](https://github.com/Arekusumt/gradian-caso-waterfront), el
[patrón transferido a otro dominio](https://github.com/Arekusumt/gradian-match) y el
[producto vivo](https://github.com/Arekusumt/gueridon). ¿Preguntas sobre cómo se dirige
esto — o esta forma de trabajar dentro de tu equipo?
**[alex@gradiangrowth.com](mailto:alex@gradiangrowth.com)** · respuesta en 24 h.

---

### English TL;DR

How this 28-agent system runs R&D on itself before adopting anything. Three parallel
research agents investigated Claude Code **agent teams** (experimental, ~7× token cost —
verified live with a context-persistence experiment: a teammate remembered its secret
codename across a resume), **multi-agent orchestration** (Anthropic's orchestrator-workers
pattern: ~15× tokens vs a single chat, +90.2% over single-agent on their internal research
eval — swarms pay for breadth-parallel work, not sequential coding), and **HyperFrames**
(HeyGen's Apache-2.0 HTML→deterministic-MP4 renderer, built for agents — which validates
the exact headless-Chrome+FFmpeg stack this factory already used for its 0-cost brand
reels). Verdicts: teams — not yet (cost discipline: the session usage limit killed two
research agents mid-flight, and the main session finished the job with minimal calls);
workflow-based audit batches with adversarial verification — pilot next; HyperFrames — 
immediate pilot, judged blind by the system's own visual critic at the usual 80/100 gate.
Mid-2026, the interesting question isn't "can you spawn agents?" — it's "do you know when
not to?"

**Results (next-day update, 2026-07-11):** HyperFrames **won the blind A/B 92/100 vs 31/100**
(the winning scene: the client's LIVE website scrolling inside a phone frame — real footage the
critic verified against the production domain) and is now the optional production path, old
engine as fallback. The audit-batch workflow ran its first real batch: 3 local businesses in
~30 wall-clock minutes, with the two adversarial skeptics **refuting and forcing a conservative
recalculation of 2 out of 3 €-opportunity figures**. The lightweight team pattern measured 2.5×
faster wall-clock on 3 independent jobs (~69k tokens/agent) and was adopted for 3-5-job batches;
the experimental 7×-cost beta stays out.
