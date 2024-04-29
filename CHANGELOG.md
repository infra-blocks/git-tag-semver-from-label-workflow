# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.0.1] - 2024-03-17

### Changed

- Use `get-current-pull-request-action` instead of `8BitJonny/gh-get-current-pr`

## [3.0.0] - 2024-01-19

### Removed

- The legacy file exporting the workflow. Now the workflow is only accessible at `.github/workflows/workflow.yml`.

## [2.1.1] - 2024-01-18

### Fixed

- Incorrectly merged previous changes.

## [2.1.0] - 2024-01-18

### Added

- A duplicate of the reusable workflow at the `.github/workflows/workflow.yml`. This is part of a change to have
  every reusable workflow exported under the same conventional path.

## [2.0.2] - 2024-01-18

### Changed

- Use conventional workflow files `.github/workflows/worfklow.yml` everywhere in the CI where applicable.

## [2.0.1] - 2024-01-14

### Fixed

- Correctly forward the `inputs.sha` to the `8BitJonny/gh-get-current-pr` action.

## [2.0.0] - 2024-01-14

### Changed

- Reviewed the workflow interface and logic. It is now more event agnostic and can run on `push` events as well.
We added the `inputs.sha` to allow the caller to specify the commit SHA to tag. We also allow to customize the
`github-token` passed to the workflow.

## [1.0.2] - 2024-01-11

### Changed

- Bumped to `check-labels-actions@v2`.

## [1.0.1] - 2024-01-03

### Changed

- Leverage `status-report-action` internally to post PR comments.

## [1.0.0] - 2023-12-29

### Added

- First release!

[3.0.1]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v3.0.0...v3.0.1
[3.0.0]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v2.1.1...v3.0.0
[2.1.1]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v2.1.0...v2.1.1
[2.1.0]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v2.0.2...v2.1.0
[2.0.2]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v2.0.1...v2.0.2
[2.0.1]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v2.0.0...v2.0.1
[2.0.0]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v1.0.2...v2.0.0
[1.0.2]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v1.0.1...v1.0.2
[1.0.1]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/compare/v1.0.0...v1.0.1
[1.0.0]: https://github.com/infrastructure-blocks/git-tag-semver-from-label-workflow/releases/tag/v1.0.0
