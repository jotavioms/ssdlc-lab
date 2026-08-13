# Exploração Manual Black-box

## Motivação

Este documento registra uma exploração manual do Juice Shop pela perspectiva de um usuário, sem acesso prévio à implementação. O objetivo é identificar o que existe na aplicação antes de verificar seu funcionamento interno.

## Atores

| Ator | Acesso observado |
|---|---|
| Visitante | Catálogo, busca, carrinho, registro, feedback, assistente e mural de fotos |
| Cliente | Conta, endereços, pagamentos, pedidos, arquivos e privacidade |
| Lojista (hipótese) | Gestão da loja e funções administrativas |

## Funcionalidades e dados

| Área | Funcionalidades | Principais dados | Acesso observado |
|---|---|---|---|
| Autenticação e conta | Registro, login, perfil, senha e 2FA | Email, senha, username, pergunta e resposta de segurança | Público e autenticado |
| Catálogo e conteúdo | Produtos, busca, avaliações, feedbacks, reclamações e mural | Produtos, preços, avaliações, mensagens e imagens | Público e autenticado |
| Carrinho e pedidos | Carrinho, checkout, histórico, entrega e recycling box | Itens, quantidades, valores, pedidos e status | Público e autenticado |
| Pagamentos | Cartões, carteira e Deluxe Membership | Cartão, validade e saldo | Autenticado |
| Endereços | Cadastro, consulta, alteração e exclusão | Nome, telefone e endereço postal | Autenticado |
| Arquivos | Foto de perfil, imagem externa e PDF do pedido | Arquivos, URLs e documentos gerados | Autenticado |
| Privacidade | Histórico de IPs, exportação e exclusão de dados | IPs e dados pessoais exportados ou excluídos | Autenticado |
| Assistente de IA | Envio e recebimento de mensagens | Texto da conversa | Público |

## Pontos de entrada

| Tipo | Exemplos observados |
|---|---|
| Formulários | Login, registro, perfil, endereço, pagamento e feedback |
| Parâmetros HTTP | IDs, busca, corpo e parâmetros das requisições |
| Arquivos e URLs | Uploads e links externos de imagem |
| Operações sensíveis | Alteração de senha, 2FA, checkout, exportação e exclusão de dados |
| Integrações percebidas | Assistente de IA e compartilhamento externo |

## Como reproduzir esta etapa

- Execute a aplicação em um ambiente local e descartável.
- Navegue como visitante e cliente, sem consultar o código-fonte.
- Agrupe atores, funcionalidades, dados e pontos de entrada por área.
- Marque como hipótese tudo o que não puder ser confirmado pela interface.
- Não teste vulnerabilidades nesta etapa.

## Prompt sugerido

```text
Com base nas minhas anotações de exploração manual desta aplicação, organize somente o que foi observado pela perspectiva do usuário.

Gere três tabelas Markdown: `Atores`, `Funcionalidades e dados` e `Pontos de entrada`, seguindo as mesmas colunas deste documento. Agrupe itens semelhantes por área, preserve hipóteses e não deduza arquitetura, vulnerabilidades ou controles internos.

Não altere arquivos. Retorne somente as três tabelas.
```
