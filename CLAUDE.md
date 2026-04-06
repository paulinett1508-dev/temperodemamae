# CLAUDE.md — Tempero de Mamãe

## Visão Geral do Projeto

Landing page de vitrine/portfólio para uma coleção de 8 materiais digitais (PDFs) sobre temperos artesanais, molhos, técnicas de conservação, precificação e branding para negócio caseiro.

**Repositório:** https://github.com/paulinett1508-dev/temperodemamae  
**Deploy:** Vercel (static site)  
**Stack:** HTML puro + CSS + JS vanilla (sem framework)

---

## Público-Alvo

Mulheres que querem renda extra transformando conhecimento culinário em negócio artesanal de temperos.

---

## Identidade Visual

| Token | Valor |
|-------|-------|
| `--cream` | `#faf6ef` |
| `--cream-dark` | `#f0e9dc` |
| `--cream-mid` | `#e8dece` |
| `--parchment` | `#ede3d3` |
| `--ink` | `#1e1509` |
| `--ink-soft` | `#3d2e18` |
| `--ink-muted` | `#6b5740` |
| `--ink-light` | `#9c8468` |
| `--gold` | `#b8902e` |
| `--gold-bright` | `#d4a843` |
| `--gold-pale` | `#e8c97a` |
| `--gold-faint` | `#f3e4b8` |
| `--gold-line` | `rgba(184,144,46,0.25)` |
| `--white` | `#ffffff` |
| `--white-soft` | `#d4c9b0` |

**Fontes (Google Fonts):**
- `Cinzel` — títulos, labels, eyebrows (elegante, serifada)
- `Cormorant Garamond` — subtítulos, depoimentos, itálicos
- `Lato` — corpo do texto, parágrafos

**Tom:** Premium / Gourmet. Preto e dourado. Nada genérico.

---

## Estrutura de Arquivos

```
temperodemamae/
├── CLAUDE.md           ← este arquivo
├── index.html          ← landing page principal
├── assets/
│   ├── css/
│   │   └── style.css   ← (futuro) extração do CSS inline
│   ├── js/
│   │   └── main.js     ← (futuro) extração do JS inline
│   └── img/            ← imagens e assets visuais
└── vercel.json         ← configuração de deploy
```

---

## Seções da Landing Page

1. **Nav** — fixo no topo, link âncora para cada seção + toggle mobile
2. **Hero** — headline, subtítulo, ornamento decorativo
3. **Stats Strip** — 4 números de impacto (181 receitas, 8 guias, 6 áreas, 200% margem)
4. **Categorias** — grid de 6 cards com leitor PDF integrado (PDF.js)
5. **Técnicas** — 2 cards de técnicas especializadas
6. **Sobre** — proposta de valor + tagline
7. **Footer** — marca, tagline, copyright

---

## Os 8 Materiais Digitais (PDFs)

| # | Arquivo | Conteúdo Principal |
|---|---------|-------------------|
| 01 | `100_TEMPEROS.pdf` | 100 receitas de temperos artesanais |
| 02 | `81_MOLHOS_INCRIVEIS.pdf` | 81 molhos (pimenta, cremes, conservas) |
| 03 | `TEMPEROS_CASEIROS_FACEIS_DO_BRASIL.pdf` | Temperos regionais brasileiros |
| 04 | `TECNICAS_DE_CONGELAMENTO.pdf` | Armazenamento profissional |
| 05 | `Como_precificar.pdf` | Precificação com margem 80%–200% |
| 06 | `DO_ZERO_AOS_3_MIL.pdf` | Criação de marca, WhatsApp, redes sociais |
| 07 | `OS_TEMPEROS_FAVORITOS_DOS_GANHADORES_DO_MASTERCHEF.pdf` | Segredos dos campeões |
| 08 | `ROTULOS_PRONTOS.pdf` | Rótulos para imprimir |

---

## Gates de Qualidade

Todo componente novo e toda mudança no `index.html` deve passar pelos gates abaixo antes de entrar em produção. Gates 1–4 são obrigatórios. Gate 5 diferencia landing pages profissionais.

**Referência completa:** `docs/compliance/landing-page-compliance.md`

### Gate 1 — Identidade Visual & Hierarquia
- Paleta exclusiva: `--black`, `--gold`, `--gold-light`, `--white`, `--white-soft`
- Fontes: `Cinzel` (títulos/labels), `Cormorant Garamond` (subtítulos/itálicos), `Lato` (corpo)
- Cores **sempre** via `var(--token)` — nunca `#hex` ou `rgb()` direto no código
- Grain overlay preservado no `body::before`
- Animações de entrada com `.reveal` + IntersectionObserver
- Scroll suave entre seções (`scroll-behavior: smooth`)
- Hierarquia visual: o elemento mais importante tem maior peso visual

