# notes-app-e2e-tests

## Overview
This repository contains end-to-end (E2E) tests for the Notes App. It is specifically designed to demonstrate **how to trigger Playwright tests when they live in a different repository to the source code**. Ideally the playwright code should live alongside the frontend code, but this is not always possible.

## Cross-Repo Triggers
Tests in this repository are automatically triggered when a deployment completes in the `notes-app-full-stack-testing` repository. This is achieved using GitHub Actions `repository_dispatch` events.

- **Frontend Deployment**: Triggers a `trigger-production-tests-frontend` event.
- **Backend Deployment**: Triggers a `trigger-production-tests-backend` event.

These events kick off the Playwright production tests workflow in this repository.