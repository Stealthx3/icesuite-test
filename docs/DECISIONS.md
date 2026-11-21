# Decision and action log

Newest first. **User** = Stealthx3's call; **Claude** = proposed by Claude, open to change.

## 2026-11-21: IceTube alpha and beta builds (Jellyfin plugin format)
- **User:** approved publishing IceTube alpha and beta builds through this repository.
- **Claude:** a fifth format, `jellyfin`, built like the Windows format: plugin zip and helper bundles as GitHub
  pre-releases tagged `<slug>-v<version>`, newest 3 per app and channel (older ones deleted with their tags).
- **Claude:** one manifest per channel (`jellyfin/alpha/manifest.json`, `jellyfin/beta/manifest.json`) for all
  IceSuite Jellyfin plugins, so users add each repository once. Manifests are plain JSON and unsigned.
- **Claude:** four-part plugin versions: alpha N → `X.Y.Z.N`, beta N → `X.Y.Z.(100+N)`, so betas outrank alphas of
  the same version. The tag's pre-release must match the channel.
- **Claude:** the plugin's GUID is fixed in the catalog and never changes; the plugin image is copied to
  `jellyfin/images/<slug>.png`.
- **Claude:** the app repository's CI publishes with a fine-grained token limited to this repository; no signing key.
- **Claude:** each pre-release carries a source tarball, because the IceTube source repository is private for now.

## 2026-11-13: one test repository for all of Icesuite
- **User:** convert `icefin-flatpak-test` into an Icesuite-wide test repository for every Icesuite code project,
  handling Flatpaks, Windows installers, `.deb` packages and Android `.apk` packages.
- **Claude:** name it `icesuite-test` (repository, Pages path, Flatpak remote, APT list file).
- **Claude:** one shared repository per platform rather than one per app: one Flatpak remote (`icesuite-test`), one
  APT source, one F-Droid repo. Users add each once; a new app needs only a catalog file.
- **Claude:** site on `gh-pages`, replaced by one parentless commit per publish (force-push with a lease), so build
  objects never pile up in git history. `main` holds the tooling.
- **Claude:** Windows installers as GitHub pre-releases on this repository, not on Pages: installers for
  Qt WebEngine apps can exceed Pages' 100 MB file limit, and release assets don't count toward the 1 GB site.
  Pages carries a JSON feed per app.
- **Claude:** `.deb` and `.apk` stay on Pages, because APT and F-Droid clients fetch packages relative to the
  repository URL. Retention: 2 versions per package and architecture (APT), 3 per app (F-Droid), 3 releases
  (Windows), Flatpak prune depth 3.
- **Claude:** keep the existing OpenPGP key (Flatpak and now APT), so installed remotes keep verifying. F-Droid gets
  its own keystore because F-Droid signs with JAR signing, not OpenPGP.
- **Claude:** move the OSTree repo from `/repo/` to `/flatpak/repo/`. The rename changes the site URL anyway, so
  existing installs need one `flatpak remote-modify` either way.
- **Action (Claude):** wrote `tools/icesuite.py`, the catalog (Icefin Player: Flatpak, `.deb`), docs and an
  end-to-end test; built `gh-pages` from the existing Icefin repo; opened the conversion pull request.
- **Action (User, pending):** rename the repository, switch Pages to `gh-pages`, merge, run `remote-modify` on
  machines with the old remote ([MIGRATION.md](MIGRATION.md)).

## 2026-11-12: every build on main is published (Icefin Player)
- **User:** publish every build that lands on Icefin Player's `main`, not only alpha releases.

## 2026-11-11: Flatpak test repository (Icefin Player)
- **User:** a signed Flatpak remote for Icefin Player test builds on GitHub Pages; new ed25519 key held only in the
  lab guest.
