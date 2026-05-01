# 🎼 Maestro AI

[![License](https://img.shields.io/badge/Licença-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-green.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-blue.svg)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-61dafb.svg)](https://reactjs.org/)
[![Python](https://img.shields.io/badge/Python-3.9%2B-yellow.svg)](https://python.org/)
[![Playwright](https://img.shields.io/badge/Playwright-1.58%2B-green.svg)](https://playwright.dev/)
[![Claude Code](https://img.shields.io/badge/Claude_Code-compatível-orange.svg)](https://claude.ai/code)

**Maestro AI** é um framework de orquestração multi-agentes construído para o [Claude Code](https://claude.ai/code). Ele permite criar, configurar e executar equipes de agentes de IA especializados — chamadas de **squads** — que automatizam fluxos de trabalho complexos: produção de conteúdo, pipelines de pesquisa, análise de dados, automação de publicações e muito mais.

---

## O que é o Maestro AI?

O Maestro AI é um framework que permite:

- **Criar squads** — equipes de agentes de IA com papéis distintos (pesquisador, escritor, revisor, designer, publicador) que colaboram através de pipelines automatizados
- **Executar pipelines** — squads executam fluxos de trabalho passo a passo com checkpoints, portões de qualidade e condições de veto que garantem resultados de alta qualidade em todas as execuções
- **Integrar skills** — expanda os squads com skills instaláveis que conectam a serviços externos (Apify para scraping, Canva para design, Instagram para publicação, Resend para e-mail e muito mais)
- **Investigar perfis** — antes de construir um squad, o agente de investigação Sherlock pode analisar perfis reais de redes sociais para extrair padrões de conteúdo, ganchos e estilos que informam seus agentes
- **Ver em ação** — um dashboard em tempo real construído com React e Phaser exibe seus agentes trabalhando em um escritório virtual 2D, com atualizações ao vivo via WebSocket

O Maestro AI foi projetado para ser usado inteiramente dentro do Claude Code através de comandos simples como `/maestro create` e `/maestro run`.

---

## Arquitetura

```
maestro-ai/
│
├── _maestro/                       # Core do Maestro AI (não modificar manualmente)
│   ├── config/
│   │   └── playwright.config.json  # Configuração do navegador Playwright
│   ├── core/
│   │   ├── best-practices/         # Guias de boas práticas por plataforma e formato
│   │   │   ├── _catalog.yaml       # Índice dos arquivos de boas práticas disponíveis
│   │   │   ├── instagram-feed.md
│   │   │   ├── copywriting.md
│   │   │   └── ...
│   │   ├── prompts/                # Prompts de fase dos agentes
│   │   │   ├── discovery.prompt.md # Fase 1: Wizard de criação de squad
│   │   │   ├── design.prompt.md    # Fase 3: Arquitetura
│   │   │   ├── build.prompt.md     # Fase 4: Geração de arquivos
│   │   │   ├── sherlock-*.md       # Fase 2: Investigação de perfis
│   │   │   └── ...
│   │   ├── architect.agent.yaml    # Definição do agente Arquiteto
│   │   ├── runner.pipeline.md      # Instruções do executor de pipeline
│   │   └── skills.engine.md        # Instruções do motor de skills
│   ├── _browser_profile/           # Sessões persistentes do navegador (gitignored)
│   ├── _investigations/            # Resultados das investigações do Sherlock
│   ├── _memory/
│   │   ├── company.md              # Contexto persistente da empresa
│   │   └── preferences.md          # Preferências do usuário
│   └── logs/                       # Logs de execução (gitignored)
│
├── squads/                         # Seus squads ficam aqui
│   └── {nome-do-squad}/
│       ├── squad.yaml              # Definição do squad
│       ├── squad-party.csv         # Manifesto dos agentes
│       ├── agents/                 # Arquivos .agent.md dos agentes
│       │   └── {agente}.agent.md
│       ├── pipeline/
│       │   ├── pipeline.yaml       # Ponto de entrada do pipeline
│       │   ├── steps/              # Arquivos de etapas
│       │   └── data/               # Materiais de referência
│       ├── output/                 # Conteúdo gerado (gitignored)
│       │   └── {run-id}/           # Saídas por execução com versionamento
│       └── _memory/
│           ├── memories.md         # Aprendizados persistentes do squad
│           └── runs.md             # Histórico de execuções
│
├── skills/                         # Skills instaladas
│   ├── apify/SKILL.md
│   ├── canva/SKILL.md
│   ├── instagram-publisher/SKILL.md
│   ├── resend/SKILL.md
│   ├── image-ai-generator/SKILL.md
│   └── maestro-skill-creator/      # Ferramenta para criar novas skills
│
├── dashboard/                      # Escritório virtual em tempo real (React + Phaser)
│   ├── src/
│   │   ├── App.tsx
│   │   ├── office/                 # Cena do escritório 2D (Phaser)
│   │   ├── hooks/                  # Hooks WebSocket
│   │   └── store/                  # Estado Zustand
│   └── package.json
│
├── setup/
│   └── maestro-skill/SKILL.md      # Skill do Maestro — copiar para .claude/skills/maestro/
│
├── .env.example                    # Template de variáveis de ambiente
├── .gitignore
├── package.json
├── CLAUDE.md                       # Instruções para o Claude Code
└── README.md
```

---

## Primeiros Passos

### Pré-requisitos

- [Claude Code](https://claude.ai/code) instalado e configurado
- [Node.js](https://nodejs.org/) 18 ou superior
- [Python](https://python.org/) 3.9 ou superior (opcional, para skills baseadas em scripts)
- Git

### Instalação

**1. Clone o repositório**

```bash
git clone https://github.com/otaviopontes/maestro-ai.git
cd maestro-ai
```

**2. Instale as dependências**

```bash
npm install
```

**3. Configure o ambiente**

```bash
cp .env.example .env
```

Abra o `.env` e preencha as credenciais das skills que deseja usar. Você só precisa configurar os serviços que seus squads vão utilizar.

**4. Instale a skill do Maestro no Claude Code**

Copie o arquivo da skill para o diretório de skills do Claude Code:

```bash
# macOS / Linux
mkdir -p ~/.claude/skills/maestro
cp setup/maestro-skill/SKILL.md ~/.claude/skills/maestro/SKILL.md

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.claude\skills\maestro"
Copy-Item setup\maestro-skill\SKILL.md "$env:USERPROFILE\.claude\skills\maestro\SKILL.md"
```

**5. Configure os servidores MCP (opcional)**

Se você planeja usar skills que requerem servidores MCP (Apify, Canva, etc.), crie um arquivo `.mcp.json` na raiz do projeto:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--config", "_maestro/config/playwright.config.json"]
    },
    "apify": {
      "command": "npx",
      "args": ["-y", "@apify/actors-mcp-server@latest"],
      "env": {
        "APIFY_TOKEN": "${APIFY_TOKEN}"
      }
    }
  }
}
```

**6. Abra o projeto no Claude Code e inicie o onboarding**

```bash
claude .
```

Em seguida, no Claude Code:

```
/maestro
```

O assistente de onboarding vai pedir o nome da sua empresa, site e preferências para criar o perfil em `_maestro/_memory/company.md`.

---

## Criando seu Primeiro Squad

```
/maestro create "Pesquisa semanal de tendências e relatório para o meu nicho"
```

O Maestro AI guia você por um wizard de múltiplas fases:

**Fase 1 — Discovery:** Até 8 perguntas para entender seu objetivo, público, formato de saída e frequência.

**Fase 2 — Investigação (opcional):** Compartilhe URLs de perfis do Instagram, YouTube, LinkedIn ou Twitter/X e o agente Sherlock vai analisar conteúdo real para extrair padrões que informam seus agentes.

**Fase 3 — Design:** Pesquisa seu domínio, projeta a equipe de agentes e apresenta a arquitetura do squad para sua aprovação:

```
Vou criar um squad com 3 agentes:

1. 🔍 Rafael Radar — Pesquisador de Tendências
   Tarefas: encontrar-tendências → classificar-por-relevância
2. 📊 Alice Análise — Analista de Insights
   Tarefas: gerar-relatório
3. ✅ Vera Veredito — Revisora de Qualidade
   Tarefas: revisar-relatório

Pipeline: checkpoint → [Rafael] → checkpoint → [Alice] → [Vera] → checkpoint
```

**Fase 4 — Build:** Gera todos os arquivos do squad com personas completas dos agentes, etapas do pipeline, condições de veto, critérios de qualidade e materiais de referência.

### Executando seu Squad

```
/maestro run meu-squad
```

O pipeline executa passo a passo. A cada checkpoint, o Maestro AI solicita sua aprovação antes de continuar.

---

## Skills Disponíveis

| Skill | Tipo | Descrição | Variáveis de Ambiente |
|-------|------|-----------|----------------------|
| `apify` | MCP | Faz scraping de qualquer site, perfis de redes sociais e mecanismos de busca | `APIFY_TOKEN` |
| `canva` | MCP | Cria e exporta designs do Canva a partir de templates | `CANVA_API_KEY` |
| `instagram-publisher` | Script | Publica carrosséis no Instagram via Graph API | `INSTAGRAM_ACCESS_TOKEN`, `INSTAGRAM_USER_ID`, `IMGBB_API_KEY` |
| `resend` | MCP | Envia e-mails transacionais e de marketing | `RESEND_API_KEY` |
| `image-ai-generator` | Script | Gera imagens usando DALL-E, Stability AI ou FAL.ai | `IMAGE_AI_PROVIDER`, `IMAGE_AI_API_KEY` |
| `maestro-skill-creator` | Prompt | Cria, testa e faz benchmark de novas skills customizadas | — |

### Instalando uma skill

```
/maestro install apify
```

### Criando uma skill customizada

```
/maestro skills → Criar uma skill customizada
```

---

## Dashboard

O dashboard do escritório virtual exibe seus agentes trabalhando em tempo real em um escritório 2D construído com Phaser.

```bash
npm run dashboard
# Abre em http://localhost:5173
```

**Stack:** React 19 + Vite + Phaser 3 + Zustand + WebSocket

---

## Todos os Comandos

| Comando | Descrição |
|---------|-----------|
| `/maestro` | Abrir o menu principal |
| `/maestro help` | Exibir todos os comandos |
| `/maestro create` | Criar um novo squad |
| `/maestro list` | Listar todos os squads |
| `/maestro run <nome>` | Executar o pipeline de um squad |
| `/maestro edit <nome>` | Modificar um squad existente |
| `/maestro delete <nome>` | Excluir um squad |
| `/maestro skills` | Navegar pelas skills instaladas |
| `/maestro install <nome>` | Instalar uma skill do catálogo |
| `/maestro uninstall <nome>` | Remover uma skill |
| `/maestro show-company` | Exibir o perfil da empresa |
| `/maestro edit-company` | Atualizar o perfil da empresa |
| `/maestro settings` | Alterar idioma e preferências |
| `/maestro reset` | Resetar toda a configuração |

---

## Como Funciona (Arquitetura Detalhada)

### Fluxo de Execução do Squad

```
/maestro run meu-squad
  │
  ├─ Carrega squad.yaml + squad-party.csv + company.md + memories.md
  ├─ Resolve skills (falha rápida se alguma skill estiver ausente)
  ├─ Inicializa state.json (dashboard atualiza)
  └─ Para cada etapa do pipeline:
       ├─ Atualiza estado do dashboard
       ├─ Valida arquivo de entrada (portão bash)
       ├─ Executa (subagente / inline / checkpoint)
       ├─ Valida arquivo de saída (portão bash)
       ├─ Verifica condições de veto
       └─ Repassa para o próximo agente
```

### Composição do Contexto do Agente

```
Agente (.agent.md)
  + Boas Práticas da Plataforma (_maestro/core/best-practices/{format}.md)
  + Instruções da Skill (skills/{skill}/SKILL.md body)
```

### Versionamento de Saídas

Cada saída é armazenada em um caminho versionado e escopado por execução:

```
squads/{squad}/output/{run-id}/v{N}/{arquivo}
```

Exemplo: `squads/meu-squad/output/2026-05-01-143022/v1/relatorio.md`

---

## Contribuindo

Leia o [CONTRIBUTING.md](CONTRIBUTING.md) para diretrizes sobre como reportar bugs, enviar pull requests, adicionar skills e melhorar prompts de agentes.

---

## Licença

Copyright 2026 Otávio Pontes

Licenciado sob a [Apache License 2.0](LICENSE).
