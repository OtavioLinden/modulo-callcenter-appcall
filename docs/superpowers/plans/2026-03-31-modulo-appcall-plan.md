# Modulo AppCall — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build 5 visual prototypes (HTML + Tailwind CSS) for the AppCall callcenter module following the Pagah design system.

**Architecture:** Each page is a standalone HTML file sharing a common layout (sidebar + yellow header + #F2F2F2 background). Tailwind CSS via CDN. Mock data hardcoded in each file. No build tooling, no JS framework — open-in-browser prototypes.

**Tech Stack:** HTML5, Tailwind CSS (CDN), Lucide Icons (CDN), Ubuntu font (Google Fonts), vanilla JS (only for interactive toggles/modals/tabs)

**Spec:** `docs/superpowers/specs/2026-03-31-modulo-appcall-design.md`
**Design System:** `../../pagah-design-system.md` (root of repo)
**Dashboard Reference:** `dash-nova-incrivel-3.html` (existing file in project)
**Platform Screenshots:** `../../referencias/` (sidebar, home, users, support screens)

---

## File Structure

```
projetos/modulo-callcenter-appcall/
  index.html                          # Landing/redirect page (links to all prototypes)
  pages/
    leads-atendimento.html            # P3 — Leads / Atendimento (CRM)
    gestao-empresas.html              # P5 — Gestao de Empresas (Admin only)
    gestao-usuarios.html              # P4 — Gestao de Usuarios (Admin/Gerente)
    dashboard-vendedor.html           # P1 — Dashboard do Vendedor
    dashboard-gerente.html            # P2 — Dashboard do Gerente/Admin
  docs/
    superpowers/
      specs/2026-03-31-modulo-appcall-design.md
      plans/2026-03-31-modulo-appcall-plan.md
  dash-nova-incrivel-3.html           # Existing reference (do not modify)
```

Each HTML file is self-contained with the full layout (sidebar + header + content). The common structure is copy-pasted per file (no build system, no partials). This is intentional — prototypes are standalone files that designers and devs can open in a browser.

---

## Conventions

**Tailwind classes used throughout (mapping to Design System):**

| Design System Token | Tailwind Class |
|---|---|
| Background geral #F2F2F2 | `bg-[#F2F2F2]` |
| Card branco | `bg-white rounded-xl shadow-sm` |
| Card destaque escuro #1C1C1C | `bg-[#1C1C1C] text-white` |
| Header amarelo #F1E52F | `bg-[#F1E52F]` |
| Botao primario | `bg-[#F1E52F] text-[#1C1C1C] font-bold rounded-lg px-5 py-2.5` |
| Botao outline | `border border-[#DADADA] text-[#575756] rounded-lg px-5 py-2.5` |
| Tabela header | `bg-[#1C1C1C] text-white text-xs uppercase` |
| Badge aprovado | `bg-green-100 text-green-700 rounded-full px-3 py-1 text-xs font-semibold` |
| Badge cancelado | `bg-red-100 text-red-700 rounded-full px-3 py-1 text-xs font-semibold` |
| Badge pendente | `bg-yellow-100 text-yellow-700 rounded-full px-3 py-1 text-xs font-semibold` |
| Badge processando | `bg-blue-100 text-blue-700 rounded-full px-3 py-1 text-xs font-semibold` |
| Texto principal | `text-[#1C1C1C]` |
| Texto secundario | `text-[#575756]` |
| Borda | `border-[#DADADA]` |
| Icones | Lucide icons, 20-24px, `text-[#575756]` |

**Common HTML head (used in every file):**

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>PAGE_TITLE | AppCall - Pagah</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: { ubuntu: ['Ubuntu', 'sans-serif'] },
        }
      }
    }
  </script>
  <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500;700&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>body { font-family: 'Ubuntu', sans-serif; }</style>
</head>
```

**Sidebar HTML structure (Vendedor version — Admin/Gerente add extra items):**

The sidebar has two states: expanded (260px, icons + labels) and compact (72px, icons only). A toggle button switches between them via vanilla JS.

```html
<!-- Sidebar -->
<aside id="sidebar" class="fixed top-0 left-0 h-screen bg-white border-r border-[#DADADA] z-50 flex flex-col transition-all duration-300 w-[260px]">
  <!-- Logo PagCall -->
  <div class="h-16 flex items-center px-5 border-b border-[#DADADA]">
    <img src="../../assets/pagcall/logo/svg/Logo-alternativo.svg" alt="pagcall" class="h-8 sidebar-expanded">
    <img src="../../assets/pagcall/logo/svg/Icone.svg" alt="pagcall" class="h-8 hidden sidebar-compact">
  </div>

  <!-- Nav items -->
  <nav class="flex-1 py-4">
    <a href="dashboard-vendedor.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
      <i data-lucide="layout-dashboard" class="w-5 h-5 shrink-0"></i>
      <span class="sidebar-label">Dashboard</span>
    </a>
    <a href="leads-atendimento.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
      <i data-lucide="clipboard-list" class="w-5 h-5 shrink-0"></i>
      <span class="sidebar-label">Leads / Atendimento</span>
    </a>
    <!-- ADMIN/GERENTE ONLY items (remove for vendedor): -->
    <a href="gestao-usuarios.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
      <i data-lucide="users" class="w-5 h-5 shrink-0"></i>
      <span class="sidebar-label">Gestao de Usuarios</span>
    </a>
    <!-- ADMIN ONLY (remove for gerente and vendedor): -->
    <a href="gestao-empresas.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
      <i data-lucide="building-2" class="w-5 h-5 shrink-0"></i>
      <span class="sidebar-label">Gestao de Empresas</span>
    </a>
  </nav>

  <!-- Toggle button -->
  <button onclick="toggleSidebar()" class="p-3 border-t border-[#DADADA] text-[#575756] hover:bg-[#F2F2F2]">
    <i data-lucide="panel-left-close" id="sidebar-toggle-icon" class="w-5 h-5"></i>
  </button>
</aside>
```

**Yellow header strip + main content wrapper:**

```html
<div id="main-wrapper" class="transition-all duration-300 ml-[260px]">
  <!-- Yellow header strip -->
  <header class="h-2 bg-[#F1E52F]"></header>

  <!-- Page header bar -->
  <div class="bg-white border-b border-[#DADADA] px-8 py-4 flex items-center justify-between">
    <h1 class="text-xl font-bold text-[#1C1C1C]">PAGE_TITLE</h1>
    <div class="flex items-center gap-4">
      <!-- Right side controls (date picker, user avatar, etc.) -->
    </div>
  </div>

  <!-- Main content -->
  <main class="bg-[#F2F2F2] min-h-screen p-8">
    <!-- Page content here -->
  </main>
</div>
```

**Sidebar toggle JS (included in every file):**

```html
<script>
  // Initialize Lucide icons
  lucide.createIcons();

  // Sidebar toggle
  let sidebarExpanded = true;
  function toggleSidebar() {
    const sidebar = document.getElementById('sidebar');
    const main = document.getElementById('main-wrapper');
    sidebarExpanded = !sidebarExpanded;
    if (sidebarExpanded) {
      sidebar.classList.replace('w-[72px]', 'w-[260px]');
      main.classList.replace('ml-[72px]', 'ml-[260px]');
      document.querySelectorAll('.sidebar-label').forEach(el => el.classList.remove('hidden'));
      document.querySelectorAll('.sidebar-expanded').forEach(el => el.classList.remove('hidden'));
      document.querySelectorAll('.sidebar-compact').forEach(el => el.classList.add('hidden'));
    } else {
      sidebar.classList.replace('w-[260px]', 'w-[72px]');
      main.classList.replace('ml-[260px]', 'ml-[72px]');
      document.querySelectorAll('.sidebar-label').forEach(el => el.classList.add('hidden'));
      document.querySelectorAll('.sidebar-expanded').forEach(el => el.classList.add('hidden'));
      document.querySelectorAll('.sidebar-compact').forEach(el => el.classList.remove('hidden'));
    }
  }
