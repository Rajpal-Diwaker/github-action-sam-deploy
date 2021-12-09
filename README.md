# github-action-sam-deploy

This repository contains GitHub Reusable Workflows (perhaps singular) to reduce boilerplate and increase consistency across SAM deployments in multiple repositories.

## Overall Usage

Simply call the workflow with the required inputs and secrets.
Please read the YML itself to see input and secret descriptions.

## `deployment.yml` Example

Place in a `.yml` file such as this one in your `.github/workflows` folder. [Refer to the documentation on workflow YAML syntax here.](https://help.github.com/en/articles/workflow-syntax-for-github-actions)

```yaml
name: Deployment
on: push

jobs:
  deploy-sam-to-production:
    name: "Deploy SAM Application to Production"

    # Please replace all X.Y.Z values with the latest tag.
    uses: rewindio/github-action-sam-deploy/.github/workflows/sam-deploy.yml@vX.Y.Z
    with:
      s3_bucket: "my-unique-bucket-name"
      stack_name: "my-stack-name"
      sam_params_repo_path: "sam-params/prod.cfg"
    secrets:
      EB_AWS_ACCESS_KEY_ID: ${{ secrets.STAGING_AWS_ACCESS_KEY_ID }}
      EB_AWS_SECRET_ACCESS_KEY: ${{ secrets.STAGING_AWS_SECRET_ACCESS_KEY }}
      # Secrets are not accessible unless they are shared, so we need these three even though they are redundant.
      DEPLOY_FAILURES_SLACK_WEBHOOK_URL: ${{ secrets.DEPLOY_FAILURES_SLACK_WEBHOOK_URL }}
      DEPLOY_SUCCESS_SLACK_WEBHOOK_URL: ${{ secrets.DEPLOY_SUCCESS_SLACK_WEBHOOK_URL }}
      LOOKUP_USER_EMAIL_SLACK_TOKEN: ${{ secrets.LOOKUP_USER_EMAIL_SLACK_TOKEN }}

