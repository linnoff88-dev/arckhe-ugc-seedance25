# INEMA Â· UGC Seedance 2.5

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

AnÃºncios UGC (creator falando pro celular, one-take de 30 s, 9:16, 1080p) com Seedance 2.5, a
partir do pack "The Seedance 2.5 UGC Ads Pack" (Zubair Trabzada Â· AI Workshop, 2026), adaptado
pro ecossistema INEMA (Magnific MCP como alvo principal; Higgsfield e fal.ai como secundÃ¡rios).

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/ugc-seedance25/guia/**
App online (modo Template): https://inematds.github.io/ugc-seedance25/app/public/

Duas entregas que compartilham o mesmo conteÃºdo:

```
ugc-seedance25/
â”œâ”€â”€ index.html                redireciona pro app (GitHub Pages)
â”œâ”€â”€ PLANO.md                  desenho da soluÃ§Ã£o, mapeamento pro Magnific, riscos
â”œâ”€â”€ docs/                     pack original: PDF, transcriÃ§Ã£o do vÃ­deo (.md), texto extraÃ­do (.txt)
â”œâ”€â”€ skill/ugc-seedance25/     skill Claude Code  â†’  symlink em ~/.claude/skills/ugc-seedance25
â”‚   â”œâ”€â”€ SKILL.md              brief â†’ 3 prompts â†’ brief.md â†’ (opcional) Magnific com gate de custo â†’ QA
â”‚   â””â”€â”€ references/           regras.md Â· formatos.md Â· ptbr.md Â· magnific.md
â””â”€â”€ app/                      gerador de brief (web) â€” padrÃ£o seedance2/app
    â”œâ”€â”€ public/index.html     single-page, INEMA dark Ã¢mbar, vanilla JS
    â”œâ”€â”€ public/formats.js     fonte Ãºnica: 9 formatos, beats, blocos fixos, alvos, regras
    â”œâ”€â”€ api/generate.ts       funÃ§Ã£o serverless â€” modo IA (tool use forÃ§ado)
    â”œâ”€â”€ api/llm-client.ts     cliente unificado: Claude OAuth (padrÃ£o) Â· OpenRouter Â· Anthropic API
    â””â”€â”€ dev-server.mjs        roda local sem Vercel CLI
```

## A receita (do pack)

1. **Produto**: foto e-commerce limpa, rÃ³tulo curto. Vira a identidade do produto.
2. **Creator**: retrato "iPhone cÃ¢mera frontal, sem retoque", no cÃ´modo exato do vÃ­deo.
3. **VÃ­deo**: Seedance 2.5, 9:16, 1080p, 30 s, Ã¡udio nativo, as duas imagens como referÃªncia.
   Prompt = shot list de 5 beats (hook Â· o que Ã© Â· demo Â· resultado Â· fecho), cada fala entre
   aspas com timestamp, bloco de realismo, bloco de Ã¡udio com sons proibidos. 70â€“80 palavras.

## Skill

Instalada por symlink. Gatilhos: "anÃºncio ugc", "ugc ad", "seedance ugc", ou `/ugc-seedance25`.

- Sempre gera `~/projetos/output/ugc-seedance25/<slug>/brief.md` com os 3 prompts.
- SÃ³ executa na Magnific se vocÃª pedir: `simulate_cost` â†’ produto â†’ creator â†’ rascunho 720p â†’
  QA em frames (contact sheet, checagem de espelho) â†’ final 1080p.
- Falas em PT-BR por padrÃ£o; fallback de voz via inemavox â†’ referÃªncia de Ã¡udio.

## App

```bash
cd app
npm install
npm run check        # tsc + syntax do formats.js
npm run dev          # http://localhost:3040
```

- **Modo Template**: roda 100% no navegador, sem servidor e sem chave. VocÃª escreve as falas; o
  app monta os 3 prompts com os cabeÃ§alhos da plataforma escolhida, conta palavras, copia e
  baixa o `.md`. Ã‰ o que o GitHub Pages serve.
- **Modo IA** (`POST /api/generate`): o modelo escreve produto, creator, aÃ§Ãµes de beat e as 5
  falas no idioma escolhido. Backend, em ordem:
  1. **Claude OAuth do Claude Code** (padrÃ£o): lÃª `~/.claude/.credentials.json` da mÃ¡quina onde
     o servidor roda. Custo zero na assinatura. Basta `claude login` e `npm run dev`.
  2. **OpenRouter** (opcional): o usuÃ¡rio escolhe em "Backend da IA" e cola a chave (fica sÃ³ no
     navegador), ou o servidor define `OPENROUTER_API_KEY`. Ã‰ o caminho pra um deploy pÃºblico.
  3. Anthropic API (`ANTHROPIC_API_KEY`) continua suportada no cliente, mas nÃ£o aparece na UI.
- Modelo padrÃ£o `claude-sonnet-5` (`LLM_MODEL` ou campo "Modelo" no app). Rate limit 5/min/IP.

Deploy serverless (opcional, pra modo IA pÃºblico): commit + push; no Vercel, Root Directory =
`app`, sem build command, output `public`, env `OPENROUTER_API_KEY` e `ALLOWED_ORIGIN`.

## Estado da verificaÃ§Ã£o (2026-09-13)

- `npm run check` passa. Dev-server sobe; pÃ¡gina, `formats.js` e validaÃ§Ãµes da API respondem.
- Modo Template verificado em Chromium headless (exemplo, montagem, troca de alvo, contador).
- Modo IA verificado ao vivo via **Claude OAuth** (formato 07, falas em PT-BR, JSON completo).
  OpenRouter nÃ£o testado ao vivo (sem chave); o cliente Ã© o mesmo jÃ¡ em produÃ§Ã£o no seedance2.
- Nada foi gerado na Magnific. Custo do one-take 30 s 1080p ainda nÃ£o medido.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>