# CLAUDE.md — Modulo AppCall (PagCall)

Este e o modulo de Call Center da Pagah, operando sob a sub-marca **PagCall**. E um modulo **separado** da plataforma principal, com sidebar e navegacao proprias.

---

## Contexto do Modulo

O AppCall permite que vendedores atendam leads de empresas clientes, com controle de acesso em duas camadas e dashboards financeiras por perfil. Veja o spec completo em `docs/superpowers/specs/2026-03-31-modulo-appcall-design.md`.

### Perfis de Acesso
- **Admin Pagah** — controle total, gestao de empresas + usuarios
- **Gerente de Operacao** — dashboard geral, gestao dos seus vendedores
- **Vendedor** — dashboard pessoal, tela de leads/CRM

---

## Design System

Este modulo segue a identidade visual da Pagah mas com sidebar propria (nao usa os 11 itens da Pagah principal).

### Regras visuais obrigatorias
- Consultar `../../pagah-design-system.md` para cores, tipografia, componentes e padroes
- **Header:** barra amarela `#F1E52F` fixa no topo, 56px, com icone PagCall + hamburger + notificacoes + perfil
- **Sidebar:** branca `#FFFFFF`, com os 4 itens do modulo (ver abaixo), toggle compacto/expandido
- **Background:** `#F2F2F2`
- **Icones:** Google Material Symbols Rounded (nunca Lucide, Font Awesome ou outros)
- **Tailwind:** via CDN, sem prefix (o modulo nao usa Bootstrap)

### Itens da Sidebar (4 itens, nesta ordem)

| # | Label | Icone Material Symbols | Visivel para |
|---|-------|----------------------|-------------|
| 1 | Dashboard Geral | `space_dashboard` | Admin, Gerente |
| 2 | Leads / Atendimento | `assignment` | Todos |
| 3 | Gestao de Usuarios | `group` | Admin, Gerente |
| 4 | Gestao de Empresas | `domain` | Admin only |

**Nota:** O Dashboard do Vendedor usa os mesmos 2 primeiros itens (Dashboard + Leads).

### Logo
- No header (a partir de `pages/`): usar `../assets/pagcall/logo/svg/Icone.svg` (icone PagCall)
- Na sidebar (se expandida): usar `../assets/pagcall/logo/svg/Logo-alternativo.svg`
- A partir da raiz do modulo: usar `assets/pagcall/logo/svg/...`
- Sempre minusculas: "pagcall" (nunca "PagCall" ou "PAGCALL" no logo)

---

## Paginas do Modulo

| Arquivo | Descricao | Perfil |
|---------|-----------|--------|
| `pages/dashboard-vendedor.html` | KPIs, gamificacao, ranking, extrato | Vendedor |
| `pages/dashboard-gerente.html` | Metricas consolidadas, filtros, ranking global | Admin/Gerente |
| `pages/leads-atendimento.html` | CRM de leads, filtros, atendimento | Todos |
| `pages/gestao-usuarios.html` | CRUD vendedores, permissoes individuais | Admin/Gerente |
| `pages/gestao-empresas.html` | Toggles ON/OFF de empresas e lojas | Admin |
| `index.html` | Pagina indice com links para todos os prototipos | — |

### Arquivo de referencia (nao editar)
- `referencias-dash-antiga.html` — versao antiga do dashboard, mantida apenas como referencia historica

---

## PROIBICOES

1. **NUNCA** usar sidebar escura (`#1C1C1C`) — sidebar e SEMPRE branca
2. **NUNCA** usar header como linha fina de 2px — e uma barra de 56px
3. **NUNCA** usar Lucide, Font Awesome ou icones solid/filled
4. **NUNCA** omitir itens da sidebar do modulo
5. **NUNCA** misturar a sidebar do AppCall com a sidebar da Pagah principal (sao modulos separados)
6. **NUNCA** usar cores fora da paleta Pagah como cores de marca

---

## Ao Criar Novas Paginas

1. Copiar o shell (header + sidebar) de uma pagina existente do modulo (ex: `pages/gestao-empresas.html`)
2. Ajustar o item ativo na sidebar
3. Substituir o conteudo dentro do `<main>` ou div principal
4. Verificar visualmente contra as screenshots do design system Pagah
5. Garantir que o toggle da sidebar funciona
