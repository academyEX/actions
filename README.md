# Pixelfusion - Github actions

The below actions can be used in your project:

## Release scripts:

- [pixelfusion/actions/tag](./tag) - Run semantic release
- [pixelfusion/actions/build](./build) - Build a docker image
- [pixelfusion/actions/detect-changes](./detect-changes) - Check for changes in a folder
- [pixelfusion/actions/detect-environment](./detect-environment) - Pick environment to use

## Quality check scripts:

- [pixelfusion/actions/composer-diff](./composer-diff) - Describe changes to composer.lock
- [pixelfusion/actions/package-diff](./package-diff) - Describe changes to package-lock.json

## Notification scripts:

- [pixelfusion/actions/slack-notify](./slack-notify) - Send a release / deploy notification to Slack
