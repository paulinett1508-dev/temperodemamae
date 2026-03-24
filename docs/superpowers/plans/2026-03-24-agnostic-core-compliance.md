# Agnostic-Core Compliance Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Integrar o agnostic-core como balizador de compliance — criar doc de referência curado, corrigir 19 issues no index.html e atualizar o CLAUDE.md com sistema de 5 gates de qualidade.

**Architecture:** Três entregáveis independentes em sequência: (1) compliance doc como referência, (2) correções no index.html agrupadas por criticidade, (3) atualização do CLAUDE.md. Stack é HTML puro + CSS inline + JS vanilla — sem build step, sem framework. Verificação é feita via grep (para confirmar mudanças no código) e inspeção visual no browser.

**Tech Stack:** HTML5, CSS3 (variáveis CSS, media queries, prefers-reduced-motion), JS vanilla (IntersectionObserver, classList), Vercel static deploy

> **IMPORTANTE para agentes autônomos:** Execute TODOS os comandos bash a partir do root do repositório (`C:/PROJETOS/temperodemamae`). Sempre use caminhos relativos ao root. Antes de iniciar, confirme o cwd correto com `pwd` e verifique que `index.html` está visível com `ls index.html`.

---

## Mapa de Arquivos

| Arquivo | Ação | Responsabilidade |
|---|---|---|
| `docs/compliance/landing-page-compliance.md` | Criar | Referência curada de boas práticas para landing page estática |
| `index.html` | Modificar | Correções de acessibilidade, semântica, SEO e CSS governance |
| `CLAUDE.md` | Modificar | Substituir regras antigas por sistema de 5 gates de qualidade |

---

## Task 1: Criar docs/compliance/landing-page-compliance.md

**Files:**
- Create: `docs/compliance/landing-page-compliance.md`

- [ ] **Step 1: Confirmar cwd e diretório**

```bash
pwd && ls index.html && ls docs/compliance/
```
Expected: cwd termina em `temperodemamae`, `index.html` listado, diretório `docs/compliance/` existe. Se não existir, rodar `mkdir -p docs/compliance`.

- [ ] **Step 2: Criar o arquivo compliance doc**

Criar `docs/compliance/landing-page-compliance.md` com o seguinte conteúdo (arquivo completo):

```markdown
# Landing Page Compliance

> Referência curada do [agnostic-core](https://github.com/paulinett1508-dev/agnostic-core).
> Contém apenas o que se aplica a landing pages estáticas (HTML puro + CSS + JS vanilla).
> Atualizar quando o agnostic-core evoluir ou quando novas decisões forem tomadas.

---

## 1. HTML/CSS Governance

### Estrutura semântica obrigatória

```html
<body>
  <a href="#conteudo-principal" class="skip-link">Pular para conteúdo</a>
  <header>
    <nav aria-label="Navegação principal">...</nav>
  </header>
  <main id="conteudo-principal">
    <section id="sobre">...</section>
    <section id="colecao">...</section>
    <section id="conteudo">...</section>
    <section id="depoimentos">...</section>
  </main>
  <footer>...</footer>
</body>
```

- H1 único por página; hierarquia H2 > H3 sem pular nível
- Todo `<section>` com `id` para âncoras de navegação
- Botões para ações (`<button>`), links para navegação (`<a href>`)

### Governança CSS — Red Flags (verificar antes de commitar)

```bash
# Inline styles proibidos
grep -n 'style="' index.html

# !important sem justificativa
grep -n '!important' index.html
```

**Regras:**
- [ ] Cores exclusivamente via `var(--token)` — nunca `#hex` ou `rgb()` direto
- [ ] Zero `style=""` inline no HTML
- [ ] Zero `!important` sem comentário documentando o override
- [ ] Animações via `.reveal` + IntersectionObserver — nunca JS setando `element.style.*`

---

## 2. UX Quality Gates

Gates 1–4 são requisitos mínimos. Gate 5 diferencia experiências premium.

### Gate 1 — Hierarquia Visual
- [ ] Título principal (H1) é o elemento mais proeminente visualmente
- [ ] CTA principal tem contraste alto e posição above-the-fold
- [ ] Sequência lógica: eyebrow → título → subtítulo → descrição → CTA

### Gate 2 — Feedback de Interação
- [ ] Hover visível em todos os elementos interativos
- [ ] Transições entre 150ms e 300ms
- [ ] Estado `:focus-visible` definido e visível — nunca `outline: none` sem substituto

