# Build Containers

`elementary-build-containers` holds the OCI (podman) container images used by the
elementary OS build and deployment pipeline, primarily by
[elementary/os](https://github.com/elementary/os).

They are made to be generic and minimal.

## Available containers

| Image | Purpose |
| --- | --- |
| `ghcr.io/elementary/mkosi:{RELEASE}` | Runs `mkosi` to build the OS images |
| `ghcr.io/elementary/xorriso:{RELEASE}` | Assembles the live ISO (squashfs and GRUB) |

Both are published for `linux/amd64` and `linux/arm64`.

## Tags

Images are tagged to match elementary OS releases, for example `:tanit`.

The tag comes from the branch name: each release has its own branch here, and pushing to
it publishes `ghcr.io/elementary/<image>:<branch>`. Every build is additionally tagged
immutably as `:sha-<commit>`, which is what you should pin to if you need a build to stay
reproducible.

To add a new release:

1. Branch from the current release branch.
2. Add the new branch to the `branches:` filter in both
   `.github/workflows/mkosi.yml` and `.github/workflows/xorriso.yml`.

## Building locally

```bash
podman build -t mkosi -f mkosi/Containerfile .
podman build -t xorriso -f xorriso/Containerfile .
```

Images rebuild automatically when their `Containerfile` changes, and on a recurring
schedule to pick up upstream package updates.
