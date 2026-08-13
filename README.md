# SSDLC Lab

Projeto prático criado para demonstrar a implantação incremental de um Secure Software Development Lifecycle (SSDLC) sobre uma aplicação web legada.

A aplicação utilizada é o OWASP Juice Shop, escolhida por representar um software intencionalmente vulnerável amplamente utilizado para fins educacionais.

O objetivo deste repositório não é corrigir a aplicação, mas demonstrar como processos, ferramentas e automações podem reduzir riscos ao longo do ciclo de desenvolvimento.

Cada etapa implementada é documentada neste repositório.

## Componentes do laboratório

O laboratório utiliza duas representações complementares da mesma versão do Juice Shop:

- Uma imagem Docker para executar a aplicação localmente;
- Uma cópia do código-fonte para as análises que não dependem da aplicação em execução.

A imagem e o código-fonte estão fixados na versão 20.1.1 para garantir que as análises sejam realizadas sobre o mesmo estado da aplicação.

## Executando a aplicação

O laboratório reproduz localmente uma instância da OWASP Juice Shop a partir de uma definição declarativa de infraestrutura utilizando Docker Compose.

Para subir o Juice Shop localmente:

```console
docker compose up -d
docker compose ps
```

Acesse a aplicação em http://localhost:3000/. O código-fonte não é necessário para esta etapa.

## Obtendo o código-fonte para análise

Algumas atividades do SSDLC precisam acessar o código e os manifests de dependências. Isso inclui o levantamento arquitetural e, futuramente, SAST e SCA.

O código-fonte é obtido separadamente, não é utilizado pelo Docker Compose e não é versionado neste repositório.

Em um terminal com Git instalado, execute:

```console
git clone --depth 1 --branch v20.1.1 https://github.com/juice-shop/juice-shop.git target/juice-shop
git -C target/juice-shop rev-parse HEAD
```

O segundo comando deve retornar o commit `f915bddd82790d0f3018902d36ae9b4241a5f51f`. O código-fonte será armazenado em `target/juice-shop`, diretório ignorado pelo Git.

## Documentação

Os documentos seguem uma sequência em que o resultado de cada etapa serve como entrada para a próxima:

| Etapa | Entrada | Produz | Usado em |
|---|---|---|---|
| 1. [Exploração Manual Black-box](docs/01-application-assessment.md) | Uso manual da aplicação como usuário | Atores, funcionalidades, dados e pontos de entrada percebidos | Etapas 2, 3 e 4 |
| 2. [Descoberta Arquitetural](docs/02-architecture-discovery.md) | Observações da etapa 1 e código-fonte | Componentes, caminhos dos dados e mudanças de controle | Etapas 3 e 4 |
| 3. [Risk Assessment](docs/03-risk-assessment.md) | Funcionalidades e dados da etapa 1; componentes e fluxos da etapa 2 | Ativos e impactos | Etapa 4 e priorização dos riscos |
| 4. [Threat Modeling com STRIDE](docs/04-threat-model.md) | Fluxos da etapa 2; ativos e impactos da etapa 3 | Ameaças possíveis | Priorização dos riscos e testes de segurança |