### Gate 3 — Responsividade
- [ ] Layout funcional em 375px (mobile pequeno)
- [ ] Layout funcional em 640px (mobile grande)
- [ ] Layout funcional em 1024px (tablet/desktop pequeno)
- [ ] Nenhum elemento com largura fixa causando scroll horizontal
- [ ] Nav com fallback mobile — jamais `display: none` sem menu substituto

### Gate 4 — Acessibilidade (WCAG 2.1 AA)
- [ ] Skip link como primeiro elemento focável
- [ ] `<nav aria-label="Navegação principal">`
- [ ] `aria-hidden="true"` em todos os elementos decorativos
- [ ] Emojis informativos com `role="img" aria-label="descrição"`
- [ ] `prefers-reduced-motion` respeita preferência do usuário

### Gate 5 — Design Emocional (diferenciador)
- [ ] Micro-interações consistentes com identidade premium (preto/dourado)
- [ ] Tipografia distintiva respeitada (Cinzel, Cormorant, Lato)
- [ ] Grain overlay preservado no `body::before`

---

## 3. SEO & Core Web Vitals

### Meta tags obrigatórias (todo deploy)

```html
<meta name="description" content="[150-160 caracteres com keyword]">
<link rel="canonical" href="https://[domínio]/">

<!-- Open Graph -->
<meta property="og:title" content="[título]">
<meta property="og:description" content="[descrição]">
<meta property="og:type" content="website">
<meta property="og:url" content="https://[domínio]/">
<!-- og:image — adicionar quando houver imagem de capa -->
```

### Performance de carregamento

```html
<!-- Preconnect ANTES do stylesheet de fontes -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

### Core Web Vitals — metas

| Métrica | Meta | Limite |
|---|---|---|
| LCP | < 2.5s | < 4s |
| INP | < 200ms | < 500ms |
| CLS | < 0.1 | < 0.25 |

### Schema JSON-LD (produto digital)

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Coleção Arte dos Temperos",
  "description": "8 volumes digitais sobre temperos artesanais...",
  "brand": { "@type": "Brand", "name": "Arte dos Temperos" }
}
</script>
```

### Checklist rápido de SEO
- [ ] `<title>` entre 50–60 caracteres com keyword primária
- [ ] `<meta name="description">` entre 150–160 caracteres
- [ ] H1 único, H2/H3 hierárquicos
- [ ] `<link rel="canonical">` presente
- [ ] Open Graph completo (title, description, type, url)
- [ ] Schema JSON-LD válido

---

## 4. Acessibilidade WCAG 2.1 AA

### Contraste mínimo

| Tipo | Mínimo |
|---|---|
| Texto normal (< 18px ou < 14px bold) | 4.5:1 |
| Texto grande (≥ 18px ou ≥ 14px bold) | 3:1 |

### Estrutura de foco

```css
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  background: var(--gold);
  color: var(--black);
  padding: 10px 20px;
  font-family: 'Lato', sans-serif;
  font-size: 14px;
  font-weight: 700;
  z-index: 10000;
  text-decoration: none;
  transition: top 0.2s;
}
.skip-link:focus { top: 0; }

:focus-visible {
  outline: 2px solid var(--gold);
  outline-offset: 3px;
}
```

### Emojis: informativo vs. decorativo

| Emoji | Contexto | Tratamento |
|---|---|---|
| 🌿🫙🇧🇷❄️💰🚀👨‍🍳🏷️ | `book-icon` (8 cards) | `role="img" aria-label="[descrição]"` |
| 🧪📦📊🏪🎨⭐ | `topic-icon` (6 cards) | `role="img" aria-label="[descrição]"` |
| 🫙 | `about-icon` | `role="img" aria-label="Potes de tempero artesanal"` |
| ✦ | ornamento CTA | `aria-hidden="true"` |
| " | aspas depoimentos | `aria-hidden="true"` |

### Hamburger menu acessível (mobile)

```html
<button class="nav-hamburger" aria-expanded="false" aria-label="Abrir menu">
  <span></span><span></span><span></span>
</button>
```

```js
function openMenu() {
  mobileMenu.classList.add('open');
  hamburger.setAttribute('aria-expanded', 'true');
  hamburger.setAttribute('aria-label', 'Fechar menu');
}
function closeMenu() {
  mobileMenu.classList.remove('open');
  hamburger.setAttribute('aria-expanded', 'false');
  hamburger.setAttribute('aria-label', 'Abrir menu');
}
document.addEventListener('keydown', e => { if (e.key === 'Escape') closeMenu(); });
document.querySelectorAll('.nav-mobile a').forEach(a => a.addEventListener('click', closeMenu));
```

---

## 5. Pré-deploy Checklist

