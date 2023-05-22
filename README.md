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
          library_name: my-super-library   # The name of the library that will be uploaded
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v1
        with:
          path: 'pages'
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v2
```


## Running tests

To ensure tests are executed in the same environment than on the CI, it might be easier to run them inside a docker container:

```shell
./test-in-docker
```

This will avoid errors that may happen with MacOS's `cp` for example, which do not treat peding slashes the same way than Ubuntu ...