# CSM K8s Api Checker

A Github action that runs tooling on k8s yaml files to detect removed or
outdated api usage. Normally this is ran in git repos containing helm charts
though repos with yaml files will do.

For now it only runs the pluto scanning tool, https://pluto.docs.fairwinds.com/
Future updates will expand this to include more tools.

In a nutshell if this action is intended to fail pull requests if removed k8a
api's are found and a comment posted. Deprecated api's will result in a comment.
Finally no deprecated or removed api findings will result in no comment at all.

Note that the same comment will be updated, or removed based on pr updates.

Each installation or usage in a repo needs to ensure that any templating needed
for helm is accounted for in th prerequisite step.

This step is simply shell commands that contain any vars needed for the repo to
enable tooling to detect outdated api usage. That means if your helm chart has
optional features that aren't on by default, you *NEED* to set them in
*prerequisites* for the tooling to find it and this action to work.

## Usage Example

```yaml
name: Run Api Checker
on:
  pull_request:
    branches:
      - main
  workflow_dispatch:
jobs:
  k8s-api-checker:
    runs-on: ubuntu-20.04

    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0

      - name: Check for removed and deprecated api usage
        uses: Cray-HPE/.github/actions/csm-k8s-api-checker@v0-csm-k8s-api-checker
        with:
          prerequisite: |
            helm template example/chart --generate-name --dry-run --set some-value=true > example-chart.yaml

```

On a repo with a helm chart that posesses removed api's this results in a comment like so:
![Removed api example](removed-api-example.png)

And if that is fixed the pr is green instead of red:
![Deprecated apis example](deprecated-apis-only-example.png)

If those are fixed the comment is removed entirely, no screenshot for this case however.

## Action Inputs

Note all are optional, but realistically prerequisite needs to be set per repo.

| Name | Description | Default |
| --- | --- | --- |
| `prerequisite` | Steps, in shell, to take to generate yaml files that can be scanned | helm version |
| `enable-pr-comment` | Comment in pr or not | true |
| `fail-on-removed` | If a pr should fail if removed apis are found | true |
| `fail-on-deprecated` | If a pr should fail if deprecated apis are found | false |
| `chart-directory` | The directory under which to search for chart files | . |

## Action outputs

A comment in your pr depending on findings.

## Action Behavior

Under the hood all this action does is checks out a pr's branch, installs pluto
and helm, and then runs whatever is in prerequisite to generate files that can
be scanned by tooling later.

## License

MIT
