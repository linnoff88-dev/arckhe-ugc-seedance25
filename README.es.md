# INEMA · UGC Seedance 2.5

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

Anuncios UGC (creador hablando a la cámara del celular, one-take de 30 s, 9:16, 1080p) con Seedance 2.5, a
partir del pack "The Seedance 2.5 UGC Ads Pack" (Zubair Trabzada · AI Workshop, 2026), adaptado
al ecosistema INEMA (Magnific MCP como objetivo principal; Higgsfield y fal.ai como secundarios).

## 📖 Guía de uso

Guía completa (landing + paso a paso): **https://inematds.github.io/ugc-seedance25/guia/es/**
App en línea (modo Template): https://inematds.github.io/ugc-seedance25/app/public/

Dos entregables que comparten el mismo contenido:

```
ugc-seedance25/
├── index.html                redirige a la app (GitHub Pages)
├── PLANO.md                  diseño de la solución, mapeo para Magnific, riesgos
├── docs/                     pack original: PDF, transcripción del video (.md), texto extraído (.txt)
├── skill/ugc-seedance25/     skill Claude Code  →  symlink en ~/.claude/skills/ugc-seedance25
│   ├── SKILL.md              brief → 3 prompts → brief.md → (opcional) Magnific con gate de costo → QA
│   └── references/           regras.md · formatos.md · ptbr.md · magnific.md
└── app/                      generador de brief (web) — estándar seedance2/app
    ├── public/index.html     single-page, INEMA dark ámbar, vanilla JS
    ├── public/formats.js     fuente única: 9 formatos, beats, bloques fijos, objetivos, reglas
    ├── api/generate.ts       función serverless — modo IA (uso de herramientas forzado)
    ├── api/llm-client.ts     cliente unificado: Claude OAuth (predeterminado) · OpenRouter · Anthropic API
    └── dev-server.mjs        se ejecuta localmente sin Vercel CLI
```

## La receta (del pack)

1. **Producto**: foto de comercio electrónico limpia, etiqueta corta. Se convierte en la identidad del producto.
2. **Creador**: retrato "cámara frontal de iPhone, sin retoques", en la habitación exacta del video.
3. **Video**: Seedance 2.5, 9:16, 1080p, 30 s, audio nativo, las dos imágenes como referencia.
   Prompt = lista de planos de 5 beats (gancho · qué es · demo · resultado · cierre), cada frase entre
   comillas con marca de tiempo, bloque de realismo, bloque de audio con sonidos prohibidos. 70–80 palabras.

## Skill

Instalada mediante symlink. Activadores: "anúncio ugc", "ugc ad", "seedance ugc", o `/ugc-seedance25`.

- Siempre genera `~/projetos/output/ugc-seedance25/<slug>/brief.md` con los 3 prompts.
- Solo ejecuta en Magnific si lo pides: `simulate_cost` → producto → creador → borrador 720p →
  QA en fotogramas (contact sheet, verificación del espejo) → final 1080p.
- Frases en PT-BR de forma predeterminada; alternativa de voz vía inemavox → referencia de audio.

## App

```bash
cd app
npm install
npm run check        # tsc + sintaxis de formats.js
npm run dev          # http://localhost:3040
```

- **Modo Template**: funciona 100% en el navegador, sin servidor ni clave. Escribes las frases; la
  app arma los 3 prompts con los encabezados de la plataforma elegida, cuenta palabras, copia y
  descarga el `.md`. Es lo que sirve GitHub Pages.
- **Modo IA** (`POST /api/generate`): el modelo escribe el producto, el creador, las acciones de beat y las 5
  frases en el idioma elegido. Backend, en este orden:
  1. **Claude OAuth de Claude Code** (predeterminado): lee `~/.claude/.credentials.json` de la máquina donde
     se ejecuta el servidor. Costo cero con la suscripción. Basta con `claude login` y `npm run dev`.
  2. **OpenRouter** (opcional): el usuario elige en "Backend da IA" y pega la clave (se queda solo en el
     navegador), o el servidor define `OPENROUTER_API_KEY`. Es la opción para un deploy público.
  3. Anthropic API (`ANTHROPIC_API_KEY`) sigue siendo compatible en el cliente, pero no aparece en la UI.
- Modelo predeterminado `claude-sonnet-5` (`LLM_MODEL` o campo "Modelo" en la app). Límite de solicitudes: 5/min/IP.

Deploy serverless (opcional, para el modo IA público): commit + push; en Vercel, Root Directory =
`app`, sin build command, output `public`, env `OPENROUTER_API_KEY` y `ALLOWED_ORIGIN`.

## Estado de la verificación (2026-09-13)

- `npm run check` pasa. El servidor de desarrollo inicia; la página, `formats.js` y las validaciones de la API responden.
- Modo Template verificado en Chromium headless (ejemplo, armado, cambio de objetivo, contador).
- Modo IA verificado en vivo vía **Claude OAuth** (formato 07, frases en PT-BR, JSON completo).
  OpenRouter no se probó en vivo (sin clave); el cliente es el mismo que ya está en producción en seedance2.
- No se generó nada en Magnific. El costo del one-take de 30 s 1080p aún no se ha medido.
