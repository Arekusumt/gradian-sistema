# Gradian — el sistema, por dentro

**Una persona dirigiendo una oficina de 28 agentes de IA** que investiga negocios locales y fabrica su
material de crecimiento — auditorías con la oportunidad en €, webs de demostración con la marca real
del negocio, identidad, contenido para redes (carruseles de marca, reels con voz IA, stories) y, desde
julio de 2026, **automatización de procesos** (línea Gradian Ops) — con una regla no negociable: **nada se envía ni se
publica sin aprobación humana**, garantizado por bloqueos técnicos.

### 🗺️ [Ver el mapa interactivo →](https://arekusumt.github.io/gradian-sistema/)

Zoom, búsqueda, filtros por tipo, ficha de cada nodo y recorridos animados (el viaje de un lead y el de
un reel). También está el **[informe completo en PDF (12 páginas)](./gradian-como-funciona.pdf)**.

![Mapa del sistema Gradian: 89 nodos y 111 conexiones](img/mapa-nodos.png)

Y así se ve **un lead recorriendo el sistema** — de negocio detectado a material enviado, pasando
por agentes, puertas de calidad y la aprobación humana (grabado del propio mapa):

![El recorrido de un lead por el sistema](img/flujo-lead.gif)

## El sistema en números — 18 días medidos, no estimados

| | |
|---|---|
| Agentes de IA especialistas | **28**, en 3 departamentos (captación · social/reels · inmobiliario) |
| Skills (procedimientos versionados) | **23** |
| Comandos de operación + bloqueos de seguridad | **9** + **4** hooks (`guard_send`, `guard_publish`…) |
| Sesiones de trabajo · llamadas al modelo | **74** · **8.312** |
| Delegaciones entre agentes · usos de herramientas | **232** · **10.542** |
| Tokens procesados | **2.483 M** (96,9 % servidos desde caché) |
| Clientes reales entregados · prospectos preparados | **2** · **80** |
| Coste marginal por pieza producida | **≈ 0 €** (render local: Chrome, ffmpeg, Piper, Whisper) |

Los dos clientes reales: **[The Waterfront](https://waterfrontirishpub.com)** (pub irlandés — su web nueva
ya está **en producción**; cómo se hizo, pieza a pieza:
**[gradian-caso-waterfront](https://github.com/Arekusumt/gradian-caso-waterfront)**) e **Inmocasa**
(inmobiliaria — entregado). El resto de piezas entregadas a ambos se implementa durante julio de 2026.

**Lo último (julio 2026): la línea Gradian Ops.** Automatización de los procesos manuales que sangran
tiempo en un negocio (facturas, mensajes, citas), empaquetada como recetas cerradas con mantenimiento
mensual. Primer vertical: **gestorías**, con la factura electrónica obligatoria en España (VeriFactu:
sociedades 1-1-2027, autónomos 1-7-2027) como ventana de entrada — y carril propio en el motor de
captación, con outreach por email que nace **en pausa** hasta el OK humano. Primera radiografía de
operaciones ya entregada a un cliente real con dos oficinas.

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

## El ecosistema: los tres repos se explican entre sí

| Repo | Qué muestra |
|---|---|
| **gradian-sistema** (estás aquí) | La fábrica: el mapa interactivo, los roles, los guardarraíles y los números medidos. *El sistema.* |
| **[gradian-caso-waterfront](https://github.com/Arekusumt/gradian-caso-waterfront)** | Una pieza terminada de esta fábrica: la web de un pub real, en producción, explicada desde las fotos de la carta hasta el deploy. *El resultado.* |
| **[gradian-match](https://github.com/Arekusumt/gradian-match)** | El mismo patrón (productor ↔ crítico con umbral numérico) aplicado a otro dominio: análisis CV ↔ oferta. *El patrón, transferido.* |

## Quién

Diseñado y dirigido por **Alex** — definió el negocio, los procesos, los roles de cada agente y los
controles de calidad, y usó la propia IA para materializarlos, revisando cada iteración. El repositorio
de producción (privado) contiene los agentes, las skills, los hooks y la CI; este repositorio es el
mapa público del sistema.

[gradiangrowth.com](https://gradiangrowth.com) · alex@gradiangrowth.com

---

### English TL;DR

An interactive map of a **real agentic system**: one person orchestrating a 28-agent AI office
(built on Claude Code) that produces sales-ready growth material for local businesses — audits,
brand-accurate demo websites, identity, social content (carousels, AI-voiced reels, stories), and a new
process-automation line (Gradian Ops: closed "recipes" automating invoices, messages and appointments;
first vertical: accounting firms, ahead of Spain's 2027 mandatory e-invoicing). One client
site is already [live in production](https://waterfrontirishpub.com); the remaining deliverables roll out
during July 2026. Human-in-the-loop by design: technical
guardrails physically block any autonomous sending or publishing. Built and shipped in 18 days;
every number above is measured from the system's own session logs.
