# Graph Report - SriLankan-Defense-Power  (2026-08-19)

## Corpus Check
- 3 files · ~65,865 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 11 nodes · 8 edges · 3 communities (0 shown, 3 thin omitted)
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `c7688349`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- [[_COMMUNITY_commanders.js|commanders.js]]
- [[_COMMUNITY_weapons.js|weapons.js]]
- [[_COMMUNITY_rebuild_data.js|rebuild_data.js]]

## God Nodes (most connected - your core abstractions)
1. `DB` - 1 edges
2. `REBUILD_DATA` - 1 edges
3. `WEAPONS` - 1 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Import Cycles
- None detected.

## Communities (3 total, 3 thin omitted)

## Knowledge Gaps
- **3 isolated node(s):** `DB`, `REBUILD_DATA`, `WEAPONS`
  These have ≤1 connection - possible missing edges or undocumented components.
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What connects `DB`, `REBUILD_DATA`, `WEAPONS` to the rest of the system?**
  _3 weakly-connected nodes found - possible documentation gaps or missing edges._