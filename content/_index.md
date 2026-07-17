---
title: isnow.js
---

<!-- markdownlint-disable-file MD014 -- shell examples intentionally use a `$` prompt without echoing output -->

**isnow** is a date/time _pattern_ language — formally _DTimpalr, a Date/Time Pattern Language for Repetition_. An **isnow** describes anything from a fixed instant to a complex recurrence and answers one question: **does it hold now?**

`@tsvsheet/isnow` is the JavaScript implementation, sharing its semantics exactly with [isnow.go](https://tsvsheet.github.io/docs.isnow.go/) — both pass the same [conformance corpus](https://github.com/tsvsheet/isnow/tree/main/conformance). Use it in the browser or in Node.

## Install

```console
$ npm install @tsvsheet/isnow
```

## Use

```js
import { parse, is } from "@tsvsheet/isnow";

const p = parse("M,W,F noon");
p.holds(new Date()); // does it hold now?
p.canonical; // "*/*/* Monday,Wednesday,Friday 12:00:00"
p.explain(); // "holds at 12:00:00 on Monday,Wednesday,Friday"
p.next(new Date()); // the next occurrence, as a Date (or null)

is("6", "2026-07-14T06:00:00Z"); // one-shot: true
```

## API

| Member | Description |
| --- | --- |
| `parse(src, { timeZone })` | Parse an isnow into a `Pattern`. Throws `IsnowError` (with `.code` ∈ `syntax`, `symbol`, `range`, `context`) on invalid input. `timeZone` is an IANA name (default UTC) used to evaluate `Date` inputs and offsetless strings. |
| `pattern.holds(at)` | Whether the isnow holds at `at` (a `Date` or ISO 8601 string). |
| `pattern.next(from)` / `prev(from)` | The next/previous occurrence as a `Date`, or `null`. |
| `pattern.canonical` | The canonical `Y/m/d w H:M:S` form (a getter). |
| `pattern.explain()` | A generated English description. |
| `is(src, at)` | Parse-and-test convenience. |

Offset-bearing RFC 3339 strings evaluate by their own offset — `Intl` is not consulted — so results are deterministic and match the Go implementation byte for byte. Weekday numbering is Sunday = 1.

## Learn the language

The full language tour, the field algebra, the shorthand ladder, and cron migration are in the [isnow.go documentation](https://tsvsheet.github.io/docs.isnow.go/) (the language is the same). The grammar and specification live in [tsvsheet/isnow](https://github.com/tsvsheet/isnow).

## Try it

The interactive **[playground](https://github.com/tsvsheet/isnow.js/tree/main/playground)** — a live "is it now?" matcher and an isnow builder — runs entirely in the browser on this library.
