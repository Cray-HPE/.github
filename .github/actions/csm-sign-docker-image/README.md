# CSM Sign Docker Image

A GitHub action to sign published Docker images using cosign.

## Usage

```yaml
  - name: Sign an image in artifactory
    uses: Cray-HPE/.github/.github/csm-run-build-prep@v1-csm-sign-docker-image
    with:
      cosign-version: 'v1.0.0'
      cosign-gcp-project-id: ${{ secrets.COSIGN_GCP_PROJECT_ID }}
      cosign-gcp-sa-key: ${{ secrets.COSIGN_GCP_SA_KEY }}
      cosign-key: ${{ secrets.COSIGN_KEY }}
      registry: artifactory.algol60.net
      registry-username: github-actions-cray-hpe
      registry-password: ${{ secrets.ARTIFACTORY_ALGOL60_TOKEN }}
      github-sha: ${{ env.GITHUB_SHA }}
      image: artifactory.algol60.net/csm-docker/stable/image:tag
```

The action tag can include just the major, or may include the minor and patch
version if desired for ensuring backwards compatibility. Versions are denoted
by the SemVer followed by the action name, e.g.

```
    v1.0.0-csm-sign-docker-image
    v1.0-csm-sign-docker-image
    v1-csm-sign-docker-image
```
where in this case they are point to the same version.

## Action Inputs

| Name | Description | Default |
| --- | --- | --- |
| `cosign-version` |Version of cosign-release to use | `v1.0.0` |
| `cosign-gcp-project-id` | cosign project id in GCP | N/A |
| `cosign-gcp-sa-key` | cosign service account key in GCP | N/A |
| `cosign-key` | cosign application key | N/A |
| `registry` | Docker registry to push image signature | `artifactory.algol60.net` |
| `registry-username` | Container registry username | N/A |
| `registry-password` | Container registry user password | N/A |
| `github-sha` | Github commit SHA used to build the image | N/A |
| `image` | Docker image to sign | N/A |

## Action outputs

None

## Action Behavior

This action installs the `cosign-version` of cosign, logs in to the container
registry, signs the image, and pushes the signature to the registry.

For most CSM images and code repos, the cosign and registry parameters should
be set by existing repository secrets.

## License

MIT
