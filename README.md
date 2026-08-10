# SSDLC Lab

Projeto prático criado para demonstrar a implantação incremental de um Secure Software Development Lifecycle (SSDLC) sobre uma aplicação web legada.

A aplicação utilizada é o OWASP Juice Shop, escolhida por representar um software intencionalmente vulnerável amplamente utilizado para fins educacionais.

O objetivo deste repositório não é corrigir a aplicação, mas demonstrar como processos, ferramentas e automações podem reduzir riscos ao longo do ciclo de desenvolvimento.

Cada etapa do projeto foi implementada incrementalmente e documentada neste repositório.

## Configurando o Juice Shop

O laboratório reproduz localmente uma instância da OWASP Juice Shop a partir de uma definição declarativa de infraestrutura utilizando Docker Compose.

Para subir o Juice Shop local:
```
docker compose up -d
docker compose ps
```

Acesse a aplicação em http://localhost:3000/