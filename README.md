# git-tag-semver-from-label-workflow
[![Release](https://github.com/infra-blocks/git-tag-semver-from-label-workflow/actions/workflows/release.yml/badge.svg)](https://github.com/infra-blocks/git-tag-semver-from-label-workflow/actions/workflows/release.yml)
[![Update From Template](https://github.com/infra-blocks/git-tag-semver-from-label-workflow/actions/workflows/update-from-template.yml/badge.svg)](https://github.com/infra-blocks/git-tag-semver-from-label-workflow/actions/workflows/update-from-template.yml)

This workflow is used to automatically manage semantic versioning tags based on PR labels.

The PR *should* have one valid label at all times and *must* have one such label upon invocation of this workflow.
See [check-has-semver-label-workflow](https://github.com/infra-blocks/check-has-semver-label-workflow) for
details on the required PR label. To enforce the presence of such a label, we also recommended setting up
[check-has-semver-label-workflow](https://github.com/infra-blocks/check-has-semver-label-workflow) on PR
events as a different workflow.

This workflow gets the current PR, extracts the semver label and tags the git tree accordingly.
See [git-tag-semver-action](https://github.com/infra-blocks/git-tag-semver-action) for more information on
the tagging behavior.

PR comments will be emitted to notify the users or errors/notices/successes.

## Inputs

|     Name      | Required | Description                                                                                                                                                                  |
|:-------------:|:--------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     skip      |  false   | A boolean indicating whether to skip the workflow. This is to workaround the required checks discrepancy when the workflow is skipped from the caller. It defaults to false. |

## Secrets

|     Name     | Required | Description                                                                                                                                                                                                                                                                      |
|:------------:|:--------:|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| github-token |  false   | The GitHub token utilized to push the tags. If pushing a tag matching a protection rule, this should be a PAT. Defaults to the $GITHUB_TOKEN otherwise. Note that the workflow still utilizes the $GITHUB_TOKEN for other operations, such as posting status report PR comments. |

## Outputs

| Name | Description                                                               |
|:----:|---------------------------------------------------------------------------|
| tags | A stringified JSON array containing the tags pushed against the git tree. |

## Permissions

|     Scope     | Level | Reason                                                       |
|:-------------:|:-----:|--------------------------------------------------------------|
|   contents    | write | Required to push tags.                                       |
| pull-requests | write | Required to post comments about the status of this workflow. |

## Concurrency controls

No concurrency is defined at this workflow's level. If any is required, it should be defined in the caller workflow.

On push events to release branch, however, we recommend setting the following concurrency:
```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
```

If you set up your PRs so that they must be up to date before merging, then the likelihood of concurrent jobs
diminishes. In the event that it still happens, this concurrency setting will queue **the most recent** invocation
until the ongoing one terminates. It would have been better if we could have a longer queue, but, at the time of
this writing, such is the limitation of the GitHub Actions engine.

## Timeouts

N/A

## Usage

### Upon merging on a release branch

```yaml
name: Git Tag Semver Release From Label

on:
  push:
    branches:
      - your-release-branch

concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}

jobs:
  git-tag-semver-from-label:
    uses: infra-blocks/git-tag-semver-from-label-workflow/.github/workflows/workflow.yml@v2
    permissions:
      contents: write
      pull-requests: write
    secrets:
      github-token: ${{ secrets.PAT }} # Required to push against protected tags
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infra-blocks/git-tag-semver-from-label-workflow).
