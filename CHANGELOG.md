# Changelog

## 0.3.0 (2026-11-20)

### Added

- Terms for regenerating a STIX 2.1 document exactly from its STONES triples
  (serialization fidelity). They are not STIX concepts, carry no `(STIX: ...)`
  key, and are deliberately not aligned to STONEWORK:
  - `stones:ListOrder` and `stones:ListItem`, with `stones:listOrder`,
    `stones:listProperty`, `stones:listItem`, `stones:listIndex`,
    `stones:listValue` (a literal member) and `stones:listRef` (an IRI member):
    the order of a list-valued property, which its separate triples don't
    carry. Index nodes rather than an `rdf:List`, because OWL 2 DL reserves
    `rdf:first` and `rdf:rest`.
  - `stones:unmodeledProperties`: the STIX properties of an object its STONES
    triples don't carry exactly, verbatim as one JSON object. An `xsd:string`
    rather than `rdf:JSON`, which is not in the OWL 2 datatype map.

  Data and queries that don't use them are unaffected.

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
