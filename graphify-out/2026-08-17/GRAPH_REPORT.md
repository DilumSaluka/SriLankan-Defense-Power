# Graph Report - SriLankan-Defense-Power  (2026-08-15)

## Corpus Check
- 2 files · ~75,756 words
- Verdict: corpus is large enough that graph structure adds value.

## Summary
- 9 nodes · 7 edges · 2 communities (0 shown, 2 thin omitted)
- Extraction: 100% EXTRACTED · 0% INFERRED · 0% AMBIGUOUS
- Token cost: 0 input · 0 output

## Graph Freshness
- Built from commit: `c7688349`
- Run `git rev-parse HEAD` and compare to check if the graph is stale.
- Run `graphify update .` after code changes (no API cost).

## Community Hubs (Navigation)
- [[_COMMUNITY_commanders.js|commanders.js]]
- [[_COMMUNITY_weapons.js|weapons.js]]

## God Nodes (most connected - your core abstractions)
1. `DB` - 1 edges
2. `WEAPONS` - 1 edges

## Surprising Connections (you probably didn't know these)
- None detected - all connections are within the same source files.

## Import Cycles
- None detected.

## Communities (2 total, 2 thin omitted)

## Knowledge Gaps
- **2 isolated node(s):** `DB`, `WEAPONS`
  These have ≤1 connection - possible missing edges or undocumented components.
- **2 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **What connects `DB`, `WEAPONS` to the rest of the system?**
  _2 weakly-connected nodes found - possible documentation gaps or missing edges._