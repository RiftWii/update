# RiftWii update file

This repository serves **https://update.riftwii.com/latest.json**, the file RiftWii reads to find its newest release. RiftWii asks here first and falls back to GitHub's API when this address cannot be reached.

The address belongs to the project, not to a git host. If RiftWii's releases ever move away from GitHub, only the contents of this file (and where it is published) change; installed copies of RiftWii keep looking in the same place.

## The file

`latest.json` uses a subset of GitHub's release answer, so RiftWii reads it with the same code as GitHub's:

- `tag_name`: the release, for example `v2611-170`.
- `assets`: one entry per release file, each with `name`, `browser_download_url`, `size` and `digest` (`sha256:` and 64 hex characters).
- `schema`, `source` and `published_at` are for people reading the file; RiftWii ignores them.

RiftWii checks every download against its `digest` and refuses a file without one.

## How it is updated

- **On every release:** the release workflow in [KakarottoCake/Riftwii](https://github.com/KakarottoCake/Riftwii) writes `latest.json` from the published release and pushes it here, using a deploy key that can write to this repository only.
- **By hand, if a push ever fails:** Actions > Rebuild latest.json > Run workflow reads the newest release from GitHub and writes the same file.

Both use the same transform, and both refuse a release that has a file without a SHA-256.
