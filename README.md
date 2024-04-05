# publish-on-pages
[![CI](https://github.com/Smartesting/publish-on-pages/actions/workflows/test.yml/badge.svg)](https://github.com/Smartesting/publish-on-pages/actions/workflows/test.yml)

Use GitHub pages to publish intermediary releases of JS packages. This is not meant to replace NPM, just to ease using libraries without having to publish them on npm.

The workflow looks something like this:

```yaml
jobs:
  # Single deploy job since we're just deploying
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Create pages
        uses: Smartesting/publish-on-pages@main
        with:
          branches: main some-other-branch # The list of branches that should be made available
          tags: "*-beta"                   # Expression to match the tags to be published
          library_name: my-super-library   # The name of the library that will be uploaded
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: 'pages'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```

You can then reference the library in your package.json file using:

```json
"dependencies": {
  "my-super-library": "https://<company-name>.github.io/my-super-library/main/my-super-library.tgz"
}
```

Or use the bundled file in an HTML tag:

```html
<script async src="https://<company-name>.github.io/my-super-library/main/my-super-library.js" type="text/javascript"
></script>
```

## Variables

There is not that much to be configured in the action:

| name              | mandatory | description                                                               | default      |
|-------------------|-----------|---------------------------------------------------------------------------|--------------|
| library_name      | ✓         | The name of the library                                                   |              |
| branches          |           | The list of branches that should be deployed to GH pages                  | `main`       |
| tags              |           | Expression to match tags that should be deployed to GH pages              |              |
| npm_build_targets |           | The list of the npm commands to be executed to create the bundle and pack | `build pack` |
| bundle            |           | The name of the bundled JS file (eg: my-library.min.js)                   |              |


## Running tests

In order to run the tests, you first have to clone a copy of [`shutnit2](https://github.com/kward/shunit2) locally:

```shell
git clone https://github.com/kward/shunit2.git
```

You can then run all the tests using the `tests/run` file:

```shell
./tests/run
```

To ensure tests are executed in the same environment than on the CI, it might be easier to run them inside a docker container:

```shell
./test-in-docker
```
