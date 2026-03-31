# Code Review Audit — Tempero de Mamãe

**Data:** 2026-03-31
**Escopo:** `index.html` (1534 linhas — HTML + CSS inline + JS inline)
**Referências:** CLAUDE.md (Quality Gates 1–5), `docs/compliance/landing-page-compliance.md`
**Branch:** `claude/audit-system-code-review-0mpfQ`

---

## Resumo Executivo

| Severidade | Quantidade |
|------------|------------|
| Critical   | 0          |
| High       | 5          |
| Medium     | 6          |
| Low        | 7          |

**Veredito:** Código bem estruturado com forte base de acessibilidade e semântica HTML. Os principais gaps estão concentrados em SEO/Performance (Gate 5) e completude de conteúdo. Nenhuma vulnerabilidade crítica de segurança encontrada.

---

## Pontos Positivos

| # | Aspecto | Detalhes |
|---|---------|----------|
| 1 | HTML semântico | Landmarks corretos (`header`, `main`, `footer`), `aria-labelledby` nas seções, H1 único, hierarquia sequencial |
| 2 | Acessibilidade | Skip link (L916), ARIA em nav/botões, `aria-expanded` no toggle mobile, focus trap no modal, navegação por teclado (setas, Escape, +/- zoom), `prefers-reduced-motion` |
| 3 | Design Tokens | Sistema completo de CSS custom properties via `:root` (L32-66), todas as cores via `var(--token)` |
| 4 | Zero inline styles | Nenhum `style=""` encontrado no HTML |
| 5 | Zero `!important` | Governança CSS limpa |
| 6 | SEO | `meta description`, `canonical`, Open Graph, JSON-LD (WebSite) |
| 7 | Fontes | Cinzel, Cormorant Garamond, Lato carregadas corretamente com `font-display:swap` |
| 8 | Preconnect | `fonts.googleapis.com`, `fonts.gstatic.com`, `cdnjs.cloudflare.com` |
| 9 | Segurança | Sem event handlers inline, sem `eval()`, sem `innerHTML` com dados do usuário (comentado explicitamente L1217) |
| 10 | Nav mobile | Hamburger toggle com atualização dinâmica de ARIA, fecha ao clicar em link |
| 11 | PDF Reader | Leitor Kindle-style completo: loading/error states, paginação, zoom, swipe touch, focus trap |

---

## Findings

### HIGH

---

### H-01 — og:image referencia arquivo inexistente

**Severidade:** HIGH
**Localização:** `index.html` L13
**Gate:** Gate 5 — SEO & Performance

**Descrição:** A meta tag `og:image` aponta para `assets/img/og-cover.jpg`, mas o diretório `assets/img/` não existe no repositório.

**Impacto:** Previews em redes sociais (Facebook, LinkedIn, WhatsApp) não exibirão imagem.

**Recomendação:** Remover a meta tag até que o asset seja criado. Quando disponível, criar imagem 1200×630px.

---

### H-02 — PDF.js carregado sincronamente

**Severidade:** HIGH
**Localização:** `index.html` L27
**Gate:** Gate 5 — SEO & Performance

**Descrição:** Script PDF.js (~108KB) carregado sem `defer` ou `async`, bloqueando o parsing do HTML e o first paint.

**Impacto:** Todos os visitantes pagam o custo de carregamento, mesmo que nunca abram o leitor.

**Recomendação:** Adicionar atributo `defer` ao script tag.

---

### H-03 — Sem SRI hash no script CDN

**Severidade:** HIGH
**Localização:** `index.html` L27
**Gate:** N/A — Segurança (best practice)

**Descrição:** O script `pdf.min.js` carregado via CDN não possui atributo `integrity` (Subresource Integrity) nem `crossorigin`.

**Impacto:** Se o CDN for comprometido, código malicioso pode executar no contexto da página.

**Recomendação:** Adicionar `integrity="sha384-..."` e `crossorigin="anonymous"`.

---

### H-04 — 2 dos 8 PDFs ausentes da interface

**Severidade:** HIGH
**Localização:** `index.html` L990-1097
**Gate:** N/A — Completude de conteúdo

**Descrição:** A página exibe apenas 6 dos 8 PDFs listados no CLAUDE.md. Faltam:
- `masterchef-temperos.pdf` (Segredos dos campeões do MasterChef)
- `rotulos-prontos.pdf` (Rótulos para imprimir)

Ambos os arquivos existem em `docs/PDFs/`.

**Impacto:** Usuários não conseguem acessar 25% do conteúdo da coleção.

**Recomendação:** Adicionar 2 cards adicionais na seção de categorias.

---

### H-05 — Ano do copyright desatualizado

**Severidade:** HIGH
**Localização:** `index.html` L1179
**Gate:** N/A — Manutenção

**Descrição:** Copyright hardcoded como `© 2025`. O ano atual é 2026.

**Impacto:** Aparência de site desatualizado/abandonado.

**Recomendação:** Atualizar para 2026.

---

### MEDIUM

---

### M-01 — `body.style.overflow` manipulado via JS

**Severidade:** MEDIUM
**Localização:** `index.html` L1423, L1431
**Gate:** Gate 2 — Semântica & Governança CSS

**Descrição:** O modal usa `document.body.style.overflow = 'hidden'` e `= ''` para controlar scroll. Isso viola a regra do compliance doc (L47): "nunca JS setando `element.style.*`".

**Recomendação:** Criar classe `.no-scroll { overflow: hidden; }` e usar `classList.add/remove('no-scroll')`.

---

### M-02 — Altura da nav hardcoded em 2 lugares

