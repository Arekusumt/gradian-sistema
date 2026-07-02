# Gradian — el sistema, por dentro

**Una persona dirigiendo una oficina de 27 agentes de IA** que investiga negocios locales y fabrica su
material de crecimiento — auditorías con la oportunidad en €, webs de demostración con la marca real
del negocio, identidad, contenido para redes — con una regla no negociable: **nada se envía ni se
publica sin aprobación humana**, garantizado por bloqueos técnicos.

### 🗺️ [Ver el mapa interactivo →](https://arekusumt.github.io/gradian-sistema/)

Zoom, búsqueda, filtros por tipo, ficha de cada nodo y recorridos animados (el viaje de un lead y el de
un reel). También está el **[informe completo en PDF (12 páginas)](./gradian-como-funciona.pdf)**.

![Mapa del sistema Gradian: 86 nodos y 107 conexiones](img/mapa-nodos.png)

## El sistema en números — 18 días medidos, no estimados

| | |
|---|---|
| Agentes de IA especialistas | **27**, en 3 departamentos (captación · social/reels · inmobiliario) |
| Skills (procedimientos versionados) | **23** |
| Comandos de operación + bloqueos de seguridad | **9** + **4** hooks (`guard_send`, `guard_publish`…) |
| Sesiones de trabajo · llamadas al modelo | **74** · **8.312** |
| Delegaciones entre agentes · usos de herramientas | **232** · **10.542** |
| Tokens procesados | **2.483 M** (96,9 % servidos desde caché) |
| Clientes reales entregados · prospectos preparados | **2** · **80** |
| Coste marginal por pieza producida | **≈ 0 €** (render local: Chrome, ffmpeg, Piper, Whisper) |

## Cómo está construido

- **Runtime:** [Claude Code](https://claude.com/claude-code) (Anthropic) con los modelos Claude como cerebro.
- **Roles estrechos:** el que produce nunca aprueba. Críticos automáticos puntúan cada pieza contra
  rúbricas (umbral 80/100) antes de que llegue a la cola de aprobación humana.
- **Guardarraíles técnicos:** interceptores que bloquean físicamente cualquier envío de email/WhatsApp
  o publicación en redes desde el sistema. La última palabra siempre es de una persona (RGPD desde el diseño).
- **Auto-mejora:** retrospectivas con datos reales + un agente de I+D que audita y corrige el propio
  sistema, con 92 tests de integridad en CI que verifican que nada se rompe.
- **Todo el mapa es un solo fichero HTML** ([`index.html`](./index.html)): SVG generado por datos —
  añadir un nodo nuevo es una línea.

## Quién

Diseñado y dirigido por **Alex** — definió el negocio, los procesos, los roles de cada agente y los
controles de calidad, y usó la propia IA para materializarlos, revisando cada iteración. El repositorio
de producción (privado) contiene los agentes, las skills, los hooks y la CI; este repositorio es el
mapa público del sistema.

[gradiangrowth.com](https://gradiangrowth.com) · alex@gradiangrowth.com

---

### English TL;DR

An interactive map of a **real agentic system**: one person orchestrating a 27-agent AI office
(built on Claude Code) that produces sales-ready growth material for local businesses — audits,
brand-accurate demo websites, identity, social content. Human-in-the-loop by design: technical
guardrails physically block any autonomous sending or publishing. Built and shipped in 18 days;
every number above is measured from the system's own session logs.
