# Threat Modeling com STRIDE

## Motivação

Este documento aplica as perguntas do STRIDE aos principais fluxos da aplicação para identificar o que pode dar errado. As ameaças levantadas são hipóteses que serão priorizadas e validadas posteriormente por controles e testes de segurança.

## Diagrama

Esta análise utiliza como base o [diagrama da descoberta arquitetural](02-architecture-discovery.md#diagrama).

## Escopo

- Autenticação: Login, envio de email e senha e retorno do JWT.
- Conta e privacidade: Registro, perfil, senha, 2FA, histórico de IPs, exportação e exclusão de dados.
- Catálogo e conteúdo: Produtos, busca, avaliações, feedbacks, reclamações e mural de fotos.
- Carrinho e pedidos: Carrinho, checkout, pedidos, entrega e recycling box.
- Pagamentos: Cartões, carteira digital e Deluxe Membership.
- Endereços: Cadastro, consulta, alteração e exclusão.
- Arquivos: Uploads, imagens externas, arquivos armazenados e PDFs de pedidos.
- Assistente de IA: Mensagens, prompts, respostas e chamadas de ferramentas.
- Funções privilegiadas: Operações restritas por autenticação ou perfil de acesso.
- Persistência: Leitura e gravação de dados no SQLite e no sistema de arquivos.

## Categorias STRIDE

| Categoria | Pergunta |
|---|---|
| Spoofing | Alguém pode se passar por outra pessoa? |
| Tampering | Algo pode ser alterado sem autorização? |
| Repudiation | Alguém pode negar uma ação sem que seja possível verificá-la? |
| Information Disclosure | Alguma informação pode ser exposta indevidamente? |
| Denial of Service | Algo pode ser impedido de funcionar? |
| Elevation of Privilege | Alguém pode obter permissões indevidas? |

## Ameaças identificadas

| ID | Fluxo | Categoria | Ameaça | Ativo afetado |
|---|---|---|---|---|
| S01 | Autenticação | Spoofing | Uso de credenciais obtidas ou descobertas para acessar a conta de outro cliente. | Contas e identidades dos clientes |
| T01 | Autenticação | Tampering | Alteração de tokens de usuário para autenticação com permissões indevidas. | Contas e identidades dos clientes; chaves e configurações de segurança |
| I01 | Autenticação | Information Disclosure | Exposição de email, senha ou JWT durante o envio ou processamento da autenticação. | Contas e identidades dos clientes |
| D01 | Autenticação | Denial of Service | Tentativas automatizadas esgotam os recursos da autenticação ou provocam o bloqueio de contas legítimas. | Contas e identidades dos clientes |
| T02 | Conta e privacidade | Tampering | Alteração de senha, 2FA ou perfil sem autorização do titular. | Contas e identidades dos clientes |
| I02 | Conta e privacidade | Information Disclosure | Acesso à exportação de dados ou ao histórico de IPs de outro cliente. | Exportação e exclusão de dados; dados de IP |
| T03 | Conta e privacidade | Tampering | Exclusão dos dados de outro cliente por meio de uma solicitação indevida. | Exportação e exclusão de dados |
| T04 | Catálogo e conteúdo | Tampering | Alteração indevida de produtos, preços ou avaliações. | Dados de produtos; avaliações de produtos |
| D02 | Catálogo e conteúdo | Denial of Service | Submissões automatizadas sobrecarregam a busca ou o recebimento de conteúdo. | Funcionamento da aplicação |
| T05 | Carrinho e pedidos | Tampering | Manipulação de itens, quantidades, valores ou status de um pedido. | Pedidos e checkout |
| I03 | Carrinho e pedidos | Information Disclosure | Consulta ao histórico ou aos detalhes de pedidos de outro cliente. | Pedidos e checkout |
| R01 | Carrinho e pedidos | Repudiation | Cliente ou operador nega uma alteração no pedido sem registros suficientes para verificá-la. | Pedidos e checkout |
| T06 | Pagamentos | Tampering | Manipulação de cartões, saldo da carteira ou valor cobrado. | Dados de pagamento |
| I04 | Pagamentos | Information Disclosure | Exposição indevida de dados de cartões ou saldo da carteira. | Dados de pagamento |
| R02 | Pagamentos | Repudiation | Cliente nega uma recarga ou pagamento sem registros suficientes para verificá-lo. | Dados de pagamento |
| T07 | Endereços | Tampering | Alteração ou exclusão do endereço de outro cliente. | Dados de endereço |
| I05 | Endereços | Information Disclosure | Consulta aos endereços cadastrados por outro cliente. | Dados de endereço |
| T08 | Arquivos | Tampering | Envio ou substituição de arquivo por conteúdo malicioso. | Arquivos enviados e armazenados |
| I06 | Arquivos | Information Disclosure | Acesso não autorizado a imagens, anexos ou PDFs armazenados. | Arquivos enviados e armazenados |
| D03 | Arquivos | Denial of Service | Uploads repetidos ou excessivos esgotam o armazenamento da aplicação. | Funcionamento da aplicação; arquivos enviados e armazenados |
| I07 | Assistente de IA | Information Disclosure | Envio de dados sensíveis ao servidor LLM externo. | Contas e identidades dos clientes |
| T09 | Assistente de IA | Tampering | Mensagens manipulam respostas ou chamadas de ferramentas executadas pelo assistente. | Funcionamento da aplicação |
| D04 | Assistente de IA | Denial of Service | Requisições excessivas esgotam os recursos do assistente ou do servidor LLM. | Funcionamento da aplicação |
| E01 | Funções privilegiadas | Elevation of Privilege | Cliente acessa funções administrativas ou de contabilidade sem o perfil necessário. | Privilégios administrativos |
| R03 | Funções privilegiadas | Repudiation | Operador privilegiado nega uma ação sem registros suficientes para atribuí-la. | Privilégios administrativos |
| T10 | Persistência | Tampering | Alteração não autorizada de registros no SQLite ou de arquivos armazenados. | Contas e identidades dos clientes; pedidos e checkout; dados de pagamento; arquivos enviados e armazenados |
| I08 | Persistência | Information Disclosure | Exposição do banco de dados ou de arquivos armazenados. | Contas e identidades dos clientes; dados de endereço; dados de pagamento; pedidos e checkout; arquivos enviados e armazenados |
| D05 | Persistência | Denial of Service | Exclusão, corrupção ou indisponibilidade do banco de dados ou dos arquivos. | Funcionamento da aplicação |
