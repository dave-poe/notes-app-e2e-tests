# notes-app-e2e-tests

**Link to Source Code**: [notes-app-full-stack-testing](https://github.com/helloitsdave/notes-app-full-stack-testing)

## Overview
This repository contains end-to-end (E2E) tests for the Notes App. It is specifically designed to demonstrate **how to trigger Playwright tests when they live in a different repository to the source code**. Ideally the playwright code should live alongside the frontend code, but this is not always possible.

## Cross-Repo Triggers
Tests in this repository are automatically triggered when a deployment completes in the `notes-app-full-stack-testing` repository. This is achieved using GitHub Actions `repository_dispatch` events.

- **Frontend Deployment**: Triggers a `trigger-production-tests-frontend` event.
- **Backend Deployment**: Triggers a `trigger-production-tests-backend` event.

These events kick off the Playwright production tests workflow in this repository.

## Trigger Configuration Example
To trigger these tests from another repository, use the `repository-dispatch` action:

```yaml
- name: Trigger E2E Tests
  if: success()
  uses: peter-evans/repository-dispatch@v3
  with:
    token: ${{ secrets.G_ACCESS_TOKEN }}
    repository: dave-poe/notes-app-e2e-tests
    event-type: trigger-production-tests-frontend
    client-payload: '{ "environment": "production" }'
```

> [!IMPORTANT]
> The `G_ACCESS_TOKEN` is a Personal Access Token (PAT) with `repo` scope. It must be created by a user with access to the target repository and added as a repository secret in the triggering repository. This is required because the default `GITHUB_TOKEN` does not have permission to trigger workflows in other repositories.

## Reading the Client Payload
In the receiving workflow (this repository), you can access the data sent in the `client-payload` using the `github.event.client_payload` context.

Example:

```yaml
jobs:
  build:
    steps:
    - name: Run Playwright Tests
      run: |
        echo "Environment: ${{ github.event.client_payload.environment }}"
        # Use the environment variable in your tests
```