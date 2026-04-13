# Velopack installer download (Promethos mirror)

Large **Ember Home Setup** builds (**`com.promethos.ember.setup-win-Setup.exe`**, portable zip, full **`.nupkg`**) do not fit GitHub’s per-file size limit. They are published on the **rsync.net** mirror linked from this org’s releases and README.

## HTTPS base URL

**https://zh5605.rsync.net/promethos/**

Open the **`/<semver>/`** folder for the release you want (example: **`https://zh5605.rsync.net/promethos/1.0.15/`**), then download **`com.promethos.ember.setup-win-Setup.exe`** (or the other artifacts listed there).

## Login and password (HTTP access)

Your **rsync.net** account may require **HTTP Basic Authentication** when you open that URL in a browser or use **`curl`**, **`wget`**, or **`Invoke-WebRequest`**.

1. **Username** — your **rsync.net account short name** (the hostname prefix, e.g. **`zh5605`** for **`zh5605.rsync.net`**), unless your provider’s docs say otherwise.
2. **Password** — the **HTTP access password** configured in the **rsync.net control panel** for Web/HTTPS access (this is **not** necessarily your SSH passphrase; check [rsync.net documentation](https://www.rsync.net/resources/howto/http.html) and your account settings).

If the browser prompts for **Name** and **Password**, enter those two values. For command-line downloads:

```bash
curl -fL -u 'YOUR_USERNAME:YOUR_PASSWORD' -O 'https://zh5605.rsync.net/promethos/1.0.15/com.promethos.ember.setup-win-Setup.exe'
```

```powershell
$cred = Get-Credential   # enter rsync.net HTTP user + password
Invoke-WebRequest -Uri 'https://zh5605.rsync.net/promethos/1.0.15/com.promethos.ember.setup-win-Setup.exe' -OutFile 'com.promethos.ember.setup-win-Setup.exe' -Credential $cred
```

### Security

**Do not commit real usernames or passwords into any public Git repository.** Share credentials only through a **team password manager**, **private** wiki, or **out-of-band** channel. Replace **`YOUR_USERNAME`** / **`YOUR_PASSWORD`** in snippets above with values from your vault when you run them locally.

## GitHub Releases vs this mirror

**GitHub Releases** on **`ember-home-setup`** may only attach **small** Velopack files (for example delta **`.nupkg`** and channel metadata). The **full Windows installer** is obtained from the mirror URL above unless your release notes state otherwise.
