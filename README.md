# MoH Reborn

**An unofficial community patch for *Medal of Honor: Allied Assault* and its *Spearhead* and *Breakthrough* expansions — for players and server admins, on Windows and Linux.**

Reborn modernizes and fixes the classic Medal of Honor game and dedicated server. It loads alongside the
original game, **never modifies your install**, and requires an original, legally purchased copy.

> 📦 This repository hosts the **public, signed release downloads** and installers.

🌐 [mohreborn.com](https://www.mohreborn.com)

---

## Requirements

- An original, legally purchased copy of *Medal of Honor: Allied Assault* (and/or *Spearhead* / *Breakthrough*).
- Windows or Linux (x86_64).

## Install

Every download is **signed**; the installer verifies the signature before touching anything (see
[Verifying downloads](#verifying-downloads)).

### Linux — one-liner

Downloads the latest signed release, verifies it, and lays down a runnable install. Run it inside your
game folder for a portable install, or point it at the game with `--game-dir`:

```sh
curl -fsSL https://github.com/mohreborn/releases/releases/latest/download/install.sh | sh
```

To pass options (or keep the installer around), download it first:

```sh
curl -fsSL https://github.com/mohreborn/releases/releases/latest/download/install.sh -o install.sh
sh install.sh --help
sh install.sh --profile full --game-dir /path/to/medal-of-honor
```

- **Profiles:** `server` (dedicated) · `client` · `full` (default: `server`).
- **Editions** are auto-detected from your game folder (or force one with `--edition aa|sh|bt`).
- **A specific release:** `--version v2026.1-GA` (or `--channel EA` for early-access builds).

**Removal** (script / archive installs) — there's no uninstaller; just delete Reborn's own files from
the install directory (the game is left untouched):

```sh
rm -rf reborn/ mohrb_*
```

## Editions

| Edition | Notes |
| --- | --- |
| Allied Assault (`aa`) | native on Windows & Linux |
| Spearhead (`sh`) | native on Windows & Linux |
| Breakthrough (`bt`) | native on Windows; on Linux runs as the Windows build under Wine |

## Verifying downloads

Every archive is published with a detached `.sig` — an **Ed25519** signature over the file's SHA-256
digest. Installers verify this automatically before extracting anything; you can also check by hand.

Trusted public key (Ed25519, base64):

```
IwLHpDnkEfAYqQFm4H4PN4c/LZFmeqUpSCGfYysFPLE=
```

<details>
<summary>Verify manually with <code>openssl</code></summary>

```sh
# Reconstruct the PEM public key from the base64 above:
{ printf '\060\052\060\005\006\003\053\145\160\003\041\000'; \
  printf '%s' 'IwLHpDnkEfAYqQFm4H4PN4c/LZFmeqUpSCGfYysFPLE=' | base64 -d; } \
  | base64 | { echo '-----BEGIN PUBLIC KEY-----'; cat; echo '-----END PUBLIC KEY-----'; } > reborn_pub.pem

# Verify an archive against its .sig:
openssl dgst -sha256 -binary <archive> > digest.bin
openssl pkeyutl -verify -pubin -inkey reborn_pub.pem -rawin -in digest.bin -sigfile <archive>.sig
```

</details>

## What's in a release

| Asset | What it is |
| --- | --- |
| `mohreborn-<version>-<edition>-<os>-<mode>.tar.gz` / `.zip` | signed archive (Linux `tar.gz`, Windows `zip`) |
| `…​.sig` | detached Ed25519 signature for each archive |
| `mohreborn-<version>-<edition>-<os>-full.…` | Full bundle (client + server in one folder) |
| `manifest.json` | machine-readable index of the release |
| `install.sh` | the Linux installer, with this release's version baked in |

## Legal

MoH Reborn requires an original, legally purchased copy of the game. The patch is developed
independently and does not modify the original game binaries or assets. This is an unofficial,
community-driven, non-commercial project, not affiliated in any way with the trademark and copyright
holders.

*Medal of Honor* is a trademark of Electronic Arts Inc. All trademarks are the property of their
respective owners.
