# Choose the language
- [English](#angular-environments-and-github-actions)
- [Português](#angular-environments-e-github-actions)

# Angular Environments And Github Actions 

Using github actions to set the environment automatically in angular projects.

## Stack utilizada

**Front-end:** Angular

**CI/CD:** Github Actions

## Problem to solve

In this context, consider angular projects that have multiple environments, e.g. development, staging and production.

Without automation, you have to remember to manually change the environments file indicating whether the environment is production or not. We also need to remember to change the URL manually.

This is a problem because it's very easy to forget to change or to mistakenly change some information.

## Repository Structure

Consider that we have 3 environments: development (develop branch), staging (staging branch) and production (main branch). For each environment, the URL changes, where each one deploys and the configurations.

The structure can be different, for example, you can use release tags instead of or your own branch names, but the process is similar.

## Solution used

The solution used in this case was, when running the CI/CD in the branches configured and defined in the workflow file, run in the build step the change to the environment URL. So, the steps are:

- Set up the basic structure of the github actions workflow, defining my branches, steps and actions.

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


