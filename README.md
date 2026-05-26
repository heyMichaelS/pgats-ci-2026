[![Code coverage badge](https://img.shields.io/badge/coverage-100%25-brightgreen)](https://stryker-mutator.io/robo-coasters-example/reports/coverage/lcov-report/index.html)
[![Mutation testing badge](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fbadge-api.stryker-mutator.io%2Fgithub.com%2Fstryker-mutator%2Frobo-coasters-example%2Fmaster)](https://dashboard.stryker-mutator.io/reports/github.com/stryker-mutator/robo-coasters-example/master)

# PGATS - CI

## Pré-requisitos

1. Instale o [git](https://git-scm.com)
2. Instale o [nodejs](https://nodejs.org/)
3. Instale o Yarn - `npm install -g yarn`
4. Faça um _Fork_ do projeto
5. Clone o repositório para sua máquina (seu fork)
6. Instale as dependências
   ```shell
   cd pgats-ci
   yarn
   ```
7. Execute os testes de unidade - isso vai gerar um relatório
   ```shell
   yarn run test
   ```
8. Abra o relatório de cobertura de código em `reports/coverage/lcov-report`
9. Execute os testes de mutação com o Stryker
   ```shell
   yarn run test:mutation
   ```
10. Abra o relatório de mutação em `reports/mutation`
11. Instale os navegadores do Playwright
    ```shell
    yarn playwright install
    ```
12. Execute os testes end-to-end com o Playwright
    ```shell
    yarn run e2e
    ```
13. Execute a aplicação com `yarn start`
14. Acesse a aplicação publicada [neste link](https://pgats-ci-example.netlify.app)

---

💜⚡️

# pgats-ci

## Documentacao do desafio de CI

Neste desafio, o projeto foi configurado para executar pipelines em mais de uma
ferramenta de Integracao Continua, alem do fluxo original em GitHub Actions.

### Pipeline com Jenkins

Foi criado um arquivo `Jenkinsfile` na raiz do projeto para executar a pipeline
no Jenkins. Essa pipeline realiza as seguintes etapas:

1. Checkout do codigo-fonte.
2. Instalacao das dependencias com Yarn.
3. Execucao do lint.
4. Execucao dos testes unitarios com Jest.
5. Execucao dos testes E2E com Playwright.
6. Publicacao/arquivamento dos relatorios gerados.

Durante a configuracao, foi necessario ajustar o ambiente do Jenkins para usar
Node.js e Playwright. Tambem foi identificado que os testes de mutacao com
Stryker podem demorar bastante, por isso eles foram configurados como opcionais
por meio do parametro `RUN_MUTATION`.

### GitHub Actions Marketplace

Foi adicionada ao workflow do GitHub Actions a action `dorny/test-reporter`,
disponivel no GitHub Marketplace.

Essa action le o arquivo `results.xml`, gerado pelo Playwright no formato JUnit,
e publica um relatorio dos testes diretamente na execucao da pipeline. Isso
melhora a visibilidade do resultado dos testes e facilita a analise de falhas.

Tambem foi avaliada a action `actions/upload-artifact`, que pode ser usada para
salvar relatorios como artefatos da execucao, como:

- `results.xml`
- `playwright-report/`
- `test-results/`
- `reports/`

### Self-hosted runner

Tambem foi configurado um self-hosted runner do GitHub Actions em uma maquina
Windows local. Esse recurso permite executar workflows em uma maquina propria,
em vez de usar apenas os runners hospedados pelo GitHub.

O runner foi configurado no GitHub em:

```text
Settings > Actions > Runners > New self-hosted runner
```

Depois da configuracao no PowerShell, o runner ficou conectado ao GitHub com a
mensagem:

```text
Connected to GitHub
Listening for Jobs
```

Foi criado o workflow `.github/workflows/02-self-hosted-agent.yaml`, configurado
para executar no agent local com:

```yaml
runs-on: [self-hosted, windows, x64]
```

Esse workflow executa checkout, instalacao do Node.js, instalacao das
dependencias, lint, testes unitarios, testes E2E, publicacao de relatorio e
upload de artefatos.

### Quando usar self-hosted runners?

Self-hosted runners fazem sentido quando o projeto precisa de mais controle
sobre o ambiente de execucao, acesso a redes internas, ferramentas especificas,
cache persistente, hardware proprio ou execucao em infraestrutura da empresa.

Por outro lado, eles exigem mais cuidado com manutencao, seguranca, atualizacao
da maquina e disponibilidade do ambiente.

### Recursos similares em outras plataformas

Outras plataformas tambem oferecem recursos parecidos:

- GitHub Actions: self-hosted runners.
- Azure DevOps: self-hosted agents.
- GitLab CI: GitLab runners.
- Jenkins: agents/nodes.

### Conclusao

Com essa configuracao, o projeto passou a demonstrar diferentes formas de
Integracao Continua: pipeline com Jenkins, uso de actions do GitHub Marketplace
para relatorios e execucao de workflow em um self-hosted runner local.
