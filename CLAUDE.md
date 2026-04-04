# CLAUDE.md — PagCall (Modulo Call Center)

Este arquivo e a instrucao definitiva para qualquer IA trabalhando neste repositorio. Leia integralmente antes de criar ou modificar qualquer arquivo.

---

## O que e este repositorio

Prototipos visuais do modulo **PagCall** (Call Center) da plataforma **pagah**. Sao sugestoes de UI para desenvolvedores implementarem no sistema real. Nao ha backend, APIs, nem integracao — tudo e puramente visual/frontend.

**Deploy:** https://modulo-callcenter-appcall.vercel.app
**Repo pai:** https://github.com/OtavioLinden/Pagah (repositorio principal com todos os prototipos)

---

## Estrutura do Projeto

```
index.html                          -- Hub de navegacao do modulo
pages/
  leads-atendimento.html            -- CRM de leads, filtros, fluxo de atendimento
  gestao-empresas.html              -- Controle global de empresas/lojas (admin)
  gestao-usuarios.html              -- CRUD vendedores, perfis, permissoes
  dashboard-vendedor.html           -- KPIs, gamificacao, ranking, extrato
  dashboard-gerente.html            -- Visao consolidada, metricas globais
assets/
  pagcall/logo/svg/                 -- Logos PagCall (SVG)
```

### Perfis de Acesso (UI)

| Pagina | Quem ve |
|--------|---------|
| Leads / Atendimento | Todos os perfis |
| Gestao de Empresas | Admin |
| Gestao de Usuarios | Admin / Gerente |
| Dashboard Vendedor | Vendedor |
| Dashboard Gerente | Admin / Gerente |

---

## Design System

### Resumo Visual Critico

| Elemento | Valor | Detalhes |
|----------|-------|---------|
| Background geral | `#F2F2F2` | Nunca `#F5F7FA` ou variacoes azuladas |
| Header | `#F1E52F`, fixo, 56px, 100% largura | Barra amarela no topo |
| Sidebar | `#FFFFFF`, 240px/68px | BRANCA. Nunca escura. 11 itens obrigatorios |
| Icones | Google Material Symbols Rounded | Nunca Font Awesome, Lucide ou Phosphor |
| Cards | Brancos, border-radius 12-16px | Sombra sutil, sem borda visivel |
| Cards destaque | `#1C1C1C`, texto branco | Cor escura SOMENTE para cards de destaque e headers de tabela |
| Botao primario | `#F1E52F`, texto `#1C1C1C` | border-radius ~8px |
| Tabelas | Header `#1C1C1C`, texto branco | Linhas brancas, bordas sutis |
| Badges status | Pill-shaped | Verde=aprovado, vermelho=cancelado, amarelo=pendente, azul=processando |
| Tipografia | Ubuntu (Google Fonts) | Sans-serif, titulos bold escuros |

### Cores da Marca

- **Amarelo pagah:** `#F1E52F` (header, botoes primarios, destaques)
- **Escuro:** `#1C1C1C` (textos, cards destaque, headers tabela)
- **Cinza texto:** `#575756` (texto secundario)
- **Cinza borda:** `#DADADA` (separadores, bordas sutis)
- **Background:** `#F2F2F2` (fundo geral)
- Verde e laranja sao apenas para **status funcionais**, nunca como cores de marca

---

## Stack e Estilizacao

As paginas usam:
- **Tailwind CSS via CDN** (algumas paginas com `prefix: 'tw-'`, outras sem)
- **Lucide Icons** (icones inline via `data-lucide`)
- **Google Material Symbols Rounded** (paginas com sidebar completa)
- **Ubuntu** (Google Fonts) para tipografia
- HTML estatico, sem build, sem framework

> **Nota:** Este repo e standalone. Paginas com sidebar completa seguem o template do repo pai (Pagah). Paginas mais simples podem ter layout proprio.

---

## PROIBICOES ABSOLUTAS

1. **NUNCA** usar fundo escuro (`#1C1C1C`) na sidebar — a sidebar e SEMPRE `#FFFFFF`
2. **NUNCA** omitir o header amarelo — toda pagina tem a barra `#F1E52F`
3. **NUNCA** usar background `#F5F7FA` ou similar — o correto e `#F2F2F2`
4. **NUNCA** usar verde ou laranja como cores de marca — sao apenas para status funcionais
5. **NUNCA** escrever o logo como "Pagah" ou "PAGAH" — e sempre minusculo: "pagah"
6. **NUNCA** inventar layout diferente do padrao (header amarelo + conteudo em cards)

---

## Assets

Os logos PagCall estao em `assets/pagcall/logo/svg/`:
- `Logo-alternativo.svg` — logo principal usado no hub
- `Icone.svg` — icone do foguete com telefone

Ao referenciar assets:
- De `index.html`: `src="assets/pagcall/logo/svg/..."`
- De `pages/*.html`: `src="../assets/pagcall/logo/svg/..."`

---

## Ao Criar Novas Paginas

- [ ] Header amarelo `#F1E52F` presente?
- [ ] Background `#F2F2F2`?
- [ ] Cards brancos com border-radius 12-16px?
- [ ] Fonte Ubuntu carregada?
- [ ] Dados mockados com valores realistas em BRL?
- [ ] Link adicionado no `index.html` (hub)?
- [ ] Caminhos de assets relativos corretos?
- [ ] Logo escrito em minusculas ("pagah" / "pagcall")?

---

## Deploy

O projeto esta na **Vercel** conectado ao GitHub. Qualquer push na branch `main` faz deploy automatico.
