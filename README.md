# Generate semver release version in Github actions

This helper **Github Action** outputs several **semantic versioning** variables based on the git branch/tag that is building. It assumes that you are using the [twgit](https://github.com/twenga/twgit) git flow with conventions of:

* Released tags: `vx.x.x`
* Upcoming releases: `release-x.x.x`

_(where `x.x.x` is your release number)_

## Outputs

* `semver_release_string`: The full semver release string. e.g. `1.0.0+000598` or `2.1.0-SNAPSHOT4994`.
* `semver_release_number`: Just the release number part, e.g. `1.0.0` or `2.1.0`
* `semver_is_stable`: `true` or `false`, whether or not this release is a stable released version or an upcoming snapshot release.
* `semver_is_snapshot`: `true` or `false`, whether or not this release is an upcoming snapshot release or a stable released version.

## Usage in a Github actions workflow

```yml
# .github/workflows/publish.yml
name: Publish 
# you may wish to trigger this for other specifics, this is an example
on: 
  push:
    tags: 
      - v*
    branches:
      - release-*

jobs:
  publish:
    name: Publish
    runs-on: ubuntu-latest
    steps:
    - id: releasenumber
      uses: pixl8/github-action-twgit-release-version-generator@v1

    # then can use ${{ steps.releasenumber.outputs.variable_name }} in 
    # your step env vars, e.g.
    - name: my-next-step
      env:
        VERSION_NUMBER: ${{ steps.releasenumber.outputs.semver_release_string }}
```
