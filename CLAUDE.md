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
| `--black` | `#0a0a0a` |
| `--gold` | `#c9a84c` |
| `--gold-light` | `#e8c97a` |
| `--white` | `#f5f0e8` |
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

1. **Nav** — fixo no topo, link âncora para cada seção
2. **Hero** — headline, subtítulo, CTA principal
3. **Stats Strip** — 4 números de impacto
4. **Sobre a Coleção** — proposta de valor + lista de benefícios
5. **Os 8 Volumes** — grid com card de cada PDF
6. **O que você vai aprender** — 6 pilares em grid
7. **Depoimentos** — 3 cards com avatar, nome e localidade
8. **CTA Final** — headline + 2 botões
9. **Footer** — marca, tagline, copyright

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

## Regras de Desenvolvimento

### Sempre fazer
- Manter paleta preto/dourado em todos os componentes novos
- Usar `reveal` + IntersectionObserver para animações de entrada
- Testar responsividade nos breakpoints: 640px, 1024px
- Comentar seções com `/* ── NOME DA SEÇÃO ── */`
- Manter grain overlay no `body::before`

### Nunca fazer
- Usar frameworks pesados (React, Vue) — projeto é HTML estático
- Usar cores fora da paleta sem aprovação
- Quebrar o scroll suave entre seções
- Remover as fontes Google (Cinzel, Cormorant Garamond, Lato)

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
