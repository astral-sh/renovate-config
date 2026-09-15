# renovate-config

Org-wide Renovate presets for Astral.

## Workflow tool versions and checksums

The preset includes a custom regex manager for annotated tool versions in `.github/workflows/*.yml` and `.yaml` files. It can update a version and its release-asset checksum together, using Renovate's [`github-release-attachments` datasource](https://docs.renovatebot.com/modules/datasource/github-release-attachments/).

For example, to update uv and its checksum in an `astral-sh/setup-uv` step:

```yaml
with:
  # renovate: datasource=github-release-attachments depName=astral-sh/uv
  version: "0.12.7"
  checksum: "788f18abea7c5f55d6216e4f5613fd89d4d59b631efeec117b2b07fe72f1da21"
```

Keep the annotation, double-quoted version, and double-quoted checksum on consecutive lines in that order. The example checksum is for uv's `x86_64-unknown-linux-gnu` archive; use the correct checksum for your runner. Renovate uses the existing checksum to identify the release asset whose checksum it should update.

If your repository sets [`enabledManagers`](https://docs.renovatebot.com/configuration-options/#enabledmanagers), include `"custom.regex"` in that list. This option replaces inherited values rather than merging them, so the preset cannot add it to your repository's list.

The preset disables Renovate's built-in updater for uv version inputs in workflow files because it leaves checksums unchanged. Annotate every uv version pin that Renovate should update; unannotated uv inputs will no longer receive updates from the built-in manager. The custom manager must also be enabled. Updates to the `astral-sh/setup-uv` action itself remain enabled.

Also remove or exclude any other custom managers that update the same version pins independently.

For tools without a checksum, omit the `checksum` line and use an appropriate datasource such as `github-releases`. For tools other than uv, disable any overlapping built-in updates in your repository's configuration.
