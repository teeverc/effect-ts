# Scheduling and Retry

Use this guide when you need repetition, backoff, or retry policies.

- A `Schedule` describes a recurring policy and can produce output values.
- Use `Effect.repeat` to repeat on success; use `Effect.schedule` to only follow the schedule without an initial run.
- Use `Effect.retry` to retry failures according to a schedule.
- Compose schedules for backoff, jitter, or rate limiting.
- Choose `repeat` for success-based recurrence and `retry` for failure-based recovery.
