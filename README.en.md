# ARCKHÉ · UGC Seedance 2.5

**🇧🇷 [Português](README.md) · 🇺🇸 [English](README.en.md) · 🇪🇸 [Español](README.es.md)**

UGC ads (creator speaking to their phone, 30-second one-take, 9:16, 1080p) with Seedance 2.5,
based on the “The Seedance 2.5 UGC Ads Pack” (Zubair Trabzada · AI Workshop, 2026), adapted
for the ARCKHÉ ecosystem (Magnific MCP as the primary target; Higgsfield and fal.ai as secondary options).

## 📖 User Guide

Complete guide (landing page + step-by-step instructions): **https://linnoff88-dev.github.io/arckhe-/guia/en/**
Online app (Template mode): https://linnoff88-dev.github.io/arckhe-/app/public/

Two deliverables that share the same content:

```
ugc-seedance25/
├── index.html                redirects to the app (GitHub Pages)
├── PLANO.md                  solution design, mapping to Magnific, risks
├── docs/                     original pack: PDF, video transcript (.md), extracted text (.txt)
├── skill/ugc-seedance25/     Claude Code skill  →  symlink in ~/.claude/skills/ugc-seedance25
│   ├── SKILL.md              brief → 3 prompts → brief.md → (optional) Magnific with cost gate → QA
│   └── references/           regras.md · formatos.md · ptbr.md · magnific.md
└── app/                      brief generator (web) — based on seedance2/app
    ├── public/index.html     single-page, ARCKHÉ dark amber, vanilla JS
    ├── public/formats.js     single source: 9 formats, beats, fixed blocks, targets, rules
    ├── api/generate.ts       serverless function — AI mode (forced tool use)
    ├── api/llm-client.ts     unified client: Claude OAuth (default) · OpenRouter · Anthropic API
    └── dev-server.mjs        runs locally without Vercel CLI
```

## The recipe (from the pack)

1. **Product**: clean e-commerce photo, short label. Becomes the product identity.
2. **Creator**: “front-facing iPhone camera, no retouching” portrait, in the exact room where the video takes place.
3. **Video**: Seedance 2.5, 9:16, 1080p, 30 s, native audio, both images as references.
   Prompt = shot list of 5 beats (hook · what it is · demo · result · close), each line between
   quotation marks with a timestamp, realism block, audio block with prohibited sounds. 70–80 words.

## Skill

Installed via symlink. Triggers: “ugc ad”, “ugc ad”, “seedance ugc”, or `/ugc-seedance25`.

- Always generates `~/projetos/output/ugc-seedance25/<slug>/brief.md` with the 3 prompts.
- Only runs in Magnific if you ask: `simulate_cost` → product → creator → 720p draft →
  frame QA (contact sheet, mirror check) → 1080p final.
- Lines in PT-BR by default; voice fallback via ARCKHÉvox → audio reference.

## App

```bash
cd app
npm install
npm run check        # tsc + formats.js syntax
npm run dev           # http://localhost:3040
```

- **Template mode**: runs 100% in the browser, with no server or key. You write the lines; the
  app builds the 3 prompts with the chosen platform’s headers, counts words, copies them, and
  downloads the `.md`. This is what GitHub Pages serves.
- **AI mode** (`POST /api/generate`): the model writes the product, creator, beat actions, and all 5
  lines in the selected language. Backend, in order:
  1. **Claude Code Claude OAuth** (default): reads `~/.claude/.credentials.json` from the machine where
     the server runs. Zero cost with the subscription. Just run `claude login` and `npm run dev`.
  2. **OpenRouter** (optional): the user selects it under “AI Backend” and pastes the key (stored only in the
     browser), or the server sets `OPENROUTER_API_KEY`. This is the route for a public deployment.
  3. Anthropic API (`ANTHROPIC_API_KEY`) is still supported in the client, but does not appear in the UI.
- Default model `claude-sonnet-5` (`LLM_MODEL` or the “Model” field in the app). Rate limit: 5/min/IP.

Optional serverless deployment (for public AI mode): commit + push; in Vercel, Root Directory =
`app`, no build command, output `public`, env `OPENROUTER_API_KEY` and `ALLOWED_ORIGIN`.

## Verification status (2026-09-13)

- `npm run check` passes. Dev server starts; the page, `formats.js`, and API validations respond.
- Template mode verified in headless Chromium (example, assembly, target switching, counter).
- AI mode verified live via **Claude OAuth** (format 07, lines in PT-BR, complete JSON).
  OpenRouter not tested live (no key); the client is the same one already in production in seedance2.
- Nothing was generated in Magnific. The cost of a 30-second 1080p one-take has not yet been measured.
