# Slack Notify

Send a release / deploy notification to a Slack
[incoming webhook](https://api.slack.com/messaging/webhooks). Renders a
Block Kit message with the status, version, environment, service URL and a
link back to the workflow run.

The payload is assembled with `jq`, so all values are JSON-escaped (safe to
pass branch names, versions, etc. without manual quoting).

## Inputs

- `webhook` (required) - Slack incoming webhook URL. Store it as a secret.
- `status` (required) - `success` or `failure`. Anything other than
  `success` is treated as a failure.
- `environment` - Label shown in the message, e.g. `UAT` or `PROD`.
- `version` - Release version / tag.
- `url` - Service URL to include in the message.
- `context` - Optional extra mrkdwn line, e.g. which stage failed.

The workflow run link and actor are derived from the `github` context
automatically.

## Behaviour

`curl --fail` is used, so a misconfigured webhook (non-2xx response) fails the
step rather than passing silently.

## Example

A single `notify` job that reports both success and failure for a build +
deploy pipeline:

```yaml
  notify:
    name: Slack notification
    runs-on: ubuntu-latest
    needs: [release-build, deploy-uat]
    if: always()
    steps:
      - name: Notify deploy success
        if: needs.deploy-uat.result == 'success'
        uses: academyEX/actions/slack-notify@master
        with:
          webhook: ${{ secrets.SLACK_WEBHOOK_URL }}
          status: success
          environment: UAT
          version: ${{ needs.release-build.outputs.version }}
          url: https://app.uat.academyex.com
      - name: Notify release failure
        if: needs.release-build.result == 'failure' || needs.deploy-uat.result == 'failure'
        uses: academyEX/actions/slack-notify@master
        with:
          webhook: ${{ secrets.SLACK_WEBHOOK_URL }}
          status: failure
          environment: UAT
          version: ${{ needs.release-build.outputs.version }}
          context: >-
            *Failed stage:* ${{ needs.release-build.result == 'failure' && 'build (release-build)' || 'deploy (deploy-uat)' }}
```
