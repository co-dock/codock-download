# Codock downloads

This repository contains signed installation packages for Codock clients and the
standalone Codock Server. Source code is maintained separately in the private
`co-dock/codock` repository.

Download packages from [the latest release](https://github.com/co-dock/codock-download/releases/latest).
Select **Watch → Custom → Releases** on GitHub to receive release notifications.

| Platform | Package |
| --- | --- |
| Linux x64 client | `codock-VERSION-linux-amd64.deb` |
| Windows x64 client | `codock-VERSION-windows-x64-setup.exe` to install; `.zip` for in-app updates |
| Android ARM64 client | `codock-VERSION-android-arm64.apk` |
| macOS Apple Silicon beta (M1/M2 and newer) | `codock-VERSION-macos-arm64-beta.dmg` |
| macOS Intel beta | `codock-VERSION-macos-x86_64-beta.dmg` |
| Linux x64 server | `codock-VERSION-server-linux-amd64.tar.gz` |
| Linux ARM64 server | `codock-VERSION-server-linux-arm64.tar.gz` |

The server archives contain `codockd` and a complete `INSTALL.md` with systemd,
HTTPS, connection checks and backup instructions. Flutter and GTK are not
required on the server. Client updates do not automatically update a separate
server; wait for active work to finish and back up SQLite before upgrading it.

Linux x64 and Windows x64 support in-app updates from managed installations.
Android packages are currently installed through ADB. The macOS beta supports
server connections only and is updated manually. Its ad-hoc signature is not
Apple Developer ID signing or notarization: if macOS blocks a trusted beta,
allow this specific Codock app through **System Settings → Privacy & Security
→ Open Anyway**. Use the DMG matching your Mac's processor.

## Install Codock Server

For a new Ubuntu 24.04+ / Debian 12+ server with systemd (x64 or ARM64):

```sh
curl -fsSL https://github.com/co-dock/codock-download/releases/download/server-installer-v0.1.0/install.sh | sudo bash
```

The versioned bootstrap verifies the manager's signature. The manager verifies
and installs the latest stable server, creates an unprivileged service account,
sets up systemd and checks the authenticated API on `127.0.0.1:8765`.
Reverse proxy, TLS, DNS and firewall configuration are separate.

Use `sudo codock-server status`, `check`, `logs`, `backup`, `update` or `rollback`
after installation. `sudo codock-server token` displays the connection token on
an interactive console. Data and tokens are preserved; active work blocks an
update. Rollback restores the binary, not database migrations. Existing manual
installations are not taken over automatically.

See the [installer release and guide](https://github.com/co-dock/codock-download/releases/tag/server-installer-v0.1.0)
for parameters, offline installation and verification details.

## Verify a download

Packages are signed with the dedicated Ed25519 key embedded in the clients.
The public key is also available in [release-public-key.pem](release-public-key.pem).
The private key is never distributed. Download your package, its matching
`.sig`, `SHA256SUMS` and `release-public-key.pem` from the release.

```sh
sha256sum --check --ignore-missing SHA256SUMS
base64 --decode YOUR_PACKAGE.sig > package.signature
openssl pkeyutl -verify -pubin -inkey release-public-key.pem -rawin \
  -in YOUR_PACKAGE -sigfile package.signature
```

These commands use Linux `sha256sum` and `base64`; on macOS use
`shasum -a 256 -c SHA256SUMS` (missing, undownloaded files are reported) and
`base64 -D -i YOUR_PACKAGE.sig -o package.signature`, with an OpenSSL version
supporting Ed25519. The client verifies release signatures before offering
installation. Release signatures do not replace Windows Authenticode or Apple
code signing.

Each release lists its source revision, verified platforms and known
installation limitations. Read its notes before installing.
