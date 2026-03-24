# Design Spec: Integração do agnostic-core como Balizador de Compliance

**Data:** 2026-03-24
**Projeto:** temperodemamae
**Status:** Aprovado

---

## Objetivo

Integrar o repositório [agnostic-core](https://github.com/paulinett1508-dev/agnostic-core) ao projeto `temperodemamae` como balizador de conhecimentos e compliance para a landing page, abrangendo:

1. Criar `docs/compliance/landing-page-compliance.md` — documento curado com as boas práticas do agnostic-core aplicáveis a landing pages estáticas
2. Auditar e corrigir **todos** os issues encontrados no `index.html` atual
3. Substituir o sistema de regras do `CLAUDE.md` por um modelo baseado em gates de qualidade

---

## Escopo

- **Stack:** HTML puro + CSS inline + JS vanilla (sem framework)
- **Deploy:** Vercel static site
- **Arquivos modificados:** `index.html`, `CLAUDE.md`
- **Arquivos criados:** `docs/compliance/landing-page-compliance.md`

---

## Parte 1 — docs/compliance/landing-page-compliance.md

**Pré-condição:** O conteúdo do repositório `agnostic-core` foi pesquisado e está disponível na sessão de trabalho. Os arquivos-fonte referenciados abaixo foram confirmados como existentes.

Arquivo Markdown único, curado a partir do agnostic-core, contendo apenas o que se aplica a uma landing page estática (sem partes de SPA, React, banco de dados ou formulários complexos).

### Estrutura do documento

| Seção | Fonte agnostic-core | Conteúdo |
|---|---|---|
| HTML/CSS Governance | `html-css-audit.md` + `anti-frankenstein.md` | Semântica, tokens de design, red flags de CSS |
| UX Quality Gates | `ux-guidelines.md` + `ui-ux-quality-gates.md` | 5 gates obrigatórios, severidades CRÍTICA→BAIXA |
| SEO & Core Web Vitals | `seo-checklist.md` | LCP/INP/CLS, meta tags, Schema JSON-LD, GEO |
| Acessibilidade WCAG 2.1 AA | `accessibility.md` | Contraste, teclado, ARIA, estrutura semântica |
| Pré-deploy Checklist | `validation-checklist.md` + `vercel-patterns.md` | Quick check + Full check adaptado para Vercel static |

Cada seção inclui um **checklist acionável** com checkboxes prontos para usar em revisões.

---

## Parte 2 — Auditoria e Correção do index.html

### Issues CRÍTICAS (10)

| # | Issue | Localização | Correção |
|---|---|---|---|
| C1 | Sem `<meta name="description">` | `<head>` | Adicionar meta description (150-160 chars) |
| C2 | Sem `<main>` landmark | body | Envolver conteúdo principal em `<main>` |
| C3 | Sem `<header>` landmark | nav | Envolver `<nav>` em `<header>` |
| C4 | Nav desaparece no mobile (640px) sem substituto | `@media 640px` | Adicionar `<button>` hamburger com `aria-expanded`, menu fecha ao clicar âncora e ao pressionar Escape |
| C5 | Sem skip link | topo do body | Adicionar "Pular para conteúdo" como 1º link focável |
| C6 | Sem `:focus-visible` | CSS | Definir foco visível para todos os elementos interativos |
| C7 | `prefers-reduced-motion` ignorado | CSS + JS | Pausar/desativar animações quando preferência ativa |
| C8 | 4× `style=""` inline no HTML | linhas 910, 931, 1002, 1049 | Extrair para classes CSS utilitárias |
| C9 | JS seta `a.style.color` inline | linha 1141 | Usar classe `.nav-active` via classList |
| C10 | Brand do nav é `<span>` sem link | linha 844 | Trocar por `<a href="#" aria-label="Arte dos Temperos — início">` |

### Issues ALTAS (6)

| # | Issue | Correção |
|---|---|---|
| A1 | Sem `<link rel="canonical">` | Adicionar no `<head>` |
| A2 | Sem Open Graph meta tags | Adicionar `og:title`, `og:description`, `og:type`, `og:url` |
| A3 | Sem Schema JSON-LD | Adicionar schema `Product` ou `Organization` |
| A4 | Sem `aria-label` no `<nav>` | `aria-label="Navegação principal"` |
| A5 | Sem preconnect para Google Fonts | `<link rel="preconnect">` antes do stylesheet de fontes |
| A6 | Elementos decorativos sem `aria-hidden` | `.hero-ornament`, `.testi-quote span`, `.hero-scroll` |

### Issues MÉDIAS (3)

| # | Issue | Correção |
|---|---|---|
| M1 | Section About sem `id` (inacessível por âncora) | Adicionar `id="sobre"` à section |
| M2 | Emojis informativos sem `role="img"` + `aria-label` | **Informativos** (recebem `role="img"` + `aria-label`): `book-icon` (🌿🫙🇧🇷❄️💰🚀👨‍🍳🏷️) e `topic-icon` (🧪📦📊🏪🎨⭐) e `about-icon` (🫙). **Decorativos** (recebem `aria-hidden="true"`): `cta-ornament-symbol` (✦), `testi-quote` (") |
| M3 | Sem `og:image` meta | Adicionar placeholder comentado para quando houver imagem |

**Critério:** Todas as 19 issues são corrigidas. Nenhuma issue fica pendente para depois.

---

## Parte 3 — Novo Sistema de Regras no CLAUDE.md

Substituir as seções "Sempre fazer" e "Nunca fazer" por **5 Gates de Qualidade**, incorporando as regras existentes de identidade visual dentro dos gates.

### Gate 1 — Identidade Visual & Hierarquia *(incorpora regras existentes)*
- Paleta exclusiva: `--black`, `--gold`, `--gold-light`, `--white`, `--white-soft`
- Fontes: Cinzel (títulos), Cormorant Garamond (subtítulos/itálicos), Lato (corpo)
- Cores sempre via variável CSS — nunca hex/rgb direto no código
- Grain overlay no `body::before`, scroll suave entre seções
- Animações de entrada com `.reveal` + IntersectionObserver
- Hierarquia visual clara: o elemento mais importante tem maior peso visual

### Gate 2 — Semântica & Governança CSS
- Estrutura semântica: `<header>` → `<main>` → `<footer>`
- H1 único por página com hierarquia H2/H3 sequencial
- Zero `style=""` inline no HTML
- Zero `!important` sem justificativa documentada em comentário
- Seções com `id` para âncoras de navegação

### Gate 3 — Acessibilidade WCAG 2.1 AA
- Contraste mínimo: 4.5:1 para texto normal, 3:1 para texto grande
- Skip link como primeiro elemento focável da página
- `:focus-visible` definido e visível em todos os elementos interativos
- `prefers-reduced-motion` respeitado em todas as animações e transições
- `aria-label` em `<nav>`, `aria-hidden="true"` em elementos decorativos

### Gate 4 — Responsividade & Mobile
- Breakpoints obrigatórios: 375px, 640px, 1024px
- Nav com fallback mobile funcional (jamais sumir sem substituto)
- Alvos de toque mínimo 44×44px
- Nenhum elemento com largura fixa causando scroll horizontal

### Gate 5 — SEO & Performance
- `<meta name="description">`, `<link rel="canonical">` e Open Graph obrigatórios
- `<link rel="preconnect">` para todas as origens externas críticas (fontes, etc.)
- Schema JSON-LD nas páginas principais
- `loading="lazy"` em imagens below-the-fold quando presentes

**Gates 1–4 são requisitos mínimos para qualquer mudança entrar em produção. Gate 5 diferencia landing pages profissionais.**

**Referência completa:** `docs/compliance/landing-page-compliance.md`

---

## Ordem de Implementação

1. Criar `docs/compliance/landing-page-compliance.md`
2. Corrigir issues CRÍTICAS no `index.html` (C1–C10)
3. Corrigir issues ALTAS no `index.html` (A1–A6)
4. Corrigir issues MÉDIAS no `index.html` (M1–M3)
5. Atualizar `CLAUDE.md` com o novo sistema de gates
6. Commit único com todas as mudanças

---

## Critérios de Sucesso

- [ ] `docs/compliance/landing-page-compliance.md` criado com 5 seções e checklists
- [ ] Todas as 10 issues CRÍTICAS corrigidas no `index.html`
- [ ] Todas as 6 issues ALTAS corrigidas no `index.html`
- [ ] Todas as 3 issues MÉDIAS corrigidas no `index.html`
- [ ] `CLAUDE.md` com sistema de 5 gates substituindo regras anteriores
- [ ] Zero `style=""` inline restante no HTML
- [ ] Zero navegação sem fallback mobile
