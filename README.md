# Github workflows

Repository with common workflows for Github Actions.

## npm package

Workflow for testing and publishing npm packages. Applicable for pull requests.

Steps:
 - checkout
 - setup node
 - install deps from package json
 - (pull requests only) lint commit messages
 - run typecheck
 - run lint
 - run tests
 - run build
 - (only if requirements are met) github release
 - (only if requirements are met) npm publish

### Configuration

```yaml
name: some CI

on:
  pull_request:
  push:
    branches:
      - master

jobs:
  test-and-release:
    uses: MegafonWebLab/github-workflows/.github/workflows/npm-package.yaml@v1.0.1-npm-package
```

### Options

All settings are optional.

| name             | type    | default         | description                                                                                    |
|------------------|---------|-----------------|------------------------------------------------------------------------------------------------|
| base-repo-owner  | string  | `MegafonWebLab` | Account where release steps should be proceeded.                                               |
| release-branch   | string  | `master`        | Branch where release steps should be proceeded.                                                |
| enable-typecheck | boolean | true            | Enable `yarn typecheck` run.                                                                   |
| enable-lint      | boolean | true            | Enable `yarn lint` run.                                                                        |
| enable-test      | boolean | true            | Enable `yarn test` run.                                                                        |
| enable-build     | boolean | true            | Enable `yarn build` run.                                                                       |
| npm-token        | secret  |                 | Npm token for publishing. Github release and npm publish won't be proceeded if token is empty. |

### Usage

Workflow usage assumes that target repo uses conventional commits. 

Any yarn scripts could be disabled with workflow inputs.

#### Release

Rules:
 - `npm-token` should be defined in workflow secrets for any release
 - `skip release` in message of the last commit will skip release
 - workflow run repo's owner should be equal with `base-repo-owner` input
 - workflow run branch should be equal with `release-branch` input

Changelog will be generated automatically from all commits with 
types `fix`, `perf`, `feat` and also from any type of 
commit with `BREAKING CHANGE:` in it's message.

All changes from previous tag to current commit will be published.

#### Example with publishing

```yaml
    uses: MegafonWebLab/github-workflows/.github/workflows/npm-package.yaml@v1.0.1-npm-package
    secrets:
      npm-token: ${{ secrets.NPM_TOKEN }}
```

#### Example from external repository

```yaml
    uses: MegafonWebLab/github-workflows/.github/workflows/npm-package.yaml@v1.0.1-npm-package
    with:
      base-repo-owner: my-account-name
```

## Contributing

Follow [CONTRIBUTING.md](CONTRIBUTING.md) and [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md).

