# reusable-terraform-apply

Reusable GitHub Actions workflow for applying Terraform stacks.

## Inputs

| Input                  | Type    | Default         | Description                                                                                                                                                                                                                         |
|------------------------|---------|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `config-file`          | string  | `.gp.cicd.json` | Path to CI/CD configuration file.                                                                                                                                                                                                   |
| `selected-stacks`      | string  | `""`            | Comma/newline-delimited list of stack patterns to deploy (e.g., `stacks/dev/{dns,iam}`, `stacks/dev/app-hello,stacks/prod/app-hello`). By default, only stacks with changed files are deployed. Set this to override that behavior. |
| `ignored-stacks`       | string  | `""`            | Comma/newline-delimited list of stack patterns to always ignore.                                                                                                                                                                    |
| `core-stacks`          | string  | `""`            | Comma/newline-delimited list of patterns of stacks that should deploy before other stacks to handle dependencies. If `override-core-stacks` is true, these replace the default core stack patterns.                                 |
| `override-core-stacks` | boolean | `false`         | If true, replace the default core stack patterns with those provided in `core-stacks`. By default, `core-stacks` is appended to a predefined list of patterns.                                                                      |

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
