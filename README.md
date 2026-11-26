ï»¿# INEMA êÂ· UGC Seedance 2.5

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

AnêÂºncios UGC (creator falando pro celular, one-take de 30 s, 9:16, 1080p) com Seedance 2.5, a
partir do pack "The Seedance 2.5 UGC Ads Pack" (Zubair Trabzada êÂ· AI Workshop, 2026), adaptado
pro ecossistema INEMA (Magnific MCP como alvo principal; Higgsfield e fal.ai como secundêÂ¡rios).

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://inematds.github.io/ugc-seedance25/guia/**
App online (modo Template): https://inematds.github.io/ugc-seedance25/app/public/

Duas entregas que compartilham o mesmo conteêÂºdo:

```
ugc-seedance25/
ê¢âÅê¢ââ¬ê¢ââ¬ index.html                redireciona pro app (GitHub Pages)
ê¢âÅê¢ââ¬ê¢ââ¬ PLANO.md                  desenho da soluêÂ§êÂ£o, mapeamento pro Magnific, riscos
ê¢âÅê¢ââ¬ê¢ââ¬ docs/                     pack original: PDF, transcriêÂ§êÂ£o do vêÂ­deo (.md), texto extraêÂ­do (.txt)
ê¢âÅê¢ââ¬ê¢ââ¬ skill/ugc-seedance25/     skill Claude Code  ê¢â â  symlink em ~/.claude/skills/ugc-seedance25
ê¢ââ   ê¢âÅê¢ââ¬ê¢ââ¬ SKILL.md              brief ê¢â â 3 prompts ê¢â â brief.md ê¢â â (opcional) Magnific com gate de custo ê¢â â QA
ê¢ââ   ê¢ââê¢ââ¬ê¢ââ¬ references/           regras.md êÂ· formatos.md êÂ· ptbr.md êÂ· magnific.md
ê¢ââê¢ââ¬ê¢ââ¬ app/                      gerador de brief (web) ê¢â¬â padrêÂ£o seedance2/app
    ê¢âÅê¢ââ¬ê¢ââ¬ public/index.html     single-page, INEMA dark êÂ¢mbar, vanilla JS
    ê¢âÅê¢ââ¬ê¢ââ¬ public/formats.js     fonte êÂºnica: 9 formatos, beats, blocos fixos, alvos, regras
    ê¢âÅê¢ââ¬ê¢ââ¬ api/generate.ts       funêÂ§êÂ£o serverless ê¢â¬â modo IA (tool use forêÂ§ado)
    ê¢âÅê¢ââ¬ê¢ââ¬ api/llm-client.ts     cliente unificado: Claude OAuth (padrêÂ£o) êÂ· OpenRouter êÂ· Anthropic API
    ê¢ââê¢ââ¬ê¢ââ¬ dev-server.mjs        roda local sem Vercel CLI
```

## A receita (do pack)

1. **Produto**: foto e-commerce limpa, rêÂ³tulo curto. Vira a identidade do produto.
2. **Creator**: retrato "iPhone cêÂ¢mera frontal, sem retoque", no cêÂ´modo exato do vêÂ­deo.
3. **VêÂ­deo**: Seedance 2.5, 9:16, 1080p, 30 s, êÂ¡udio nativo, as duas imagens como referêÂªncia.
   Prompt = shot list de 5 beats (hook êÂ· o que êÂ© êÂ· demo êÂ· resultado êÂ· fecho), cada fala entre
   aspas com timestamp, bloco de realismo, bloco de êÂ¡udio com sons proibidos. 70ê¢â¬â80 palavras.

## Skill

Instalada por symlink. Gatilhos: "anêÂºncio ugc", "ugc ad", "seedance ugc", ou `/ugc-seedance25`.

- Sempre gera `~/projetos/output/ugc-seedance25/<slug>/brief.md` com os 3 prompts.
- SêÂ³ executa na Magnific se vocêÂª pedir: `simulate_cost` ê¢â â produto ê¢â â creator ê¢â â rascunho 720p ê¢â â
  QA em frames (contact sheet, checagem de espelho) ê¢â â final 1080p.
- Falas em PT-BR por padrêÂ£o; fallback de voz via inemavox ê¢â â referêÂªncia de êÂ¡udio.

## App

```bash
cd app
npm install
npm run check        # tsc + syntax do formats.js
npm run dev          # http://localhost:3040
```

- **Modo Template**: roda 100% no navegador, sem servidor e sem chave. VocêÂª escreve as falas; o
  app monta os 3 prompts com os cabeêÂ§alhos da plataforma escolhida, conta palavras, copia e
  baixa o `.md`. êâ° o que o GitHub Pages serve.
- **Modo IA** (`POST /api/generate`): o modelo escreve produto, creator, aêÂ§êÂµes de beat e as 5
  falas no idioma escolhido. Backend, em ordem:
  1. **Claude OAuth do Claude Code** (padrêÂ£o): lêÂª `~/.claude/.credentials.json` da mêÂ¡quina onde
     o servidor roda. Custo zero na assinatura. Basta `claude login` e `npm run dev`.
  2. **OpenRouter** (opcional): o usuêÂ¡rio escolhe em "Backend da IA" e cola a chave (fica sêÂ³ no
     navegador), ou o servidor define `OPENROUTER_API_KEY`. êâ° o caminho pra um deploy pêÂºblico.
  3. Anthropic API (`ANTHROPIC_API_KEY`) continua suportada no cliente, mas nêÂ£o aparece na UI.
- Modelo padrêÂ£o `claude-sonnet-5` (`LLM_MODEL` ou campo "Modelo" no app). Rate limit 5/min/IP.

Deploy serverless (opcional, pra modo IA pêÂºblico): commit + push; no Vercel, Root Directory =
`app`, sem build command, output `public`, env `OPENROUTER_API_KEY` e `ALLOWED_ORIGIN`.

## Estado da verificaêÂ§êÂ£o (2026-09-13)

- `npm run check` passa. Dev-server sobe; pêÂ¡gina, `formats.js` e validaêÂ§êÂµes da API respondem.
- Modo Template verificado em Chromium headless (exemplo, montagem, troca de alvo, contador).
- Modo IA verificado ao vivo via **Claude OAuth** (formato 07, falas em PT-BR, JSON completo).
  OpenRouter nêÂ£o testado ao vivo (sem chave); o cliente êÂ© o mesmo jêÂ¡ em produêÂ§êÂ£o no seedance2.
- Nada foi gerado na Magnific. Custo do one-take 30 s 1080p ainda nêÂ£o medido.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>