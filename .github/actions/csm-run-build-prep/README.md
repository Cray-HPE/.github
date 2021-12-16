# CSM Run Build Prep

A GitHub action to compute basic quantites for later use in workflows generating artifacts.

CSM Run Build Prep provides:

1. A version computed either from `gitversion` (default) or from a specified
   version file (`.version` by default).
2. A boolean of if this particular version is considered "stable" or not. A
   stable build is denoted by a SemVer version with no prerelease information
   (build metadata is okay).
3. A build timestamp. This timestamp can be used for artifacts that should have
   the same build metadata (from, say, the same workflow run) but may not been
   built at the same time. For instance, if building a Docker image and Helm
   chart in a single workflow, the build timestamp can be used in both of their
   build metadata to get consistent version strings on the artifacts:

```
      csm-example-docker-image:1.2.3+20210728032600
      csm-example-helm-chart-1.2.3+20210728032600.tgz
```

## Usage

**NOTE**: this action should be run with the `self-hosted` CSM runner. This
runner has `gitversion` pre-installed and will greatly reduce the run time
of the action.

```yaml
  - name: Prep build metadata and fetch version (use gitversion)
    uses: Cray-HPE/.github/.github/csm-run-build-prep@v1-csm-run-build-prep

  - name: Prep build metadata and fetch version (use a file in the repo)
    uses: Cray-HPE/.github/.github/csm-run-build-prep@v1-csm-run-build-prep
    with:
      version-file: .some-version-file.txt
```
The tag can include just the major, or may include the minor and patch version
if desired for ensuring backwards compatibility. Versions are denoted by the
SemVer followed by the action name, e.g.

```
    v1.0.0-csm-run-build-prep
    v1.0-csm-run-build-prep
    v1-csm-run-build-prep
```
where in this case they are point to the same version.


## Action Inputs

| Name | Description | Default |
| --- | --- | --- |
| `use-gitversion` | Use `gitversion` to determine the version based on the branch/tag | `true` |
| `gitversion-version` | Version of the `gitversion` binary to install (if used) | `'5.8.1'`
| `gitversion-config` | Path relative to base repo of `gitversion` config file (if used) | `GitVersion.yml` |
| `version-file` | If not using `gitversion`, the path of the file to grab the SemVer version from` | `.version` |

## Action outputs

The following outputs can be used by subsequent workflow steps.

- `version` - The computed version (string)
- `is-stable` - If this build will produce stable artifacts based on `version` (boolean)
- `build-date-time` - A timestamp useful as metadata for multiple artifacts in different jobs (format: `%Y%m%d%H%M%S`)

Step outputs can be accessed as in the following example.
Note that in order to read the step outputs the action step must have an id.

```yml
      - name: Prep build metdata and fetch version
        id: buildprep
        uses: Cray-HPE/.github/.github/csm-run-build-prep@v1-csm-run-build-prep
      - name: Check outputs
        run: |
          echo "Version - ${{ steps.buildprep.outputs.version }}"
          echo "Stable - ${{ steps.buildprep.outputs.is-stable }}"
          echo "Build Timestamp - ${{ steps.buildprep.outputs.build-date-time }}"
```

## Action Behavior

The default behavior is to use `gitversion` to compute a version. If you do not
want to use this tool, or your repo is not configured for it (with gitflow),
you can simply specify a file to `cat` the version out into a variable.

## License

MIT