**Severidade:** MEDIUM
**Localização:** `index.html` L216, L274
**Gate:** Gate 1 — Identidade Visual & Hierarquia

**Descrição:** O valor `64px` da altura da navegação está duplicado no CSS sem ser um design token.

**Recomendação:** Adicionar `--nav-height: 64px` ao `:root` e referenciar `var(--nav-height)`.

---

### M-03 — Sem `<noscript>` fallback

**Severidade:** MEDIUM
**Localização:** `index.html` (ausente)
**Gate:** Gate 3 — Acessibilidade

**Descrição:** O leitor PDF e as animações de reveal dependem de JavaScript, mas não há mensagem para usuários com JS desabilitado.

**Recomendação:** Adicionar `<noscript>` após o skip link.

---

### M-04 — Sem Twitter Card meta tags

**Severidade:** MEDIUM
**Localização:** `index.html` `<head>`
**Gate:** Gate 5 — SEO & Performance

**Descrição:** Faltam `twitter:card`, `twitter:title` e `twitter:description`.

**Impacto:** Previews no Twitter/X serão genéricos.

**Recomendação:** Adicionar as meta tags no `<head>`.

---

### M-05 — Link de erro com `href="#"` padrão

**Severidade:** MEDIUM
**Localização:** `index.html` L1220
**Gate:** Gate 2 — Feedback de Interação

**Descrição:** O link de download de erro do leitor inicia com `href="#"`. Se clicado antes do JS definir o href real, causa scroll indesejado ao topo.

**Recomendação:** Iniciar com `href=""` ou esconder o link via CSS até que o href seja definido.

---

### M-06 — Modal fora do `<main>` (nota arquitetural)

**Severidade:** MEDIUM
**Localização:** `index.html` L1186
**Gate:** N/A

**Descrição:** O modal do leitor está posicionado entre o `<footer>` e o `<script>`. Tecnicamente válido com `aria-modal="true"`, mas posicioná-lo como último filho de `<body>` seria mais claro.

**Recomendação:** Aceitável como está. Documentado para consideração futura.

---

### LOW

---

### L-01 — Google Fonts sem aviso de privacidade

**Severidade:** LOW
**Localização:** `index.html` L26
**Descrição:** Carregamento externo de fontes sem aviso LGPD. Baixo risco para mercado BR.

### L-02 — Sem font preload para caminho crítico de renderização

**Severidade:** LOW
**Localização:** `index.html` `<head>`
**Descrição:** Poderia adicionar `<link rel="preload" as="font">` para a primeira fonte exibida.

### L-03 — Números do stats em `<span>` ao invés de `<data>`

**Severidade:** LOW
**Localização:** `index.html` L961-973
**Descrição:** Usar `<data value="181">` melhoraria a semântica para máquinas.

### L-04 — Infraestrutura de debug em produção

**Severidade:** LOW
**Localização:** `index.html` L1248-1252
**Descrição:** `URLSearchParams` roda em todo page load. Impacto negligível, mas desnecessário.

### L-05 — Entidade `&copy;` vs literal `©`

**Severidade:** LOW
**Localização:** `index.html` L1179
**Descrição:** Puramente cosmético no código-fonte.

### L-06 — `aria-label` do canvas é estático

**Severidade:** LOW
**Localização:** `index.html` L1222
**Descrição:** "Página do documento" não indica número da página. Poderia ser atualizado dinamicamente.

### L-07 — Touch swipe sem prevenção de scroll

**Severidade:** LOW
**Localização:** `index.html` L1494-1512
**Descrição:** Handlers com `passive: true` (correto) mas pode causar scroll indesejado durante swipe.

---

## Matriz de Compliance — Quality Gates

| Gate | Nome | Status | Observações |
|------|------|--------|-------------|
| 1 | Identidade Visual & Hierarquia | PASS | Minor: nav-height não tokenizado (M-02) |
| 2 | Semântica & Governança CSS | PASS com ressalva | `body.style.overflow` viola regra "no JS style" (M-01) |
| 3 | Acessibilidade WCAG 2.1 AA | PASS | Sem `<noscript>` fallback (M-03), canvas label estático (L-06) |
| 4 | Responsividade & Mobile | REQUER VERIFICAÇÃO | Testar em 375px, 640px, 1024px |
| 5 | SEO & Performance | PARCIAL | og:image inexistente (H-01), PDF.js sync (H-02), sem SRI (H-03), sem Twitter cards (M-04) |

---

## Ações Prioritárias

### Quick wins (< 15 min cada)
- H-05: Atualizar copyright para 2026
- H-02: Adicionar `defer` ao PDF.js
- M-01: Trocar `body.style.overflow` por classe CSS
- M-02: Criar variável `--nav-height`
- M-03: Adicionar `<noscript>`
- M-04: Adicionar Twitter Card meta tags
- M-05: Corrigir href padrão do link de erro

### Esforço médio (30-60 min)
- H-03: Gerar e adicionar SRI hash
- H-04: Adicionar 2 cards de PDF faltantes

### Esforço maior (1-2h)
- H-01: Criar asset `og-cover.jpg` (requer design)

---

## Comandos de Verificação

```bash
# Sem inline styles
grep -n 'style="' index.html

# Sem !important não documentado
grep -n '!important' index.html

# Todos os 8 PDFs referenciados
grep -c 'data-pdf' index.html  # esperado: 8

# SRI presente
grep -n 'integrity=' index.html

# Copyright atualizado
grep -n '2026' index.html

# Twitter cards presentes
grep -n 'twitter:' index.html

# Noscript presente
grep -n '<noscript>' index.html

# Verificar og:image removido
grep -n 'og:image' index.html
```
