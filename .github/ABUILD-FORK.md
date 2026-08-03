# Abuild squashfs-tools fork

This repository retains the historical SquashFS reader used by Abuild.
The upstream project is maintained at
<https://github.com/plougher/squashfs-tools>.

The branches have deliberately separate roles:

- `master` follows the public upstream repository;
- `abuild` is the exact source revision formerly pinned by Abuild; and
- `abuild-gh` adds only GitHub CI and its supporting files to `abuild`.

The `abuild` branch is based on the official SquashFS tools 4.5 tag and adds
two secunet changes:

1. `05a0836e` prints extended attributes in the long `unsquashfs` listing.
2. `b5cb93ff` decodes the binary `security.capability` value into capability
   names and flags.

Abuild no longer builds this private reader; it switched to distribution
inspection tools in 2026.  The branch remains public so historical Abuild
revisions and the provenance of their reader behavior stay reproducible.

## Continuous integration

Run the same check locally with Docker:

```sh
.github/ci/run .github/ci/check
```

The image build compiles `mksquashfs` and `unsquashfs` from the checkout and,
while it still has the required container capability, generates a small image
containing both an ordinary user xattr and `security.capability`.  The test
container then runs without network access, Linux capabilities or root and
checks both human-readable presentations plus ordinary extraction.  This
executes the behavior introduced by both secunet commits.
