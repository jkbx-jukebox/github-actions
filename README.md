# github-actions

This repository is managed by the JKBX Dev Team in order to provide Github Actions templates. This allows JKBX's Dev Team to improve the pipelines over time and seamlessly integrate updates into the respective projects.

## Example

Here is an example of leveraging this resource from another repository. The `uses` keyword allows the external project to reference a specific implementation within this repository as well as feed in values from their own project, organization, etc. 

File: `.github/workflows/deploy.yml`
```yaml
name: Deploy Container to AWS

on:
  push:
    branches:
      - production
      - staging
  
permissions:
  id-token: write
  contents: read
  pull-requests: write

jobs:
  ecs-deployment-production:
    if: github.ref_name == 'production'
    uses: ./.github/workflows/ecs-production.yml
    with:
      account_number: ""
      aws_default_region: us-west-2
      aws_ecr_repository_version: v2
      dd_agent_major_version: 7
      dd_apm_enabled: true
      dd_env: prd
      dd_service: backstage-ecs-v2
      dd_site: datadoghq.com
      ref_name: ${{ github.ref_name }}
      repo_name: "backstage"
      sha: ${{ github.sha }}
      tfc_org_name: ""
      tfc_workspace_id: ""
      tfc_workspace_name: ""
    secrets: inherit
```

The above example utilizes the inherit secrets and environment variables to support deployment to ECS, these secrets can be from the Github Environment, Respository, Organization level.

If you are having trouble with the secrets, you can check to make sure the names are assigned correctly in the respective repository's settings. Inside of the repository settings, go to "Secrets and variables" and select "Actions". This will display all of the secret keys which you can confirm align with the github actions definition. 

If the keys are correct and you suspect that the secret values are incorrect please reach out to the Platform Team so that we can go over the keys from the different  services being leveraged through github actions.
