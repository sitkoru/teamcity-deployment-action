# teamcity-deployment-action

Action to manage Teamcity Deployments

# Usage

## Trigger

```yml
name: Release

on:
  release:
    types:
      - released
  workflow_dispatch:

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Prepare
        id: prep
        shell: bash
        run: |
          VERSION=${GITHUB_REF#refs/tags/}
          echo ::set-output name=version::${VERSION}
      # some build actions
      - uses: sitkoru/teamcity-deployment-action@v1
        name: Create Teamcity deployment
        with:
          token: ${{ secrets.TEAMCITY_TOKEN }}
          url: ${{ secrets.TEAMCITY_URL }}
          ids: ${{ secrets.TEAMCITY_BUILD_IDS }}
          version: ${{ github.event.deployment.description }}
```