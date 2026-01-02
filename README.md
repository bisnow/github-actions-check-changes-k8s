# Check for Changes (Build, K8s, CloudFormation)

A GitHub composite action that detects changes in build files, Kubernetes manifests, and CloudFormation templates between commits.

## What it does

This action analyzes git diffs to determine what changed in your repository, outputting three boolean flags:
- `build_container`: Whether non-infrastructure files changed (triggers container builds)
- `k8s_changed`: Whether Kubernetes manifests changed
- `cf_changed`: Whether CloudFormation templates changed

On manual runs (`workflow_dispatch`), all outputs are set to `true`.

## Inputs

| Input | Description | Default |
|-------|-------------|---------|
| `k8s-path` | Path pattern for k8s files | `^\.k8s/` |
| `cf-file` | CloudFormation file name | `aws-resources\.yaml` |
| `event-name` | GitHub event name | `github.event_name` |
| `event-before` | Git commit SHA before | `github.event.before` |
| `event-sha` | Git commit SHA | `github.sha` |

## Outputs

| Output | Description |
|--------|-------------|
| `build_container` | `true` if build files changed, `false` otherwise |
| `k8s_changed` | `true` if k8s files changed, `false` otherwise |
| `cf_changed` | `true` if CloudFormation changed, `false` otherwise |

## Example 

```yaml
jobs:
  check-changes:
    name: Check for Changes (Build, K8s, CloudFormation)
    runs-on: arc-runners-biscred
    outputs:
      build_container: ${{ steps.check.outputs.build_container }}
      k8s_changed: ${{ steps.check.outputs.k8s_changed }}
      cf_changed: ${{ steps.check.outputs.cf_changed }}
    steps:
      - name: Check for changes
        id: check
        uses: bisnow/github-actions-check-changes-k8s@main
```
