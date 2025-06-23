<<<<<<< HEAD
# deploy-demo-preview-example
Example showing the use of Github actions to deploy an app to preview changes and spin down the app once the PR is merged
=======
## GitHub Workflows

This repository includes two GitHub workflows to automate preview deployments of your application using the [deploy demo app](https://github.com/aptible/deploy-demo-app) as an example:

### 1. Preview Deployment (preview.yml)

The `preview.yml` workflow automatically deploys a preview app for every pull request. This allows reviewers to test changes in an environment of their choice before merging to the main branch.

**What this workflow does:**
- Triggers on pull request creation and when pushing changes to existing pull requests
- Creates a PostgreSQL database and Redis database for the preview app
- Configures a new Aptible app with the necessary environment variables to connect to the databases from the previous steps
- Builds and pushes a Docker image tagged with the PR number
- Deploys the application to Aptible using the image built in the previous step
- Creates an HTTPS endpoint for accessing the preview app

**How to use it:**
1. Simply create a pull request against this repository
2. The workflow will automatically provision the infrastructure and deploy your changes
3. Access your preview app at the endpoint created by the workflow

**Inputs:**
- **Required inputs** (must be configured as GitHub secrets and variables):
  - `vars.APTIBLE_ROBOT_USERNAME`: Aptible robot account email for authentication
  - `secrets.APTIBLE_ROBOT_PASSWORD`: Aptible robot account password
  - `secrets.DOCKERHUB_USERNAME`: DockerHub username for pushing images
  - `secrets.DOCKERHUB_TOKEN`: DockerHub access token for authentication
  - `secrets.DOCKERHUB_PASSWORD`: DockerHub password for Aptible deployment

- **Configurable environment variables** (can be modified in the workflow file):
  - `APTIBLE_ENVIRONMENT`: The Aptible environment to deploy to (default: `preview-apps`)
  - `IMAGE_NAME`: Docker image name to use (default: `aptible/deploy-demo-app`)
  - `APTIBLE_APP_NAME`: Name pattern for the Aptible app (default: `demo-pr-${{ github.event.number }}`)
  - `TAG`: Docker image tag to use (default: `pr-${{ github.event.number }}`)
  - `CLI_URL`: URL for the Aptible CLI tool (default: Ubuntu 16.04 version)

### 2. Preview Cleanup (deprovision_preview.yml)

The `deprovision_preview.yml` workflow handles cleanup of preview resources when a pull request is closed.

**What this workflow does:**
- Triggers when a pull request is closed (merged or rejected)
- Deprovisions the Aptible app associated with the PR
- Deprovisions the PostgreSQL and Redis databases created for the preview

**How to use it:**
- No manual action required - cleanup happens automatically when PRs are closed

**Inputs:**
- **Required inputs** (must be configured as GitHub secrets and variables):
  - `vars.APTIBLE_ROBOT_USERNAME`: Aptible robot account email for authentication
  - `secrets.APTIBLE_ROBOT_PASSWORD`: Aptible robot account password

- **Configurable environment variables** (can be modified in the workflow file):
  - `APTIBLE_ENVIRONMENT`: The Aptible environment to deploy to (default: `preview-apps`)
  - `APTIBLE_APP_NAME`: Name pattern for the Aptible app (default: `demo-pr-${{ github.event.number }}`)
  - `CLI_URL`: URL for the Aptible CLI tool (default: Ubuntu 16.04 version)

## Copyright

Copyright (c) 2024 [Aptible](https://www.aptible.com). All rights reserved.

[<img src="https://avatars2.githubusercontent.com/u/1580788?v=4&s=60" />](https://github.com/UserNotFound)
>>>>>>> eddbf47 (initial import)
