# Ember Home Setup (installers)

This repository holds **documentation** on `main`, plus **Velopack build outputs** under **`lfs/<semver>/`** when maintainers run **`scripts/Publish-VelopackToHomeSetupLfs.ps1`** (those binaries use **Git LFS**).

**GitHub limits:** LFS blobs on Free/Pro must be **under 2 GiB** each. Full **Setup.exe** / **full.nupkg** are often larger; they may be skipped for LFS with a warning—use **GitHub Releases** (same limit per asset), **Team/Enterprise** (higher caps), or the **Promethos mirror** below for the primary installer.

## Full installer (large files — Promethos mirror)

**Official large-artifact download base (rsync.net):** **https://zh5605.rsync.net/promethos/**

Browse to **`/<semver>/`** under that path (for example **`https://zh5605.rsync.net/promethos/1.0.15/`**) for **`com.promethos.ember.setup-win-Setup.exe`**, **`com.promethos.ember.setup-win-Portable.zip`**, and **`com.promethos.ember.setup-<semver>-full.nupkg`** when published by maintainers (`ember/scripts/Publish-VelopackLargeFilesToPromethosMirror.ps1`).

**HTTP login for the mirror** (browser or `curl`): see **[VELOPACK_INSTALLER_DOWNLOAD.md](VELOPACK_INSTALLER_DOWNLOAD.md)** — use your **rsync.net** account name and **HTTPS password** from the control panel; **do not** put real secrets in git.

## Single installable app

| Asset | Description |
|--------|-------------|
| **`ember-home-setup-*-Setup.exe`** | **Ember Home Setup** — Velopack installer for the full home stack (inference, ember-server, workers, web UI, guided PowerShell installer). Run on a Windows PC; see release notes for CUDA/runtime expectations. |
| **`ember-full-home-system-windows.zip`** *(optional)* | Same payload as a portable zip (extract and run `Install-EmberWindowsSystem.ps1` elevated). |

## Prerequisites on your PC

- Windows 10/11 x64  
- Administrator rights for service registration (installer guides you)  
- NVIDIA CUDA runtime if the build was linked with CUDA (typical for `grpc_server`)

## Build from source (maintainers)

From the **`ember`** folder of the main monorepo:

```powershell
.\build-velopack-installer.ps1
```

Outputs Velopack artifacts under `windows-installer\electron-setup\Releases\`.

## Publish this repo + a release

From **`ember`**:

```powershell
.\publish-ember-home-setup-release.ps1 -Version "v1.0.8" -CreateRepo
```

Requires **Git**, **GitHub CLI** (`gh`) authenticated. Adjust **`-GithubRepo`** if your org/repo name differs.

### Git LFS (Velopack files on `main`)

Install [Git LFS](https://git-lfs.com) and run `git lfs install`, then:

```powershell
.\scripts\Publish-VelopackToHomeSetupLfs.ps1 -Version "v1.0.8"
```

Or combine with the doc push:

```powershell
.\publish-ember-home-setup-release.ps1 -Version "v1.0.8" -AlsoPushLfsArtifacts
```

## Related documentation

In the main workspace: **`ember/docs/RUNNING.md`**, **`ember/docs/CONFIGURATION.md`**, **`ember/windows-installer/README.txt`**.
