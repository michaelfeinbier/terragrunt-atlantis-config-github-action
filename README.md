# terragrunt-atlantis-config-github-action

Github action to ensure terragrunt-atlantis-config has been run, making sure that `atlantis.yaml` is up to date

## Inputs

### `version`

**Required** Revision of terragrunt-atlantis-config to use, such as 'v0.4.0', 'master', or some git sha

### `extra_args`

**Optional** Extra arguments to pass to the `generate` command. Here you can specify workflow names, autoplan settings, etc. You should not specify the `output` flag, but anything else is fair game from whatever version of terragrunt-atlantis-config you want to use.

## Example Usage

Here is an example that uses v0.5.0 of terragrunt-atlantis-config, passing args to enable autoplanning but to disable parallelization

```yaml
name: terragrunt-atlantis-config
on:
  push:
    paths:
    - '**.hcl'
    - '**.tf'
    - '**.hcl.json'

jobs:
  terragrunt_atlantis_config:
    runs-on: ubuntu-latest
    name: Validate atlantis.yaml
    steps:
      - uses: actions/checkout@v4
      - name: Ensure atlantis.yaml is up to date using terragrunt-atlantis-config
        id: atlantis_validator
        uses: transcend-io/terragrunt-atlantis-config-github-action@v0.0.5
        with:
          version: v0.5.0
          extra_args: '--autoplan --parallel=false'
```

If you want to save money on billable minutes, you could update to run on pull requests instead of each push
