# Descoberta Arquitetural

## Motivação

Este documento complementa o levantamento manual com os principais fluxos identificados no código-fonte do Juice Shop. O objetivo é criar uma base sólida para o Threat Modeling, indicando quais dados trafegam, onde o controle muda e quais perguntas de segurança precisam ser respondidas.

## Diagrama

![Diagrama arquitetural do Juice Shop](images/architecture-discovery.svg)

## Fluxos e perguntas de segurança

| Fluxo | Dados | Mudança de controle? | Pergunta para o Threat Modeling | Evidência |
|---|---|---|---|---|
| Navegador → backend | Credenciais, JWT, formulários, IDs e uploads | Sim: o navegador é controlado pelo usuário | O backend valida a entrada, a identidade e a autorização? | `server.ts:596`, `server.ts:699` |
| Backend → navegador | Páginas, respostas das APIs, JWT e arquivos | Sim: os dados são entregues ao ambiente do usuário | Algum dado sensível é exposto ou apresentado sem tratamento adequado? | `server.ts:288`, `routes/login.ts:26` |
| Backend → SQLite | Contas, endereços, cartões, pedidos e outros dados da aplicação | Não no escopo atual: o SQLite é acessado pelo mesmo processo dentro do container | As operações preservam a confidencialidade e a integridade dos dados? | `models/index.ts:33` |
| Backend → sistema de arquivos | Uploads, logs e arquivos gerados | Não no escopo atual: o sistema de arquivos pertence ao mesmo container | Arquivos controlados pelo usuário são validados e acessados com segurança? | `server.ts:699` |
| Backend → servidor LLM (configurado) | Mensagens, prompts e chamadas de ferramentas | Sim: o processamento ocorre em outro serviço | Quais dados saem da aplicação e como as respostas e chamadas de ferramentas são validadas? | `routes/chat.ts:108` |
| Requisição de cliente → funções privilegiadas | Requisições autenticadas | Sim: o nível de privilégio muda | A autorização diferencia corretamente clientes, contabilidade e administradores? | `lib/insecurity.ts:142`, `server.ts:431` |

## Como reproduzir esta etapa

- Use o levantamento manual para definir o escopo da leitura.
- Rastreie no código apenas os principais fluxos de dados e privilégios.
- Registre mudanças de controle e uma evidência `arquivo:linha`.
- Represente os componentes e fluxos em um diagrama simples.
- Valide no código todas as informações produzidas com apoio de IA.

## Prompt sugerido

```text
Analise o código-fonte desta aplicação e mapeie somente os principais fluxos relevantes para Threat Modeling: autenticação, dados sensíveis ou de negócio, arquivos, serviços externos e funções privilegiadas.

Gere uma tabela Markdown com as colunas `Fluxo`, `Dados`, `Mudança de controle?`, `Pergunta para o Threat Modeling` e `Evidência`. Considere mudança de controle entre usuários, processos, serviços ou privilégios diferentes. Cite evidências como `arquivo:linha` e sinalize hipóteses ou informações não confirmadas.

Não altere arquivos, não procure vulnerabilidades e não liste componentes internos sem relação com esses fluxos. Retorne somente a tabela.
```
