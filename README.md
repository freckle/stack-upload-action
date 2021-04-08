# Stack Upload Action

GitHub Action to run `stack sdist` and upload to Hackage using an API Key.

## Usage

```yaml
jobs:
  release:
    runs-on: ubuntu-latest
    env:
      # Can be created at https://hackage.haskell.org/user/{username}/manage
      HACKAGE_API_KEY: ${{ secrets.HACKAGE_API_KEY }}
    steps:
      - uses: actions/checkout@v2
      - uses: freckle/stack-upload-action@main
```

## Inputs

- `working-directory`: directory to work in, default is `.`
- `pvp-bounds`: `--pvp-bounds` argument to pass, default is `none`.

## FAQ

- Why not `stack upload`?

  `stack upload` supports only Username and Password; it does not support API
  Keys in requests to Hackage. API Keys can be created or revoked for specific
  uses or repositories, and so make much more sense in CI.

---

[LICENSE](./LICENSE)
