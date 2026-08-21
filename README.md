# toolkit-versions

`AllowedVersions.txt` is the remote switch for the **iChemistry Support Toolkit**, an internal
browser extension used by the Intersolia support team. The extension fetches this file every time it
opens and refuses to run if the build is older than `minimumVersion`.

Maintained by Kasper Mikkelsen. Nothing here is secret — it is a version number, published so that a
build handed out as a `.zip` can still be withdrawn after the fact.

## The file

```json
{
  "minimumVersion": "4.2.0"
}
```

That is the entire contract. Unknown keys are ignored.

## To withdraw a build

1. Build and hand out the replacement **first**.
2. Then raise `minimumVersion` to the new version. Every older build freezes the next time it is
   opened — or within a minute, if it is already open.

Allow up to ~5 minutes for GitHub's raw CDN to serve the change.

## Three ways to lock out the entire team by accident

- **Deleting the `minimumVersion` line.** A file with no `minimumVersion` verifies nothing, and the
  extension fails *closed*. To lift a block, **lower the number** (`"0"` allows every build) — never
  delete the line.
- **Invalid JSON.** A stray comma or a missing quote has the same effect as the file being gone.
- **Setting `minimumVersion` above any version that was actually distributed.** That blocks
  everyone, including whoever is trying to fix it.

Recovery from all three is the same: correct the file. The lockout is not permanent — each open
window re-checks every 30 seconds and unfreezes itself once the file reads cleanly again.