</script>
```

**Active nav item styling:** The current page link gets `bg-[#F2F2F2] text-[#1C1C1C] font-semibold border-r-[3px] border-[#F1E52F]` instead of the default classes.

---

## Task 1: Index Page + Project Setup

**Files:**
- Create: `projetos/modulo-callcenter-appcall/index.html`
- Create: `projetos/modulo-callcenter-appcall/pages/` (directory)

This is the landing page that links to all prototypes. Simple grid of cards.

- [ ] **Step 1: Create pages directory**

```bash
mkdir -p projetos/modulo-callcenter-appcall/pages
```

- [ ] **Step 2: Create index.html**

Create `projetos/modulo-callcenter-appcall/index.html` with this content:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Modulo AppCall | Pagah - Prototipos</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500;700&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>body { font-family: 'Ubuntu', sans-serif; }</style>
</head>
<body class="bg-[#F2F2F2] min-h-screen">
  <!-- Yellow header strip -->
  <header class="h-2 bg-[#F1E52F]"></header>

  <div class="max-w-4xl mx-auto py-16 px-6">
    <div class="flex items-center gap-4 mb-2">
      <img src="../assets/pagcall/logo/svg/Logo-alternativo.svg" alt="pagcall" class="h-10">
    </div>
    <p class="text-[#575756] mb-10">Prototipos visuais do modulo de Call Center</p>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
      <!-- P3 — Leads -->
      <a href="pages/leads-atendimento.html" class="bg-white rounded-xl shadow-sm p-6 hover:shadow-md transition-shadow border border-transparent hover:border-[#F1E52F]">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-10 h-10 bg-[#F2F2F2] rounded-lg flex items-center justify-center">
            <i data-lucide="clipboard-list" class="w-5 h-5 text-[#575756]"></i>
          </div>
          <h2 class="font-bold text-[#1C1C1C]">Leads / Atendimento</h2>
        </div>
        <p class="text-sm text-[#575756]">CRM de leads — filtros, listagem e fluxo de atendimento para vendedores</p>
        <span class="inline-block mt-3 text-xs font-semibold bg-green-100 text-green-700 rounded-full px-3 py-1">Todos os perfis</span>
      </a>

      <!-- P5 — Empresas -->
      <a href="pages/gestao-empresas.html" class="bg-white rounded-xl shadow-sm p-6 hover:shadow-md transition-shadow border border-transparent hover:border-[#F1E52F]">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-10 h-10 bg-[#F2F2F2] rounded-lg flex items-center justify-center">
            <i data-lucide="building-2" class="w-5 h-5 text-[#575756]"></i>
          </div>
          <h2 class="font-bold text-[#1C1C1C]">Gestao de Empresas</h2>
        </div>
        <p class="text-sm text-[#575756]">Controle global de empresas e lojas — toggles ON/OFF para o callcenter</p>
        <span class="inline-block mt-3 text-xs font-semibold bg-red-100 text-red-700 rounded-full px-3 py-1">Admin only</span>
      </a>

      <!-- P4 — Usuarios -->
      <a href="pages/gestao-usuarios.html" class="bg-white rounded-xl shadow-sm p-6 hover:shadow-md transition-shadow border border-transparent hover:border-[#F1E52F]">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-10 h-10 bg-[#F2F2F2] rounded-lg flex items-center justify-center">
            <i data-lucide="users" class="w-5 h-5 text-[#575756]"></i>
          </div>
          <h2 class="font-bold text-[#1C1C1C]">Gestao de Usuarios</h2>
        </div>
        <p class="text-sm text-[#575756]">CRUD de vendedores, perfis e permissoes individuais por usuario</p>
        <span class="inline-block mt-3 text-xs font-semibold bg-blue-100 text-blue-700 rounded-full px-3 py-1">Admin / Gerente</span>
      </a>

      <!-- P1 — Dashboard Vendedor -->
      <a href="pages/dashboard-vendedor.html" class="bg-white rounded-xl shadow-sm p-6 hover:shadow-md transition-shadow border border-transparent hover:border-[#F1E52F]">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-10 h-10 bg-[#F2F2F2] rounded-lg flex items-center justify-center">
            <i data-lucide="layout-dashboard" class="w-5 h-5 text-[#575756]"></i>
          </div>
          <h2 class="font-bold text-[#1C1C1C]">Dashboard Vendedor</h2>
        </div>
        <p class="text-sm text-[#575756]">KPIs financeiros, gamificacao semanal, ranking e extrato de vendas</p>
        <span class="inline-block mt-3 text-xs font-semibold bg-green-100 text-green-700 rounded-full px-3 py-1">Vendedor</span>
      </a>

      <!-- P2 — Dashboard Gerente -->
      <a href="pages/dashboard-gerente.html" class="bg-white rounded-xl shadow-sm p-6 hover:shadow-md transition-shadow border border-transparent hover:border-[#F1E52F]">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-10 h-10 bg-[#F2F2F2] rounded-lg flex items-center justify-center">
            <i data-lucide="bar-chart-3" class="w-5 h-5 text-[#575756]"></i>
          </div>
          <h2 class="font-bold text-[#1C1C1C]">Dashboard Gerente / Admin</h2>
        </div>
        <p class="text-sm text-[#575756]">Visao consolidada — metricas globais, filtros por vendedor/equipe</p>
        <span class="inline-block mt-3 text-xs font-semibold bg-blue-100 text-blue-700 rounded-full px-3 py-1">Admin / Gerente</span>
      </a>

      <!-- Reference -->
      <a href="dash-nova-incrivel-3.html" class="bg-white rounded-xl shadow-sm p-6 hover:shadow-md transition-shadow border border-dashed border-[#DADADA] opacity-60">
        <div class="flex items-center gap-3 mb-3">
          <div class="w-10 h-10 bg-[#F2F2F2] rounded-lg flex items-center justify-center">
            <i data-lucide="eye" class="w-5 h-5 text-[#575756]"></i>
          </div>
          <h2 class="font-bold text-[#1C1C1C]">Referencia — Dashboard Original</h2>
        </div>
        <p class="text-sm text-[#575756]">Prototipo anterior (nao segue design system Pagah — apenas referencia de dados)</p>
        <span class="inline-block mt-3 text-xs font-semibold bg-gray-100 text-gray-600 rounded-full px-3 py-1">Referencia</span>
      </a>
    </div>
  </div>

  <script>lucide.createIcons();</script>
</body>
</html>
```

- [ ] **Step 3: Verify in browser**

Open `projetos/modulo-callcenter-appcall/index.html` in browser. Should show a grid of 6 cards linking to each prototype page. Links will 404 for now (pages not created yet).

- [ ] **Step 4: Commit**

```bash
git add projetos/modulo-callcenter-appcall/index.html
git commit -m "feat(appcall): add index page with links to all prototypes"
```

---

## Task 2: P3 — Leads / Atendimento (CRM)

**Files:**
- Create: `projetos/modulo-callcenter-appcall/pages/leads-atendimento.html`

The main CRM page used by vendors daily. Includes: sidebar (vendedor version with 2 items), filters bar, leads table with mock data, and "Atender" action with confirmation modal.

**Mock data:** 10 lead rows with realistic Brazilian names, phones, addresses, product kits with BRL values.

- [ ] **Step 1: Create leads-atendimento.html with full layout**

Create `projetos/modulo-callcenter-appcall/pages/leads-atendimento.html` with this content:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Leads / Atendimento | AppCall - Pagah</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: { extend: { fontFamily: { ubuntu: ['Ubuntu', 'sans-serif'] } } }
    }
  </script>
  <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500;700&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>body { font-family: 'Ubuntu', sans-serif; }</style>
</head>
<body class="bg-[#F2F2F2]">

  <!-- Sidebar (Vendedor: Dashboard + Leads) -->
  <aside id="sidebar" class="fixed top-0 left-0 h-screen bg-white border-r border-[#DADADA] z-50 flex flex-col transition-all duration-300 w-[260px]">
    <div class="h-16 flex items-center px-5 border-b border-[#DADADA]">
      <img src="../../assets/pagcall/logo/svg/Logo-alternativo.svg" alt="pagcall" class="h-8 sidebar-expanded">
      <img src="../../assets/pagcall/logo/svg/Icone.svg" alt="pagcall" class="h-8 hidden sidebar-compact">
    </div>
    <nav class="flex-1 py-4">
      <a href="dashboard-vendedor.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
        <i data-lucide="layout-dashboard" class="w-5 h-5 shrink-0"></i>
        <span class="sidebar-label">Dashboard</span>
      </a>
      <a href="leads-atendimento.html" class="flex items-center gap-3 px-5 py-3 bg-[#F2F2F2] text-[#1C1C1C] font-semibold border-r-[3px] border-[#F1E52F] transition-colors">
        <i data-lucide="clipboard-list" class="w-5 h-5 shrink-0"></i>
        <span class="sidebar-label">Leads / Atendimento</span>
      </a>
    </nav>
    <button onclick="toggleSidebar()" class="p-3 border-t border-[#DADADA] text-[#575756] hover:bg-[#F2F2F2]">
      <i data-lucide="panel-left-close" class="w-5 h-5"></i>
    </button>
  </aside>

  <!-- Main -->
  <div id="main-wrapper" class="transition-all duration-300 ml-[260px]">
    <header class="h-2 bg-[#F1E52F]"></header>

    <div class="bg-white border-b border-[#DADADA] px-8 py-4 flex items-center justify-between">
      <h1 class="text-xl font-bold text-[#1C1C1C]">Leads / Atendimento</h1>
      <div class="flex items-center gap-3">
        <span class="text-sm text-[#575756]">Vendedor: <strong>Joao Silva</strong></span>
        <div class="w-9 h-9 bg-[#F2F2F2] rounded-full flex items-center justify-center">
          <i data-lucide="user" class="w-4 h-4 text-[#575756]"></i>
        </div>
      </div>
    </div>

    <main class="bg-[#F2F2F2] min-h-screen p-8">
      <!-- Filters Card -->
      <div class="bg-white rounded-xl shadow-sm p-6 mb-6">
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-4">
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Empresa</label>
            <select class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm bg-white focus:border-[#F1E52F] focus:outline-none">
              <option>Todas as empresas</option>
              <option>EMP001 - NutriVida</option>
              <option>EMP002 - BelezaPura</option>
              <option>EMP003 - FitMax</option>
              <option>EMP004 - DermaPlus</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Tipo de Lead</label>
            <select class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm bg-white focus:border-[#F1E52F] focus:outline-none">
              <option>Todos os tipos</option>
              <option>Cartoes Aprovados KITS Site</option>
              <option>Cartoes Aprovados AMOSTRAS Site</option>
              <option>Abandonos Site</option>
              <option>Pix Pagos Site</option>
              <option>Pix Pendentes Site</option>
              <option>Boletos Pagos Site</option>
              <option>Boletos Pendentes Site</option>
              <option>Cartao Recusado KIT Site</option>
              <option>Cartao Recusado Falta de Saldo</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Nome</label>
            <input type="text" placeholder="Nome do cliente" class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm focus:border-[#F1E52F] focus:outline-none">
          </div>
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Telefone</label>
            <input type="text" placeholder="Telefone do cliente" class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm focus:border-[#F1E52F] focus:outline-none">
          </div>
        </div>
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-4">
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">CPF</label>
            <input type="text" placeholder="CPF do cliente" class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm focus:border-[#F1E52F] focus:outline-none">
          </div>
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Valor</label>
            <select class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm bg-white focus:border-[#F1E52F] focus:outline-none">
              <option>Todos</option>
              <option>Ate R$ 100</option>
              <option>R$ 100 - R$ 300</option>
              <option>R$ 300 - R$ 500</option>
              <option>Acima de R$ 500</option>
            </select>
          </div>
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Periodo inicial</label>
            <input type="date" value="2026-03-01" class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm focus:border-[#F1E52F] focus:outline-none">
          </div>
          <div>
            <label class="block text-xs font-semibold text-[#575756] mb-1.5">Periodo final</label>
            <input type="date" value="2026-03-31" class="w-full border border-[#DADADA] rounded-lg px-3 py-2.5 text-sm focus:border-[#F1E52F] focus:outline-none">
          </div>
        </div>
        <div class="flex items-center justify-between">
          <label class="flex items-center gap-2 text-sm text-[#575756] cursor-pointer">
            <input type="checkbox" class="rounded border-[#DADADA] text-[#F1E52F] focus:ring-[#F1E52F]">
            Somente nao atendidos
          </label>
          <button class="bg-[#F1E52F] text-[#1C1C1C] font-bold rounded-lg px-6 py-2.5 text-sm hover:brightness-95 transition">
            Filtrar
          </button>
        </div>
      </div>

      <!-- Results count -->
      <div class="flex items-center justify-between mb-4">
        <span class="text-sm text-[#575756] font-medium">Exibindo 1-10 de 847 leads</span>
      </div>

      <!-- Leads Table -->
      <div class="bg-white rounded-xl shadow-sm overflow-hidden">
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="bg-[#1C1C1C] text-white text-xs uppercase tracking-wide">
                <th class="px-4 py-3 text-left font-semibold">ID Empresa</th>
                <th class="px-4 py-3 text-left font-semibold">Nome Cliente</th>
                <th class="px-4 py-3 text-left font-semibold">Telefone</th>
                <th class="px-4 py-3 text-left font-semibold">Data Ocorrencia</th>
                <th class="px-4 py-3 text-left font-semibold">Erro</th>
                <th class="px-4 py-3 text-left font-semibold">Endereco</th>
                <th class="px-4 py-3 text-left font-semibold">KIT</th>
                <th class="px-4 py-3 text-center font-semibold">Acoes</th>
              </tr>
            </thead>
            <tbody class="text-sm">
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP001</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Carlos Eduardo Silva</td>
                <td class="px-4 py-3 text-[#575756]">(11) 98765-4321</td>
                <td class="px-4 py-3 text-[#575756]">30/03/2026 14:30</td>
                <td class="px-4 py-3"><span class="bg-red-100 text-red-700 text-xs font-semibold px-2 py-1 rounded-full">Visa recusado</span></td>
                <td class="px-4 py-3 text-[#575756] text-xs">Rua das Flores, 123 - SP</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Emagrecedor 3x</div>
                  <div class="text-xs text-[#575756]">R$ 297,00</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Carlos Eduardo Silva')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP002</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Ana Maria Souza</td>
                <td class="px-4 py-3 text-[#575756]">(21) 97654-3210</td>
                <td class="px-4 py-3 text-[#575756]">30/03/2026 11:15</td>
                <td class="px-4 py-3 text-[#575756]">—</td>
                <td class="px-4 py-3 text-[#575756] text-xs">Av. Brasil, 456 - RJ</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Capilar Premium</div>
                  <div class="text-xs text-[#575756]">R$ 189,90</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Ana Maria Souza')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP001</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Roberto Fernandes</td>
                <td class="px-4 py-3 text-[#575756]">(31) 99876-5432</td>
                <td class="px-4 py-3 text-[#575756]">29/03/2026 16:45</td>
                <td class="px-4 py-3"><span class="bg-yellow-100 text-yellow-700 text-xs font-semibold px-2 py-1 rounded-full">Saldo insuficiente</span></td>
                <td class="px-4 py-3 text-[#575756] text-xs">Rua Minas Gerais, 789 - BH</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Whey + Creatina</div>
                  <div class="text-xs text-[#575756]">R$ 347,00</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Roberto Fernandes')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP003</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Juliana Costa</td>
                <td class="px-4 py-3 text-[#575756]">(41) 98543-2109</td>
                <td class="px-4 py-3 text-[#575756]">29/03/2026 09:20</td>
                <td class="px-4 py-3 text-[#575756]">—</td>
                <td class="px-4 py-3 text-[#575756] text-xs">Rua XV de Novembro, 321 - CWB</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Amostra Serum Facial</div>
                  <div class="text-xs text-[#575756]">R$ 49,90</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Juliana Costa')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP004</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Marcos Oliveira</td>
                <td class="px-4 py-3 text-[#575756]">(51) 97432-1098</td>
                <td class="px-4 py-3 text-[#575756]">28/03/2026 13:50</td>
                <td class="px-4 py-3"><span class="bg-red-100 text-red-700 text-xs font-semibold px-2 py-1 rounded-full">Master recusado</span></td>
                <td class="px-4 py-3 text-[#575756] text-xs">Av. Ipiranga, 654 - POA</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Derma Anti-idade 5x</div>
                  <div class="text-xs text-[#575756]">R$ 497,00</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Marcos Oliveira')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA] bg-green-50">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP001</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Fernanda Alves</td>
                <td class="px-4 py-3 text-[#575756]">(61) 96321-0987</td>
                <td class="px-4 py-3 text-[#575756]">28/03/2026 10:00</td>
                <td class="px-4 py-3 text-[#575756]">—</td>
                <td class="px-4 py-3 text-[#575756] text-xs">SQS 308, Bloco A - BSB</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Emagrecedor 1x</div>
                  <div class="text-xs text-[#575756]">R$ 127,00</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <span class="bg-green-100 text-green-700 text-xs font-semibold px-3 py-1.5 rounded-full">Atendido</span>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP002</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Lucas Mendes</td>
                <td class="px-4 py-3 text-[#575756]">(71) 95210-9876</td>
                <td class="px-4 py-3 text-[#575756]">27/03/2026 15:30</td>
                <td class="px-4 py-3"><span class="bg-yellow-100 text-yellow-700 text-xs font-semibold px-2 py-1 rounded-full">Pix expirado</span></td>
                <td class="px-4 py-3 text-[#575756] text-xs">Rua da Bahia, 987 - SSA</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Capilar Basico</div>
                  <div class="text-xs text-[#575756]">R$ 89,90</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Lucas Mendes')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP003</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Patricia Souza</td>
                <td class="px-4 py-3 text-[#575756]">(81) 94109-8765</td>
                <td class="px-4 py-3 text-[#575756]">27/03/2026 08:45</td>
                <td class="px-4 py-3 text-[#575756]">—</td>
                <td class="px-4 py-3 text-[#575756] text-xs">Av. Boa Viagem, 555 - REC</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit FitMax Completo</div>
                  <div class="text-xs text-[#575756]">R$ 397,00</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Patricia Souza')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA] bg-green-50">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP004</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Rafael Costa</td>
                <td class="px-4 py-3 text-[#575756]">(85) 93098-7654</td>
                <td class="px-4 py-3 text-[#575756]">26/03/2026 17:10</td>
                <td class="px-4 py-3 text-[#575756]">—</td>
                <td class="px-4 py-3 text-[#575756] text-xs">Rua Fortaleza, 222 - FOR</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Amostra DermaPlus</div>
                  <div class="text-xs text-[#575756]">R$ 39,90</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <span class="bg-green-100 text-green-700 text-xs font-semibold px-3 py-1.5 rounded-full">Atendido</span>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-4 py-3"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP001</span></td>
                <td class="px-4 py-3 font-medium text-[#1C1C1C]">Camila Rodrigues</td>
                <td class="px-4 py-3 text-[#575756]">(91) 92087-6543</td>
                <td class="px-4 py-3 text-[#575756]">26/03/2026 11:30</td>
                <td class="px-4 py-3"><span class="bg-red-100 text-red-700 text-xs font-semibold px-2 py-1 rounded-full">Elo recusado</span></td>
                <td class="px-4 py-3 text-[#575756] text-xs">Tv. do Chaco, 100 - BEL</td>
                <td class="px-4 py-3">
                  <div class="font-medium text-[#1C1C1C]">Kit Emagrecedor 5x</div>
                  <div class="text-xs text-[#575756]">R$ 447,00</div>
                </td>
                <td class="px-4 py-3 text-center">
                  <button onclick="openAtenderModal('Camila Rodrigues')" class="bg-[#F1E52F] text-[#1C1C1C] font-bold text-xs rounded-lg px-4 py-2 hover:brightness-95 transition">
                    Atender
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- Pagination -->
        <div class="flex items-center justify-between px-4 py-3 border-t border-[#F2F2F2]">
          <span class="text-xs text-[#575756]">Pagina 1 de 85</span>
          <div class="flex gap-1">
            <button class="px-3 py-1.5 text-xs font-bold bg-[#1C1C1C] text-white rounded">1</button>
            <button class="px-3 py-1.5 text-xs font-medium text-[#575756] border border-[#DADADA] rounded hover:bg-[#F2F2F2]">2</button>
            <button class="px-3 py-1.5 text-xs font-medium text-[#575756] border border-[#DADADA] rounded hover:bg-[#F2F2F2]">3</button>
            <button class="px-3 py-1.5 text-xs font-medium text-[#575756] border border-[#DADADA] rounded hover:bg-[#F2F2F2]">Prox</button>
          </div>
        </div>
      </div>
    </main>
  </div>

  <!-- Modal Confirmar Atendimento -->
  <div id="modal-atender" class="fixed inset-0 bg-black/50 z-[100] hidden items-center justify-center">
    <div class="bg-white rounded-xl shadow-lg p-6 w-full max-w-md mx-4">
      <div class="flex items-center gap-3 mb-4">
        <div class="w-10 h-10 bg-[#F1E52F] rounded-full flex items-center justify-center">
          <i data-lucide="phone-call" class="w-5 h-5 text-[#1C1C1C]"></i>
        </div>
        <div>
          <h3 class="font-bold text-[#1C1C1C]">Confirmar Atendimento</h3>
          <p class="text-sm text-[#575756]">Cliente: <strong id="modal-client-name"></strong></p>
        </div>
      </div>
      <p class="text-sm text-[#575756] mb-6">O cadastro do cliente sera aberto em uma nova guia. Confirme o atendimento para registrar no sistema.</p>
      <div class="flex gap-3 justify-end">
        <button onclick="closeAtenderModal()" class="border border-[#DADADA] text-[#575756] rounded-lg px-5 py-2.5 text-sm font-medium hover:bg-[#F2F2F2]">
          Cancelar
        </button>
        <button onclick="confirmarAtendimento()" class="bg-[#F1E52F] text-[#1C1C1C] font-bold rounded-lg px-5 py-2.5 text-sm hover:brightness-95">
          Confirmar Atendimento
        </button>
      </div>
    </div>
  </div>

  <script>
    lucide.createIcons();

    // Sidebar toggle
    let sidebarExpanded = true;
    function toggleSidebar() {
      const sidebar = document.getElementById('sidebar');
      const main = document.getElementById('main-wrapper');
      sidebarExpanded = !sidebarExpanded;
      if (sidebarExpanded) {
        sidebar.classList.replace('w-[72px]', 'w-[260px]');
        main.classList.replace('ml-[72px]', 'ml-[260px]');
        document.querySelectorAll('.sidebar-label').forEach(el => el.classList.remove('hidden'));
        document.querySelectorAll('.sidebar-expanded').forEach(el => el.classList.remove('hidden'));
        document.querySelectorAll('.sidebar-compact').forEach(el => el.classList.add('hidden'));
      } else {
        sidebar.classList.replace('w-[260px]', 'w-[72px]');
        main.classList.replace('ml-[260px]', 'ml-[72px]');
        document.querySelectorAll('.sidebar-label').forEach(el => el.classList.add('hidden'));
        document.querySelectorAll('.sidebar-expanded').forEach(el => el.classList.add('hidden'));
        document.querySelectorAll('.sidebar-compact').forEach(el => el.classList.remove('hidden'));
      }
    }

    // Modal Atender
    function openAtenderModal(clientName) {
      document.getElementById('modal-client-name').textContent = clientName;
      const modal = document.getElementById('modal-atender');
      modal.classList.remove('hidden');
      modal.classList.add('flex');
    }

    function closeAtenderModal() {
      const modal = document.getElementById('modal-atender');
      modal.classList.add('hidden');
      modal.classList.remove('flex');
    }

    function confirmarAtendimento() {
      // In a real app this would open a new tab and mark as attended
      window.open('#cadastro-cliente', '_blank');
      closeAtenderModal();
      alert('Atendimento confirmado!');
    }
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `pages/leads-atendimento.html`. Verify:
- Sidebar with PagCall logo, 2 nav items, "Leads" highlighted
- Sidebar toggle works (expand/compact)
- Yellow strip at top
- Filters card with 8 filter fields + checkbox + Filtrar button
- Table with 10 rows, dark header, correct columns
- 2 rows showing "Atendido" badge (green bg), 8 with "Atender" button
- Click "Atender" → modal opens with client name → "Confirmar" works
- Pagination at bottom

- [ ] **Step 3: Commit**

```bash
git add projetos/modulo-callcenter-appcall/pages/leads-atendimento.html
git commit -m "feat(appcall): add leads/atendimento CRM page with filters, table and modal"
```

---

## Task 3: P5 — Gestao de Empresas (Admin Only)

**Files:**
- Create: `projetos/modulo-callcenter-appcall/pages/gestao-empresas.html`

Admin-only page. Toggle ON/OFF per company, "Lojas" button opens modal with store toggles. Sidebar has 4 items (admin version).

**Mock data:** 12 companies with realistic names, IDs, and varying ON/OFF states. Modal shows 3-5 stores per company.

- [ ] **Step 1: Create gestao-empresas.html**

Create `projetos/modulo-callcenter-appcall/pages/gestao-empresas.html` with this content:

```html
<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Gestao de Empresas | AppCall - Pagah</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      theme: { extend: { fontFamily: { ubuntu: ['Ubuntu', 'sans-serif'] } } }
    }
  </script>
  <link href="https://fonts.googleapis.com/css2?family=Ubuntu:wght@300;400;500;700&display=swap" rel="stylesheet">
  <script src="https://unpkg.com/lucide@latest"></script>
  <style>body { font-family: 'Ubuntu', sans-serif; }</style>
</head>
<body class="bg-[#F2F2F2]">

  <!-- Sidebar (Admin: 4 items) -->
  <aside id="sidebar" class="fixed top-0 left-0 h-screen bg-white border-r border-[#DADADA] z-50 flex flex-col transition-all duration-300 w-[260px]">
    <div class="h-16 flex items-center px-5 border-b border-[#DADADA]">
      <img src="../../assets/pagcall/logo/svg/Logo-alternativo.svg" alt="pagcall" class="h-8 sidebar-expanded">
      <img src="../../assets/pagcall/logo/svg/Icone.svg" alt="pagcall" class="h-8 hidden sidebar-compact">
    </div>
    <nav class="flex-1 py-4">
      <a href="dashboard-gerente.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
        <i data-lucide="layout-dashboard" class="w-5 h-5 shrink-0"></i>
        <span class="sidebar-label">Dashboard Geral</span>
      </a>
      <a href="leads-atendimento.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
        <i data-lucide="clipboard-list" class="w-5 h-5 shrink-0"></i>
        <span class="sidebar-label">Leads / Atendimento</span>
      </a>
      <a href="gestao-usuarios.html" class="flex items-center gap-3 px-5 py-3 text-[#575756] hover:bg-[#F2F2F2] transition-colors">
        <i data-lucide="users" class="w-5 h-5 shrink-0"></i>
        <span class="sidebar-label">Gestao de Usuarios</span>
      </a>
      <a href="gestao-empresas.html" class="flex items-center gap-3 px-5 py-3 bg-[#F2F2F2] text-[#1C1C1C] font-semibold border-r-[3px] border-[#F1E52F] transition-colors">
        <i data-lucide="building-2" class="w-5 h-5 shrink-0"></i>
        <span class="sidebar-label">Gestao de Empresas</span>
      </a>
    </nav>
    <button onclick="toggleSidebar()" class="p-3 border-t border-[#DADADA] text-[#575756] hover:bg-[#F2F2F2]">
      <i data-lucide="panel-left-close" class="w-5 h-5"></i>
    </button>
  </aside>

  <!-- Main -->
  <div id="main-wrapper" class="transition-all duration-300 ml-[260px]">
    <header class="h-2 bg-[#F1E52F]"></header>

    <div class="bg-white border-b border-[#DADADA] px-8 py-4 flex items-center justify-between">
      <h1 class="text-xl font-bold text-[#1C1C1C]">Gestao de Empresas</h1>
      <div class="flex items-center gap-3">
        <span class="text-sm text-[#575756]">Admin: <strong>Pedro Admin</strong></span>
        <div class="w-9 h-9 bg-[#F2F2F2] rounded-full flex items-center justify-center">
          <i data-lucide="shield" class="w-4 h-4 text-[#575756]"></i>
        </div>
      </div>
    </div>

    <main class="bg-[#F2F2F2] min-h-screen p-8">
      <!-- Info banner -->
      <div class="bg-yellow-50 border border-yellow-200 rounded-xl p-4 mb-6 flex items-start gap-3">
        <i data-lucide="info" class="w-5 h-5 text-yellow-600 shrink-0 mt-0.5"></i>
        <div class="text-sm text-yellow-800">
          <strong>Configuracao global.</strong> Empresas desativadas nao serao visiveis para nenhum vendedor do callcenter, independente das permissoes individuais. Clique em "Lojas" para controlar lojas especificas de cada empresa.
        </div>
      </div>

      <!-- Search -->
      <div class="flex items-center gap-4 mb-6">
        <div class="flex-1 relative">
          <i data-lucide="search" class="w-4 h-4 text-[#575756] absolute left-3 top-1/2 -translate-y-1/2"></i>
          <input type="text" placeholder="Buscar empresa por nome ou ID..." class="w-full border border-[#DADADA] rounded-lg pl-10 pr-4 py-2.5 text-sm focus:border-[#F1E52F] focus:outline-none">
        </div>
        <span class="text-sm text-[#575756]">98 empresas cadastradas</span>
      </div>

      <!-- Companies list -->
      <div class="bg-white rounded-xl shadow-sm overflow-hidden">
        <div class="overflow-x-auto">
          <table class="w-full">
            <thead>
              <tr class="bg-[#1C1C1C] text-white text-xs uppercase tracking-wide">
                <th class="px-6 py-3 text-left font-semibold">ID</th>
                <th class="px-6 py-3 text-left font-semibold">Empresa</th>
                <th class="px-6 py-3 text-center font-semibold">Lojas</th>
                <th class="px-6 py-3 text-center font-semibold">Status</th>
                <th class="px-6 py-3 text-center font-semibold">Acoes</th>
              </tr>
            </thead>
            <tbody class="text-sm" id="companies-table">
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-6 py-4"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP001</span></td>
                <td class="px-6 py-4 font-medium text-[#1C1C1C]">NutriVida Suplementos</td>
                <td class="px-6 py-4 text-center text-[#575756]">4 lojas</td>
                <td class="px-6 py-4 text-center">
                  <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" checked class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
                  </label>
                </td>
                <td class="px-6 py-4 text-center">
                  <button onclick="openLojasModal('NutriVida Suplementos', ['Loja SP Centro', 'Loja SP Zona Sul', 'Loja RJ Centro', 'Loja MG BH'])" class="border border-[#DADADA] text-[#575756] rounded-lg px-4 py-2 text-xs font-medium hover:bg-[#F2F2F2] transition inline-flex items-center gap-1.5">
                    <i data-lucide="store" class="w-3.5 h-3.5"></i> Lojas
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-6 py-4"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP002</span></td>
                <td class="px-6 py-4 font-medium text-[#1C1C1C]">BelezaPura Cosmeticos</td>
                <td class="px-6 py-4 text-center text-[#575756]">3 lojas</td>
                <td class="px-6 py-4 text-center">
                  <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" checked class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
                  </label>
                </td>
                <td class="px-6 py-4 text-center">
                  <button onclick="openLojasModal('BelezaPura Cosmeticos', ['Loja Online', 'Loja Fisica SP', 'Loja Fisica RJ'])" class="border border-[#DADADA] text-[#575756] rounded-lg px-4 py-2 text-xs font-medium hover:bg-[#F2F2F2] transition inline-flex items-center gap-1.5">
                    <i data-lucide="store" class="w-3.5 h-3.5"></i> Lojas
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA] bg-red-50/50">
                <td class="px-6 py-4"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP003</span></td>
                <td class="px-6 py-4 font-medium text-[#1C1C1C] line-through opacity-60">FitMax Esportes</td>
                <td class="px-6 py-4 text-center text-[#575756] opacity-60">5 lojas</td>
                <td class="px-6 py-4 text-center">
                  <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
                  </label>
                </td>
                <td class="px-6 py-4 text-center">
                  <button disabled class="border border-[#DADADA] text-[#DADADA] rounded-lg px-4 py-2 text-xs font-medium cursor-not-allowed inline-flex items-center gap-1.5">
                    <i data-lucide="store" class="w-3.5 h-3.5"></i> Lojas
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-6 py-4"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP004</span></td>
                <td class="px-6 py-4 font-medium text-[#1C1C1C]">DermaPlus Saude</td>
                <td class="px-6 py-4 text-center text-[#575756]">2 lojas</td>
                <td class="px-6 py-4 text-center">
                  <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" checked class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
                  </label>
                </td>
                <td class="px-6 py-4 text-center">
                  <button onclick="openLojasModal('DermaPlus Saude', ['Loja Principal', 'Loja Outlet'])" class="border border-[#DADADA] text-[#575756] rounded-lg px-4 py-2 text-xs font-medium hover:bg-[#F2F2F2] transition inline-flex items-center gap-1.5">
                    <i data-lucide="store" class="w-3.5 h-3.5"></i> Lojas
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-6 py-4"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP005</span></td>
                <td class="px-6 py-4 font-medium text-[#1C1C1C]">VitalFarma</td>
                <td class="px-6 py-4 text-center text-[#575756]">6 lojas</td>
                <td class="px-6 py-4 text-center">
                  <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" checked class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
                  </label>
                </td>
                <td class="px-6 py-4 text-center">
                  <button onclick="openLojasModal('VitalFarma', ['Loja SP', 'Loja RJ', 'Loja BH', 'Loja CWB', 'Loja POA', 'Loja SSA'])" class="border border-[#DADADA] text-[#575756] rounded-lg px-4 py-2 text-xs font-medium hover:bg-[#F2F2F2] transition inline-flex items-center gap-1.5">
                    <i data-lucide="store" class="w-3.5 h-3.5"></i> Lojas
                  </button>
                </td>
              </tr>
              <tr class="border-b border-[#F2F2F2] hover:bg-[#FAFAFA]">
                <td class="px-6 py-4"><span class="bg-[#F2F2F2] text-[#575756] text-xs font-semibold px-2 py-1 rounded">EMP006</span></td>
                <td class="px-6 py-4 font-medium text-[#1C1C1C]">CorpoIdeal Fitness</td>
                <td class="px-6 py-4 text-center text-[#575756]">3 lojas</td>
                <td class="px-6 py-4 text-center">
                  <label class="relative inline-flex items-center cursor-pointer">
                    <input type="checkbox" checked class="sr-only peer">
                    <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
                  </label>
                </td>
                <td class="px-6 py-4 text-center">
                  <button onclick="openLojasModal('CorpoIdeal Fitness', ['Loja Online', 'Loja SP', 'Loja RJ'])" class="border border-[#DADADA] text-[#575756] rounded-lg px-4 py-2 text-xs font-medium hover:bg-[#F2F2F2] transition inline-flex items-center gap-1.5">
                    <i data-lucide="store" class="w-3.5 h-3.5"></i> Lojas
                  </button>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- Pagination -->
        <div class="flex items-center justify-between px-6 py-3 border-t border-[#F2F2F2]">
          <span class="text-xs text-[#575756]">Pagina 1 de 10</span>
          <div class="flex gap-1">
            <button class="px-3 py-1.5 text-xs font-bold bg-[#1C1C1C] text-white rounded">1</button>
            <button class="px-3 py-1.5 text-xs font-medium text-[#575756] border border-[#DADADA] rounded hover:bg-[#F2F2F2]">2</button>
            <button class="px-3 py-1.5 text-xs font-medium text-[#575756] border border-[#DADADA] rounded hover:bg-[#F2F2F2]">3</button>
            <button class="px-3 py-1.5 text-xs font-medium text-[#575756] border border-[#DADADA] rounded hover:bg-[#F2F2F2]">Prox</button>
          </div>
        </div>
      </div>
    </main>
  </div>

  <!-- Modal Lojas -->
  <div id="modal-lojas" class="fixed inset-0 bg-black/50 z-[100] hidden items-center justify-center">
    <div class="bg-white rounded-xl shadow-lg w-full max-w-lg mx-4">
      <div class="flex items-center justify-between p-6 border-b border-[#F2F2F2]">
        <div>
          <h3 class="font-bold text-[#1C1C1C]">Lojas</h3>
          <p class="text-sm text-[#575756]" id="modal-empresa-name"></p>
        </div>
        <button onclick="closeLojasModal()" class="text-[#575756] hover:text-[#1C1C1C]">
          <i data-lucide="x" class="w-5 h-5"></i>
        </button>
      </div>
      <div class="p-6" id="modal-lojas-list">
        <!-- Populated by JS -->
      </div>
      <div class="flex justify-end gap-3 p-6 border-t border-[#F2F2F2]">
        <button onclick="closeLojasModal()" class="border border-[#DADADA] text-[#575756] rounded-lg px-5 py-2.5 text-sm font-medium hover:bg-[#F2F2F2]">
          Cancelar
        </button>
        <button onclick="closeLojasModal()" class="bg-[#F1E52F] text-[#1C1C1C] font-bold rounded-lg px-5 py-2.5 text-sm hover:brightness-95">
          Salvar
        </button>
      </div>
    </div>
  </div>

  <script>
    lucide.createIcons();

    let sidebarExpanded = true;
    function toggleSidebar() {
      const sidebar = document.getElementById('sidebar');
      const main = document.getElementById('main-wrapper');
      sidebarExpanded = !sidebarExpanded;
      if (sidebarExpanded) {
        sidebar.classList.replace('w-[72px]', 'w-[260px]');
        main.classList.replace('ml-[72px]', 'ml-[260px]');
        document.querySelectorAll('.sidebar-label').forEach(el => el.classList.remove('hidden'));
        document.querySelectorAll('.sidebar-expanded').forEach(el => el.classList.remove('hidden'));
        document.querySelectorAll('.sidebar-compact').forEach(el => el.classList.add('hidden'));
      } else {
        sidebar.classList.replace('w-[260px]', 'w-[72px]');
        main.classList.replace('ml-[260px]', 'ml-[72px]');
        document.querySelectorAll('.sidebar-label').forEach(el => el.classList.add('hidden'));
        document.querySelectorAll('.sidebar-expanded').forEach(el => el.classList.add('hidden'));
        document.querySelectorAll('.sidebar-compact').forEach(el => el.classList.remove('hidden'));
      }
    }

    function openLojasModal(empresaName, lojas) {
      document.getElementById('modal-empresa-name').textContent = empresaName;
      const list = document.getElementById('modal-lojas-list');
      list.innerHTML = lojas.map(loja => `
        <div class="flex items-center justify-between py-3 border-b border-[#F2F2F2] last:border-0">
          <span class="text-sm font-medium text-[#1C1C1C]">${loja}</span>
          <label class="relative inline-flex items-center cursor-pointer">
            <input type="checkbox" checked class="sr-only peer">
            <div class="w-11 h-6 bg-gray-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full rtl:peer-checked:after:-translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:start-[2px] after:bg-white after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-green-500"></div>
          </label>
        </div>
      `).join('');
      const modal = document.getElementById('modal-lojas');
      modal.classList.remove('hidden');
      modal.classList.add('flex');
    }

    function closeLojasModal() {
      const modal = document.getElementById('modal-lojas');
      modal.classList.add('hidden');
      modal.classList.remove('flex');
    }
  </script>
</body>
</html>
```

- [ ] **Step 2: Verify in browser**

Open `pages/gestao-empresas.html`. Verify:
- Admin sidebar with 4 items, "Gestao de Empresas" highlighted
- Yellow info banner at top explaining global config
- Search bar with company count
- Table with 6 companies, dark header
- EMP003 (FitMax) is OFF: name struck through, opacity reduced, Lojas button disabled
- Toggle switches work visually
- Click "Lojas" on any active company → modal with store list and individual toggles
- Modal has Cancelar / Salvar buttons

- [ ] **Step 3: Commit**

```bash
git add projetos/modulo-callcenter-appcall/pages/gestao-empresas.html
git commit -m "feat(appcall): add gestao de empresas page with toggles and lojas modal"
```

---

## Task 4: P4 — Gestao de Usuarios (Admin/Gerente)

**Files:**
- Create: `projetos/modulo-callcenter-appcall/pages/gestao-usuarios.html`

Users management page. Listing with filters, badges per profile. "Editar" opens edit modal. "Permissoes" opens permissions modal with checkboxes for companies and lead types.

**Mock data:** 8 users with mix of Admin/Gerente/Vendedor profiles.

- [ ] **Step 1: Create gestao-usuarios.html**

Create `projetos/modulo-callcenter-appcall/pages/gestao-usuarios.html`. This file follows the same structure as the previous pages but with:

**Sidebar:** Admin version (4 items), "Gestao de Usuarios" highlighted.

**Page header bar:** Title "Gestao de Usuarios" + right side with admin name.

**Content area:**
- Filter row: Status select (Todos/Ativo/Inativo) + search input + "Novo Usuario" button (yellow primary)
- Stats badges: "8 usuarios · 1 admin · 2 gerentes · 5 vendedores"
- Table with dark header, columns: Nome, Email, Perfil (badge), Gerente Vinculado, Ativo (toggle), Acoes (Editar + Permissoes buttons)
- 8 mock user rows:
  - Pedro Admin (Admin, ativo)
  - Maria Gerente (Gerente, ativo)
  - Ricardo Gerente (Gerente, ativo)
  - Joao Silva (Vendedor, vinculado a Maria, ativo)
  - Ana Santos (Vendedor, vinculado a Maria, ativo)
  - Lucas Mendes (Vendedor, vinculado a Ricardo, ativo)
  - Fernanda Costa (Vendedor, vinculado a Ricardo, ativo)
  - Carlos Oliveira (Vendedor, vinculado a Maria, **inativo**)
- Profile badges: Admin = `bg-red-100 text-red-700`, Gerente = `bg-blue-100 text-blue-700`, Vendedor = `bg-green-100 text-green-700`
- Vendedor rows show their linked gerente name. Admin/Gerente rows show "—"
- Inactive user row has `opacity-60` on the name

**Edit User Modal** (`id="modal-editar"`):
- Fields: Nome (text input), Email (text input), Perfil (select: Admin/Gerente/Vendedor), Gerente Vinculado (select, shown only for Vendedor profile)
- Toggle ativo/inativo
- Cancelar + Salvar buttons

**Permissions Modal** (`id="modal-permissoes"`):
- Two sections side by side:
  - Left: "Empresas permitidas" — checkboxes for: NutriVida, BelezaPura, DermaPlus, VitalFarma, CorpoIdeal (only companies ON in camada 1, excluding FitMax which is OFF)
  - Right: "Tipos de lead permitidos" — checkboxes for all 10 lead types from the spec
- Some checkboxes pre-checked to show it's not all-or-nothing
- Cancelar + Salvar buttons

**JS:** Same sidebar toggle as other pages + modal open/close functions for both modals.

- [ ] **Step 2: Verify in browser**

Open `pages/gestao-usuarios.html`. Verify:
- Admin sidebar, "Gestao de Usuarios" highlighted
- Filter bar with status select + search + yellow "Novo Usuario" button
- Stats badges showing user counts
- Table with 8 rows, correct profile badges
- Vendedores show linked gerente name
- Carlos Oliveira is visually dimmed (inactive)
- "Editar" button → edit modal with fields
- "Permissoes" button → permissions modal with checkboxes for companies and lead types
- Both modals open/close properly

- [ ] **Step 3: Commit**

```bash
git add projetos/modulo-callcenter-appcall/pages/gestao-usuarios.html
git commit -m "feat(appcall): add gestao de usuarios page with edit and permissions modals"
```

---

## Task 5: P1 — Dashboard do Vendedor

**Files:**
- Create: `projetos/modulo-callcenter-appcall/pages/dashboard-vendedor.html`

The most complex page. Based on `dash-nova-incrivel-3.html` reference but rebuilt with Pagah design system (white sidebar, yellow header, #F2F2F2 background, Tailwind CSS).

**Key sections to implement (in order within the page):**

1. **Page header**: "Dashboard Vendedor" + date range display
2. **Resultados Financeiros**: 3 KPI cards (Comissao Liquida dark card, Vendas Aprovadas, Aguardando Pagamento)
3. **Debitos**: 2 cards (Debitos do Periodo, Debitos Retroativos with red tint)
4. **Resultados da Semana**: 4 compact gamification cards (Ticket Medio, Total Bonus, Maior Venda, Bonus Batidos) with position badges and leader comparisons
5. **Ranking de Vendedores**: Table with position medals, "VOCE" highlight row, conversion bars
6. **Detalhamento**: Tabbed table (Extrato de Vendas / Ocorrencias / Retroativos) with toolbar filters

**Sidebar:** Vendedor version (2 items), "Dashboard" highlighted.

**Mock data must match the reference file closely** — same structure, similar values, realistic Brazilian financial data in BRL.

**Modals:**
- Comissao por Origem: dark header with gross/deductions/net summary, table by origin with expandable company breakdown
- Bonus Batidos: ranking table of sellers who hit bonuses

**JS:** Sidebar toggle + tab switching for detalhamento + modal open/close + collapsible rows in comissao modal.

This is a large file. Implement all sections following the exact Tailwind conventions defined in the Conventions section above. Use the mock data structure from the reference `dash-nova-incrivel-3.html` but apply Pagah design system colors and components.

- [ ] **Step 1: Create dashboard-vendedor.html**

Create `projetos/modulo-callcenter-appcall/pages/dashboard-vendedor.html` with the complete page including all 6 sections, 2 modals, sidebar, and JS. Follow the conventions section exactly for all Tailwind classes.

The file should be fully self-contained and match the structure described in spec section "P1 — Dashboard do Vendedor". Reference `dash-nova-incrivel-3.html` for the data layout but apply Pagah design system visuals throughout.

- [ ] **Step 2: Verify in browser**

Open `pages/dashboard-vendedor.html`. Verify each section:
- White sidebar with PagCall logo, Dashboard highlighted
- Yellow strip header
- 3 KPI cards (dark comissao card with yellow accent, green vendas, blue aguardando)
- 2 debito cards (gray period, red retro)
- 4 gamification cards with position badges and leader comparisons
- Ranking table with medals, VOCE highlighted row, conversion bars
- Tabbed detalhamento: switch between Vendas / Ocorrencias / Retroativos
- Click comissao card → modal with origin breakdown
- Click bonus batidos "Ver todos" → modal with full ranking

- [ ] **Step 3: Commit**

```bash
git add projetos/modulo-callcenter-appcall/pages/dashboard-vendedor.html
git commit -m "feat(appcall): add dashboard vendedor page with KPIs, ranking, detalhamento and modals"
```

---

## Task 6: P2 — Dashboard do Gerente/Admin

**Files:**
- Create: `projetos/modulo-callcenter-appcall/pages/dashboard-gerente.html`

Derived from dashboard-vendedor.html with these modifications:

**Sidebar:** Admin version (4 items), "Dashboard Geral" highlighted.

**Page header:** "Dashboard Geral" + date range + additional filter selects (Vendedor, Equipe).

**Changes from vendedor dashboard:**
1. KPIs show **consolidated** values (larger numbers — sum of all sellers)
2. "Resultados da Semana" cards show **only TOPs** — no "Seu" comparisons. Each card shows: metric name, leader name, leader value. No position badge for "you".
3. Ranking shows **all** sellers without "VOCE" highlight
4. **Additional KPI row**: "Atendimento" section with 2 cards: "Leads Atendidos Hoje" (count + percentage) and "Leads Pendentes" (count)
5. Filter toolbar in Detalhamento adds "Vendedor" select
6. Admin name in header instead of vendedor name

**Mock data:** Consolidated financial values (multiply vendedor values by ~8 to simulate 8 sellers). Leads atendidos: 342 (78%), Leads pendentes: 95.

- [ ] **Step 1: Create dashboard-gerente.html**

Copy the structure from dashboard-vendedor.html (Task 5) and apply the modifications listed above. The file must be self-contained with its own sidebar (admin 4-item version), header, and full content.

- [ ] **Step 2: Verify in browser**

Open `pages/dashboard-gerente.html`. Verify:
- Admin sidebar (4 items), "Dashboard Geral" highlighted
- Extra filters in header (Vendedor select, Equipe select)
- KPIs with consolidated (larger) values
- Gamification cards show only TOPs (no personal comparison)
- Atendimento section with leads atendidos/pendentes
- Ranking without "VOCE" highlight
- All tabs and modals work

- [ ] **Step 3: Commit**

```bash
git add projetos/modulo-callcenter-appcall/pages/dashboard-gerente.html
git commit -m "feat(appcall): add dashboard gerente page with consolidated KPIs and admin filters"
```

---

## Task 7: Final Review + Update Index

**Files:**
- Modify: `projetos/modulo-callcenter-appcall/index.html`

- [ ] **Step 1: Open all pages and verify cross-links**

Open each page and click through the sidebar navigation. Verify all links point to existing pages and the correct sidebar item is highlighted on each page.

- [ ] **Step 2: Verify visual consistency**

Check all pages side by side for:
- Same sidebar width and style
- Same yellow header strip height
- Same card border-radius and shadows
- Same table header style (#1C1C1C with white text)
- Same button styles (yellow primary, outline secondary)
- Same typography (Ubuntu, same sizes)
- Same badge styles across pages

- [ ] **Step 3: Commit final state**

```bash
git add -A projetos/modulo-callcenter-appcall/
git commit -m "feat(appcall): complete all 5 prototype pages for callcenter module"
```