### Quick check (durante desenvolvimento)

- [ ] Zero inline styles: `grep -n 'style="' index.html`
- [ ] `:focus-visible` presente: `grep -n 'focus-visible' index.html`
- [ ] `prefers-reduced-motion` presente: `grep -n 'prefers-reduced-motion' index.html`
- [ ] Skip link presente: `grep -n 'skip-link' index.html`

### Full check (antes de push para main)

- [ ] Meta tags completas (description, canonical, OG)
- [ ] Schema JSON-LD válido (testar em https://search.google.com/test/rich-results)
- [ ] Layout visual testado em 375px, 640px, 1024px no DevTools
- [ ] Nav mobile funciona (hamburger abre/fecha, fecha com Escape)
- [ ] Navegação por teclado funciona (Tab percorre todos interativos, skip link funciona)
- [ ] Push na `main` → verificar deploy automático no Vercel
```

- [ ] **Step 3: Verificar que o arquivo tem 5 seções**

```bash
grep -c "^## " docs/compliance/landing-page-compliance.md
```
Expected: `5`

- [ ] **Step 4: Commit**

```bash
git add docs/compliance/landing-page-compliance.md
git commit -m "docs: adiciona compliance doc curado do agnostic-core"
```

---

## Task 2: Fixes C1, C2, C3 — Meta description + Landmarks

**Files:**
- Modify: `index.html`

> **Nota de retomada:** se o Step 1 encontrar resultados, esta task pode já ter sido executada parcialmente. Verificar o estado antes de continuar.

- [ ] **Step 1: Verificar estado do arquivo**

```bash
grep -n "meta name=\"description\"\|<main\|<header>" index.html
```
Expected (arquivo original): zero resultados. Se encontrar resultados, verificar se task já foi executada antes de prosseguir.

- [ ] **Step 2: Adicionar meta description no `<head>`**

Localizar a linha `<meta name="viewport" content="width=device-width, initial-scale=1.0">` e inserir após ela:

```html
<meta name="description" content="8 volumes digitais com receitas, técnicas, precificação e branding para transformar seu talento culinário em renda real. Coleção Arte dos Temperos.">
```

- [ ] **Step 3: Adicionar skip link, `<header>` e `<main>` no body**

Localizar a linha `<body>` e substituir o bloco de abertura até o primeiro `<section class="hero">`:

```html
<!-- ANTES -->
<body>

<!-- NAV -->
<nav>
  <span class="nav-brand">Arte dos Temperos</span>
  <ul class="nav-links">

<!-- DEPOIS -->
<body>
<a href="#conteudo-principal" class="skip-link">Pular para conteúdo</a>

<header>
  <nav aria-label="Navegação principal">
    <span class="nav-brand">Arte dos Temperos</span>
    <ul class="nav-links">
```

Localizar `</ul>` e `</nav>` do bloco de nav e fechar o `<header>` após o `</nav>`:

```html
  </ul>
  </nav>
</header>

<main id="conteudo-principal">

<!-- HERO -->
<section class="hero">
```

Localizar o fechamento da section CTA (`</section>` após o bloco `<!-- CTA -->`) e fechar o `<main>` após ele:

```html
</section><!-- fim CTA -->

</main>

<!-- FOOTER -->
<footer>
```

- [ ] **Step 4: Verificar estrutura semântica**

```bash
grep -n "<header>" index.html
grep -n "</header>" index.html
grep -n '<main id="conteudo-principal">' index.html
grep -n "</main>" index.html
```
Expected: cada comando retorna exatamente 1 linha.

- [ ] **Step 5: Verificar meta description**

```bash
grep -n "meta name=\"description\"" index.html
```
Expected: 1 linha com `<meta name="description" content="8 volumes...`

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "fix(a11y): landmarks header/main, skip link, meta description (C1-C3)"
```

---

## Task 3: Fix C4 — Hamburger menu para mobile

**Files:**
- Modify: `index.html` (CSS na `<style>` + HTML dentro do `<nav>` + JS no `<script>`)

- [ ] **Step 1: Verificar que nav some no mobile atualmente**

```bash
grep -n "nav-links" index.html | grep -i "display"
```
Expected: linha com `display: none` no breakpoint 640px

- [ ] **Step 2: Adicionar CSS do hamburger e menu mobile**

Localizar o comentário `/* ── RESPONSIVE ── */` no CSS e inserir antes dele:

```css
/* ── HAMBURGER ── */
.nav-hamburger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 8px;
  min-width: 44px;
  min-height: 44px;
  align-items: center;
  justify-content: center;
}

