# Alma Estética — Site institucional + Agendamento

Projeto acadêmico (PAC Extensionista) referente à Avaliação N1 — Planejamento e Levantamento de Requisitos. Propõe um sistema web para uma clínica de estética, com uma área pública (informações da clínica, serviços, preços, profissionais, horários e solicitação de agendamento) e uma área administrativa autenticada (gestão de serviços e agendamentos).

🔗 **Protótipo publicado:** https://brunow27.github.io/projeto_clinica/

## Diagramas de caso de uso

Foram escolhidas 3 funcionalidades críticas do sistema para representação técnica:

**Agendamento (paciente)**
![Diagrama de caso de uso - Agendamento](uc_agendamento.jpeg)

**Gerenciamento de agendamentos (administrador)**
![Diagrama de caso de uso - Gerenciamento de agendamentos](uc_gerenciamento_agendamentos.jpeg)

**Gerenciamento de serviços (administrador)**
![Diagrama de caso de uso - Gerenciamento de serviços](uc_gerenciamento_servicos.jpeg)

## Arquitetura

**Escolha: REST**

| Critério | REST | GraphQL |
|---|---|---|
| Complexidade das consultas | Baixa — poucas entidades, poucos relacionamentos aninhados | Vantagem só aparece com consultas muito complexas/variáveis |
| Cache HTTP | Nativo (ETags, cache de navegador/CDN) — importante para RNF02 (desempenho) | Mais difícil de cachear |
| Curva de aprendizado | Menor, mais documentação/exemplos didáticos | Exige schema e resolvers — overhead desnecessário aqui |
| Compatibilidade | Simples de consumir de qualquer front (RNF07) | Exige biblioteca cliente (Apollo/urql) |

O sistema é essencialmente CRUD (serviços, profissionais, agendamentos), sem necessidade de o cliente escolher dinamicamente quais campos buscar — REST com endpoints bem definidos (`/servicos`, `/profissionais`, `/agendamentos`, etc.) atende perfeitamente, sem a complexidade extra do GraphQL.

## Stack tecnológica

| Camada | Tecnologia | Justificativa |
|---|---|---|
| Backend | Python + Flask (SQLAlchemy, JWT) | Simples de estruturar em endpoints REST; SQLAlchemy cobre a persistência das entidades; JWT restringe o acesso administrativo (RNF11) |
| Frontend | HTML5 + CSS3 + Bootstrap (ou Tailwind via CDN) + JavaScript (fetch/AJAX) | Sem necessidade de build tools (Node/npm/webpack); Bootstrap resolve RNF01 (responsividade) e RNF06 (acessibilidade) |
| Banco de dados | MySQL | Integração direta com SQLAlchemy; ferramentas amigáveis (MySQL Workbench/phpMyAdmin); suporte fácil a backups periódicos (RNF10) |
| Comunicação | REST (JSON sobre HTTPS) | Atende RNF04 (HTTPS) e é compatível com qualquer navegador (RNF07) |
| Autenticação | Flask + JWT (ou sessão simples) | Restringe o acesso administrativo (RNF11) |

**Sobre LGPD (RNF12):** independentemente da stack escolhida, os dados informados nos formulários (nome, telefone, e-mail) devem ter política de retenção e uso definida — mais uma decisão de processo do que de tecnologia, mas que deve constar na documentação final do projeto.

## Protótipo funcional

`index.html` é um protótipo estático (HTML/CSS/JS puro, sem dependências) das telas do painel interno da clínica, com dados de exemplo já carregados. Pode ser aberto direto no navegador (duplo clique) ou acessado pelo link do GitHub Pages acima.

Telas disponíveis:
- **Painel** — resumo do dia, com atalho para novo agendamento
- **Agenda** — visão semanal por horário; clicar em um horário livre abre o modal de novo agendamento; clicar em uma consulta existente mostra detalhes com opções de confirmar/cancelar
- **Pacientes** — lista com busca por nome/telefone
- **Cadastro** — formulário simples para registrar um novo paciente (dados pessoais)
- **Registro do paciente** — dados pessoais editáveis + histórico de procedimentos

## Estrutura do repositório

```
index.html    → protótipo funcional (também é a página servida pelo GitHub Pages)
README.md
uc_agendamento.jpeg
uc_gerenciamento_agendamentos.jpeg
uc_gerenciamento_servicos.jpeg
```
