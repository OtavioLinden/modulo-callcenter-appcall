# Redesign dos Filtros — Leads / Atendimento

**Data:** 2026-04-06
**Pagina:** `pages/leads-atendimento.html`

---

## Problema

Os filtros atuais misturam dois propositos diferentes num layout flat de 2 linhas x 4 colunas:

- **Busca por contato** (Nome, Telefone, CPF) — serve para encontrar um lead especifico
- **Filtros de categoria** (Empresa, Tipo de Lead, Valor, Periodo, Status) — servem para filtrar a lista por criterios

Isso gera confusao visual e ocupa espaco desnecessario.

---

## Design Aprovado

Um unico card branco com 3 camadas verticais:

### 1. Filtros de categoria (topo)
- Grid de **5 colunas** + botao Filtrar alinhado a direita
- Campos: Empresa | Tipo de Lead | Valor | Periodo | Status
- Todos como dropdowns (selects)
- **Status** substitui o antigo checkbox "Somente nao atendidos" e oferece 3 opcoes: `Todos` | `Atendidos` | `Nao atendidos`

### 2. Barra de busca (meio)
- Input de texto full-width dentro do card
- Placeholder: "Buscar por nome, telefone ou CPF..."
- Icone de lupa no placeholder
- Substitui os 3 campos separados (Nome, Telefone, CPF) por um campo unificado que aceita qualquer um dos tres

### 3. Contador (base)
- Texto: "Exibindo **X** de **Y** leads"

### Campos removidos
- Input "Nome" — absorvido pela busca unificada
- Input "Telefone" — absorvido pela busca unificada
- Input "CPF" — absorvido pela busca unificada
- Checkbox "Somente nao atendidos" — substituido pelo dropdown Status

### Campos adicionados
- Dropdown "Status" (Todos | Atendidos | Nao atendidos)

### Especificacoes visuais
- Card: `background: #fff`, `border-radius: 12px`, `padding: 12px 16px`, sombra sutil
- Labels: `10px`, uppercase, `#575756`, `letter-spacing: 0.05em`
- Inputs/selects: `border: 1px solid #DADADA`, `border-radius: 6px`, `padding: 6px 10px`, `font-size: 12px`
- Botao Filtrar: `background: #F1E52F`, `color: #1C1C1C`, `font-weight: 700`, `border-radius: 8px`
- Busca: `border-radius: 6px`, `padding: 7px 12px`, `font-size: 12px`
- Contador: `font-size: 11px`, `color: #575756`

---

## Mockup de referencia

O mockup aprovado esta em `.superpowers/brainstorm/1750-1775477359/content/layout-b-v3.html`
