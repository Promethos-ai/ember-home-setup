# Ember Home Setup (installers)

This repository holds **documentation only**. The installable **Windows application** is published under **GitHub Releases** (not in the `main` branch tree).

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

## Related documentation

In the main workspace: **`ember/docs/RUNNING.md`**, **`ember/docs/CONFIGURATION.md`**, **`ember/windows-installer/README.txt`**.
