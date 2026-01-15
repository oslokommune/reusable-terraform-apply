# reusable-terraform-apply

Reusable GitHub Actions workflow for applying Terraform stacks.

## Usage

```yaml
jobs:
  apply:
    uses: oslokommune/reusable-terraform-apply/.github/workflows/reusable-terraform-apply.yml@v1
    secrets:
      ssh-private-key: ${{ secrets.GP_IAC_DEPLOY_KEY }}
```
