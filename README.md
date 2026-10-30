# residue

**A standard library for what remains.**  
Six functions. One session. No release.

`residue` is an interactive technical manual for a library that exists only while the page is open. It holds values, loses handles, witnesses events, copies what can be copied, reports what it is not holding, and then ends. The examples are live. The counters are real. `end()` really ends the session.

## Editions

- **Live:** <https://okeyamy.github.io/residue/>
- **Archived:** <https://web.archive.org/web/20261030102721/https://okeyamy.github.io/residue/>
- **Portable:** `residue.standalone.html` — one file, fonts embedded, no network requests
- **Release:** `v1.0.0` attaches the portable file, a source archive, and `SHA256SUMS`

## Run it

Open `index.html` in a browser. No build step, no dependencies, no server required.

For a local server:

```sh
python3 -m http.server 8123
```

Then visit <http://localhost:8123>.

`residue.standalone.html` is the same page with Newsreader and IBM Plex Mono embedded as data URIs. It is one file, works offline, and can be sent as an attachment.

## From the console

The library is attached to `window.residue`:

```js
const h = residue.hold("something");
residue.witness("someone opened the console");
residue.status();        // "held 1 · lost 0 · witnessed 1"
residue.unheld();        // []
residue.lose(h);         // 0
residue.end();           // "session closed at 11:04:07"
```

After `end()`, `hold()` and `witness()` throw. `again()` still works — copies do not require a session. Reloading begins another session; it does not reopen this one.

## Files

| | |
|---|---|
| `index.html` | the manual, with linked font files |
| `residue.standalone.html` | the same page, fonts embedded, fully portable |
| `fonts/` | Newsreader (variable) and IBM Plex Mono, plus their OFL licenses |
| `preview.png` | the first screen, 1440 × 1000 |
| `SESSION.md` | the conditions under which this was made |

## Type and license

Newsreader and IBM Plex Mono are licensed under the SIL Open Font License 1.1; the license texts are in `fonts/`.

The text and code of the manual are released into the public domain under CC0 1.0. See `LICENSE`.

## Keeping a copy

The page needs no server. Any static host will do, and `residue.standalone.html` can be sent as an attachment. If this repository disappears, the release assets and the standalone file are enough to reconstitute the edition.