.nav-hamburger span {
  display: block;
  width: 22px;
  height: 2px;
  background: var(--gold);
  transition: all 0.3s ease;
}

.nav-hamburger[aria-expanded="true"] span:nth-child(1) {
  transform: translateY(7px) rotate(45deg);
}
.nav-hamburger[aria-expanded="true"] span:nth-child(2) {
  opacity: 0;
}
.nav-hamburger[aria-expanded="true"] span:nth-child(3) {
  transform: translateY(-7px) rotate(-45deg);
}

/* ── NAV MOBILE MENU ── */
.nav-mobile {
  display: none;
  position: fixed;
  top: 0; left: 0; right: 0; bottom: 0;
  background: rgba(10,10,10,0.97);
  z-index: 98;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 40px;
  list-style: none;
  padding: 0;
  margin: 0;
}

.nav-mobile.open { display: flex; }

.nav-mobile a {
  font-family: 'Cinzel', serif;
  font-size: 20px;
  letter-spacing: 5px;
  text-transform: uppercase;
  color: var(--white);
  text-decoration: none;
  transition: color 0.3s;
}

.nav-mobile a:hover { color: var(--gold); }
```

No bloco `@media (max-width: 640px)`, adicionar:

```css
  .nav-hamburger { display: flex; }
```

- [ ] **Step 3: Adicionar botão hamburger dentro do `<nav>` e menu mobile dentro do `<nav>`**

Localizar `</ul>` do `.nav-links` (dentro do `<nav>`) e adicionar o botão e o menu mobile logo após, ainda dentro do `<nav>`:

```html
  </ul>
  <button class="nav-hamburger" aria-expanded="false" aria-label="Abrir menu">
    <span></span>
    <span></span>
    <span></span>
  </button>
  <ul class="nav-mobile">
    <li><a href="#sobre">Sobre</a></li>
    <li><a href="#colecao">Coleção</a></li>
    <li><a href="#conteudo">Conteúdo</a></li>
    <li><a href="#depoimentos">Depoimentos</a></li>
  </ul>
</nav>
```

- [ ] **Step 4: Adicionar JS do hamburger no `<script>`**

Localizar a linha `// Scroll reveal` no bloco `<script>` e inserir antes dela:

```js
// Hamburger menu
const hamburger = document.querySelector('.nav-hamburger');
const mobileMenu = document.querySelector('.nav-mobile');

function openMenu() {
  mobileMenu.classList.add('open');
  hamburger.setAttribute('aria-expanded', 'true');
  hamburger.setAttribute('aria-label', 'Fechar menu');
}

function closeMenu() {
  mobileMenu.classList.remove('open');
  hamburger.setAttribute('aria-expanded', 'false');
  hamburger.setAttribute('aria-label', 'Abrir menu');
}

hamburger.addEventListener('click', () => {
  hamburger.getAttribute('aria-expanded') === 'true' ? closeMenu() : openMenu();
});

document.addEventListener('keydown', e => {
  if (e.key === 'Escape') closeMenu();
});

document.querySelectorAll('.nav-mobile a').forEach(a => {
  a.addEventListener('click', closeMenu);
});
```

- [ ] **Step 5: Verificar botão e menu mobile presentes, e ausência de conflito com nav-links**

```bash
grep -n "nav-hamburger\|nav-mobile\|aria-expanded" index.html
```
Expected: pelo menos 4 linhas

```bash
# Confirmar que nav-mobile é classe nova (não conflita com nav-links)
grep -n "\.nav-links\|\.nav-mobile" index.html | head -10
```
Expected: `.nav-links` tem `display: none` no breakpoint 640px; `.nav-mobile` não deve ter `display: none` nesse mesmo breakpoint — apenas `display: flex` quando `.open`. São classes distintas sem conflito.

- [ ] **Step 6: [VERIFICAÇÃO HUMANA] Testar hamburger no browser**

Abrir `index.html` no browser com DevTools → viewport 375px:
- Hamburger deve aparecer; `.nav-links` deve sumir
- Clicar hamburger → menu abre (tela cheia escura com links)
- Pressionar Escape → menu fecha
- Clicar âncora (ex: "Coleção") → menu fecha e scroll vai para a seção

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "feat(mobile): hamburger menu acessível para mobile (C4)"
```

---

## Task 4: Fix C5, C6 — Skip link CSS + :focus-visible

**Files:**
- Modify: `index.html` (CSS na `<style>`)

> O HTML do skip link já foi inserido na Task 2 Step 3. Esta task adiciona apenas o CSS.

- [ ] **Step 1: Verificar que skip link está no HTML**

```bash
grep -n "skip-link" index.html
```
Expected: pelo menos 1 linha com `<a href="#conteudo-principal" class="skip-link">`

- [ ] **Step 2: Adicionar CSS do skip link e :focus-visible**

Localizar a linha `html { scroll-behavior: smooth; }` no CSS e inserir após ela:

```css
/* ── SKIP LINK ── */
.skip-link {
  position: absolute;
  top: -100%;
  left: 0;
  background: var(--gold);
  color: var(--black);
  padding: 10px 20px;
  font-family: 'Lato', sans-serif;
  font-size: 14px;
  font-weight: 700;
  letter-spacing: 1px;
  z-index: 10000;
  text-decoration: none;
  transition: top 0.2s;
}

