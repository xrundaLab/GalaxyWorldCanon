# Galaxy CHANGESET Template

```yaml
changeset:
  change_id:
  release_id:

  change_class:
    # PATCH | CONTENT_UPDATE | SCHEMA_CHANGE | CANON_BREAKING_CHANGE | RUNTIME_CHANGE | REFERENCE_UPDATE

  summary:

  changed_sources: []

  added: []
  changed: []
  deprecated: []
  removed: []

  impact:
    content: NONE
    schema: NONE
    engineering: NONE
    state: NONE
    episodes: NONE
    qa: NONE

  migration:
    required: false
    migration_type:
      # NONE | FORWARD_COMPAT | DATA_BACKFILL | STATE_MIGRATION |
      # SCHEMA_MIGRATION | CONTENT_REGENERATION | CANON_RECONCILIATION |
      # FULL_BREAKING_MIGRATION

  regeneration:
    required: false
    scope: []

  affected_episode_ids: []

  required_issues: []

  approval_required:
    canon_owner: false
    engineering_owner: false
```
