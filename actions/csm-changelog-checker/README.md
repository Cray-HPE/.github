# CSM Changelog Checker

A GitHub action to ensure changelogs are updated on pull requests.

Action is skipped if the pull request contains either of the
`gitflow-maintenance` or `dependabot` labels.

## Usage

```yaml
name: Changelog Checker
on:
  pull_request:
    types: [opened, synchronize, reopened, labeled, unlabeled]

jobs:
  check-changelog:
    steps:
      - name: Check for a modified changelog
        uses: Cray-HPE/.github/actions/csm-check-changelog@v1-csm-check-changelog
```

The tag can include just the major, or may include the minor and patch version
if desired for ensuring backwards compatibility. Versions are denoted by the
SemVer followed by the action name, e.g.

```
    v1.1.5-csm-check-changelog
    v1.1-csm-check-changelog
    v1-csm-check-changelog
```
where in this case they point to the same version.


## Action Inputs

| Name | Description | Default |
| --- | --- | --- |
| `changelog-file` | relative path in repository to changelog file to check | `CHANGELOG.md` |

## Action outputs

None

## Action Behavior

The default behavior is to fail if the `changelog-file` file has not been modified between the head and base branches.

If the pull request is labeled with `gitflow-maintenance` or `dependabot`, then this check will be skipped.

## License

MIT
