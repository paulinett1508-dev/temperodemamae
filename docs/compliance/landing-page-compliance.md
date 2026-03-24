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

### Quick Accessibility Checklist

- [ ] Contraste mínimo 4.5:1 para texto normal — verificar com https://webaim.org/resources/contrastchecker/
- [ ] Contraste mínimo 3:1 para texto grande e componentes UI
- [ ] Skip link presente e funcional (Tab → link aparece no topo)
- [ ] `:focus-visible` definido para todos os elementos interativos
- [ ] `<nav aria-label="Navegação principal">` presente
- [ ] `aria-hidden="true"` em todos os elementos decorativos
- [ ] Emojis informativos com `role="img" aria-label="descrição"`
- [ ] `prefers-reduced-motion` respeitado nas animações
- [ ] Hamburger com `aria-expanded`, fecha com Escape e ao clicar âncora

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
