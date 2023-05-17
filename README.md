# publish-on-pages
[![CI](https://github.com/Smartesting/publish-on-pages/actions/workflows/test.yml/badge.svg)](https://github.com/Smartesting/publish-on-pages/actions/workflows/test.yml)

Use GitHub pages to publish intermediary releases of JS packages

## Running tests

To ensure tests are executed in the same environment than on the CI, it might be easier to run them inside a docker container:

```shell
./test-in-docker
```

This will avoid errors that may happen with MacOS's `cp` for example, which do not treat peding slashes the same way than Ubuntu ...