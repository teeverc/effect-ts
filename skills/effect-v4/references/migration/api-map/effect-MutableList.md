# v3 → v4 API map: effect/MutableList

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/MutableList`

- `MutableList.MutableList` -> `MutableList.MutableList`: The model remains but was redesigned from an iterable doubly linked list into a bucketed FIFO structure.

- `MutableList.TypeId` -> `none`: V4 MutableList has no public runtime marker.

- `MutableList.empty` -> `MutableList.make`: Constructor rename; MutableList.Empty is the take sentinel, not a constructor.

- `MutableList.forEach` -> `MutableList.toArray + Array.forEach`: No direct traversal helper remains; iterate a snapshot produced by MutableList.toArray.

- `MutableList.fromIterable` -> `MutableList.make + MutableList.appendAll`: Create an empty list with MutableList.make, then append the iterable with MutableList.appendAll.

- `MutableList.head` -> `MutableList.toArrayN`: Use MutableList.toArrayN(self, 1)[0]; the redesigned FIFO exposes buckets rather than the old Option-returning accessor.

- `MutableList.isEmpty` -> `none`: Read self.length === 0; no named isEmpty helper remains.

- `MutableList.length` -> `none`: Read the public self.length field; no named length helper remains.

- `MutableList.pop` -> `none`: The bucketed FIFO has no remove-last operation; migrate code to front draining or use a different mutable collection.

- `MutableList.reset` -> `MutableList.clear`: Direct behavioral replacement; the return type is now void.

- `MutableList.shift` -> `MutableList.take`: Front removal remains synchronous, but emptiness is reported with MutableList.Empty instead of undefined.

- `MutableList.tail` -> `MutableList.toArray`: Use MutableList.toArray(self).at(-1); the public self.tail field is an internal bucket, not the old last-element accessor.
