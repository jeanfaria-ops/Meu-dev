---
tipo: stack
status: ativo
atualizado: 2026-10-16
fonte: inventário da sessão Claude Code (ListConnectors, registro MCP, catálogo de plugins, npm) + carrosséis Instagram (Artisys, @guilhermemorais.ia)
tags: [stack, mcp, plugins, conectores, dev]
---

# Stack de conectores, MCPs e plugins — Utarget / Pico Labs

Atualizado em 16/10/2026. Critério: só entra o que resolve dor real da firma (OSC, editais, prestação de contas, agência). Contagem de estrelas e instalações dos carrosséis não foi verificada e não entra aqui.

## 1. Núcleo — já conectado (manter)

| Conector | Papel | Regra |
|---|---|---|
| Google Drive | Vault NEO e documento oficial | Escrita |
| Google Calendar | Agenda | Escrita |
| Microsoft 365 | Outlook (e-mail da firma), Teams, SharePoint | Somente leitura: texto pronto para envio manual |
| Brevo | CRM | Consulta e rascunho |
| GitHub | Código (`jeanfaria-ops/meu-dev`) | Escrita no branch de trabalho |
| Microsoft Learn | Documentação técnica M365/Azure | Leitura |
| AdWhispr Ads | Anúncios de concorrentes (Pico Labs) | Não lançar campanha sem aprovação |

Pendências de conexão nesta sessão (fato):
- `plugin:github` falha com erro de autorização; o MCP `github` nativo funciona. Remover o plugin duplicado.
- `desktop-commander` deu timeout: é o motor de execução na máquina local (G:). Reconectar no Claude Desktop.
- Notion aparece pedindo login: é legado só de leitura. Não autenticar.
- Asana, BigQuery, Definite, Amplitude, Hex, Monday, ClickUp, Linear, Atlassian, Slack: fora da operação. Desativar para limpar o contexto.

## 2. Adicionar — MCPs (prioridade)

| # | MCP | Onde | Uso na firma | Custo/risco |
|---|---|---|---|---|
| 1 | **Playwright** (`@playwright/mcp`, Apache-2.0) | `.mcp.json` deste repo | Navegar em portais com renderização (Transferegov, SMADS, prefeituras), baixar edital, testar sistemas da Pico Labs | Grátis. Não resolve CAPTCHA |
| 2 | **Firecrawl** (`firecrawl-mcp`, MIT) | `.mcp.json` deste repo; também existe como conector claude.ai | Radar de editais (30_Radar/), extrair PDF/HTML de chamamentos, pesquisa de concorrentes da Pico Labs | Precisa de `FIRECRAWL_API_KEY` (plano pago a confirmar) |
| 3 | **Excalidraw** (conector claude.ai, sem login) | claude.ai → Conectores | Fluxogramas de processo de OSC, organograma, arquitetura de sistemas | Grátis |
| 4 | **Figma** (conector claude.ai) | claude.ai → Conectores | Pico Labs: design → código | Só se houver conta Figma da agência (a confirmar) |
| 5 | **Supabase** (conector claude.ai) | claude.ai → Conectores | Backend dos sistemas internos, se o projeto escolher Supabase em vez de PocketBase | Só quando houver projeto |

Gmail aparece no registro: não conectar (descontinuado).

## 3. Adicionar — plugins e skills

Já disponíveis nesta conta (usar, não instalar de novo): `xlsx`, `docx`, `pptx`, `pdf`, `theme-factory`, `deep-research`, `skill-creator`, `mcp-builder`, `code-review`, `security-review`, `proposta-comercial-utarget`, `precificacao-honorarios-utarget`, `consulta-cnpj-receita`, `radar-noticias-terceiro-setor`.

Precedência: `identidade-visual-utarget` prevalece sobre `theme-factory` e sobre qualquer plugin de marca.

| Plugin (Anthropic Directory) | Decisão | Motivo |
|---|---|---|
| **frontend-design** (Anthropic) | Instalar | Interfaces dos sistemas internos e sites da Pico Labs |
| **code-review** (Anthropic, multiagente para PR) | Instalar | Revisão automática de PR neste repo |
| **Design** (Anthropic: acessibilidade, UX copy, handoff) | Instalar quando Pico Labs tiver projeto de UI | Traz MCPs de Asana/Slack/Notion junto: desativar esses |
| **Brand Voice** (Tribe AI) | Não instalar | Redundante com utarget-os + identidade-visual-utarget |
| `architect-review`, `prd`, `escalation`, `market-research-reports` (carrossel) | Avaliar depois | Fonte é post de Instagram; repositórios não verificados. Ler o SKILL.md antes de instalar |

## 4. Stack de desenvolvimento (bibliotecas open source do carrossel Artisys)

Para quando a firma construir sistema próprio (ex.: painel de clientes OSC, controle de certidões e mandatos).

| Camada | Escolha | Licença | Observação |
|---|---|---|---|
| Painel admin / CRUD | Refine | MIT | Base do sistema interno |
| UI | shadcn/ui + Tailwind | MIT | Tokens da marca Utarget: #0071D4, #FF9100, #404040, Montserrat |
| Tabelas | TanStack Table | MIT | Carteira de clientes, certidões, vencimentos |
| Gráficos | Apache ECharts | Apache-2.0 | Painel do funil e da carteira (tabela v2 × legada) |
| Backend MVP | PocketBase | MIT | SQLite, auth e arquivos num binário. Migrar para Supabase/Postgres se crescer |
| PDF a partir de template | pdfme | MIT | Recibo, proposta, relatório de prestação de contas |
| Testes E2E | Playwright | Apache-2.0 | Mesmo motor do MCP do item 2 |
| Automação | Node-RED | Apache-2.0 | Substituto self-hosted do Make (descontinuado). Só se houver servidor; a confirmar |
| App desktop | Tauri | MIT/Apache-2.0 | Baixa prioridade |
| ERP pronto | ERPNext | GPL-3.0 | Não adotar: copyleft e não atende ITG 2002/OSC sem customização pesada |

## 5. Regra de segurança

- Nenhuma chave no repositório. `.mcp.json` lê `${FIRECRAWL_API_KEY}` do ambiente.
- Versões dos MCPs fixadas (`@playwright/mcp@0.0.83`, `firecrawl-mcp@3.30.2`). Atualizar com revisão.
- Plugin de terceiro (tier community) só entra depois de ler o código.
