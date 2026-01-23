# Exit and Cause

Use this guide when you need to inspect or report effect results.

- Use `Exit` as the data type that represents the result of running an effect.
- `Exit` is either `Success` or `Failure`.
- A `Failure` carries a `Cause`, which captures the reasons for failure.
- Use `Exit`/`Cause` for diagnostics, logging, or retry/reporting logic that needs structured failure data.