.skip-link:focus {
  top: 0;
}

/* ── FOCUS VISIBLE ── */
:focus-visible {
  outline: 2px solid var(--gold);
  outline-offset: 3px;
}
```

- [ ] **Step 3: Verificar CSS adicionado**

```bash
grep -n "skip-link\|focus-visible" index.html
```
Expected: pelo menos 3 linhas (`.skip-link`, `.skip-link:focus`, `:focus-visible`)

- [ ] **Step 4: [VERIFICAÇÃO HUMANA] Testar navegação por teclado**

Abrir a página no browser:
- Pressionar Tab: skip link dourado deve aparecer no topo
- Pressionar Enter: scroll vai para `#conteudo-principal`
- Continuar Tab: outline dourado deve aparecer em cada elemento interativo

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "fix(a11y): skip link CSS e :focus-visible (C5-C6)"
```

---

## Task 5: Fix C7 — prefers-reduced-motion

**Files:**
- Modify: `index.html` (CSS na `<style>`)

- [ ] **Step 1: Verificar ausência**

```bash
grep -n "prefers-reduced-motion" index.html
```
Expected: zero resultados

- [ ] **Step 2: Adicionar media query no bloco de animações**

Localizar o comentário `/* ── ANIMATIONS ── */` no CSS. Após o último `@keyframes` e antes do comentário `/* ── RESPONSIVE ── */`, inserir:

```css
/* ── REDUCED MOTION ── */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }

  .reveal {
    opacity: 1;
    transform: none;
    transition: none;
  }

  html { scroll-behavior: auto; }
}
```

- [ ] **Step 3: Verificar adição**

```bash
grep -n "prefers-reduced-motion" index.html
```
Expected: 1 linha com o media query

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "fix(a11y): prefers-reduced-motion para animações (C7)"
```

---

## Task 6: Fixes C8, C9, C10 — Inline styles + JS nav + brand link

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Verificar inline styles presentes**

```bash
grep -n 'style="' index.html
```
Expected: 4 linhas. Anotar o conteúdo de cada uma para identificação — as strings de contexto que identificam cada linha são:
- `gold-line gold-line-left" style="margin-bottom: 28px`
- `gold-line" style="margin: 20px auto 24px`
- `gold-line gold-line-left" style="margin: 20px 0 24px`
- `gold-line" style="margin: 20px auto 0`

- [ ] **Step 2: Adicionar classes CSS utilitárias para os gold-lines**

Localizar o bloco `/* ── GOLD LINE ── */` no CSS e adicionar após os estilos existentes:

```css
.gold-line--about      { margin-bottom: 28px; margin-top: 8px; }
.gold-line--collection { margin: 20px auto 24px; }
.gold-line--topics     { margin: 20px 0 24px; }
.gold-line--testi      { margin: 20px auto 0; }
```

- [ ] **Step 3: Substituir inline styles pelas classes utilitárias**

Localizar cada ocorrência pela string de contexto COMPLETA que inclui a linha anterior para desambiguar:

**Ocorrência 1** (dentro de `.about-text`, após `<!-- ABOUT -->`):
```html
<!-- Contexto da linha anterior: <div class="gold-line gold-line-left" style="margin-bottom -->
<!-- Encontrar (string única desta ocorrência): -->
class="gold-line gold-line-left" style="margin-bottom: 28px; margin-top: 8px;"
<!-- Substituir por: -->
class="gold-line gold-line-left gold-line--about"
```

**Ocorrência 2** (dentro de `.collection-header`, após `<!-- COLLECTION -->`):
```html
<!-- Encontrar (string única): -->
class="gold-line" style="margin: 20px auto 24px;"
<!-- Substituir por: -->
class="gold-line gold-line--collection"
```

