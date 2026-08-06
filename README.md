# zuma-kernel

Kernel source mirror for the **Pixel Zuma (Tensor G3)** family — Pixel 8, 8 Pro, and 8a.

This repository is a **mirror**, not a fork with local development. The kernel
source itself lives on the `android14-6.1-2025-12` branch and tracks upstream
Google automatically.

## Branches

| Branch                  | Contents                                                          | How it's updated |
|-------------------------|-------------------------------------------------------------------|------------------|
| `main`                  | CI workflows + this README (no kernel source)                     | manual           |
| `android14-6.1-2025-12` | The kernel source, mirrored from AOSP common                      | automated daily  |
| `wild`                  | Alias of `android14-6.1-2025-12` (kept for compatibility)         | manual           |

## How the sync works

The GitHub Actions workflow `.github/workflows/sync-upstream.yml` runs
**daily at midnight UTC** (and can be triggered manually via
**Actions → Sync android14-6.1-2025-12 with upstream → Run workflow**).

What it does:

1. Fetches the tip of `android14-6.1-2025-12` from this repo and the tip of
   the same branch from
   [`android.googlesource.com/kernel/common`](https://android.googlesource.com/kernel/common/+/refs/heads/android14-6.1-2025-12)
   — shallow, blob-free fetches so it stays fast.
2. Compares the two commits.
3. If upstream is ahead, it **fast-forwards** `android14-6.1-2025-12` and
   pushes. GitHub rejects any non-fast-forward push, so a diverged branch can
   never be overwritten.
4. Writes a summary of the applied commits into the workflow run page.

If the mirror has commits upstream doesn't (e.g. someone pushed locally), the
workflow **skips the push** and reports a divergence warning instead.

## Using the kernel

The [Zuma_KernelSU_SUSFS](https://github.com/TheWildJames/Zuma_KernelSU_SUSFS)
repo builds kernels from this mirror's `android14-6.1-2025-12` branch, adding
KernelSU and SUSFS support.
