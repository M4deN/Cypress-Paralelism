# Cypress-Paralelism-CI

curso-cypress-playground

---

# End-to-End Tests - Paralelism CI

Este projeto utiliza **Cypress** para realizar testes end-to-end, executados em paralelo usando **GitHub Actions**. O projeto também inclui a configuração do **ESLint** para garantir que o código dos testes siga boas práticas antes da execução. Além disso, o plugin **`cypress-split`** é utilizado para dividir os testes em múltiplas execuções paralelas, otimizando ainda mais o tempo de execução dos testes.

## Funcionalidades

- **Execução de testes end-to-end** usando o framework **Cypress**.
- **Análise estática de código** com o **ESLint**, garantindo que o código dos testes esteja de acordo com as melhores práticas antes da execução.
- **Execução de testes em paralelo** no GitHub Actions, otimizada pelo plugin **`cypress-split`** para distribuir os testes de forma eficiente entre múltiplos containers.
- **Testes padrão do Cypress**, configurados automaticamente após a instalação do Cypress.

## Como rodar os testes localmente

1. **Clone o repositório**:

   ```bash
   git clone https://github.com/SeuUsuario/Cypress-Paralelism-CI.git
   cd Cypress-Paralelism-CI
   ```

2. **Instale as dependências**:

   Certifique-se de ter o **Node.js** e o **npm** instalados. Para instalar as dependências do projeto, rode:

   ```bash
   npm install
   ```

3. **Rodar o ESLint (opcional, para verificar o código antes de rodar os testes)**:

   Antes de rodar os testes, você pode rodar o **ESLint** para garantir que o código dos testes siga as regras definidas:

   ```bash
   npx eslint "cypress/e2e/**/*.js" --fix
   ```

4. **Rodar os testes no Cypress**:

   Após instalar as dependências e rodar o ESLint (opcional), execute os testes no Cypress com o comando:

   ```bash
   npx cypress open
   ```

   Isso abrirá o Cypress em modo interativo, permitindo que você execute os testes manualmente. Para rodar os testes automaticamente em headless mode, use:

   ```bash
   npx cypress run
   ```

## GitHub Actions

O projeto está configurado com um pipeline de **GitHub Actions** para rodar os testes em múltiplos containers em paralelo sempre que houver um push no repositório.

### Como funciona

Quando um novo commit é feito no repositório (ou um pull request é aberto), o GitHub Actions irá:

1. Baixar o código.
2. Instalar as dependências com `npm install`.
3. Rodar o **ESLint** para garantir a qualidade do código.
4. Executar os testes do **Cypress** em paralelo, distribuindo os testes para múltiplos containers, o que acelera a execução dos testes.

![image](https://github.com/user-attachments/assets/f78723ef-073f-4f80-aef7-319b93049001)


### Uso do **`cypress-split`**

O plugin **`cypress-split`** é utilizado para dividir os testes do Cypress em múltiplas execuções paralelas, otimizando o tempo de execução. Ele funciona bem com a estratégia de **matriz** do GitHub Actions, permitindo que os testes sejam divididos igualmente entre os containers especificados.

- **Configuração do GitHub Actions**:
  - **matrix.containers**: O número de containers paralelos que irão executar os testes. Atualmente, está configurado para 15 containers.
  - **SPLIT** e **SPLIT_INDEX** são passados como variáveis de ambiente para dividir os testes em múltiplos processos. O plugin **`cypress-split`** utiliza essas variáveis para distribuir os testes entre os containers de forma eficiente.

- **Plugin cypress-split**:
  - Esse plugin foi configurado com a versão **`^1.24.5`** para dividir os testes do Cypress em paralelo, o que reduz significativamente o tempo de execução dos testes. Ele automaticamente distribui os testes baseados no número de containers configurados na matriz de GitHub Actions.

### Parâmetros de configuração do GitHub Actions

- **matrix.containers**: O número de containers paralelos que irão executar os testes. Atualmente, está configurado para 15 containers. Esse número pode ser alterado conforme necessário.
  
- **Estratégia de falha**: Está configurado com `fail-fast: false`, o que significa que, mesmo se algum container falhar, o restante dos containers continuará executando seus testes.

## Estrutura do projeto

O projeto segue a estrutura padrão do Cypress, com algumas modificações para permitir a execução paralela dos testes e garantir a qualidade do código com o ESLint:

```
Cypress-Paralelism-CI/
├── .github/
│   └── workflows/
│       └── cypress.yml            # Configuração do GitHub Actions
├── cypress/
│   └── e2e/
│       ├── 1-getting-started/
│       │   └── example.cy.js      # Teste padrão do Cypress
│       └── 2-advanced-examples/
│           └── waiting.cy.js      # Exemplo de teste avançado
├── package.json                   # Dependências do projeto
├── .eslintrc.json                  # Configuração do ESLint
└── README.md                      # Este arquivo
```

## Dependências

- **Cypress**: Framework para testes end-to-end.
- **ESLint**: Ferramenta para análise estática de código, garantindo que o código siga boas práticas.
- **cypress-split**: Plugin para dividir os testes do Cypress em execuções paralelas.
- **GitHub Actions**: Para automação de CI/CD, rodando os testes em paralelo.

---

### Notas adicionais:

- **Testes padrão do Cypress**: O projeto usa os testes padrão gerados pelo Cypress ao instalar o framework (`example.cy.js` e outros). Isso facilita o setup e a validação inicial, mas você pode adicionar ou modificar esses testes conforme necessário para o seu fluxo de trabalho.
  
- **Uso do `cypress-split`**: O plugin **`cypress-split`** é utilizado para dividir os testes em múltiplos containers, proporcionando uma execução mais rápida e eficiente dos testes em paralelo.