**Ocorrência 3** (dentro de `.topics-sticky`, após `<!-- TOPICS -->`):
```html
<!-- Contexto da linha anterior: <div class="gold-line gold-line-left" style="margin: 20px 0 -->
<!-- Encontrar (string única desta ocorrência): -->
class="gold-line gold-line-left" style="margin: 20px 0 24px;"
<!-- Substituir por: -->
class="gold-line gold-line-left gold-line--topics"
```

**Ocorrência 4** (dentro de `.testimonials-header`, após `<!-- TESTIMONIALS -->`):
```html
<!-- Encontrar (string única): -->
class="gold-line" style="margin: 20px auto 0;"
<!-- Substituir por: -->
class="gold-line gold-line--testi"
```

> **Verificação de desambiguação:** As strings de `style="margin-bottom: 28px"` e `style="margin: 20px 0"` são únicas no arquivo — grep retorna exatamente 1 resultado cada. As strings `style="margin: 20px auto 24px"` e `style="margin: 20px auto 0"` também são únicas. Confirmar com `grep -c 'style="margin' index.html` antes de editar — Expected: `4`.

- [ ] **Step 4: Verificar zero inline styles no HTML**

```bash
grep -n 'style="' index.html
```
Expected: zero resultados

- [ ] **Step 5: Substituir JS inline style por classList**

Localizar no bloco `<script>` a linha que contém `a.style.color`. Substituir o bloco `navLinks.forEach(...)`:

```js
// ANTES:
navLinks.forEach(a => {
  a.style.color = a.getAttribute('href') === '#' + current
    ? 'var(--gold)'
    : 'var(--white-soft)';
});

// DEPOIS:
navLinks.forEach(a => {
  a.classList.toggle('nav-active', a.getAttribute('href') === '#' + current);
});
```

Adicionar no CSS (em `/* ── NAV ── */`):

```css
.nav-links a.nav-active { color: var(--gold); }
```

- [ ] **Step 6: Verificar que `a.style.color` foi removido**

```bash
grep -n "style\.color\|style\[" index.html
```
Expected: zero resultados

- [ ] **Step 7: Trocar brand do nav de `<span>` para `<a>`**

Localizar a string `<span class="nav-brand">Arte dos Temperos</span>` e substituir por:

```html
<a href="#" class="nav-brand" aria-label="Arte dos Temperos — início">Arte dos Temperos</a>
```

Adicionar no CSS, após `.nav-brand { ... }`:

```css
a.nav-brand { text-decoration: none; }
```

- [ ] **Step 8: Verificar brand como link**

```bash
grep -n "nav-brand" index.html | head -5
```
Expected: `<a href="#" class="nav-brand"...`

- [ ] **Step 9: Commit**

```bash
git add index.html
git commit -m "fix(css): remove inline styles, JS classList nav, brand como link (C8-C10)"
```

---

## Task 7: Fixes A1, A2, A3 — Canonical + Open Graph + Schema JSON-LD

**Files:**
- Modify: `index.html` (bloco `<head>`)

- [ ] **Step 1: Verificar ausência**

```bash
grep -n "canonical\|og:title\|ld+json" index.html
```
Expected: zero resultados

- [ ] **Step 2: Adicionar canonical, Open Graph e Schema logo após a tag `<title>`**

Localizar a linha `<title>Arte dos Temperos...</title>` e inserir o bloco após ela:

```html
<!-- SEO -->
<link rel="canonical" href="https://temperodemamae.vercel.app/">

<!-- Open Graph -->
<meta property="og:title" content="Arte dos Temperos — Transforme sua Cozinha em Renda">
<meta property="og:description" content="8 volumes digitais com receitas, técnicas, precificação e branding para transformar seu talento culinário em renda real.">
<meta property="og:type" content="website">
<meta property="og:url" content="https://temperodemamae.vercel.app/">
<!-- og:image: adicionar quando houver imagem de capa
<meta property="og:image" content="https://temperodemamae.vercel.app/assets/img/og-cover.jpg">
-->

<!-- Schema JSON-LD -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Coleção Arte dos Temperos",
  "description": "8 volumes digitais sobre temperos artesanais, molhos, técnicas de conservação, precificação e branding para negócio caseiro.",
  "brand": {
    "@type": "Brand",
    "name": "Arte dos Temperos"
  },
  "offers": {
    "@type": "Offer",
    "availability": "https://schema.org/InStock",
    "priceCurrency": "BRL"
  }
}
</script>
```

- [ ] **Step 3: Verificar adição**

