# CSM Gitflow Mergeback

A GitHub action to create a pull request from master to develop after a gitflow
production release. This action along with a workflow with the proper PR event
hooks can be used to automate the housekeeping of Gitflow when release branch
and hotfix branch changes need to be brought back to the develop branch.

See https://github.com/Cray-HPE/community/wiki/Gitflow-Development-Process#differences-with-original-git-flow
for an explanation of the usage of this action.

## Usage

See the csm-gitflow-mergeback.tml workflow template in this repository.

When using this action, you can specify just the major version, or you may
include the minor and patch version if desired for ensuring backwards
compatibility. Versions are denoted by the SemVer followed by the action name,
e.g.

```
    v1.1.5-csm-gitflow-mergeback
    v1.1-csm-gitflow-mergeback
    v1-csm-gitflow-mergeback
```
where in this case they point to the same version.


## Action Inputs

| Name | Description | Default |
| --- | --- | --- |
| `base-branch` | The pull request base branch (almost always "develop" | `develop` |
| `source-branch` | The branch that needs to be merged back (almost always "master" or "main" | `master`
| `pr-reviewers` | Default team or person to add as a reviewer to the pull request | `''` |
| `automerge` | Boolean on whether to set the PR to automerge (see "Automerge Conditions" below) | `false` |

### Automerge Conditions

In order for automerge to work, the repository must meet the conditions specified in https://github.com/peter-evans/enable-pull-request-automerge#conditions.


## Action outputs

The following outputs can be used by subsequent workflow steps.

| Name | Description | Example |
| --- | --- | --- |
| `pr-number` | The number of the PR that was generated | `34` |
| `pr-url` | The url to the generated pull request | `https://github.com/Cray-HPE/myrepo/pulls/34` |

## Action Behavior

This action will create a pull request by first checking out the
`source-branch` and branching from it. A pull request is then created to
the `base-branch` using the CSM Gitflow Mergeback Bot App account.

If automerge is enabled on the repository, meets the repository conditions above, and is enabled,
the PR should automerge when the PR conditions are met.

## License

MIT
