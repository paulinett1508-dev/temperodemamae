# Pending Tasks — Retomar na próxima sessão

**Data de pausa:** 2026-03-24
**Plano completo:** `docs/superpowers/plans/2026-03-24-agnostic-core-compliance.md`
**Spec:** `docs/superpowers/specs/2026-03-24-agnostic-core-compliance-design.md`

---

## Estado atual

| Commit | Descrição |
|---|---|
| `3bd9367` | docs: adiciona checklists na seção de acessibilidade ← HEAD |
| `1b83e07` | docs: adiciona compliance doc curado do agnostic-core |
| `517bfcd` | docs: adiciona plano de implementação |
| `0945f32` | docs: adiciona spec de compliance |

---

## ✅ Concluído

- **Task 1** — `docs/compliance/landing-page-compliance.md` criado, revisado (spec ✅), commit `3bd9367`
  - Ainda falta: code quality review (foi interrompido — pode pular e ir para Task 2, o doc está correto)

---

## 🔲 Pendente — executar com subagent-driven-development

> Usar skill `superpowers:subagent-driven-development` para continuar.
> Plano tem 11 tasks no total. Continuar da **Task 2**.

### Task 2: Fixes C1, C2, C3 — Meta description + Landmarks

Adicionar no `index.html`:
- `<meta name="description" content="8 volumes digitais com receitas, técnicas, precificação e branding para transformar seu talento culinário em renda real. Coleção Arte dos Temperos.">`
- Skip link HTML como primeiro elemento do body: `<a href="#conteudo-principal" class="skip-link">Pular para conteúdo</a>`
- Envolver `<nav>` em `<header>` com fechamento explícito após `</nav>`
- Envolver conteúdo em `<main id="conteudo-principal">` e fechar antes do `<footer>`
- `<nav aria-label="Navegação principal">`

Commit: `fix(a11y): landmarks header/main, skip link, meta description (C1-C3)`

### Task 3: Fix C4 — Hamburger menu mobile

No `index.html`, adicionar CSS + HTML + JS:
- CSS: `.nav-hamburger`, `.nav-mobile`, `.nav-mobile.open` (ver plano para código completo)
- `@media (max-width: 640px)`: `.nav-hamburger { display: flex; }`
- HTML: `<button class="nav-hamburger" aria-expanded="false" aria-label="Abrir menu">` dentro do `<nav>`, após `</ul>`
- HTML: `<ul class="nav-mobile">` com 4 links, ainda dentro do `<nav>`
- JS: funções `openMenu()` / `closeMenu()`, Escape listener, click em âncoras fecha menu

Commit: `feat(mobile): hamburger menu acessível para mobile (C4)`

### Task 4: Fix C5, C6 — Skip link CSS + :focus-visible

No `index.html`, adicionar CSS após `html { scroll-behavior: smooth; }`:
- `.skip-link` com `position: absolute; top: -100%;` e `top: 0` no `:focus`
- `:focus-visible { outline: 2px solid var(--gold); outline-offset: 3px; }`

Commit: `fix(a11y): skip link CSS e :focus-visible (C5-C6)`

### Task 5: Fix C7 — prefers-reduced-motion

