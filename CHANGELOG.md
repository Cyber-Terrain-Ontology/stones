# Changelog

## 0.3.1 (2026-11-21)

### Fixed

- `created` and `modified` are now required only where STIX 2.1 requires them
  (#4). `stones:StixCoreObject` required both (`owl:minCardinality 1`), but
  Cyber-observable Objects are Core Objects and have neither, so a reasoner
  concluded that every observable had timestamps the data doesn't state, and a
  closed-world check built from the restrictions would reject every SCO. The
  restrictions move to the classes STIX requires them on:

  | Class | `created` | `modified` |
  |---|---|---|
  | `stones:StixDomainObject` | required | required |
  | `stones:StixRelationshipObject` | required | required |
  | `stones:StixCyberobservableObject` | no restriction | no restriction |
  | `stones:MarkingDefinitionObject` | required | no restriction (marking definitions don't change) |
  | `stones:LanguageContentObject` | required | required |
  | `stones:ExtensionDefinitionObject` | required | required |

  `stones:StixCoreObject`'s definition no longer says every Core Object carries
  the timestamps.
- `dcterms:title`, `dcterms:description`, `dcterms:creator` and
  `dcterms:license`, used in the ontology header, are declared as
  `owl:AnnotationProperty`, so STONES itself is within the OWL 2 DL profile.
  (The remaining profile warnings come from the imported W3C SKOS file.)
- `owl:priorVersion` names 0.3.0 (it still named 0.1.0).

Checked with HermiT (consistent, no unsatisfiable classes) and ROBOT's OWL 2 DL
profile validation.

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
