# sf-transit

San Francisco's transit stations as data: which stops make up each station, what
each platform faces and is signed as, and how stations connect, layered on top of
511's own data for the city.

This is the data behind [Muni+](https://github.com/narenh/MuniPlus). It's edited
with the Muni+ station editor, which commits here.

**Report problems in [MuniPlus issues](https://github.com/narenh/MuniPlus/issues).**
Issues on this repo are not monitored.

## Layout

| path | what | written by |
|---|---|---|
| `curation/stations.json` | every station: its name, its platforms (511 stop id, heading, signage), transfers, notes, and when it was last checked on the map | people, through the editor |
| `curation/lines.json` | only where a line differs from 511: display names, colours, the F as a streetcar, which lines stand in for which (`SF:LOWL` replaces `SF:L`) | people |
| `curation/ignored.json` | 511 stops deliberately left out, each with the reason | people |
| `snapshot/SF/` | stops, lines and stop patterns from 511's GTFS feed for the current service period | the editor's snapshot refresh, never by hand |

Nothing 511 already publishes is repeated in `curation/`: coordinates, stop names
and which lines serve a platform all come from `snapshot/`. A refresh overwrites
the snapshot in place, so the diff of a refresh commit is exactly what 511
changed.

The files are written deterministically (sorted by id, unset fields omitted), so
an edit to one field is a one-line diff. The formats are defined in
[`api/app/models`](https://github.com/narenh/MuniPlus/tree/master/api/app/models)
in the Muni+ repo.

## Ids

- **Platforms and lines** are `<511 operator code>:<511's id>`: `SF:16992`, `SF:LOWL`.
- **Stations** have our own ids (`embarcadero`, `churchMarket`). They are
  permanent, because apps store them. A station that is renamed keeps its old id
  in `formerIds`.

## Renames

| old id | new id | date | why |
|---|---|---|---|
| `mongomery` | `montgomery` | 2026-09-22 | typo |
| `bayshoreLEland` | `bayshoreLeland` | 2026-09-22 | one corner split in two by a feed typo, "&L Eland Ave" |
| `middlePointFairFax` | `middlePointFairfax` | 2026-09-22 | one corner split in two by a feed typo, "Fair Fax Ave" |

## Source

`snapshot/` is derived from [511 SF Bay open data](https://511.org/open-data/transit).
