# Changelog

## 0.2.0 (2026-10-15)

### Changed

- The STIX common properties `stones:created`, `stones:modified`, `stones:createdByRef`,
  `stones:objectMarkingRefs`, `stones:granularMarkings` and `stones:externalReferences` now
  have `rdfs:domain stones:StixObject` (was `stones:StixCoreObject`).

  STIX 2.1 gives these properties to Meta Objects as well as Core Objects: a
  `marking-definition` has `created` and may have `created_by_ref`, `external_references` and
  the marking properties; an `extension-definition` and a `language-content` object carry
  `created`, `modified` and `created_by_ref`. With the old domain, a reasoner inferred that
  any such Meta Object was a `stones:StixCoreObject`, which the ontology declares disjoint
  from `stones:StixMetaObject`, so valid STIX data was inconsistent.

  The new domain only lets a reasoner conclude what STIX already says: that the subject is a
  STIX Object. Data and queries that use these properties are unaffected.

## 0.1.0

First public release.
