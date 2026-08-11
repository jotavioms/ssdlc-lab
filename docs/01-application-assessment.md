# Exploração Manual Black-box

## Motivação

Este documento registra uma exploração manual inicial da aplicação pela perspectiva de um usuário, sem conhecimento prévio de como o sistema foi implementado (black-box).

O objetivo é identificar funcionalidades, dados, pontos de entrada e componentes percebidos. As hipóteses e informações desconhecidas encontradas aqui serão verificadas posteriormente por meio da análise do código-fonte.

## 0. Contexto da aplicação

Juice Shop v20.1.1 é uma aplicação de vendas (e-commerce) voltada à comercialização de sucos.

## 1. Atores da aplicação

| Ator | Interesse | Autenticado? |
|----------|----------|----------|
| Visitante | Analisa os produtos da loja sem compromisso de compra. | Não |
| Cliente | Realiza compras na loja, acompanha suas compras. | Sim |
| Lojista (hipótese) | Faz a gestão de produtos, estoque, e vendas. | Verificar |

## 2. Principais funcionalidades

| Funcionalidade | Autenticado? |
|----------|----------|
| Listagem de produtos | Não |
| Adicionar produto ao carrinho | Não |
| Busca por produto | Não |
| Carrinho de compras | Não |
| Mudar a linguagem | Não |
| Fazer login | Não |
| Submeter um feedback | Não |
| Assistente de IA | Não |
| Mural de fotos | Não |
| Registro de conta | Não |
| Upload de foto de usuário | Sim |
| Link de imagem de usuário | Sim |
| Alteração de username | Sim |
| Listar histórico de compras | Sim |
| Solicitar recycling box | Sim |
| Cadastrar um novo endereço | Sim |
| Listar endereços cadastrados | Sim |
| Editar endereço cadastrado | Sim |
| Excluir endereço cadastrado | Sim |
| Adicionar método de pagamento | Sim |
| Listar métodos de pagamento | Sim |
| Excluir método de pagamento | Sim |
| Adicionar fundos na carteira digital | Sim |
| Exportação de dados | Sim |
| Exclusão de dados (GDPR/LGPD) | Sim |
| Alterar senha | Sim |
| Configurar 2FA | Sim |
| Histórico de IPs logados | Sim |
| Checkout - Selecionar endereço de entrega | Sim |
| Checkout - Escolher método de entrega | Sim |
| Checkout - Escolher método de pagamento | Sim |
| Checkout - Finalizar pagamento | Sim |
| Checkout - Dados da compra | Sim |
| Checkout - Compartilhar pedido no Twitter | Sim |
| Checkout - Imprimir PDF do pedido | Sim |
| Comprar Deluxe Membership | Sim |
| Escrever review do produto | Sim |
| Acompanhar entrega do pedido | Sim |

## 3. Dados de entrada e saída

| Dado | Entrada | Saída |
|----------|----------|----------|
| Nome do produto | Sim (busca) | Sim (listagem) |
| Preço do produto | Não | Sim (listagem) |
| Idioma do site | Sim | Não diretamente |
| Email | Sim (login, registro) | Sim (profile, recycle) |
| Senha | Sim (login, registro) | Não |
| Pergunta de segurança | Sim (registro, erasure) | Sim |
| Resposta de segurança | Sim (registro, erasure) | Não |
| Arquivo de imagem | Sim (profile) | Sim (profile) |
| Link de imagem | Sim (profile) | Sim (profile) |
| Username | Sim (profile) | Sim (profile) |
| Quantidade de itens no pedido | Não | Sim (order) |
| Valor total do pedido | Não | Sim |
| Avaliação do produto | Sim | Sim |
| ID do pedido | Não | Sim (pedido) |
| Status da entrega | Não | Sim (pedido) |
| Quantidade de recycling boxes | Sim | Sim |
| Endereço - País | Sim (endereço) | Sim (endereço, recycling) |
| Endereço - Nome | Sim (endereço) | Sim (endereço, recycling) |
| Endereço - Telefone | Sim (endereço) | Sim (endereço, recycling) |
| Endereço - ZIP Code | Sim (endereço) | Sim (endereço, recycling) |
| Endereço - Cidade | Sim (endereço) | Sim (endereço, recycling) |
| Endereço - Estado | Sim (endereço) | Sim (endereço, recycling) |
| Método de pagamento - Nome | Sim (pagamento) | Sim (pagamento) |
| Método de pagamento - Número do cartão | Sim (pagamento) | Sim (pagamento - mascarado) |
| Método de pagamento - Mês de expiração | Sim (pagamento) | Sim (pagamento) |
| Método de pagamento - Ano de expiração | Sim (pagamento) | Sim (pagamento) |
| Quantia da carteira digital | Sim (carteira) | Sim (carteira) |
| Política de privacidade | Não | Sim |
| Formato da exportação de dados | Sim | Não |
| Token 2FA | Sim | Não |
| IP logado | Não | Sim |
| Mensagem de feedback | Sim | Não |
| Captcha do feedback | Sim | Não |
| Mensagem de reclamação | Sim | Não |
| Texto no chat de IA | Sim | Sim |
| Imagens no photo wall | Não | Sim |


## 4. Componentes e fronteiras percebidas

### Componentes observados

- Usuário
- Navegador
- Aplicação Juice Shop executada em container

### Fluxos observados

- Navegador envia requisições HTTP para a aplicação.
- Aplicação retorna páginas, dados e arquivos ao navegador.

### Trust boundaries percebidas

- Entre o navegador, controlável pelo usuário, e a aplicação.
- Entre usuários não autenticados e funcionalidades que exigem autenticação.
- Entre usuários comuns e possíveis funções administrativas (hipótese).

### Componentes ainda desconhecidos

- Banco de dados ou outro mecanismo de persistência.
- Implementação da autenticação.
- Processamento dos pagamentos.
- Implementação do assistente.

### Interfaces HTTP observadas

- /rest/admin/application-version
- /rest/admin/application-configuration
- /rest/user/whoami
- /api/Quantitys/
- /rest/products/search
- /api/Recycles/
- /api/Cards
- /rest/languages
- /rest/basket/id
- /rest/wallet/balance
- /rest/user/data-export
- /api/dataerasure
- /rest/user/change-password
- /rest/captcha/
- /api/Feedbacks/
- /api/Complaints/
- /rest/deluxe-membership
- /rest/saveLoginIp
- /api/Challenges/

## 5. Pontos de entrada

- Formulário de login
- Formulário de criação de conta
- Formulários CRUD (endereço, pagamento)
- Formulário de alteração de senha
- Configuração de 2FA
- Exportação/exclusão de dados
- Adição de fundos
- Busca de produtos
- Upload e URL externa de imagem
- Comentários, feedbacks e reclamações
- Chat/Assistente
- Parâmetros, corpo e identificadores das requisições HTTP

## 6. Análise de impacto e criticidade

| Dados | Impacto | Severidade inicial |
|----------|----------|----------|
| Login, senha, recuperação e 2FA | Comprometimento da conta | Crítico |
| Endereços, pedidos, IPs e exportação | Exposição de dados pessoais | Alto |
| Métodos de pagamento e carteira | Impacto financeiro | Crítico |
| Upload e links de imagem | Armazenamento e processamento de conteúdo malicioso | Médio |
| Exclusão de dados | Ação destrutiva e impacto de disponibilidade/integridade | Crítico |
| Funções administrativas (hipótese) | Alto privilégio | Crítico |
| Checkout e pedidos | Integridade das transações | Alto |
| APIs que recebem IDs | Possível acesso indevido a objetos de outros usuários (hipótese) | Médio |
