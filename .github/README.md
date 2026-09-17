# ckan-docker-base (Manaaki Whenua fork)

This is Manaaki Whenua - Landcare Research's fork of [ckan/ckan-docker-base](https://github.com/ckan/ckan-docker-base), the official CKAN Docker images. We build our CKAN base images from it, so a fix we contribute upstream reaches our images before upstream releases it.

For the images themselves, see upstream's [README](../README.md).

## Branches

| Branch | What it is |
|---|---|
| `downstream` (default) | The branch we build from: an upstream release plus the carried patches below. Protected; changes arrive by pull request. |
| `main` | A mirror of `ckan/ckan-docker-base` `main`. Never commit to it. |
| `fix/*`, `feat/*`, `docs/*` | Cut from `main`, for pull requests to upstream. |
| `build/*` | Cut from `downstream`, for changes that only make sense for us. |

- **Mirror:** `main` from `ckan/ckan-docker-base`
- **Adopted upstream release:** `v20260826.1` (CKAN 2.12.0, 2.11.6, 2.10.11, 2.9.11)

Anything that builds from this fork pins a commit on `downstream`, never a branch name.

## Carried patches

Our own fixes, merged into `downstream` while their upstream pull request is open. Remove a row once the adopted release contains the fix.

| Branch | Upstream pull request | What it does |
|---|---|---|
| `fix/prerun-plugins-before-db-init` | [ckan/ckan-docker-base#140](https://github.com/ckan/ckan-docker-base/pull/140) | `prerun.py` writes `CKAN__PLUGINS` to `ckan.ini` before `ckan db init`, so CKAN 2.11+ migrates every enabled plugin; README section on what happens at start-up |

## Keeping up with upstream

Upstream publishes a dated release (`vYYYYMMDD`, `vYYYYMMDD.N`) when it rebuilds its images, including for security updates. When a new one appears:

```shell
git fetch upstream --tags
git remote set-head upstream --auto
git switch main && git merge --ff-only upstream/HEAD && git push origin main
git switch -c build/adopt-<release> downstream && git merge <release>
```

Resolve any conflicts, update the adopted release and the carried-patches table above, and open a pull request into `downstream`. Then bump the pinned commit wherever this fork is built.

## GitHub Actions

Actions are disabled on this fork. Upstream's workflows publish images to Docker Hub and are not meant to run here.
