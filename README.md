# store

The Redvars store: the signed application packages, catalogue and keyring that Redvars instances and the `redvars` CLI download, served by GitHub Pages at https://store.redvars.com. Platform releases and CLI executables are this repository's [releases](https://github.com/redvars/store/releases). The design is ADR-0013 in the `platform` repository.

**Nothing here is edited by hand.** Only the publish workflow in `platform` writes to this repository, through a GitHub App. Every file is checked by signature where it is used, so a change made any other way is refused by every instance. The history of `main` is the log of every publish, and nothing rewrites it.

## Layout

```
v1/
├── keyring.json, keyring.json.sig      the signing keys, signed by two of the three root keys
├── catalogue.json, catalogue.json.sig  every application version and release; signed again at least weekly
├── icons/<sha256>.png
└── packages/<scope>/<scope>-<version>.rvpkg
```

A version, once published, never changes; a fix is a new version. Release tags: `platform-<version>` holds `redvars-<version>-<os>-<arch>.tar.gz`, and `cli-<version>` holds `redvars-<version>-<os>-<arch>[.exe]`, each with its `.sha256` and `.sig`.

`v1/` arrives with the first keyring, after the root key ceremony. Until then there is nothing to download.

## Mirrors

Clone this repository, serve it as static files, and copy the release assets. Instances read `PLATFORM_STORE_URL` from `instance.env`. Nothing is trusted for where it came from, so a mirror is as safe as this copy.

`CNAME` gives GitHub Pages the domain; `.nojekyll` has it serve the files as they are.
