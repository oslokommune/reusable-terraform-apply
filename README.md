# reusable-terraform-apply

Reusable GitHub Actions workflow for applying Terraform stacks.

## Usage

```yaml
name: "Terraform apply"

on:
  push:
    branches:
      - "main"
    paths:
      - stacks/**

  workflow_dispatch:
    inputs:
      selected-stacks:
        description: 'A comma-separated list of stacks to apply. Supports glob patterns (e.g., "stacks/dev/{dns,iam}", "stacks/dev/app-*", "stacks/**")'
        required: true
        type: string

concurrency:
  group: ${{ github.workflow }}
  cancel-in-progress: false

jobs:
  apply:
    name: Terraform apply
    uses: oslokommune/reusable-terraform-apply/.github/workflows/reusable-terraform-apply.yml@v1
    with:
      selected-stacks: ${{ inputs.selected-stacks }}
    secrets:
      ssh-private-key: ${{ secrets.GOLDEN_PATH_IAC_PRIVATE_DEPLOY_KEY }}
```
