
# deploy-demo-preview-example
Example use of Github actions to automatically deploy an app to preview changes and spin down the app once the PR is closed.
## GitHub Workflows

This repository includes two GitHub workflows to automate preview deployments of your application using the [deploy demo app](https://github.com/aptible/deploy-demo-app) as an example:

### 1. Preview Deployment (preview.yml)

The `preview.yml` workflow automatically deploys a preview app for every pull request. This allows reviewers to deploy the changes introduced in their PR to a separate app in the Aptible environment of their choice to complete the necessary testing before merging their changes to main. 

**What this workflow does:**
- Triggers on pull request creation and when pushing changes to the pull request
- Creates a PostgreSQL database and Redis database for the preview app
- Configures a new Aptible app configured with the necessary environment variables to connect to the databases from the previous step
- Builds and pushes a Docker image tagged with the PR number
- Deploys the application to Aptible using the image built in the previous step
- Creates an HTTPS endpoint for accessing the preview app

**Inputs:**
- **Required inputs** (must be configured as GitHub secrets):
  - `vars.APTIBLE_ROBOT_USERNAME`: Aptible robot account email for authentication
  - `secrets.APTIBLE_ROBOT_PASSWORD`: Aptible robot account password
  - `secrets.DOCKERHUB_USERNAME`: DockerHub username for pushing images and deploying to Aptible
  - `secrets.DOCKERHUB_TOKEN`: DockerHub access token for authentication
  - `secrets.DOCKERHUB_PASSWORD`: DockerHub password for Deplpy to Aptible step

- **Configurable environment variables** (can be modified in the workflow file):
  - `APTIBLE_ENVIRONMENT`: The Aptible environment to deploy to (default: `preview-apps`)
  - `IMAGE_NAME`: Docker image name to use (default: `aptible/deploy-demo-app`)
  - `APTIBLE_APP_NAME`: Name pattern for the Aptible app (default: `demo-pr-${{ github.event.number }}`)
  - `TAG`: Docker image tag to use (default: `pr-${{ github.event.number }}`)

### 2. Preview Cleanup (deprovision_preview.yml)

The `deprovision_preview.yml` workflow handles cleanup of preview resources when a pull request is closed.

**What this workflow does:**
- Triggers when a pull request is closed (merged or rejected)
- Deprovisions the Aptible app and endpoint associated with the PR
- Deprovisions the PostgreSQL and Redis databases created for the preview

**Inputs:**
- **Required inputs** (must be configured as GitHub secrets and variables):
  - `vars.APTIBLE_ROBOT_USERNAME`: Aptible robot account email for authentication
  - `secrets.APTIBLE_ROBOT_PASSWORD`: Aptible robot account password

- **Configurable environment variables** (can be modified in the workflow file):
  - `APTIBLE_ENVIRONMENT`: The Aptible environment to deploy to (default: `preview-apps`)
  - `APTIBLE_APP_NAME`: Name pattern for the Aptible app (default: `demo-pr-${{ github.event.number }}`)

## Considerations

These workflows cover the simplest use case so they can be customized further using the options provided by using different [events to trigger](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)the workflows. The [push](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#push) and [pull_request](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#pull_request)event documentation in particular includes useful examples for common use cases.

## Copyright

Copyright (c) 2025 [Aptible](https://www.aptible.com). All rights reserved.
