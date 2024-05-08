# Purpose

This custom GitHub Action were created to integrate GitHub Actions releases and Slack Notifications. It allows you to send slack notifications after release is done.

# Github Action: Datadog CI `flipdishbytes/github-release-slack-notification@v1.0`

To use this Datadog CI action, add it to your pipeline workflow YAML file. Here are examples of adding traces to the pipeline depending on your needs.

### How it works?

1. Creates version with the format like 
1. Reads `SlackWebhookURL` AWS Secret using aws cli (preinstalled on all GitHub agents) and using it for sending slack webhooks.


**There is `SlackWebhookURL` secret stored at AWS SSM in Flipdish Management account and separate role created for GitHub Actions which can be assumed only by Flipdish Org GitHub Actions repositories and has IAM permissions only for getting this secret value.**

**P.S. this role can't be assumed by any repo outside of Flipdish GitHub Org.**

### How to use?

#### `flipdishbytes/datadog-ci@v1.1` - no need for DD_API_KEY being set. Execution time 5s.
Should be used by default in Flipdish Org. It doesn't require `DD_API_KEY` secret being set in the repository.

```yaml
name: GH Action workflow with Creating GH Release and Sending Slack Notification

on:
  pull_request:
    types:
      - opened
      - reopened
      - synchronize
      - edited
    branches:
      - 'main'

permissions: # v1.1 requires id-token write permission to assume AWS role. Makes sure you added this to your yml.
  contents: write # grants permissions to create gh releases
  id-token: write # grants permissions to assume aws role and write tokens

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: GitHub Release and Notification
        uses: flipdishbytes/github-release-slack-notification@v1.1
        with:
          projectName: 'Template'
          sendSlackNotification: true
          slackChannel: 'test-notifications'

```
