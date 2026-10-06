# v3 → v4 API map: effect/Graph

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Graph`

- `Graph.Edge` -> `Graph.Edge`: The type remains as a structural interface, but its Data.Class constructor/value export was removed. Replace new Graph.Edge({ source, target, data }) with an object literal.

- `Graph.Graph` -> `Graph.Graph`: The immutable type remains, but storage is opaque; replace field access with Graph nodes, edges, count, lookup, neighbor, and acyclicity APIs.

- `Graph.MutableGraph` -> `Graph.MutableGraph`: The mutable type remains but no longer shares a public base interface with immutable Graph; obtain it through Graph.mutate or Graph.beginMutation and use public mutation/query functions.

- `Graph.Proto` -> `none`: The common graph protocol was removed. Use Graph.Graph or Graph.MutableGraph as appropriate and replace storage-field access with public graph query and mutation functions.

- `Graph.SearchConfig` -> `Graph.SearchConfig`: The type remains; direction is now Graph.TraversalDirection and also accepts undirected, while radius limits traversal depth.
