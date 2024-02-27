**ARCHIVED**: support for API key authentication was added to Stack some time
ago, and the version installed on GitHub is new enough. This means this
action is longer necessary and we will not be maintianing it.

The following is equivalent to using this action:

```yaml
jobs:
  release:
    runs-on: ubuntu-latest
    env:
      HACKAGE_KEY: ${{ secrets.HACKAGE_API_KEY }}
    steps:
      - uses: actions/checkout@v4
      - run: stack upload --pvp-bounds lower
```

# Stack Upload Action

GitHub Action to run `stack sdist` and upload to Hackage using an API Key.

## Usage

```yaml
jobs:
  release:
    runs-on: ubuntu-latest
    env:
      HACKAGE_API_KEY: ${{ secrets.HACKAGE_API_KEY }}
    steps:
      - uses: actions/checkout@v2
      - uses: freckle/stack-upload-action@v2
```

## Inputs

- `working-directory`: directory to work in, default is `.`
- `pvp-bounds`: `--pvp-bounds` argument to pass, default is `lower`.

## Environment

- `HACKAGE_API_KEY`: an Authentication Token with rights to upload (required)

  See `https://hackage.haskell.org/user/{username}/manage` to create one.

- `STACK_YAML`: the `stack.yaml` to use for bounds-setting (optional)

## PVP Bounds

Under certain circumstances, this Action may choose a "better" `stack.yaml` for
the purposes of bounds-setting.

If you:

- Are using `pvp-bounds: lower` (the default)
- Have not set a `$STACK_YAML`
- Have multiple files of the form `stack-lts-*.yaml`

the Action will select the oldest, version-sorted `stack-lts-*.yaml` file.

This leads to wider (and more accurate) compatibility for your package, assuming
this is the oldest set of dependencies you test against and thus more accurately
describes your true lower bounds.

To avoid this, ensure any one of the conditions above is not met.

## FAQ

- Why not `stack upload`?

  `stack upload` supports only Username and Password; it does not support API
  Keys in requests to Hackage\*. API Keys can be created or revoked for specific
  uses or repositories, and so make much more sense in CI.

  \*This has [been implemented][stack-commit], but not yet released.

  [stack-commit]: https://github.com/hololeap/stack/commit/a2eff4d023148aeac288029b91ec531e5e120092

---

[LICENSE](./LICENSE)
