# Managed Workflow Templates

## NPM Package

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

## Netlify

**Notes:** Please remember to update the base URL in the `NETLIFY_URL` section of the below script.

{% raw %}

```
name: Deploy Website
on:
  push:
    branches:
      - main
      - staging
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]
  pull_request_target:
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
  Version:
    uses: tech3k/actions/.github/workflows/bump-version.yml@main
    secrets:
      PAT: ${{ secrets.PAT }}

  Netlify:
    needs: Version
    uses: tech3k/actions/.github/workflows/netlify.yml@main
    with:
      NETLIFY_URL: mywebsite.co.uk
    secrets:
      NETLIFY_AUTH_TOKEN: ${{ secrets.NETLIFY_AUTH_TOKEN }}
      NETLIFY_SITE_ID: ${{ secrets.NETLIFY_SITE_ID }}
      NETLIFY_SITE_STAGING_ID: ${{ secrets.NETLIFY_SITE_STAGING_ID }}

  Review:
    needs: Netlify
    uses: tech3k/actions/.github/workflows/review-request.yml@main
    secrets:
      PAT: ${{ secrets.PAT }}
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

{% endraw %}

## Microservice

{% raw %}

```
name: Deploy Microservice
on:
  push:
    branches:
      - main
      - staging
  pull_request:
    types: [opened, synchronize, reopened, ready_for_review]
  pull_request_target:
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
  Version:
    uses: tech3k/actions/.github/workflows/bump-version.yml@main
    secrets:
      PAT: ${{ secrets.PAT }}

  CloudRun:
    needs: Version
    uses: tech3k/actions/.github/workflows/cloud-run.yml@main
    secrets:
      RUN_PROJECT_PRODUCTION: ${{ secrets.RUN_PROJECT_PRODUCTION }}
      RUN_PROJECT_STAGING: ${{ secrets.RUN_PROJECT_STAGING }}
      RUN_SA_KEY: ${{ secrets.RUN_SA_KEY }}

  Review:
    needs: CloudRun
    uses: tech3k/actions/.github/workflows/review-request.yml@main
    secrets:
      PAT: ${{ secrets.PAT }}
      SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

{% endraw %}