### Gate 2 — Semântica & Governança CSS
- Estrutura semântica: `<header>` → `<main id="conteudo-principal">` → `<footer>`
- H1 único por página; H2/H3 em hierarquia sequencial
- Zero `style=""` inline no HTML — verificar com `grep -n 'style="' index.html`
- Zero `!important` sem comentário documentando o override
- Todo `<section>` com `id` para âncoras de navegação

### Gate 3 — Acessibilidade WCAG 2.1 AA
- Skip link como primeiro elemento focável do `<body>`
- `<nav aria-label="Navegação principal">`
- `:focus-visible` definido e visível em todos os elementos interativos
- `prefers-reduced-motion` respeitado em todas as animações e transições
- `aria-hidden="true"` em todos os elementos puramente decorativos
- Emojis informativos com `role="img" aria-label="descrição"`

### Gate 4 — Responsividade & Mobile
- Testar em 375px, 640px e 1024px antes de qualquer merge
- Nav com fallback mobile funcional — jamais `display: none` sem menu substituto
- Alvos de toque mínimo 44×44px (botões, links)
- Nenhum elemento com largura fixa causando scroll horizontal

### Gate 5 — SEO & Performance
- `<meta name="description">`, `<link rel="canonical">` e Open Graph obrigatórios
- `<link rel="preconnect">` para origens externas (fonts.googleapis.com, fonts.gstatic.com)
- Schema JSON-LD (`Product` ou `Organization`) presente e válido
- `loading="lazy"` em imagens below-the-fold quando presentes

---

## Git Workflow

### Branch Strategy
- **Nunca** commitar direto na `main`. Sempre trabalhar em feature branches.
- Branch de desenvolvimento atual é definida por sessão (ex: `claude/feature-name-XXXXX`).
- Push automático é feito via hook `PostToolUse` — não é necessário rodar `git push` manualmente.

### Commits
- Commitar com frequência — mudanças pequenas e atômicas.
- Usar **Conventional Commits**: `feat:`, `fix:`, `refactor:`, `docs:`, `chore:`, `style:`, `test:`.
- Mensagem em português ou inglês, concisa e descritiva.

### Auto-Push (Hook)
O projeto usa um hook `PostToolUse` configurado em `.claude/settings.json` que:
1. Detecta quando um `git commit` é executado via Bash.
2. Faz `git push -u origin HEAD` automaticamente.
3. Retry com backoff exponencial (2s → 4s → 8s → 16s) em caso de falha de rede.

> **Importante:** O diretório `.claude/` está no `.gitignore` — o hook é local e não vai para o repositório. As regras de workflow ficam neste CLAUDE.md.

---

## Próximas Features Planejadas

- [ ] Modal/lightbox com preview de cada PDF
- [ ] Seção de FAQ (accordion)
- [ ] Integração com link de compra (Hotmart/Kiwify/WhatsApp)
- [ ] Formulário de captura de leads (e-mail/WhatsApp)
- [ ] Contador regressivo de oferta
- [ ] Dark/light mode toggle
- [ ] Analytics (Google Analytics / Vercel Analytics)

---

## Deploy (Vercel)

O projeto é um **static site** — o Vercel serve o `index.html` diretamente.

```json
// vercel.json
{
  "rewrites": [{ "source": "/(.*)", "destination": "/index.html" }]
}
```

Para deployar:
```bash
vercel --prod
```

Ou via GitHub Integration: push na branch `main` → deploy automático.

---

## Comandos Úteis (Claude Code)

```bash
# Ver estrutura do projeto
ls -la

# Abrir no browser local
open index.html

# Commitar e subir
git add .
git commit -m "feat: descrição da mudança"
git push origin main
```

## Uso de Subagents

- Use subagents para pesquisa de referências visuais, análise de paleta e sugestões de copy em paralelo
- Offload verificação de links, imagens quebradas e performance para subagents dedicados
- Para a landing: subagent por seção (hero, produtos, depoimentos, CTA) ao trabalhar em paralelo

## Verificação antes de Concluir

- Nunca marque tarefa como concluída sem verificar o deploy Vercel + abertura em mobile (viewport 390px)
- Checagem: nenhuma cor hardcoded fora dos tokens CSS, imagens com `alt`, CTAs funcionando
- Pergunta padrão: *"Uma cliente do público-alvo entenderia o que fazer ao ver essa página?"*

## Elegância (features não-triviais)

- Para mudanças que tocam 3+ seções: pause e avalie consistência visual antes de implementar
- Se o CSS está acumulando overrides: reestruturar usando os tokens já definidos no topo do arquivo
- **Exceção:** ajustes de texto e cor pontuais — não criar abstração para 1 uso
