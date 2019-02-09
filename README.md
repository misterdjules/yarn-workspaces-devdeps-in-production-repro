# Repro

Run:

```
$ yarn install --production
```

and note that the only dev dependency at the root of this repo (`some-module`)
is installed, along with its dependencies (in this case `prettier`.

See https://github.com/yarnpkg/yarn/issues/6323 for a potentially related issue
in the Yarn repo.

This looks more like intended behavior than a bug, since workspaces are not
necessarily meant to be dev dependencies (they're an integral part of a module).

However, interestingly, setting the `devDependency` to be at version `^2.0.0` is
looking for `some-module` at version `^2.0.0`, which doesn't exist in the
workspace, and so it tries to fetch it from a registry.

Now, when removing `some-module` from the root's `devDependencies` and running
`yarn install --production`, `prettier` is still installed.

Given that, it seems that what's going on is:

1. Modules in all workspaces are always installed, whether in production or
   development.

2. If a module versioned in a workspace in mentioned in `devDependencies`, `yarn
   install --production` will try to install a version that is compatible with
   both the development and production use case.

As a result, one should not use `devDependencies` as a way to *not* install
packages that are part of any workspace.