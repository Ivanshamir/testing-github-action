# Learning github action
This repository is mainly for personal learning of github actions. 
Before dive into github action, you need to know about YAML.The basics are:
### Scalars
Scalars are defined by integers, floats, strings, and Booleans. Given the flexibility that YAML provides, all the following are acceptable:
```
integer: 10
#different ways to write booleans
boolean: true
another-boolean: yes
yet-another-boolean: off
a key with spaces: a value with spaces
#different ways to write strings
string-with-quotes: "a string with quotes"
string-without-quotes: a string with quotes
new-lines-are-kept-as-new-lines: |
  This is line number 1, and it will show exactly this way
  This is line number 2, and it will show exactly this way
  This is line number 3… you get it
multi-lines-here-that-will-render-as-one-line: >
  When you want a block of text made of many lines
  To show all in one single line
  You can use the special character greater than
```
### Sequences
Sequences are also known as lists of data. Items in a sequence are identified by the dash-space-item syntax. The following workflow file has an example of a block sequence:
```
runs-on: ubuntu-latest
steps:
- name: Close Issue
```
### Mappings
Mappings allow for the creation of more complex structures, using a combination of sequences and scalars. Note how the following example has scalars (strings) and a sequence (list):
```
steps:
- name: Close Issue
  uses: peter-evans/close-issue@v1
  if: contains(github.event.issue.labels.*.name, 'support')
  with:
    comment: |
      Sorry, but we'd like to keep issues related to code in this repository. Thank you
```

# GitHub Actions' core concepts and components
• Events
• Jobs
• Steps
• Actions
• Runners
1. Events: GitHub Actions are event-driven. This means that you can define what happens after a specific event occurs.Events are specific activities that trigger workflows. Workflows can be triggered by three groups of events:
• Scheduled events
• Manual events
• Webhook events
Let's look at each in detail:
#### Scheduled events
Scheduled events trigger a workflow run at a specified time. They use the POSIX cron syntax.
The following example shows part of a workflow file, written in YAML, where the workflow will be triggered every 5 minutes:
```
on:
  schedule:
    - cron: '*/5 * * * *'
```

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
