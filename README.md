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

Os documentos seguem a ordem de leitura abaixo:

1. [Exploração manual black-box](docs/01-application-assessment.md) — Levantamento da aplicação pela perspectiva de um usuário.
2. [Descoberta arquitetural](docs/02-architecture-discovery.md) — Principais fluxos identificados no código-fonte e perguntas para o Threat Modeling.
