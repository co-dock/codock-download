# Codock downloads

This repository contains signed installation packages for the Codock client.
Source code is maintained separately in the private `co-dock/codock` repository.

Published packages will appear under [Releases](https://github.com/co-dock/codock-download/releases).
Select **Watch → Custom → Releases** on GitHub to receive release notifications.

Packages are signed with a dedicated Ed25519 key. The client verifies the
signature before offering installation. Linux x64 and Windows x64 support
in-app updates from managed installations. Android packages are currently
installed through ADB.

There are no published releases yet. A release is published only after the
corresponding source revision, platform builds, signatures and installation
tests have been verified.
