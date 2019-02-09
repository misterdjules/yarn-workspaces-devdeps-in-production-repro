# Repro

Run:

```
$ yarn install --production
```

and note that the only dev dependency at the root of this repo (`some-module`)
is installed, along with its dependencies (in this case `prettier`.

See https://github.com/yarnpkg/yarn/issues/6323 for a potentially related issue
in the Yarn repo.

Interestingly, setting the `devDependency` to be at version `^2.0.0` is looking
for `some-module` at version `^2.0.0`, which doesn't exist in the workspace, and
so it tries to fetch it from a registry.