No `index.html`, adicionar após os `@keyframes`:
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
  .reveal { opacity: 1; transform: none; transition: none; }
  html { scroll-behavior: auto; }
}
```

Commit: `fix(a11y): prefers-reduced-motion para animações (C7)`

### Task 6: Fixes C8, C9, C10 — Inline styles + JS nav + brand link

No `index.html`:
- Adicionar 4 classes CSS utilitárias: `.gold-line--about`, `.gold-line--collection`, `.gold-line--topics`, `.gold-line--testi`
- Substituir 4 `style=""` inline pelos respectivos modificadores de classe
  - `style="margin-bottom: 28px; margin-top: 8px;"` → `gold-line--about`
  - `style="margin: 20px auto 24px;"` → `gold-line--collection`
  - `style="margin: 20px 0 24px;"` → `gold-line--topics`
  - `style="margin: 20px auto 0;"` → `gold-line--testi`
- No JS: substituir `a.style.color = ...` por `a.classList.toggle('nav-active', ...)`
- Adicionar CSS: `.nav-links a.nav-active { color: var(--gold); }`
- Trocar `<span class="nav-brand">` por `<a href="#" class="nav-brand" aria-label="Arte dos Temperos — início">`
- Adicionar CSS: `a.nav-brand { text-decoration: none; }`

Commit: `fix(css): remove inline styles, JS classList nav, brand como link (C8-C10)`

### Task 7: Fixes A1, A2, A3 — Canonical + Open Graph + Schema JSON-LD

No `index.html`, após `<title>`, adicionar:
```html
<link rel="canonical" href="https://temperodemamae.vercel.app/">
<meta property="og:title" content="Arte dos Temperos — Transforme sua Cozinha em Renda">
<meta property="og:description" content="8 volumes digitais com receitas, técnicas, precificação e branding para transformar seu talento culinário em renda real.">
<meta property="og:type" content="website">
<meta property="og:url" content="https://temperodemamae.vercel.app/">
<!-- og:image: comentado aguardando imagem de capa -->
```
E Schema JSON-LD com `@type: Product` para a coleção.

Commit: `feat(seo): canonical, Open Graph e Schema JSON-LD (A1-A3)`

### Task 8: Fixes A4, A5, A6 — preconnect + aria-hidden decorativos

No `index.html`:
- Adicionar antes do link de fontes: `<link rel="preconnect" href="https://fonts.googleapis.com">` e `<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>`
- Confirmar `aria-label="Navegação principal"` no nav (deve vir da Task 2)
- Adicionar `aria-hidden="true"` em: `.hero-ornament`, `.hero-scroll`, 3x `.testi-quote`, `.cta-ornament-symbol`

Commit: `fix(a11y): preconnect fontes, aria-hidden em decorativos (A4-A6)`

### Task 9: Fixes M1, M2, M3 — About id + emojis aria + og:image

No `index.html`:
- Adicionar `id="sobre"` na `<section>` do About
- Adicionar link `<li><a href="#sobre">Sobre</a></li>` como primeiro item do `.nav-links`
- Adicionar `role="img" aria-label="[descrição]"` nos 15 emojis informativos:
  - 1x `about-icon` (🫙)
  - 8x `book-icon` (🌿🫙🇧🇷❄️💰🚀👨‍🍳🏷️)
  - 6x `topic-icon` (🧪📦📊🏪🎨⭐)
- `og:image` já foi adicionado como comentado na Task 7

Commit: `fix(a11y): aria em emojis informativos, id=sobre na section About (M1-M3)`

### Task 10: Atualizar CLAUDE.md com sistema de 5 gates

No `CLAUDE.md`:
- Substituir a seção `## Regras de Desenvolvimento` (incluindo "Sempre fazer" e "Nunca fazer") por `## Gates de Qualidade` com 5 gates (ver plano para texto completo)
- Gates 1–4 obrigatórios, Gate 5 diferenciador
- Adicionar referência para `docs/compliance/landing-page-compliance.md`

Commit: `chore: substitui regras por sistema de 5 gates baseado no agnostic-core`

### Task 11: Verificação Final

Smoke tests via grep + push:
```bash
grep -n 'style="' index.html           # Expected: zero
grep -n "<header>\|</header>" index.html  # Expected: 2 linhas
grep -c "aria-hidden\|aria-label\|role=\"img\"\|focus-visible\|skip-link\|prefers-reduced-motion" index.html  # Expected: 20+
grep -c "^## " docs/compliance/landing-page-compliance.md  # Expected: 5
grep -c "^### Gate" CLAUDE.md           # Expected: 5
git push origin main
```

---

## Como retomar

1. Abrir nova sessão no projeto `C:/PROJETOS/temperodemamae`
2. Mencionar este arquivo: `docs/superpowers/PENDING_TASKS.md`
3. Pedir para continuar da Task 2 usando subagent-driven-development
4. O plano completo com todos os detalhes está em `docs/superpowers/plans/2026-03-24-agnostic-core-compliance.md`
