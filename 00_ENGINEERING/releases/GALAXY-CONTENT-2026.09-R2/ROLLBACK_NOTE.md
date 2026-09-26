# Rollback Note — Approved Reference Asset Registry v0.1

Before any Resolver consumer is activated, rollback may revert the release that introduced the Registry/Catalog route or publish a superseding Catalog revision marking the Registry inactive.

After a Resolver consumer is activated:

1. stop new runs from resolving the affected Registry revision;
2. restore a previous safe Registry/Catalog revision or publish a superseding revision;
3. keep existing historical run snapshots unchanged;
4. re-run Resolver/Deep Note retrieval acceptance checks before resuming new-run creation.

Deprecating one asset does not rewrite historical runs. New runs must use only records eligible at initialization.
