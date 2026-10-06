# v3 → v4 API map: effect/Mailbox

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Mailbox`

- `Mailbox.Mailbox` -> `Queue.Queue<A, E | Cause.Done>`: Mailbox was folded into Queue; include Cause.Done in the error channel when normal end signaling is used.

- `Mailbox.ReadonlyMailbox` -> `Queue.Dequeue<A, E | Cause.Done>`: Use explicit Queue taking operations; Queue.Dequeue is not itself an Effect yielding message chunks.

- `Mailbox.ReadonlyTypeId` -> `Queue.isDequeue`: The public Mailbox type id was removed; use the Queue.isDequeue guard instead.

- `Mailbox.TypeId` -> `Queue.isQueue`: The public Mailbox type id was removed; use the Queue.isQueue guard instead.

- `Mailbox.fromStream` -> `Stream.toQueue`: Mailbox was renamed and folded into Queue; Stream.toQueue returns a scoped Queue.Dequeue whose error includes Cause.Done.

- `Mailbox.into` -> `Queue.into`: Use Queue.into with a Queue.Enqueue whose error channel includes Cause.Done.

- `Mailbox.isMailbox` -> `Queue.isQueue`: Mailbox became the completion-aware v4 Queue model.

- `Mailbox.isReadonlyMailbox` -> `Queue.isDequeue`: ReadonlyMailbox became Queue.Dequeue.

- `Mailbox.make` -> `Queue.make`: Pass the v4 options object with optional capacity and strategy; a numeric capacity argument must become { capacity }.

- `Mailbox.toStream` -> `Stream.fromQueue`: Convert a Queue.Dequeue to a Stream; Cause.Done is excluded from the resulting stream error type.
