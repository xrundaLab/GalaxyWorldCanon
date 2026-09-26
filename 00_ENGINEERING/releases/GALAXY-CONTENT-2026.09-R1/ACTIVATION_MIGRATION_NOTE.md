# Map / Prehistory Candidate Activation Note

**Lifecycle:** proposal only; no GalaxyWorldCanon files or Catalog authority have been changed.

The package prepares two structured navigation/continuity projections from Owner-approved decisions. The source Canon prose remains unchanged. Projections organize reviewed existing statements and eligibility decisions; they create no new Canon facts. The B2 Catalog remains `CANDIDATE_NOT_ACTIVE`; it does not activate the resources, Resolver, Approved Reference Asset Registry, Curriculum projection, or Plan B consumers.

## Activation sequence (future, separately approved)

1. Review projection records and source statement fidelity.
2. Publish the Map and Prehistory projections through the GalaxyWorldCanon Update / Release / Migration Protocol as reviewed resources, recording the exact post-commit path, commit, Git blob, and SHA-256.
3. Update the Catalog in that same approved resource release with complete exact pins and active lifecycle metadata.
4. Run future Resolver acceptance tests against those exact bytes. Missing or hash-drift required resources block new runs.
5. Historical Story runs remain pinned to their original dependencies and are not migrated or refreshed.

No Story run consumes these candidates before activation.


## RC1 non-cyclic pin model

Future Story runs pin `00_ENGINEERING/resources/GALAXY_RESOURCE_CATALOG.json` externally by repo/path/commit/blob/SHA-256. The Catalog does not self-hash or embed its own publishing commit. Same-repository Map/Prehistory projections resolve at the pinned Catalog revision and carry independent blob/content hashes.
