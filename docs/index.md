# Tech3k Managed Deployment Scripts

This page and its links describes how you can use each part of the Managed deployment scripts to ensure that deployment for all projects is extremely easy.

## Initial Setup

To install a Managed Deploy Script the first step is to create a default github action script. First create a blank file in the `.github/workflows` directory. We suggest this be called `deploy.yml` for conventions sake.

```sh
mkdir .github
mkdir .github/workflows
touch .github/workflows/deploy.yml
```

Next, we need to create some basic headers for the dwployment script. This will define a name for the script and when it gets ran. This should be the same across all repositories, but the name can be customised in the format `Deploy ...` e.g. `Deploy to Netlify` or `Deploy Expo Application`

{% raw %}

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

{% endraw %}

## Jobs

Jobs are tasks that will be ran during the deployment script. As we are using Managed Workflows these jobs should only define the workflow that will be ran, some variables or secrets that will be passed to the workflow, and possibly an order to be ran in.

The jobs section should look something like the following...

{% raw %}

```
jobs:
  Version:
    uses: tech3k/actions/.github/workflows/bump-version.yml@main
    secrets:
      NPM_TOKEN: ${{ secrets.NPM_TOKEN }}
      PAT: ${{ secrets.PAT }}

  Review:
    needs: Version
    uses: tech3k/actions/.github/workflows/review-request.yml@main
    secrets:
      PAT: ${{ secrets.PAT }}
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

{% endraw %}

In each job to be ran the initial defined name (i.e. `Version` and `Review` in the code above) are identifiers that can be changed to whatever you wish, but again we recommend keeping these to the standard we specify on this page.

| Option      | Description                                                                      |
| ----------- | -------------------------------------------------------------------------------- |
| **needs**   | This job will not run unless all jobs in the needs section have been ran         |
| **uses**    | This is the link to the Managed Workflow being used                              |
| **secrets** | This section holds a list of secrets that will be passed to the Managed Workflow |
| **with**    | This section shows any hardcoded values to be passed through                     |

## Job Descriptions

## Ready Made Templates

The links below will take you to some ready made templates that can just be copy and pasted into the appropriate repository. They should work out of the box as long as all secrets have been correctly added.

- [NPM Package](templates.md#npm-package)
- [Netlify](templates.md#netlify)
- [Microservice](templates.md#microservice)
