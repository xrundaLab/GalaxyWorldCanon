# Approved Reference Asset Registry Activation Note

**Release:** GALAXY-CONTENT-2026.09-R2

This release publishes the first machine-readable Owner-approved Reference Asset shelf.

## Active records

- RAC-001
- RAC-002
- RAC-003
- RAC-004
- RAC-006
- RAC-007
- RAC-008
- RAC-009
- RAC-010

RAC-005 remains Owner HOLD and is not present on the active Story shelf.

## Retrieval semantics

- Only records satisfying the approved-state predicate may be returned.
- A future Story Kernel scan may retrieve at most 0–3 relevant candidates.
- Zero candidates is valid.
- Deep Note opportunity scan remains required; inclusion remains optional.
- There is no frequency or coverage quota.

## Freshness

Freshness is evaluated when a new run freezes context. A newly initialized run must not use an expired/recheck-required asset until it is reverified.

Historical runs validate the provenance and integrity frozen at their creation time. They do not become invalid later merely because today's date passes a resource review_due.

## Source/evidence boundary

The Registry records scoped claims and evidence pointers. It does not permit Writer free web search. Agents may research future assets but may not self-approve them for Story use.

## Runtime boundary

This release activates the shared Registry resource and Catalog route only.

It does **not**:
- implement Story Kernel Resolver code;
- alter current Writer prompts;
- alter Plan B;
- migrate historical runs;
- retroactively modify L015.
