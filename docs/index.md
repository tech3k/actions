## Tech3k Managed Deployment Scripts

This page and its links describes how you can use each part of the Managed deployment scripts to ensure that deployment for all projects is extremely easy.

### Initial Setup

To install a Managed Deploy Script the first step is to create a default github action script. First create a blank file in the `.github/workflows` directory. We suggest this be called `deploy.yml` for conventions sake.

```sh
mkdir .github
mkdir .github/workflows
touch .github/workflows/deploy.yml
```

Next, we need to create some basic headers for the dwployment script. This will define a name for the script and when it gets ran. This should be the same across all repositories, but the name can be customised in the format `Deploy ...` e.g. `Deploy to Netlify` or `Deploy Expo Application`

```
name: Deploy NPM Package
on:
  push:
    branches:
      - main
      - staging
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]
  pull_request_target:
    types: [opened, synchronize, reopened, ready_for_review]
```
