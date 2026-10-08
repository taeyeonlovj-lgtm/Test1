# Mattermost Docker
[![ShellCheck](https://github.com/mattermost/docker/actions/workflows/shellcheck.yml/badge.svg)](https://github.com/mattermost/docker/actions/workflows/shellcheck.yml)
[![Docker Test](https://github.com/mattermost/docker/actions/workflows/docker-test.yml/badge.svg)](https://github.com/mattermost/docker/actions/workflows/docker-test.yml)

The official Docker deployment solution for Mattermost.

## Install & Usage

Refer to the [Mattermost Docker deployment guide](https://docs.mattermost.com/deployment-guide/server/deploy-containers.html) for instructions on how to install and use this Docker image.

### Build from the Mattermost fork

To build and run the source repository [taeyeonlovj-lgtm/mattermost_RE](https://github.com/taeyeonlovj-lgtm/mattermost_RE), use the source override file. The Git URL is source code, not a container image; the override builds a local image from the selected Git ref before starting Mattermost. Copy `env.example` to `.env` (or add the source-build variables to your existing `.env`); the defaults point to this repository's `master` branch.

Without Nginx:

```sh
docker compose -f docker-compose.yml -f docker-compose.fork.yml -f docker-compose.without-nginx.yml up -d --build
```

With Nginx:

```sh
docker compose -f docker-compose.yml -f docker-compose.nginx.yml -f docker-compose.fork.yml up -d --build
```

Set `MATTERMOST_SOURCE_REF` in `.env` to a branch, tag, or commit SHA to choose a source revision. To fetch the latest commit from a moving branch after a previous build, rebuild without cache:

```sh
docker compose -f docker-compose.yml -f docker-compose.fork.yml build --no-cache mattermost
```

Then rerun the `up` command above for your Nginx or non-Nginx setup.

The build uses the Go and Node versions set by `MATTERMOST_GO_VERSION` and `MATTERMOST_NODE_VERSION`; update these to match `server/.go-version` and `.nvmrc` for the selected source revision. Building Mattermost from source takes substantially longer than pulling a published image and requires network access for Go, npm, and prepackaged plugins. The public source build produces Team Edition; Enterprise features require the separate Enterprise source. Back up the database and Mattermost data before switching an existing instance to a different source revision, since startup can apply database migrations.

### Using a prebuilt custom Mattermost image

If you publish a custom Mattermost server image to a registry, set its repository and tag in `.env`:

```env
MATTERMOST_IMAGE_REPOSITORY=ghcr.io/your-org/mattermost-custom
MATTERMOST_IMAGE_TAG=your-tag
```

`MATTERMOST_IMAGE_REPOSITORY` is the image name without its tag. Leave it empty to use the official image selected by `MATTERMOST_IMAGE`. Authenticate on the Docker host first if the image is in a private registry.

## Contribute
PRs are welcome, refer to our [contributing guide](https://developers.mattermost.com/contribute/getting-started/) for an overview of the Mattermost contribution process.

## Upgrading from `mattermost-docker`

This repository replaces the [deprecated mattermost-docker repository](https://github.com/mattermost/mattermost-docker). For an in-depth guide to upgrading, please refer to [this document](https://github.com/mattermost/docker/blob/main/scripts/UPGRADE.md).
