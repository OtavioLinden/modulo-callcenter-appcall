# Redesign Filtros Leads Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Redesenhar a area de filtros da pagina Leads/Atendimento, separando busca por contato (campo unificado) dos filtros de categoria, num layout compacto de card unico.

**Architecture:** Modificacao de um unico arquivo HTML. Substituir o bloco de filtros atual (2 rows de 4 colunas) por um card com 3 camadas: filtros em grid de 5 colunas + botao, busca unificada full-width, contador. Adicionar dropdown Status e multi-select createMultiSelect. Remover inputs de Nome, Telefone, CPF e checkbox "Somente nao atendidos".

**Tech Stack:** HTML, Tailwind CSS (sem prefix), Google Material Symbols Rounded, JavaScript vanilla (funcoes createMultiSelect e createDateRangePicker ja existentes)

---

### Task 1: Substituir o HTML dos filtros

**Files:**
- Modify: `pages/leads-atendimento.html:232-277` (bloco `<!-- Filters card -->`)

- [ ] **Step 1: Substituir o bloco de filtros (linhas 232-277) pelo novo layout**

Substituir todo o conteudo entre `<!-- Filters card -->` e `<!-- Table card -->` por:

```html
      <!-- Filters card -->
      <div class="bg-white rounded-xl shadow-sm p-4 mb-5">

        <!-- Row: filtros de categoria + botao -->
        <div class="flex items-end gap-3 mb-3">
          <div class="flex-1 grid grid-cols-2 lg:grid-cols-5 gap-3">
            <div class="relative" id="ms-empresa"></div>
            <div class="relative" id="ms-tipo-lead"></div>
            <div class="relative" id="ms-valor"></div>
            <div class="relative" id="drp-periodo"></div>
            <div class="relative" id="ms-status"></div>
          </div>
          <button class="flex items-center gap-2 px-5 py-[7px] bg-[#F1E52F] text-[#1C1C1C] font-bold rounded-lg text-xs hover:brightness-95 transition-all whitespace-nowrap shrink-0">
            <span class="material-symbols-rounded" style="font-size:14px">search</span>
            Filtrar
          </button>
        </div>

        <!-- Busca unificada -->
        <div class="mb-2">
          <input type="text" placeholder="Buscar por nome, telefone ou CPF..." class="filter-input text-xs" style="padding:7px 12px;">
        </div>

        <!-- Contador -->
        <span class="text-[11px] text-[#575756]">Exibindo <strong class="text-[#1C1C1C]">10</strong> de <strong class="text-[#1C1C1C]">847</strong> leads</span>
      </div>
```

- [ ] **Step 2: Verificar visualmente no browser**

Abrir `pages/leads-atendimento.html` no browser. Confirmar:
- Grid de 5 dropdowns na primeira linha
- Campo de busca full-width abaixo
- Contador na base
- Botao Filtrar alinhado a direita dos dropdowns

---

### Task 2: Adicionar o multi-select Status e remover o checkbox

**Files:**
- Modify: `pages/leads-atendimento.html:973-999` (bloco de inicializacao JS)

- [ ] **Step 1: Adicionar createMultiSelect para Status**

Apos a linha `createDateRangePicker('drp-periodo', 'Periodo');` (linha 999), adicionar:

```javascript
    createMultiSelect('ms-status', [
      { value: 'atendidos', label: 'Atendidos' },
      { value: 'nao_atendidos', label: 'Não atendidos' }
    ], 'Todos', 'Status');
```

- [ ] **Step 2: Verificar visualmente no browser**

Abrir a pagina e confirmar que o dropdown Status aparece como 5o filtro com as opcoes "Todos", "Atendidos" e "Nao atendidos".

---

### Task 3: Limpar CSS nao utilizado

**Files:**
- Modify: `pages/leads-atendimento.html:16-132` (bloco `<style>`)

- [ ] **Step 1: Verificar se ha CSS orfao**

O checkbox `#toggle-nao-atendidos` foi removido do HTML. Verificar se havia CSS especifico para ele. Neste caso nao ha CSS customizado para o checkbox (usava apenas classes Tailwind inline), entao nenhuma limpeza e necessaria.

- [ ] **Step 2: Verificacao final no browser**

Abrir a pagina e testar:
- Todos os 5 dropdowns abrem e fecham corretamente
- Date range picker funciona
- Campo de busca recebe foco com borda amarela
- Botao Filtrar esta visivel e alinhado
- Layout responsivo: em telas menores, grid cai para 2 colunas

- [ ] **Step 3: Commit**

```bash
git add pages/leads-atendimento.html
git commit -m "refactor(leads): redesenha filtros com busca unificada e dropdown Status

Separa filtros de categoria (Empresa, Tipo, Valor, Periodo, Status) da
busca por contato (campo unico para nome/telefone/CPF). Layout compacto
em card unico com 3 camadas. Substitui checkbox 'Somente nao atendidos'
por dropdown Status com 3 opcoes."
```
