# BrainCache Marketplace

The official, signed app catalog for [BrainCache](https://github.com/manwhoshreds/braincache).
It is published with GitHub Pages and read by production builds of BrainCache from:

```
https://manwhoshreds.github.io/braincache-marketplace/v1/
```

## Layout

```text
v1/
├── repository.json       # the catalog
├── repository.json.sig   # Ed25519 signature of repository.json
├── icons/
├── screenshots/
└── packages/<id>-<version>.brainapp
```

BrainCache rejects any catalog whose signature is not from a trusted publisher
key, and any catalog older than the one it has already seen. Every package is
pinned by SHA-256 in the signed catalog.

## Publishing an app

From a BrainCache checkout:

1. Copy the app's `.brainapp` into `v1/packages/` and its icon into `v1/icons/`.
2. Add its entry to `v1/repository.json` (`id`, `name`, `summary`,
   `description`, `author`, `category`, `version`, `package`, `sha256`, and
   optionally `icon`, `screenshots`, `minBrainCacheVersion`).
3. Sign: `go run ./cmd/marketplacectl sign -root ../braincache-marketplace/v1`
4. Commit and push `repository.json` and `repository.json.sig` together with
   the new files.
5. Verify the live catalog:
   `go run ./cmd/marketplacectl verify -url https://manwhoshreds.github.io/braincache-marketplace/v1/`

Never commit the signing key.
