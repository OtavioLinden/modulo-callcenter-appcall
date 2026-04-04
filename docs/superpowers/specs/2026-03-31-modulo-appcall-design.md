# Modulo AppCall — Design Spec

**Data:** 2026-03-31
**Projeto:** `projetos/modulo-callcenter-appcall/`
**Tipo:** Prototipos visuais (frontend only, sem backend)
**Stack:** HTML + Tailwind CSS + dados mockados
**Design System:** `pagah-design-system.md` (sidebar branca, header amarelo, cards brancos, fundo gelo #F2F2F2)

---

## 1. Visao Geral

O modulo AppCall e o sistema de call center da Pagah (sub-marca PagCall). Permite que vendedores atendam leads de empresas clientes, com controle de acesso em duas camadas e dashboards financeiras por perfil.

**Contexto operacional:**
- ~50 callcenters ativos hoje, crescendo para ~150 ate final de 2026
- ~100 empresas ativas (crescendo significativamente)
- Cada empresa tem N lojas
- Cada vendedor e um usuario individual de callcenter

---

## 2. Perfis de Acesso

### Admin Pagah
- Controle total do sistema
- Dashboard geral consolidada (todos os vendedores/equipes)
- Gestao de TODOS os usuarios + permissoes individuais de qualquer vendedor
- Gestao de empresas (on/off global) — EXCLUSIVO do admin
- Pode promover outros usuarios a admin ou gerente
- Pode vincular vendedores a gerentes

### Gerente de Operacao
- Dashboard geral (visao apenas dos vendedores **vinculados a ele**)
- Acompanha metricas e ranking dos seus vendedores
- Gestao de usuarios — apenas dos vendedores **vinculados a ele** (editar dados, ajustar permissoes individuais)
- **NAO** tem acesso a Gestao de Empresas (camada 1 e exclusiva do admin)
- **NAO** pode criar/promover admins ou outros gerentes

### Vendedor (Operador)
- Dashboard pessoal com gamificacao
- Tela de leads / CRM
- Ve apenas leads que passam nas duas camadas de permissao
- Atende leads e confirma atendimento

---

## 3. Modelo de Acesso (2 Camadas)

### Camada 1 — Gestao de Empresas (global)

Configuracao global que afeta TODOS os vendedores. Gerenciada pelo Admin em menu proprio ("Gestao de Empresas").

- Lista todas as empresas com toggle ON/OFF
- Tudo ON por padrao
- Botao "Lojas" em cada empresa abre modal com lojas e toggles individuais
- Empresa OFF = nenhum vendedor ve leads dela, independente de permissoes individuais

### Camada 2 — Permissoes do Vendedor (individual)

Configuracao por vendedor, dentro da "Gestao de Usuarios".

- Cada vendedor tem **empresas permitidas** (dentre as que estao ON na camada 1)
- Cada vendedor tem **tipos de lead permitidos**

### Regra de resolucao

Um vendedor so ve um lead se:
1. A empresa do lead esta ON na camada 1
2. A loja do lead esta ON na camada 1
3. A empresa esta nas permissoes individuais do vendedor (camada 2)
4. O tipo de lead esta nas permissoes individuais do vendedor (camada 2)

**Exemplo:** Gerente libera empresas A, B, C (camada 1). Vendedor Joao tem permissao para empresas A e B, tipos "Abandonos" e "Cartao Recusado" (camada 2). Joao so ve leads de abandono e cartao recusado das empresas A e B.

---

## 4. Paginas do Modulo

### P1 — Dashboard do Vendedor

Pagina inicial do vendedor. Referencia: `dash-nova-incrivel-3.html`.

**Header:**
- Titulo "Dashboard Vendedor"
- Date Range Picker (seletor de periodo)

**Secao: Resultados Financeiros** (3 KPI cards em row)
- **Comissao Liquida** (card destaque escuro #1C1C1C, borda amarela): valor em R$, qtd transacoes, clicavel → modal com breakdown por origem de vendas (tabela expansivel por empresa)
- **Vendas Aprovadas**: valor em R$, qtd vendas, clicavel → modal detalhes
- **Aguardando Pagamento**: valor em R$, qtd pedidos pendentes

**Secao: Debitos** (2 cards em row)
- **Debitos do Periodo**: soma de estornos + chargebacks do periodo, com contagem e tooltip explicativo
- **Debitos Retroativos** (card com fundo vermelho sutil): estornos/CB de periodos anteriores descontados retroativamente

**Secao: Resultados da Semana** (4 cards compactos com gamificacao)
- **Seu Ticket Medio**: valor, posicao no ranking, comparacao com lider
- **Seu Total Bonus**: valor, posicao, quanto falta pro lider
- **Maior Venda (equipe)**: quem fez, valor, comparacao com a sua maior
- **Bonus Batidos (equipe)**: lista de quem bateu meta, sua posicao, link "Ver todos" → modal

**Secao: Ranking de Vendedores** (tabela)
- Colunas: Posicao, Vendedor, Total Vendido, Pedidos, Ticket Medio, Bonus do Mes, Conversao (com barra visual)
- Linha do vendedor logado destacada com badge "VOCE" e background diferenciado
- Medalhas visuais para top 3

**Secao: Detalhamento** (card com toolbar + abas)
- Filtros: Empresa (select), Metodo de pagamento (select), Busca livre (texto)
- Aba "Extrato de Vendas": ID Empresa, ID Pedido, Data, Cliente/CPF, Origem, Telefone, Metodo, Status, Valor Venda, Comissao, Acoes
- Aba "Ocorrencias (Periodo)": ID Empresa, ID Pedido, Data, Cliente/CPF, Origem, Tipo (Estorno/Chargeback), Motivo, Valor, Status, Acoes
- Aba "Retroativos" (com badge de contagem e fundo vermelho): mesmas colunas de ocorrencias, linhas com fundo vermelho sutil
- Paginacao na base

**Modais:**
- Comissao por Origem: resumo (bruto → deducoes → liquido), tabela por origem expansivel com breakdown por empresa
- Bonus Batidos: ranking completo de quem bateu meta com valores

### P2 — Dashboard do Gerente/Admin

Mesma estrutura da dashboard do vendedor, com estas diferencas:

- KPIs financeiros sao **consolidados** (todos os vendedores)
- Secao "Resultados da Semana" mostra apenas os **TOPs** de cada metrica (sem comparacao pessoal "seu")
- Ranking completo de todos os vendedores
- **Filtros adicionais**: por vendedor especifico, por equipe de gerente
- **Metricas adicionais**: leads atendidos vs pendentes
- Detalhamento com as mesmas abas

### P3 — Leads / Atendimento (CRM)

Tela principal de trabalho do vendedor. Acessivel por todos os perfis.

**Filtros:**
- Empresa (select — mostra apenas empresas permitidas pro vendedor)
- Tipo de Lead (select — mostra apenas tipos permitidos)
- Nome (texto)
- Telefone (texto)
- CPF (texto)
- Valor (select ou range)
- Periodo inicial / Periodo final (date pickers)
- Toggle "Somente nao atendidos"

**Colunas da tabela:**
| Coluna | Descricao |
|--------|-----------|
| ID Empresa | Identificador da empresa |
| Nome Cliente | Nome do contato/lead |
| Telefone | Numero para contato |
| Data Ocorrencia | Data do evento (abandono, recusa, etc.) |
| Erro | Motivo quando aplicavel (ex: bandeira recusada) |
| Endereco | Endereco completo preenchido |
| KIT | Produto + valor total do pedido/abandono |
| Acoes | Botao "Atender" |

**Fluxo de atendimento:**
1. Vendedor clica em "Atender" no lead
2. Abre em **nova guia** o cadastro/pedido do cliente (sistema Pagah principal — fora do prototipo)
3. Na tela de leads, abre **modal de confirmacao de atendimento** (simples, apenas botao "Confirmar Atendimento")
4. Lead marcado como atendido

### P4 — Gestao de Usuarios (Admin/Gerente)

**Listagem:**
- Filtros: status (ativo/inativo), busca por nome/email
- Botao "Novo Usuario" (amarelo, primario)
- Tabela com colunas: Nome, Email, Perfil (badge: Admin/Gerente/Vendedor), Ativo (toggle), Acoes (editar, permissoes)

**Edicao de usuario (modal ou pagina):**
- Campos: Nome, Email, Perfil (select: Admin, Gerente, Vendedor)
- Toggle ativo/inativo

**Permissoes individuais (dentro da edicao ou secao dedicada):**
- **Empresas permitidas**: lista com checkboxes (mostra apenas empresas ON na camada 1)
- **Tipos de lead permitidos**: lista com checkboxes de todos os tipos

### P5 — Gestao de Empresas (Admin)

**Listagem:**
- Busca/filtro por nome de empresa
- Lista de empresas com:
  - Nome da empresa
  - ID da empresa
  - Toggle ON/OFF (tudo ON por padrao)
  - Botao "Lojas"
- Paginacao (para suportar 100+ empresas)

**Modal de Lojas (ao clicar "Lojas" em uma empresa):**
- Lista todas as lojas da empresa
- Toggle ON/OFF individual por loja
- Tudo ON por padrao
- Botao salvar

---

## 5. Sidebar / Navegacao

Sidebar segue o padrao Pagah: branca, compactavel (icones) / expandivel (icones + labels).

**Vendedor ve:**
1. Dashboard (icone: grafico)
2. Leads / Atendimento (icone: lista/clipboard)

**Gerente ve:**
1. Dashboard Geral (icone: grafico) — filtrado pelos vendedores vinculados a ele
2. Leads / Atendimento (icone: lista/clipboard)
3. Gestao de Usuarios (icone: pessoas/grupo) — apenas vendedores vinculados

**Admin ve:**
1. Dashboard Geral (icone: grafico) — visao de todos
2. Leads / Atendimento (icone: lista/clipboard)
3. Gestao de Usuarios (icone: pessoas/grupo) — todos os usuarios
4. Gestao de Empresas (icone: predio/empresa) — EXCLUSIVO admin

O logo PagCall (foguete com telefone) aparece no topo da sidebar.

---

## 6. Tipos de Lead (existentes)

1. Cartoes Aprovados KITS Site
2. Cartoes Aprovados AMOSTRAS Site
3. Recuperacao de amostras recusados SITE
4. Abandonos Site
5. Pix Pagos Site
6. Pix Pendentes Site
7. Boletos Pagos Site
8. Boletos Pendentes Site
9. Cartao Recusado KIT Site
10. Cartao Recusado Falta de Saldo

---

## 7. Regras Visuais (Design System Pagah)

- Background geral: `#F2F2F2`
- Cards: brancos, border-radius 12-16px, sombra sutil
- Header: faixa amarela `#F1E52F` no topo
- Sidebar: branca, icones outline/line
- Botao primario: fundo amarelo `#F1E52F`, texto preto
- Tabelas: header `#1C1C1C` texto branco, linhas brancas
- Cards de destaque: fundo `#1C1C1C`, texto branco, acentos amarelos
- Badges de status: pill-shaped (verde=aprovado, vermelho=cancelado, amarelo=pendente, azul=processando)
- Icones: outline/line style ~20-24px (Lucide ou Phosphor)
- Tipografia: Ubuntu (Google Fonts)
- Estilizacao: Tailwind CSS

---

## 8. Ordem de Implementacao Sugerida

1. **Componentes compartilhados** — sidebar, header, layout base (reusavel em todas as paginas)
2. **P3 — Leads / Atendimento** — tela principal do dia a dia, mais critica operacionalmente
3. **P5 — Gestao de Empresas** — camada 1 de acesso
4. **P4 — Gestao de Usuarios** — camada 2 de acesso + CRUD de usuarios
5. **P1 — Dashboard do Vendedor** — mais complexa visualmente, baseada na referencia existente
6. **P2 — Dashboard do Gerente** — derivada da P1 com ajustes