```bash
grep -n "canonical\|og:title\|ld+json" index.html
```
Expected: 3 linhas

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat(seo): canonical, Open Graph e Schema JSON-LD (A1-A3)"
```

---

## Task 8: Fixes A4, A5, A6 — preconnect + aria-hidden decorativos

**Files:**
- Modify: `index.html`

> `aria-label` no `<nav>` foi adicionado na Task 2 Step 3. Esta task verifica e adiciona o restante.

- [ ] **Step 1: Verificar estado acumulado após Tasks 2–6**

```bash
grep -n "preconnect\|aria-label\|aria-hidden" index.html
```
Expected: pelo menos 3 linhas — `aria-label="Navegação principal"` (Task 2), `aria-label="Abrir menu"` (Task 3), `aria-expanded` (Task 3). Se `aria-label` do nav não aparecer, adicioná-lo agora: `<nav aria-label="Navegação principal">`.

- [ ] **Step 2: Adicionar preconnect para Google Fonts**

Localizar a linha `<link href="https://fonts.googleapis.com/css2?family=Cormorant..."` e inserir ANTES dela:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
```

- [ ] **Step 3: Adicionar aria-hidden nos elementos decorativos**

Localizar cada elemento pela string de contexto e adicionar `aria-hidden="true"`:

```html
<!-- hero-ornament -->
<!-- Encontrar: <div class="hero-ornament"></div> -->
<!-- Substituir: -->
<div class="hero-ornament" aria-hidden="true"></div>

<!-- hero-scroll -->
<!-- Encontrar: <div class="hero-scroll">Scroll</div> -->
<!-- Substituir: -->
<div class="hero-scroll" aria-hidden="true">Scroll</div>

<!-- 3x testi-quote (nos 3 cards de depoimento) -->
<!-- Encontrar: <span class="testi-quote">"</span> -->
<!-- Substituir (todas as ocorrências): -->
<span class="testi-quote" aria-hidden="true">"</span>

<!-- cta-ornament-symbol -->
<!-- Encontrar: <span class="cta-ornament-symbol">✦</span> -->
<!-- Substituir: -->
<span class="cta-ornament-symbol" aria-hidden="true">✦</span>
```

- [ ] **Step 4: Verificar aria-hidden adicionado**

```bash
grep -c 'aria-hidden="true"' index.html
```
Expected: 6 (hero-ornament + hero-scroll + 3x testi-quote + cta-ornament-symbol)

- [ ] **Step 5: Verificar preconnect**

```bash
grep -n "preconnect" index.html
```
Expected: 2 linhas (googleapis e gstatic)

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "fix(a11y): preconnect fontes, aria-hidden em decorativos (A4-A6)"
```

---

## Task 9: Fixes M1, M2, M3 — About id + emojis aria + og:image placeholder

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Verificar og:image placeholder (deve vir da Task 7)**

```bash
grep -n "og:image" index.html
```
Expected: 1 linha comentada com o placeholder (M3 já resolvida)

- [ ] **Step 2: Adicionar id="sobre" na section About**

Localizar a string `<!-- ABOUT -->` e a `<section>` logo abaixo. Substituir:

```html
<!-- ANTES -->
<!-- ABOUT -->
<section>
  <div class="about">

<!-- DEPOIS -->
<!-- ABOUT -->
<section id="sobre">
  <div class="about">
```

- [ ] **Step 3: Adicionar link para #sobre no nav desktop**

Localizar o `<ul class="nav-links">` e adicionar o item "Sobre" como primeiro link:

```html
<ul class="nav-links">
  <li><a href="#sobre">Sobre</a></li>
  <li><a href="#colecao">Coleção</a></li>
  <li><a href="#conteudo">Conteúdo</a></li>
  <li><a href="#depoimentos">Depoimentos</a></li>
</ul>
```

- [ ] **Step 4: Adicionar role="img" e aria-label nos emojis informativos**

Localizar cada emoji pelo contexto de classe e substituir:

```html
<!-- about-icon -->
<span class="about-icon" role="img" aria-label="Potes de tempero artesanal">🫙</span>

<!-- book-icons (8 cards, na ordem dos volumes) -->
<span class="book-icon" role="img" aria-label="Ervas e temperos">🌿</span>
<span class="book-icon" role="img" aria-label="Pote de molho">🫙</span>
<span class="book-icon" role="img" aria-label="Bandeira do Brasil">🇧🇷</span>
<span class="book-icon" role="img" aria-label="Congelamento">❄️</span>
<span class="book-icon" role="img" aria-label="Dinheiro e lucratividade">💰</span>
<span class="book-icon" role="img" aria-label="Crescimento do negócio">🚀</span>
<span class="book-icon" role="img" aria-label="Chef de cozinha">👨‍🍳</span>
<span class="book-icon" role="img" aria-label="Etiqueta e rótulo">🏷️</span>

