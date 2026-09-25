# AI Zeny updates

This public repository distributes compiled **AI Zeny** Windows updates. The
application's source code and development history are kept in a separate
private repository and are not published here.

## What is here

| Path | Content |
|---|---|
| `channels/stable.json` | The signed stable channel: the newest AI Zeny version, its packages, sizes and SHA-256 hashes (schema 2). |
| `channels/stable.sig` | The detached ECDSA P-256 signature over the exact bytes of `stable.json`. |
| `schemas/` | The JSON schema of the channel and release manifests. |
| GitHub Releases `v<version>` | Immutable release assets for each version (below). |

Each release `v<version>` carries:

| Asset | Content |
|---|---|
| `AIZeny-<version>-win-x64-full.zip` | Every file of the version, with `AIZeny.Setup.exe`, `manifest.json` (file inventory: path, size, SHA-256) and `SHA256SUMS.txt`. It is also the manual installer. |
| `AIZeny-<base>-to-<version>-win-x64-delta.zip` | Only the files that changed since `<base>`, with setup, the full inventory and `delta.json`. Installed AI Zeny rebuilds the version from it and checks every file; if anything differs it uses the full package. |
| `AIZeny-<version>-manifest.json` / `.sig` | The release-specific signed manifest (identical to the channel file when it was published). |
| `AIZeny-<version>-release-notes.md` | What changed. |
| `AIZeny-<version>-SHA256SUMS.txt` | SHA-256 of every asset of the release. |

## How installed AI Zeny uses it

The **AI Zeny Updater** Windows service checks `channels/stable.json` when it
starts and every three hours, verifies the signature with the public key built
into AI Zeny, refuses older, replayed or foreign manifests, downloads the delta
(or the full package), verifies every hash, and installs the update itself.

AI Zeny executables and installers are **not Authenticode-signed**, so Windows
may show **Unknown publisher**. Update authorization comes from the separately
signed manifest; hashes in this repository detect corruption but do not on their
own protect against a compromised repository.

Do not post secrets, credentials or customer data in a public issue. See
[SECURITY.md](SECURITY.md).
