:rotating_light: <b>Please remember that this repo is public.</b> :rotating_light:

# ga-workflows

Reusable CI/CD workflows.

## How to use

```yml
name: Build and deploy master

on:
  workflow_dispatch: {}

jobs:
  build_from_master:
    uses: IndependentIP/ga-workflows/.github/workflows/gradle-build.yml@master
    secrets: inherit # Calling repository must provide the secrets needed for anything to run 
    with:
      branch: 'master'
      sonar_scan: true
      docker_push: true

  deploy_to_test:
    uses: IndependentIP/ga-workflows/.github/workflows/k8s-phoenix-deploy.yml@master
    needs: build_from_master
    secrets: inherit
    with:
      tag: ${{ needs.build_from_master.outputs.tag }} # output from gradle-build.
      environment: integ

```