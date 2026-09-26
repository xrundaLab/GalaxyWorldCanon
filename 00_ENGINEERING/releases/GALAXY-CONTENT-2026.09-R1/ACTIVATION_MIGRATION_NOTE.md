# Map / Prehistory Resource Activation Note

**Release:** GALAXY-CONTENT-2026.09-R1  
**Effect:** active shared resource routing when this release is present on `main`.

This release publishes reviewed structured projections derived from the existing Map and Prehistory Source-of-Truth files. The source Canon prose is unchanged and no new Canon fact is created.

## Active resource behavior

- `GALAXY_MAP_INDEX_V0.1.json`
  - L01–L06: discoverable/selectable provisional persistent-place identities.
  - L07: inventory-visible, non-selectable, source status `UNDECIDED`.
  - L08: discoverable Personal State/user-space candidate, excluded from ordinary physical-location selection.
  - source names remain provisional structural placeholders.
  - character/place associations remain soft context, never hard gates.

- `GALAXY_PREHISTORY_INDEX_V0.1.json`
  - H01–H12 are active as authoring-background continuity facts.
  - default exposure policy: `AUTHORING_BACKGROUND_ONLY_UNTIL_EXPOSURE_REVIEW`.
  - no direct character/User disclosure permission is granted.
  - no dates, motives, locations, participant lists, private memories or biographies are inferred.

- `GALAXY_RESOURCE_CATALOG.json`
  - routes the active Map and Prehistory projections.
  - Curriculum remains inactive.
  - Approved Reference Asset Registry is not activated by this release.

## Non-cyclic pin model

The Catalog never self-hashes and does not embed its own publishing commit.

A future Story run must pin the Catalog externally in `authority_dependencies.json` by exact repo/path/commit/blob/SHA-256. Same-repository resources whose `source_revision` is `SAME_AS_PINNED_CATALOG_REVISION` resolve at that exact Catalog commit and are additionally checked by their own blob/SHA-256.

## Runtime / migration boundary

This release activates the **resource shelf**, not the Story Kernel Resolver.

- no Plan B code changes;
- no Resolver code changes;
- no historical run refresh or migration;
- no content regeneration.

Existing historical runs keep their frozen dependencies. New Story runs cannot consume these resources until the later Resolver implementation is explicitly activated.
