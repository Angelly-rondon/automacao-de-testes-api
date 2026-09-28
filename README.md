# Automação testes de API

Projeto de automação de testes para a API de usuários utilizando **Postman** e **Newman**.

A API utilizada no projeto é a [ServeRest](https://serverest.dev/).

## Tecnologias

* Postman
* Newman
* Node.js
* JavaScript
* GitHub Actions

## Estrutura

```text
.
├── .github/
│   └── workflows/
│       └── api-tests.yml
│
├── collection/
│   └── serverest.postman_collection.json
├── environment/
│   └── serverest.environment.json
│
├── reports/
├── .gitignore
├── package.json
├── package-lock.json
└── README.md
```

## Testes

Os testes cobrem as principais operações da API de usuários:

* Login;
* Listagem de usuários;
* Criação de usuário;
* Busca de usuário por ID;
* Atualização de usuário;
* Exclusão de usuário.

Também foram considerados cenários de sucesso e erro, como:

* Dados obrigatórios não informados;
* E-mail inválido;
* E-mail já cadastrado;
* Usuário inexistente;
* ID inválido;
* Credenciais inválidas;
* Validação dos status HTTP;
* Validação dos dados retornados pela API.

### Fluxo principal

Alguns testes dependem do resultado de requisições anteriores. Por exemplo, após criar um usuário, o ID retornado pela API é salvo em uma variável e utilizado para consultar, atualizar e excluir esse mesmo usuário.

Dessa forma, é possível testar o ciclo completo do usuário sem precisar informar IDs fixos na collection.

Os dados necessários entre as requisições são armazenados nas variáveis do ambiente do Postman.


## Requisitos
Para executar o projeto localmente, é recomendado utilizar:

- Node.js: 22.x (LTS);
- npm: 10.x ou superior;
- Newman: 6.x;
- Postman: versão 11.x ou superior.

Verifique as versões instaladas:

```bash
node --version
npm --version
npx newman --version
```
O Postman é necessário caso seja necessário visualizar ou editar a collection. Para apenas executar os testes, o Newman é suficiente.

## Instalação

Clone o projeto e instale as dependências:

```bash
git clone https://github.com/Angelly-rondon/automacao-de-testes-api

cd automacao-de-testes-API

npm install
```

## Executando os testes

Os testes podem ser executados utilizando o script configurado no `package.json`:

```bash
npm run test:api
```

Também é possível executar diretamente pelo Newman:

```bash
newman run postman/collections/serverest.postman_collection.json -e postman/environments/serverest.environment.json --reporters cli,htmlextra,junit --reporter-htmlextra-export reports/api-report.html --reporter-junit-export reports/api-results.xml
```

## Relatório

Além do resultado exibido no terminal, o Newman é configurado para gerar um relatório em HTML.

<br>

O relatório contém informações sobre:

* Testes executados;
* Testes aprovados;
* Testes que falharam;
* Requisições realizadas;
* Status HTTP;
* Assertions;
* Tempo de execução.

## GitHub Actions

Os testes são executados automaticamente através do GitHub Actions.

A pipeline realiza:

1. Configuração para inicialização do projeto
2. Execução dos testes com Newman;
3. Geração do relatório HTML;
4. Upload do relatório como artifact.

O workflow está localizado em:

```text
.github/workflows/api-tests.yml
```

O relatório pode ser acessado nos **Artifacts** da execução da pipeline no GitHub Actions.

## Cobertura

| Método | Endpoint         | Cenários                        |
| ------ | ---------------- | ------------------------------- |
| POST   | `/login`         | Sucesso e falha                 |
| GET    | `/usuarios`      | Consulta e validações           |
| POST   | `/usuarios`      | Criação e validações            |
| GET    | `/usuarios/{id}` | Usuário existente e inexistente |
| PUT    | `/usuarios/{id}` | Atualização e validações        |
| DELETE | `/usuarios/{id}` | Exclusão e usuário inexistente  |

A suíte busca cobrir os fluxos principais e os casos de erro esperados para cada endpoint.

## Limite de requisições

A API possui limite de **100 requisições por minuto**. A collection foi criada considerando esse limite para evitar requisições desnecessárias durante a execução dos testes.
