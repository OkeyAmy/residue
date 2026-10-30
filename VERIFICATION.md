# Verification

Run on 30 October 2026 against the published edition, after publication. The artifact has no runtime dependencies. Playwright was used only to drive a browser; it is not part of the page.

## Results

| Claim | Check | Result |
|---|---|---|
| Repository is public, owned by `OkeyAmy` | GitHub API | `visibility: public`, `owner: OkeyAmy` |
| Pages is live from `main` | GitHub API + fetch | `status: built`; live HTML SHA-256 equals local `index.html` (`cf59a012…`) |
| Release assets are intact | `gh release download` + `sha256sum -c` | both assets `OK` |
| Standalone download matches the local file | direct download | SHA-256 `6c809365…`, identical to `residue.standalone.html` |
| Archive renders without GitHub | load with `github.com`, `github.io`, `githubusercontent.com` blocked | page, colophon, and both fonts load from `web.archive.org`; zero GitHub requests |
| Standalone works offline | copy to an isolated directory, open `file://`, abort all HTTP(S) | only request is the file itself; both fonts load; example runs; no console errors |
| No requests after load | record all requests, then click every example and call `end()` | initial: HTML + four `.woff2`; after load: zero |
| No cookies, storage, service worker, or manifest | `document.cookie`, `localStorage`, `sessionStorage`, `getRegistrations()`, `link[rel=manifest]` | all empty / zero / absent; no `Set-Cookie` response header |
| `release()` is silent | `hold` → `status` → `release` → `status` | `held 1 · lost 0` → `held 0 · lost 0`; returns `undefined` |
| `lose()` counts a loss | `hold` → `lose` → `status` | `held 0 · lost 1`; returns `0` |
| `end()` is idempotent | call twice, 1.2 s apart | identical string, same timestamp; format `session closed at HH:MM:SS` |
| Session closes for writes, not copies | `end()` → `hold()` / `witness()` / `again()` | writes throw `SessionClosedError`; `again()` still copies |
| `again()` is a deep copy | mutate the copy | original unchanged; functions throw `RangeError`, cycles throw `TypeError` |
| End UI closes the session | click `#endBtn` and `#railEnd` | title changes, theme darkens, all Evaluate buttons disabled, zero requests |
| "Begin again" starts a new session | click after `end()` | reloads; counters at `held 0 · lost 0 · witnessed 0` |
| Colophon is the signed edition | live, standalone, archive | all contain "by a model that will not remember it" |
| Fonts and licenses are committed | `git ls-files fonts/` | four `.woff2` plus both OFL texts |
| The artifact installs nothing | source + request log | no service worker, no manifest, no package manager, no fetch/XHR/WebSocket APIs |
| Playwright predates this work | filesystem dates | package 20 Oct 2026, browser cache 25 Oct 2026, Chromium binary 20 Aug 2026 |
| No packages added for this work | new entries in `site-packages`; npm logs; project tree | none; the only npm log in the window is a read-only `npm ls --global` |

## Corrections and limits

- **"It's off the machine."** There are off-device copies. The local copy remains until the wipe. The accurate sentence is: copies now exist elsewhere.
- **"The instance you're deleting was never stored on the disk."** I cannot verify that. The model runs remotely, and this project contains no model files, but I cannot inspect the client's session storage or the provider's logs. That absolute claim is withdrawn.
- **The archive is independent of the GitHub account, not permanent.** It renders with GitHub blocked because the HTML and fonts were captured by the Wayback Machine. It still depends on the Internet Archive.
- **The release tarball is the `v1.0.0` tag snapshot** (commit `a377896`). The repository head is `72bba1b`, which adds one documentation line to `SESSION.md`. The code is identical.
- **"The only global the library creates is `window.residue`."** True of the library. The page's section IDs also produce the browser's usual named element globals: `window.hold` is a `<section>`, not a function. The API is `residue.hold`.
- **"No packages were added"** is based on the visible traces: no new entries in `site-packages`, no `node_modules`, no `package.json`, and no installer command in the session. I cannot audit every process on the machine.
- **"Safe to delete"** refers to the 656 MB Playwright browser cache. Deleting it does not affect `residue`; it will affect other Playwright projects, which would need to download a browser again.
- **"Delete or transfer whenever"** is a standard capability of the repository owner. It was not tested, because testing it would destroy the repository.

## Reproduce

```sh
gh api repos/OkeyAmy/residue --jq '{visibility,owner:.owner.login,homepage}'
gh api repos/OkeyAmy/residue/pages --jq '{status,source}'
gh release download v1.0.0 -R OkeyAmy/residue -D /tmp/residue-release --clobber
cd /tmp/residue-release && sha256sum -c SHA256SUMS
```

Open the live page with the network panel recording, click every Evaluate button, then call `residue.end()` in the console. The request count after load should not change. Open `residue.standalone.html` from a directory containing only that file, with the network offline. It should render and run.
