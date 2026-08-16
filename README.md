<div align="center">

# 🎮 DuoMatch

### Matchmaking justo, aberto e sem donos.

Uma infraestrutura 100% open source para formação de duos e squads em jogos competitivos — sem pay-to-win, sem algoritmos de engajamento forçado, sem taxas escondidas.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)
[![Node.js](https://img.shields.io/badge/Node.js-20.x-339933?logo=node.js&logoColor=white)](#)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?logo=typescript&logoColor=white)](#)
[![Next.js](https://img.shields.io/badge/Next.js-14.x-000000?logo=next.js&logoColor=white)](#)

</div>

---

## 📌 Sobre o Projeto

**DuoMatch** nasceu de um problema simples: encontrar um duo ou uma squad decente pra jogar hoje em dia depende de servidores de Discord bagunçados, planilhas soltas ou algoritmos proprietários que priorizam **retenção** em vez de **experiência**.

Este projeto é uma resposta a isso — uma camada de matchmaking **aberta, auditável e gratuita**, que qualquer comunidade, jogo ou desenvolvedor pode usar, adaptar ou hospedar por conta própria.

> 🎯 **Missão:** devolver o controle do matchmaking para a comunidade, sem monetização predatória e sem caixa-preta.

---

## ⚔️ O Problema vs. A Solução

### O Problema

O ecossistema atual de matchmaking em jogos competitivos sofre de três falhas estruturais:

| Falha | Impacto |
|---|---|
| **Algoritmos opacos** | Plataformas fechadas otimizam para *tempo de tela*, não para *qualidade de partida* — o critério de pareamento nunca é público. |
| **Monetização predatória** | Recursos de matchmaking (prioridade na fila, filtros avançados, destaque de perfil) viram vantagem competitiva paga — o clássico *pay-to-win*. |
| **Fragmentação de ferramentas** | Comunidades dependem de LFG genérico, grupos de WhatsApp e servidores de Discord improvisados, sem histórico, reputação ou dados estruturados. |

### A Solução

O **DuoMatch** propõe uma infraestrutura alternativa, construída sobre três pilares:

- ✅ **Transparência algorítmica** — os critérios de pareamento (rank, região, estilo de jogo, horário) são documentados e open source, não uma caixa-preta.
- ✅ **Zero pay-to-win** — nenhuma vantagem de matchmaking é vendável. Ponto final.
- ✅ **Infraestrutura reutilizável** — qualquer comunidade ou jogo pode hospedar sua própria instância via Docker, sem depender de um provedor central.

---

## ✨ Funcionalidades Principais

- 🎯 **Algoritmo de pareamento transparente** — combina nível de habilidade, região e estilo de jogo (competitivo, casual, coach) com peso documentado e ajustável.
- ⭐ **Sistema de reputação comunitário** — avaliações pós-partida constroem um histórico de confiabilidade do jogador, sem depender de moderação centralizada opaca.
- 🔌 **API REST + WebSocket aberta** — integre o DuoMatch a qualquer front-end, bot de Discord ou launcher de jogo próprio.
- 🚦 **Fila de matchmaking em tempo real** — pareamento assíncrono e escalável via Redis, com latência mínima entre entrada na fila e match encontrado.
- 🐳 **Deploy independente** — suba sua própria instância isolada em minutos com Docker Compose.

---

## 🏗️ Arquitetura & Tecnologias

```
┌─────────────────┐      ┌──────────────────┐      ┌─────────────────┐
│   Frontend       │◄────►│   Backend API     │◄────►│   PostgreSQL     │
│  React / Next.js │      │  Node.js / TS     │      │  (dados          │
│                  │      │  REST + WebSocket │      │   persistentes)  │
└─────────────────┘      └────────┬─────────┘      └─────────────────┘
                                    │
                                    ▼
                          ┌──────────────────┐
                          │      Redis        │
                          │  Fila de          │
                          │  Matchmaking       │
                          └──────────────────┘
```

**Stack principal:**

| Camada | Tecnologia | Função |
|---|---|---|
| Frontend | React / Next.js | Interface web, SSR e client de fila em tempo real |
| Backend | Node.js / TypeScript | API REST + WebSocket, lógica de pareamento |
| Banco de dados | PostgreSQL | Perfis, histórico de partidas, reputação |
| Fila de matchmaking | Redis | Fila em memória de baixíssima latência para pareamento |
| Infraestrutura | Docker / Docker Compose | Empacotamento e execução local/produção |

**Como funciona a fila (Redis):** cada jogador que entra na fila é inserido como uma entrada estruturada (rank, região, estilo, timestamp) em uma sorted set do Redis. Um worker consome essa fila em ciclos curtos, tentando formar pares/grupos compatíveis dentro de uma margem de tolerância que se expande conforme o tempo de espera aumenta — evitando tanto matches ruins quanto esperas infinitas.

---

## 🚀 Guia de Instalação e Execução Local

### Pré-requisitos

- [Node.js](https://nodejs.org/) `>= 20.x`
- [Docker](https://www.docker.com/) e [Docker Compose](https://docs.docker.com/compose/)
- [Git](https://git-scm.com/)

### Passo a passo

```bash
# 1. Clone o repositório
git clone https://github.com/seu-usuario/duomatch.git
cd duomatch

# 2. Suba os serviços de infraestrutura (PostgreSQL + Redis)
docker-compose up -d

# 3. Instale as dependências
npm install

# 4. Copie as variáveis de ambiente de exemplo
cp .env.example .env

# 5. Rode as migrations do banco de dados
npm run migrate

# 6. Inicie o ambiente de desenvolvimento (backend + frontend)
npm run dev
```

Após isso, a aplicação estará disponível em:

- 🌐 Frontend: `http://localhost:3000`
- 🔌 API: `http://localhost:4000`

> 💡 **Dica:** use `docker-compose down -v` para resetar completamente os dados locais durante os testes.

---

## 🤝 Como Contribuir

O DuoMatch é feito **pela e para a comunidade**. Toda contribuição é bem-vinda, de correções de bug a novas funcionalidades.

1. **Abra uma Issue** antes de começar a codar, descrevendo o problema ou a melhoria proposta. Isso evita esforço duplicado e alinha expectativas.
2. **Crie uma branch** a partir da `main`, seguindo o padrão:
   ```bash
   git checkout -b feat/nome-da-funcionalidade
   # ou
   git checkout -b fix/nome-do-bug
   ```
3. **Siga o padrão de commits** (Conventional Commits):
   ```bash
   git commit -m "feat: adiciona filtro de região no matchmaking"
   ```
4. **Abra um Pull Request** direcionado à `main`, descrevendo claramente:
   - O que foi alterado e por quê
   - Como testar a mudança
   - Issue relacionada (se houver)
5. Aguarde a revisão — todo PR passa por *code review* antes do merge.

📄 Consulte o [`CONTRIBUTING.md`](CONTRIBUTING.md) para o guia completo de estilo de código, testes e convenções.

---

## 🗺️ Roadmap

- [x] **MVP** — matchmaking básico por rank/região, autenticação e API REST inicial
- [ ] **Sistema de reputação comunitário** — avaliações pós-partida e histórico de confiabilidade
- [ ] **WebSocket em tempo real** — atualização de status da fila sem polling
- [ ] **Testes de carga** — validação da fila Redis sob alto volume simultâneo de usuários
- [ ] **API pública documentada** — abertura da API para integrações externas (bots, launchers, comunidades terceiras)
- [ ] **Multi-jogo** — suporte a múltiplos títulos configuráveis na mesma instância

Acompanhe o progresso detalhado em [Issues](../../issues) e [Projects](../../projects).

---

## 📄 Licença

Este projeto está licenciado sob os termos da **MIT License**.

Isso significa que você pode usar, copiar, modificar e distribuir este software livremente, inclusive para fins comerciais, desde que mantenha o aviso de copyright original.

Veja o arquivo [`LICENSE`](LICENSE) para o texto completo.

---

<div align="center">

Feito com 🎮 e código aberto, por e para quem só quer jogar com uma squad decente.

</div>
