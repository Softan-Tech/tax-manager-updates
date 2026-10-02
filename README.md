# TaxManager Updates

Public Windows installers and update artifacts for TaxManager. Application source is maintained
separately in a private repository. This repository does not contain application source or client data.

## Downloads and Feeds

Use the [stable release](https://github.com/Softan-Tech/tax-manager-updates/releases/tag/stable)
to download the desktop installer or the web-services setup wizard.

- Desktop update feed: `https://github.com/Softan-Tech/tax-manager-updates/releases/download/stable`
- Signed web manifest: `https://github.com/Softan-Tech/tax-manager-updates/releases/download/stable/latest-web.json`
- Desktop metadata: `latest.yml`; associated versioned `.exe` and `.blockmap` files.
- Web metadata: `latest-web.json`; associated versioned update ZIP and `TaxManager-Web-VERSION-Setup.exe`.

Clients do not need GitHub credentials. Initial channel setup contains no binaries: downloads and
automatic updates become available only after a tested release is published. Never use placeholder
update metadata.

## Maintainer Publishing Rules

1. Build and test on Windows. Increment the application version for each update.
2. Sign the web manifest with the Ed25519 private key held outside this repository. Clients must
   already have its trusted public key. OTA manifest signing is separate from Windows Authenticode signing.
3. Upload versioned binaries first. Never replace existing versioned assets with different builds.
4. Upload the matching latest metadata last (`--clobber` is permitted only for channel metadata).
5. Keep desktop and web assets together on `stable`; publishing one must not delete the other.
6. Publish only backward-compatible migrations through OTA. Retain recovery binaries and backups.

Example: `gh release upload stable VERSIONED_FILE --repo Softan-Tech/tax-manager-updates`.
After verifying uploads, replace `latest.yml` or `latest-web.json` on that release.

Do not upload source archives, private keys, signing certificates, tokens, `.env` files, runtime
configuration, client ecosystem files, logs or databases. Inspect every release asset before publishing.

## Security

The web updater verifies signed metadata and ZIP checksums. Do not bypass those checks or trust
a replacement public key delivered with an untrusted update. Windows may show SmartScreen warnings
for installers without trusted Authenticode signatures.
