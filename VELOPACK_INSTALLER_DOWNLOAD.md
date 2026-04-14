# Velopack installer download (Promethos mirror)

Large **Ember Home Setup** builds (**`com.promethos.ember.setup-win-Setup.exe`**, portable zip, full **`.nupkg`**) do not fit GitHub’s per-file size limit. They are published on the **rsync.net** mirror linked from this org’s releases and README.

## HTTPS base URL

**https://zh5605.rsync.net/promethos/**

Open the **`/<semver>/`** folder for the release you want (example: **`https://zh5605.rsync.net/promethos/1.0.17/`**), then download **`com.promethos.ember.setup-win-Setup.exe`** (or the other artifacts listed there).

## Login and password (HTTP access)

Your **rsync.net** account may require **HTTP Basic Authentication** when you open that URL in a browser or use **`curl`**, **`wget`**, or **`Invoke-WebRequest`**.

1. **Username** — your **rsync.net account short name** (the hostname prefix, e.g. **`zh5605`** for **`zh5605.rsync.net`**), unless your provider’s docs say otherwise.
2. **Password** — the **HTTP access password** configured in the **rsync.net control panel** for Web/HTTPS access (this is **not** necessarily your SSH passphrase; check [rsync.net documentation](https://www.rsync.net/resources/howto/http.html) and your account settings).

If the browser prompts for **Name** and **Password**, enter those two values. For command-line downloads:

```bash
curl -fL -u 'YOUR_USERNAME:YOUR_PASSWORD' -O 'https://zh5605.rsync.net/promethos/1.0.17/com.promethos.ember.setup-win-Setup.exe'
```

```powershell
$cred = Get-Credential   # enter rsync.net HTTP user + password
Invoke-WebRequest -Uri 'https://zh5605.rsync.net/promethos/1.0.17/com.promethos.ember.setup-win-Setup.exe' -OutFile 'com.promethos.ember.setup-win-Setup.exe' -Credential $cred
```

### Security

**Do not commit real usernames or passwords into any public Git repository.** Share credentials only through a **team password manager**, **private** wiki, or **out-of-band** channel. Replace **`YOUR_USERNAME`** / **`YOUR_PASSWORD`** in snippets above with values from your vault when you run them locally.

## Mapping drive `Z:` to the Promethos folder (Windows)

The HTTPS URL **`https://zh5605.rsync.net/promethos/`** is for **browsers and HTTP clients** (`curl`, `Invoke-WebRequest`). Windows **does not** treat that URL like a file share: **`net use Z: https://zh5605.rsync.net/promethos`** is **not** supported the same way as `\\server\share`, unless rsync.net has enabled **WebDAV** for your account (uncommon for simple static HTTPS).

To get a **drive letter** (e.g. **`Z:`**) that shows the same tree as **`/promethos`**, use **SFTP** to **`zh5605.rsync.net`** (rsync.net’s native protocol for Explorer-style access), not the `https://` URL string.

### Option A — Mountain Duck (recommended by rsync.net)

rsync.net [documents](https://www.rsync.net/resources/howto/windows_map.html) that **Mountain Duck** is the reliable way to mount an rsync.net account as a drive on **Windows 10/11**.

1. Install [Mountain Duck](https://mountainduck.io/).
2. Add a new bookmark / connection:
   - **Protocol:** **SFTP** (SSH).
   - **Server:** **`zh5605.rsync.net`**
   - **Username / password:** your **SSH/SFTP** login for that account (from the rsync.net control panel), or use an **SSH key** if you use key auth.
   - **Path / remote folder:** **`promethos`** (so you land in the same folder as **`https://zh5605.rsync.net/promethos/`**).
3. Choose **drive letter `Z:`** (or any free letter) and connect.

You should then see **`Z:\`** (or the letter you picked) containing **`1.0.17`**, **`README.md`**, etc., matching the HTTPS layout under **`/promethos`**.

### Option B — SSHFS-Win (free, advanced)

You can mount SFTP with **WinFsp** + **SSHFS-Win** (see community guides). Typical pattern is a **`\\sshfs\...`** UNC that maps to a drive letter; exact syntax depends on your SSHFS-Win version. Prefer Mountain Duck if you want a supported, click-through workflow.

### Option C — No drive letter

Use **WinSCP**, **FileZilla** (SFTP), or **browser/curl** to **`https://zh5605.rsync.net/promethos/...`** with HTTP credentials—no **`Z:`** mapping required.

## GitHub Releases vs this mirror

**GitHub Releases** on **`ember-home-setup`** may only attach **small** Velopack files (for example delta **`.nupkg`** and channel metadata). The **full Windows installer** is obtained from the mirror URL above unless your release notes state otherwise.

## x64 app vs Velopack runtime

The Ember Home Setup shell is built with **electron-builder** for **Windows x64 only** (not x86): **`ember/windows-installer/electron-setup/package.json`** sets **`build.win.defaultArch`** to **`x64`** and the **`package:win`** script passes **`--x64`**. The **`--packDir`** for **`vpk pack`** is that build’s **win-unpacked** tree: **`pack-out/win-unpacked`** after a plain **`npm run package:win`**, or **`pack-out-<timestamp>/win-unpacked`** when **`build-velopack-installer.ps1`** runs **`npm run package:win:unique`** (path echoed in **`last-pack-win-unpacked.txt`** under **`electron-setup`**).

Velopack is invoked with **`--runtime win10-x64`**, which encodes “Windows 10 or newer, x64,” including **Windows 11** on x64 hardware—aligned with that x64 electron output (analogous to a **`publish/win-x64`** layout for .NET).

**Distribute and run the generated `…-Setup.exe`** (and related Velopack channel files as needed). **Do not** treat the loose EXE under **`…-full.nupkg\lib\app\`** as the normal install path.

## “This app can’t run on your PC” (Windows 11)

1. **Run the Velopack installer, not the `.nupkg` contents.**  
   Use **`com.promethos.ember.setup-win-Setup.exe`**. Do **not** open **`…-full.nupkg`** in Explorer and run **`lib\app\*.exe`** — that tree is the **update package** for Velopack, not the supported first-run path; the main EXE there can trigger this error if run out of context or after a bad extract.

2. **CPU architecture.** This build is **Windows x64 (AMD64)**. On **Windows 11 ARM** (Snapdragon, etc.), x64 apps usually run under emulation, but if you still see this message, try another x64 PC or ask the publisher for an **arm64** Electron build.

3. **Corrupt or incomplete file.** Re-download **`…-Setup.exe`**, compare file size to the publisher, and unblock: **Properties → General → Unblock** if present.

4. **Check the binary (optional):** In PowerShell:  
   `(Get-Item '.\com.promethos.ember.setup-win-Setup.exe').VersionInfo | Format-List`  
   If the file won’t even show version info, the download is likely truncated.

