# Generate, Attach, and Sign container image SBOM

A GitHub action to generate SPDX Software Build of Materials (SBOM) using Syft
from a given container image. The SBOM artifact is attached to the given 
container image in a registry using cosign, and cosign is used to sign the SBOM. 


## Usage

```yaml
  - name: Generate, Attach, and Sign container image SBOM
    uses: Cray-HPE/.github/actions/csm-generate-attach-sign-sbom@v1-csm-generate-attach-sign-sbom
    id: sbom
    with:
      cosign-gcp-project-id: ${{ secrets.COSIGN_GCP_PROJECT_ID }}
      cosign-gcp-sa-key: ${{ secrets.COSIGN_GCP_SA_KEY }}
      cosign-key: ${{ secrets.COSIGN_KEY }}
      registry: artifactory.algol60.net
      registry-username: ${{ secrets.ARTIFACTORY_ALGOL60_USERNAME }}
      registry-password: ${{ secrets.ARTIFACTORY_ALGOL60_TOKEN }}
      github-sha: ${{ env.GITHUB_SHA }}
      image: ${{ steps.image-name.outputs.name }}:${{ steps.image-tag.outputs.tag }}
```

The action tag can include just the major, or may include the minor and patch
version if desired for ensuring backwards compatibility. Versions are denoted
by the SemVer followed by the action name, e.g.

```
    v1.0.0-csm-generate-attach-sign-sbom
    v1.0-csm-generate-attach-sign-sbom
    v1-csm-generate-attach-sign-sbom
```
where in this case they point to the same version.

## Action Inputs

| Name | Description | Default |
| --- | --- | --- |
| `cosign-version` | Version of cosign-release to use | `v1.4.1` |
| `cosign-gcp-project-id` | cosign project id in GCP | N/A |
| `cosign-gcp-sa-key` | cosign service account key in GCP | N/A |
| `cosign-key` | cosign application key | N/A |
| `registry` | Docker registry to push image SBOM | `artifactory.algol60.net` |
| `registry-username` | Container registry username | N/A |
| `registry-password` | Container registry user password | N/A |
| `github-sha` | Github commit SHA used to build the image | N/A |
| `image` | Container image to generate a SBOM of | N/A |

## Action outputs

| Name | Description |
| --- | --- |
| `sbom-location` | Location in the registry where the SBOM was pushed to |


## Action Behavior

This action installs the `cosign-version` of cosign, logs in to the container
registry, generates SBOM against the image using Syft, signs the SBOM, and 
pushes the SBOM to the registry.

For most CSM images and code repos, the cosign and registry parameters should
be set by existing repository secrets.

## License

MIT
