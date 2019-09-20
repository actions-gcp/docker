# Generate Docker configuration for gcr.io

This [GitHub Action][action] generates a Docker configuration file
with access to [Container Registry][gcr] using a temporary OAuth 2.0
access token.

`docker/config` requires a [service account][sa] with permissions
to access Container Registry. A JSON key for that service account
should be stored as a GitHub secret and supplied as the `key` input.

`docker/config` runs as a Docker Action, so the configuration file
should be written to a path accessible from Actions that run outside
of Docker, for example `/github/workspace` which is available as
`$GITHUB_WORKSPACE`. The `DOCKER_CONFIG` environment variable should
be set to the directory containing the generated `config.json` for
any steps that invoke docker.

## Inputs

| Name          | Description                     |
| ------------- | ------------------------------- |
| *config*      | path to generated config file   |
| *key*         | JSON [service account key][key] |

## Usage

```yaml
      - uses: actions-gcp/docker/config@master
        with:
          config: /github/workspace/.docker/config.json
          key:    ${{ secrets.SERVICE_ACCOUNT_KEY }}
      - name: build
        run: |
          export DOCKER_CONFIG=$GITHUB_WORKSPACE/.docker

          docker ...
```

[action]: https://github.com/features/actions
[gcr]:    https://cloud.google.com/container-registry
[sa]:     https://cloud.google.com/compute/docs/access/service-accounts
[key]:    https://console.cloud.google.com/apis/credentials/serviceaccountkey
