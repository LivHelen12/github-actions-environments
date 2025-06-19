# Choose the language / Escolha o idioma
[🇺🇸 English](#angular-environments-and-github-actions)   |   [🇧🇷 Português](#angular-environments-e-github-actions)

# Angular Environments And Github Actions 

Using github actions to set the environment automatically in angular projects.

## 🛠 Stack

**Front-end:** Angular

**CI/CD:** Github Actions

## ❗ Problem to solve

In this context, consider angular projects that have multiple environments, e.g. development, staging and production.

Without automation, you have to remember to manually change the `environments` file indicating whether the environment is production or not. We also need to remember to change the URL manually.

This is a problem because it's very easy to forget to change or to mistakenly change some information.

## 🗂 Repository Structure

Consider that we have 3 environments: development (`develop` branch), staging (`staging` branch) and production (`main` branch). For each environment, the URL changes, where each one deploys and the configurations.

The structure can be different, for example, you can use release tags instead of or your own branch names, but the process is similar.

## ✅ Solution used

The solution used in this case was, when running the **CI/CD** in the branches configured and defined in the `workflow` file, run in the `build` step the change to the environment URL. So, the steps are:

- Set up the basic structure of the `Github Actions Workflow`, defining my branches, steps and actions.

```yml
on:
  push:
    branches:
      - develop
      - staging
      - main

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Build for development
        if: github.ref == 'refs/heads/develop'
        run: npm run build:dev

      - name: Build for staging
        if: github.ref == 'refs/heads/staging'
        run: npm run build:staging

      - name: Build for production
        if: github.ref == 'refs/heads/main'
        run: npm run build:prod

```

- Define the different environments needed in the project developed with angular.

```
  - environments
        - environment.production.ts
        - environment.staging.ts
        - environment.ts
```


- Add specific build scripts for each environment in package-json.

```json
  "scripts": {
    "build:dev": "ng build --configuration=development",
    "build:staging": "ng build --configuration=staging",
    "build:prod": "ng build --configuration=production",
  }
```


- Change angular.json to automatically replace the URL using fileReplacements.

```json
"staging": {
    "fileReplacements": [
        {
           "replace": "src/environments/environment.ts",
           "with": "src/environments/environment.staging.ts"
        }
    ]
}
```

```json
"production": {
    "fileReplacements": [
        {
            "replace": "src/environments/environment.ts",
            "with": "src/environments/environment.production.ts"
        }
    ],
}
```
-------

# Angular Environments e GitHub Actions

Utilizando GitHub Actions para configurar automaticamente os ambientes em projetos Angular.

## 🛠 Stack utilizada

**Front-end:** Angular  
**CI/CD:** GitHub Actions

## ❗ Problema a ser resolvido

Neste contexto, considere projetos Angular que possuem múltiplos ambientes, como por exemplo: **development**, **staging** e **production**.

Sem automação, é necessário lembrar de alterar manualmente o arquivo de `environments`, indicando se o ambiente é de produção ou não. Também é preciso lembrar de alterar manualmente a **URL** correspondente.

Isso é um problema porque é muito fácil esquecer de fazer essa troca ou acabar modificando alguma informação incorretamente.

## 🗂 Estrutura do repositório

Considere que temos 3 ambientes: **development** (branch `develop`), **staging** (branch `staging`) e **production** (branch `main`). Para cada ambiente, a **URL** muda, assim como o local de deploy e as configurações.

Em alguns casos, a estrutura pode ser diferente, por exemplo, você pode usar **release tags** ou outros nomes de branches, mas o processo é semelhante, mudando apenas as regras definidas no `Github Workflow`.


## ✅ Solução utilizada

A solução utilizada neste caso foi, ao executar o **CI/CD** nas branches configuradas e definidas no arquivo de `workflow`, realizar na etapa de `build` a troca da **URL** do ambiente. Os passos foram:

- Configurar a estrutura básica do arquivo de `GitHub Actions workflow`, definindo as branches, etapas (steps) e ações.

```yml
on:
  push:
    branches:
      - develop
      - staging
      - main

jobs:
  build-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Build for development
        if: github.ref == 'refs/heads/develop'
        run: npm run build:dev

      - name: Build for staging
        if: github.ref == 'refs/heads/staging'
        run: npm run build:staging

      - name: Build for production
        if: github.ref == 'refs/heads/main'
        run: npm run build:prod

```

- Definir os diferentes ambientes necessários no projeto desenvolvido com Angular:

```
  - environments
        - environment.production.ts
        - environment.staging.ts
        - environment.ts
```

- Adicionar scripts de `build` específicos para cada ambiente no `package.json`:

```json
"scripts": {
  "build:dev": "ng build --configuration=development",
  "build:staging": "ng build --configuration=staging",
  "build:prod": "ng build --configuration=production"
}
```

- Alterar o `angular.json` para substituir automaticamente a URL utilizando `fileReplacements`:

```json
"staging": {
  "fileReplacements": [
    {
      "replace": "src/environments/environment.ts",
      "with": "src/environments/environment.staging.ts"
    }
  ]
}
```

```json
"production": {
  "fileReplacements": [
    {
      "replace": "src/environments/environment.ts",
      "with": "src/environments/environment.production.ts"
    }
  ]
}
```