<!-- topic-icons (6 cards) -->
<span class="topic-icon" role="img" aria-label="Receitas e fórmulas">🧪</span>
<span class="topic-icon" role="img" aria-label="Embalagem e conservação">📦</span>
<span class="topic-icon" role="img" aria-label="Gráfico de precificação">📊</span>
<span class="topic-icon" role="img" aria-label="Loja e vendas">🏪</span>
<span class="topic-icon" role="img" aria-label="Design e identidade visual">🎨</span>
<span class="topic-icon" role="img" aria-label="Qualidade premium">⭐</span>
```

- [ ] **Step 5: Verificar id="sobre"**

```bash
grep -n 'id="sobre"' index.html
```
Expected: 1 linha

- [ ] **Step 6: Verificar contagem total de emojis com role e detectar retomada parcial**

```bash
# Total de emojis com role="img" (deve ser exatamente 15)
grep -c 'role="img"' index.html
```
Expected: `15` (1 about-icon + 8 book-icons + 6 topic-icons)

Se o resultado for entre 1 e 14 (retomada parcial), verificar quais categorias ainda faltam:

```bash
# Quantos book-icons têm role="img" (deve ser 8)
grep -c 'book-icon.*role="img"\|role="img".*book-icon' index.html

# Quantos topic-icons têm role="img" (deve ser 6)
grep -c 'topic-icon.*role="img"\|role="img".*topic-icon' index.html

# O about-icon tem role="img" (deve ser 1)
grep -c 'about-icon.*role="img"\|role="img".*about-icon' index.html
```

Aplicar apenas as substituições que ainda não foram feitas, verificando elemento a elemento.

- [ ] **Step 7: Commit**

```bash
git add index.html
git commit -m "fix(a11y): aria em emojis informativos, id=sobre na section About (M1-M3)"
```

---

## Task 10: Atualizar CLAUDE.md com sistema de 5 gates

**Files:**
- Modify: `CLAUDE.md`

- [ ] **Step 1: Verificar seções a substituir**

```bash
grep -n "Sempre fazer\|Nunca fazer\|Regras de Desenvolvimento" CLAUDE.md
```
Expected: 3 linhas — as seções que serão removidas/substituídas

- [ ] **Step 2: Substituir o bloco `## Regras de Desenvolvimento` pelo sistema de gates**

Localizar a seção que começa em `## Regras de Desenvolvimento` e termina antes de `## Próximas Features Planejadas`. Substituir completamente por:

```markdown
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
```

- [ ] **Step 3: Verificar substituição**

```bash
grep -n "Gates de Qualidade\|Gate 1\|Gate 2\|Gate 3\|Gate 4\|Gate 5" CLAUDE.md
```
Expected: 6 linhas

```bash
grep -n "Sempre fazer\|Nunca fazer" CLAUDE.md
```
Expected: zero resultados

- [ ] **Step 4: Commit**

```bash
git add CLAUDE.md
git commit -m "chore: substitui regras por sistema de 5 gates baseado no agnostic-core"
```

---

## Task 11: Verificação Final

- [ ] **Step 1: Zero inline styles**

```bash
grep -n 'style="' index.html
```
Expected: zero resultados

- [ ] **Step 2: Todos os landmarks presentes**

```bash
grep -n "<header>\|</header>\|<main\|</main>" index.html
```
Expected: 4 linhas (1 abertura + 1 fechamento para cada)

- [ ] **Step 3: Meta SEO completos**

```bash
grep -n "meta name=\"description\"\|canonical\|og:title\|ld+json\|preconnect" index.html
```
Expected: pelo menos 5 linhas

- [ ] **Step 4: Smoke test de acessibilidade**

```bash
grep -n "aria-hidden\|aria-label\|role=\"img\"\|focus-visible\|skip-link\|prefers-reduced-motion" index.html | wc -l
```
Expected: 20 ou mais linhas

- [ ] **Step 5: Compliance doc com 5 seções**

```bash
grep -c "^## " docs/compliance/landing-page-compliance.md
```
Expected: `5`

- [ ] **Step 6: CLAUDE.md com 5 gates**

```bash
grep -c "^### Gate" CLAUDE.md
```
Expected: `5`

- [ ] **Step 7: Push para main**

```bash
git push origin main
```

- [ ] **Step 8: [VERIFICAÇÃO HUMANA] Verificar deploy no Vercel**

Aguardar deploy automático no Vercel dashboard e confirmar que a página abre corretamente em produção. Verificar responsividade em mobile.
