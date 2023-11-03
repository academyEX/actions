# Build

This action will build a docker container in your project, pushing
the image to a designated repository.

## Inputs

All env vars are mandatory, but build-dir can be omitted if building
from root. Make sure that your keys are provided via secrets,
not plain vars.

- role-to-assume: Required assume role arn
- build-dir: Optional folder name to the dockerfile to build
- VERSION: Must be the version to release as
- SENTRY_PROJECT: Sentry project slug
- AWS_REPOSITORY: The slug of the aws repository
- AWS_ACCOUNT: The Account ID
- AWS_REGION: AWS Region

## Outputs

This action has the below outputs:

- docker_image - The path to the full docker image versioned tag.
  Also set as DOCKER_IMAGE env var.
- docker_image_latest - The path to the docker image tagged as latest.
  Also set as DOCKER_IMAGE_LATEST env var.
- sentry_release - The sentry release version.
  Also set as SENTRY_RELEASE env var.

## Example

```yaml
  - name: Build docker image for admin
    uses: academyEX/actions/build@master
    with:
      role-to-assume: 'arn:aws:iam::503467447491:role/academyex-dev-iam-gh-baseghwebsitecd8C5935B6-4JS99IPOV767'
      build-dir: 'admin'
    env:
      VERSION: ${{ needs.release.outputs.version }}
      SENTRY_PROJECT: ${{ env.SLUG }}
      AWS_REPOSITORY: ${{ env.SLUG }}/admin
      AWS_ACCOUNT: ${{ vars.AWS_ACCOUNT }}
      AWS_REGION: ${{ vars.AWS_REGION }}
      AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
      AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
```
