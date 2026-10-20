# GitHub Actions DevOps Activity

This repository is a hands-on collection of GitHub Actions examples. Each
workflow in `.github/workflows` demonstrates a different GitHub Actions
feature, from basic commands and event triggers to matrices, caching, service
containers, secrets, and custom webhooks.

## What is covered?

| Workflow | What it demonstrates | How it runs |
| --- | --- | --- |
| `hello.yml` | A basic workflow, shell commands, and the runner date | Push to `main` |
| `event.yml` | Push and pull request events and event context | Push or pull request to `main` or `develop` |
| `manual.yml` | Required and optional `workflow_dispatch` inputs | Run manually |
| `schedule.yml` | Scheduled workflows with a cron expression | Every five minutes |
| `matrix.yml` | Testing across Ubuntu, Windows, and macOS with Python 3.11 and 3.12 | Push to `main` or run manually |
| `multi-os.yml` | OS-specific commands and conditional steps | Push to `main` or run manually |
| `cache.yml` | Python setup, pip dependency caching, and installation | Push to `main` or run manually |
| `contexts.yml` | GitHub, runner, repository variable, environment, and conditional contexts | Push to `main` or run manually |
| `postgres.yml` | A PostgreSQL service container and a database connection check | Push to `main` or run manually |
| `secrets.yml` | Repository variables, secrets, permissions, and `GITHUB_TOKEN` | Push to `main` or run manually |
| `self-hosted.yml` | Running a job on a self-hosted runner | Run manually |
| `webhook.yml` | Handling a `repository_dispatch` webhook and its payload | `customwebhook` dispatch event |
| `custom-action.yml` | Calling the published `hello-action` custom action | Push to `main` or run manually |

## Requirements

- A GitHub repository with Actions enabled.
- Permission to view workflow runs and, when needed, configure repository
  variables, secrets, and Actions runners.
- Python 3.11 or newer for the matrix and dependency-cache examples.
- A self-hosted runner configured with the `self-hosted` label if you want to
  run `self-hosted.yml`.
- The GitHub CLI or another way to send a repository dispatch event if you want
  to run `webhook.yml`.

The Python dependency used by the caching example is listed in
[`requirements.txt`](requirements.txt).

## Repository setup

Most workflows work immediately after pushing this repository to GitHub. Before
running the configuration and runner examples, complete the following setup.

### Repository variable and secret

The `contexts.yml` and `secrets.yml` workflows use the following repository
configuration:

1. Add a repository variable named `APP_NAME`, for example `DevOpsDemo`.
2. Add a repository secret named `MY_SECRET` with a non-empty test value.

`secrets.yml` only checks that the secret exists; it does not print the secret.
The workflows request `contents: read` permission and verify that the built-in
`GITHUB_TOKEN` is available.

### Self-hosted runner

To run `self-hosted.yml`, add a self-hosted runner from the repository's
**Settings → Actions → Runners** page and ensure it is online. The workflow is
manual because self-hosted jobs execute on a machine that you manage.

### Webhook dispatch

`webhook.yml` listens for the `customwebhook` `repository_dispatch` type. For
example, with the GitHub CLI:

```bash
gh api repos/OWNER/REPOSITORY/dispatches \
  --method POST \
  -f event_type=customwebhook \
  -F client_payload[message]='Hello from a webhook'
```

Replace `OWNER/REPOSITORY` with the repository name. The workflow prints the
event name, event type, and the message from `client_payload`.

## Running workflows

- Push a commit to `main` or `develop` for workflows configured for those
  branches.
- Open the **Actions** tab, select a workflow, and choose **Run workflow** for
  workflows with `workflow_dispatch`.
- Review the job logs to see the commands and values each example produces.
- Scheduled workflows can be delayed by GitHub and may be disabled in inactive
  repositories. The five-minute schedule is intended for learning, not
  production polling.

## Notes

- `custom-action.yml` uses the published
  [`Muhammad-Roshaan-Idrees/hello-action`](https://github.com/Muhammad-Roshaan-Idrees/hello-action)
  action at `v1.0.1`.
- `postgres.yml` starts PostgreSQL 16 as a service container and installs the
  PostgreSQL client on the Ubuntu runner.
- Workflows that run on multiple operating systems intentionally use
  platform-specific shells and commands to demonstrate runner differences.

## License

This project is licensed under the [MIT License](LICENSE).
All seven GitHub Actions practical modules have been implemented and tested.
