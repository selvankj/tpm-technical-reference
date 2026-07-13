# Template: Pipeline-Change PR Checklist

_Last reviewed: 2026-07-03 · Review cadence: quarterly_

Paste into your PR template for any change to a pipeline, stored procedure, or transformation. Born from the [data failure scenarios](../reference/data-failure-scenarios.md) catalog.

```markdown
## Pipeline-change checklist

- [ ] **Who consumes this output?** Lineage checked; downstream owners notified if the shape changes
- [ ] **Ran in stage** against representative data — not just "compiles"
- [ ] **Data verified, not just job status** — evidence attached (counts, sample output, DQ results)
- [ ] Output **format asserted** — contract test updated if the interface changed
- [ ] **Re-run safe?** Idempotency confirmed for the changed step
- [ ] **Rollback path** identified (previous version / partition restore)
- [ ] Failure is **loud** — if this change breaks, the pipeline actually goes red
```

> The last question is the one that matters most. **Green means the job ran; only data checks mean it worked.** A pipeline that can fail green is a defect in itself.

[← Back to index](../README.md)
