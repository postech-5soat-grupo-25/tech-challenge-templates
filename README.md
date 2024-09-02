# `templates` | PosTech 5SOAT • Grupo 25

Este repositório contém templates para serem usados nos microserviços que compõem a solução do Tech Challenge. Os templates visam padronizar e facilitar a configuração de diferentes aspectos do nosso desenvolvimento, como integrações contínuas, cobertura de código etc.

## GitHub Actions

### Code Coverage

O template `code-coverage.yml` configura uma GitHub Action para calcular a cobertura de código do projeto utilizando `cargo-tarpaulin` e fazer o upload dos resultados para o Codecov. Abaixo está uma breve descrição dos componentes do template:

- **container:** Especifica o uso de um container com a imagem `xd009642/tarpaulin:develop-nightly` para garantir a consistência do ambiente de execução.
- **steps:** Inclui os passos do workflow:
  - **actions/checkout:** Faz o checkout do repositório.
  - **Run `cargo-tarpaulin`:** Executa o `cargo-tarpaulin` para calcular a cobertura de código.
  - **Upload to codecov.io:** Faz o upload dos resultados da cobertura para o Codecov.
  - **Archive Results:** Arquiva os resultados da cobertura como um artefato.

#### Como Usar

Para utilizar o template de cobertura de código em seu repositório, siga os passos abaixo:

1. Crie um arquivo `.github/workflows/code-coverage.yml` no seu repositório.

2. Adicione o seguinte conteúdo ao arquivo criado:

```yaml
name: Code Coverage

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  coverage:
    name: Check
    uses: postech-5soat-grupo-25/tech-challenge-templates/.github/workflows/code-coverage.yml@main
    secrets:
      codecov_token:  ${{ secrets.CODECOV_TOKEN }}
```

3. Garanta que o segredo `CODECOV_TOKEN` esteja configurado nas secrets do seu repositório.

Com isso, toda vez que houver um _push_ para a branch main ou um _pull request_ for aberto, sincronizado ou reaberto, a ação de cobertura de código será executada automaticamente, gerando e enviando o relatório para o Codecov.

### Code Quality

O template `code-quality.yml` configura uma GitHub Action para verificar a qualidade do código utilizando várias ferramentas e fazer o upload dos resultados para o SonarCloud. Abaixo está uma breve descrição dos componentes do template:

- **steps:** Inclui os passos do workflow:
  - **actions/checkout:** Faz o checkout do repositório.
  - **Setup Environment:** Configura as variáveis de ambiente necessárias para o SonarCloud.
  - **Cache `cargo` Registry:** Faz cache do registro do cargo para acelerar builds.
  - **Cache `cargo` Index:** Faz cache do índice do cargo para acelerar builds.
  - **Cache `cargo` Build:** Faz cache das builds do cargo para acelerar builds.
  - **Setup Rust:** Configura o Rust e instala os componentes `rustfmt` e `clippy`.
  - **Install Tools:** Instala as ferramentas `cargo-audit`, `cargo-udeps` e `cargo-sonar`.
  - **Run `clippy`:** Executa o `clippy` para análise estática do código.
  - **Run `cargo-audit`:** Executa o `cargo-audit` para verificar vulnerabilidades.
  - **Run `cargo-udeps`:** Executa o `cargo-udeps` para detectar dependências não utilizadas.
  - **Run `cargo-sonar`:** Executa o `cargo-sonar` para coletar relatórios de qualidade.
  - **Upload to sonarcloud.io:** Faz o upload dos resultados para o SonarCloud.
  - **Archive Results:** Arquiva os resultados da análise como um artefato.

#### Como Usar

Para utilizar o template de qualidade de código em seu repositório, siga os passos abaixo:

1. Crie um arquivo `.github/workflows/code-quality.yml `no seu repositório.

2. Adicione o seguinte conteúdo ao arquivo criado:

```yaml
name: Code Quality

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  quality:
    name: Check
    uses: postech-5soat-grupo-25/tech-challenge-templates/.github/workflows/code-quality.yml@main
    secrets:
      sonarcloud_token: ${{ secrets.SONARCLOUD_TOKEN }}
```

3. Garanta que o segredo `SONARCLOUD_TOKEN` esteja configurado nas secrets do seu repositório.

Com isso, toda vez que houver um _push_ para a branch main ou um _pull request_ for aberto, sincronizado ou reaberto, a ação de qualidade de código será executada automaticamente, gerando e enviando o relatório para o SonarCloud.

### Unit Tests

O template `unit-tests.yml` configura uma GitHub Action para executar testes unitários no projeto utilizando `cargo test`, gerar um relatório JUnit e arquivar os resultados. Abaixo está uma breve descrição dos componentes do template:

- **steps:** Inclui os passos do workflow:
  - **actions/checkout:** Faz o checkout do repositório.
  - **Setup Rust:** Configura o Rust com a toolchain nightly.
  - **Run `cargo test`:** Executa os testes unitários e gera um relatório em formato JSON.
  - **Prepare JUnit Report:** Converte o relatório JSON para o formato JUnit.
  - **Publish Test Report:** Publica o relatório de testes.
  - **Archive Results:** Arquiva os resultados dos testes como um artefato.

#### Como Usar

Para utilizar o template de testes unitários em seu repositório, siga os passos abaixo:

1. Crie um arquivo `.github/workflows/unit-tests.yml` no seu repositório.

2. Adicione o seguinte conteúdo ao arquivo criado:

```yaml
name: Unit Tests
permissions:
  checks: write

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  tests:
    name: Check
    uses: postech-5soat-grupo-25/tech-challenge-templates/.github/workflows/unit-tests.yml@main
```

Com isso, toda vez que houver um _push_ para a branch main ou um _pull request_ for aberto, sincronizado ou reaberto, a ação de testes unitários será executada automaticamente, gerando e publicando o relatório de testes.
