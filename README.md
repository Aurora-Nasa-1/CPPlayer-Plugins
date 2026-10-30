# CPPlayer Plugins

Static plugin catalog for [CPPlayer-KMP](https://github.com/Aurora-Nasa-1/CPPlayer-KMP).
No store server, account system, tracking SDK or automatic installation.

Catalog URL:

```text
https://raw.githubusercontent.com/Aurora-Nasa-1/CPPlayer-Plugins/main/index.json
```

In a CPPlayer build that includes Plugin center, open Settings > Plugin center >
Repositories, enable repository networking, then check for updates. Add the URL
above if it is not already listed. Browse packages and confirm an installation.
Native provider updates take effect after restarting the app.

## Trust

This catalog links to existing publisher release assets. It does not mirror or
modify plugin binaries. Listing a plugin is not a security audit or endorsement.
Native plugins run with the application's privileges. SHA-256 checks that the
download matches the catalog; a compromised publisher/catalog can change both.
Only install code from publishers you trust.

The initial entry is the existing `netease-cloudmusic-rs` 1.1.1 release, with
Windows x64, Linux x64 and Android arm64/armv7/x64 packages. No macOS package is
published in that release. Artifact metadata and manifests were checked without
running native code. Runtime/API availability remains the publisher's responsibility.

## Publishing

1. Publish a single-plugin ZIP with a root `manifest.json`. Use a fixed release
   URL, never a mutable `latest/download` URL.
2. Use the existing [Provider manifest](https://github.com/Aurora-Nasa-1/CPPlayer-KMP/blob/main/docs/dev/MANIFEST_REFERENCE.md)
   or [Lyrico manifest](https://github.com/Aurora-Nasa-1/CPPlayer-KMP/blob/main/docs/dev/LYRICS_PLUGIN_DEV_GUIDE.md).
   Catalog `version`, API fields, ID and runtime must match the ZIP.
3. Add/update `index.json`. Provider versions use strict SemVer; lyric plugins
   also need a positive, increasing `versionCode`.
4. Set the precise ZIP byte count and lowercase SHA-256. A native artifact must
   declare platform/architecture and contain its exact entry filename at the root
   or `lib/<abi>/`. Do not include tokens, cookies, credentials or personal data.
5. Run `python scripts/validate_catalog.py --verify-packages`. The validator
   downloads but never executes or extracts plugin code. CI checks metadata on
   pull requests, and metadata plus packages on pushes/manual runs.
6. Submit a reviewed change. Do not replace an already-published version's bytes.
   Remove a bad entry instead of pretending to remotely uninstall existing users' code.

Full protocol and networking rules: [Plugin repository v1](PROTOCOL.md).

## Format

`schemaVersion: 1`, a display `name`, and a `plugins` array.
Each plugin has `id`, `kind` (`provider` / `lyrics`), `name`, `version`,
optional description/author/changelog/homepage, API fields and an `artifacts` array.
Each artifact has `platform`, `arch`, `runtime`, `url`, `sha256`, `bytes`.
See the live `index.json` and the validator for an executable format reference.

The app defaults to no repository networking and no automatic update checking.
Enabled automatic checking only fetches catalogs at most once per day while
running. It never silently downloads or installs plugins.

## License

Catalog metadata, documentation and validation tooling use Apache-2.0.
Referenced plugins keep their own upstream licenses.
