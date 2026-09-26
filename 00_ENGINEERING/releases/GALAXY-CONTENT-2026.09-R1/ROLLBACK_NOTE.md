# Rollback Note — Map / Prehistory Resource Activation

If this resource release must be rolled back before any Resolver consumer is activated, revert the GalaxyWorldCanon PR/merge that introduced `GALAXY-CONTENT-2026.09-R1` or publish a superseding Resource Catalog revision marking the projections inactive.

After a Resolver consumer is activated, rollback must:

1. stop new runs from resolving the affected Catalog revision;
2. restore/pin the prior Catalog revision or a superseding safe revision;
3. keep already-created historical run snapshots unchanged;
4. re-run Resolver acceptance checks against the restored exact pins before resuming new-run creation.

No Canon source prose, user state, or historical Story run is rewritten by this rollback.
