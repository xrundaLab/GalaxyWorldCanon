# Rollback Note — Map / Prehistory Candidate Activation

This B2 package makes no active change, so there is nothing to roll back now.

For a future activation release: retain the previous Catalog and projection revisions by exact commit/blob/SHA. If validation fails, restore the prior Catalog routing revision (or publish a superseding Catalog revision marking these projections inactive/deprecated), stop new runs from resolving the failed revisions, and keep already-created run snapshots unchanged. Do not rewrite or migrate historical runs. Re-run the Resolver acceptance checks against the restored exact pins before resuming new-run creation. The prior release ID and rollback commit must be recorded by the future activation release owner.
