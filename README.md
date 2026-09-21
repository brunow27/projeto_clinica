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

O protótipo é estático (HTML/CSS/JS puro, sem dependências) e está dividido em duas páginas, refletindo a área pública e a área administrativa descritas no documento de requisitos:

### `site.html` — área pública (sem login)
- **Início** — apresentação institucional da clínica (RF01)
- **Serviços** — lista de procedimentos com preços, ou "Sob consulta" quando não informado (RF02, RF03)
- **Profissionais** — nome e especialidade da equipe (RF04)
- **Horários** — dias e horários de atendimento, sempre visíveis (RF05)
- **Contato e localização** — telefone, WhatsApp, e-mail, endereço e mapa incorporado (RF08, RF09)
- **Agendar** — formulário público de solicitação de agendamento com validação de nome, telefone, e-mail e procedimento (RF06, RF07)

### `index.html` — área administrativa autenticada (protótipo)
- **Painel** — resumo do dia, com atalho para novo agendamento
- **Agenda** — visão semanal por horário; clicar em um horário livre abre o modal de novo agendamento; clicar em uma consulta existente mostra detalhes com opções de confirmar/cancelar (RF11)
- **Pacientes** — lista com busca por nome/telefone
- **Serviços** — cadastro, edição e remoção de procedimentos e preços, refletidos na lista usada pelos formulários de agendamento (RF10)
- **Cadastro** — formulário simples para registrar um novo paciente (dados pessoais)
- **Registro do paciente** — dados pessoais editáveis + histórico de procedimentos

> Nota: o link "Área administrativa" no rodapé/menu do site público e o link "Ver site público" na barra lateral do painel conectam as duas páginas para navegação durante a avaliação. Em produção, apenas a área administrativa exigiria login.
>
> A tela de "Registro do paciente" (prontuário/histórico de procedimentos) foi mantida no protótipo por já existir na versão anterior, mas o documento de requisitos define histórico clínico do paciente como **fora do escopo** desta etapa — vale reavaliar se essa tela deve ou não compor a entrega final.

## Estrutura do repositório

```
site.html     → área pública do site (recomenda-se publicar esta como página inicial no GitHub Pages)
index.html    → painel administrativo interno
README.md
uc_agendamento.jpeg
uc_gerenciamento_agendamentos.jpeg
uc_gerenciamento_servicos.jpeg
```
