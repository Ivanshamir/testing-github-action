# Testing github action
This repository is mainly for personal learning of github actions. 
1. We create simple action which is responsible for giphy comment, when this has found to this string: **looks good to me.**
```
name: Test LGTM giphy

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the master branch
  issue_comment:
    types: [created]
  pull_request_review:
    types: [submitted]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2
      - uses: micnncim/action-lgtm-reaction@master # Set some version.
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          GIPHY_API_KEY: ${{ secrets.GIPHY_API_KEY }}
        with:
          trigger: '[".*looks good to me.*"]'
          overide: true
          source: 'giphy'

```
In here we only need to set ```GIPHY_API_KEY``` secret. ```GITHUB_TOKEN``` will automatically create when this action will execute.
When you enable GitHub Actions in your repository, GitHub automatically does two things: it installs a GitHub App on your repository and creates a GITHUB_TOKEN .
GITHUB_TOKEN works as a GitHub App token, which means that you can use it to authenticate on behalf of the GitHub App. GITHUB_TOKEN is short-lived and expires when the job is finished. GitHub then obtains an installation access token for the next job before the job starts.
2.