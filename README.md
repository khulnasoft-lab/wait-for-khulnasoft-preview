# Wait for Khulnasoft Preview — A GitHub Action ⏱

Do you have other Github actions (Lighthouse, Cypress, etc) that depend on the Khulnasoft Preview URL? This action will wait until the url is available before running the next task.

Please note that this action is supposed to be run on the `pull_request` or `push` events.

## Inputs

### `token` (Required)

The github secret `${{ secrets.GITHUB_TOKEN }}`

### `environment`

Optional — The name of the environment that was deployed to (e.g., staging or production)

### `max_timeout`

Optional — The amount of time to spend waiting on Khulnasoft. Defaults to `60` seconds

### `allow_inactive`

Optional - Use the most recent inactive deployment (previously deployed preview) associated with the pull request if
no new deployment is available. Defaults to `false`.

### `check_interval`

Optional - How often (in seconds) should we make the HTTP request checking to see if the deployment is available? Defaults to `2` seconds.

### `khulnasoft_password`

Optional - The [password](https://khulnasoft.com/docs/concepts/projects/overview#password-protection) for the deployment

### `path`

Optional - The URL that tests should run against (eg. `path: "https://khulnasoft.com"`).

## Outputs

### `url`

The khulnasoft deploy preview url that was deployed.

### `khulnasoft_jwt`

If accessing a password protected site, the JWT from the login event. This can be passed on to e2e tests, for instance.

## Example usage

Basic Usage

```yaml
steps:
  - name: Waiting for 200 from the Khulnasoft Preview
    uses: khulnasoft-lab/wait-for-khulnasoft-preview@v1.3.1
    id: waitFor200
    with:
      token: ${{ secrets.GITHUB_TOKEN }}
      max_timeout: 60
  # access preview url
  - run: echo ${{steps.waitFor200.outputs.url}}
```

## Building

The Action is bundled via [ncc](https://github.com/khulnasoft/ncc). See [this discussion](https://github.com/actions/hello-world-javascript-action/issues/12) for more information.

```sh
npm run build
# outputs the build to dist/index.js
```

## Tests

Unit tests with [Jest](https://jestjs.io/) and [Mock Service Worker](https://mswjs.io/)

```
npm test
```