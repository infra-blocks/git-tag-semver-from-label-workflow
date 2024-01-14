# git-tag-semver-from-label-workflow

This workflow is used to automatically manage semantic tags on a release branch leveraging PR labels.

It's meant to be run on PR events (including labeling events) and parse out 1 of the 4 listed
labels:
- no version
- patch
- minor
- major

Every PR should have a label. If the PR isn't meant to release a version, then `no version` label
should be applied to it.

All other 3 labels will produce/update 3 tags on the repository upon merging the PR.
See [here](https://github.com/infrastructure-blocks/git-tag-semver-action) for more information on the tagging behavior.

PR comments will be emitted to notify the users or errors/notices/successes.

## Inputs

N/A

## Secrets

N/A

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

Because this workflow is meant to be called on several different types of pull_request events, we mostly
only care about the state of the PR of the latest event. Hence, we cancel any ongoing workflow.

|       Field        |                  Value                   |
|:------------------:|:----------------------------------------:|
|       group        | ${{ github.workflow }}-${{ github.ref }} |
| cancel-in-progress |                   true                   |

## Usage

```yaml
name: Git Tag Semver From Label

# Running on all those events allow to check for the proper existence of a versioning tag.
on:
  pull_request:
    branches:
      - master
    types:
      - opened
      - reopened
      - synchronize
      - labeled
      - unlabeled
      - closed

jobs:
  git-tag-semver-from-label:
    uses: infrastructure-blocks/git-tag-semver-from-label-workflow/.github/workflows/git-tag-semver-from-label.yml@v1
    permissions:
      contents: write
      pull-requests: write
```

### Releasing

The releasing is handled at git level with semantic versioning tags. Those are automatically generated and managed
by the [git-tag-semver-from-label-workflow](https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow).
