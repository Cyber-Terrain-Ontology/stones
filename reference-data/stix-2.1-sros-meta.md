# STIX 2.1 Relationship Objects, Meta-Objects, Bundle & Relationship Type Table

Source: STIX Version 2.1 OS — https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.html

---

## STIX Relationship Objects (SROs)

SROs inherit all **Common Properties** (`type`, `spec_version`, `id`, `created`, `modified`, etc.). Only type-specific properties are listed here.

---

### 1. `relationship`

**Description:** A directed link between two STIX objects. The semantics of the link are defined by the `relationship_type` property. Can link any two STIX Core Objects (SDOs, SCOs, or other SROs).

| Property | Type | Required | Notes |
|---|---|---|---|
| `relationship_type` | string | Required | The type of relationship (e.g., `"uses"`, `"targets"`, `"indicates"`) |
| `description` | string | Optional | Human-readable description of the relationship |
| `source_ref` | identifier | Required | The STIX object at the start of the relationship |
| `target_ref` | identifier | Required | The STIX object at the end of the relationship |
| `start_time` | timestamp | Optional | When the relationship began |
| `stop_time` | timestamp | Optional | When the relationship ended (MUST be ≥ `start_time`) |

---

### 2. `sighting`

**Description:** Records that a STIX object (typically an indicator, malware, tool, or threat actor) was seen. Wraps observation metadata around a reference to the sighted object.

| Property | Type | Required | Notes |
|---|---|---|---|
| `description` | string | Optional | Human-readable description |
| `first_seen` | timestamp | Optional | Earliest time the object was sighted |
| `last_seen` | timestamp | Optional | Latest time the object was sighted (MUST be ≥ `first_seen`) |
| `count` | integer (≥0) | Optional | Number of times the object was sighted |
| `sighting_of_ref` | identifier | Required | Reference to the STIX object that was sighted |
| `observed_data_refs` | list of identifiers | Optional | References to `observed-data` SDOs supporting the sighting |
| `where_sighted_refs` | list of identifiers | Optional | References to `identity` SDOs for who/where sighted |
| `summary` | boolean | Optional | If `true`, this sighting is a summary (not a specific instance) |

---

## STIX Meta-Objects

Meta-objects do **not** inherit all common properties. Each has its own required subset.

---

### 3. `marking-definition`

**Description:** Defines a data marking used to restrict or label how STIX data can be shared or handled (e.g., TLP markings). Referenced by `object_marking_refs` or `granular_markings`.

| Property | Type | Required | Notes |
|---|---|---|---|
| `type` | string | Required | MUST be `"marking-definition"` |
| `spec_version` | string | Required | MUST be `"2.1"` |
| `id` | identifier | Required | `marking-definition--<UUIDv4>` |
| `created` | timestamp | Required | Creation time |
| `created_by_ref` | identifier | Optional | Reference to an `identity` |
| `external_references` | list | Optional | External references |
| `object_marking_refs` | list | Optional | Markings applied to this definition itself |
| `granular_markings` | list | Optional | Property-level markings |
| `name` | string | Optional | Human-readable name (e.g., `"TLP:GREEN"`) |
| `definition_type` | string | Required | Type of marking: `"tlp"` or `"statement"` |
| `definition` | object | Required | The marking definition content (see below) |

**TLP definition object:**

| Property | Type | Notes |
|---|---|---|
| `tlp` | string | One of: `"white"`, `"green"`, `"amber"`, `"red"` |

**Statement definition object:**

| Property | Type | Notes |
|---|---|---|
| `statement` | string | A human-readable statement (e.g., copyright notice) |

**Built-in TLP marking definition identifiers:**

| Identifier | TLP Level |
|---|---|
| `marking-definition--613f2e26-407d-48c7-9eca-b8e91df99dc9` | TLP:WHITE |
| `marking-definition--34098fce-860f-48ae-8e50-ebd3cc5e41da` | TLP:GREEN |
| `marking-definition--f88d31f6-486f-44da-b317-01333bde0b82` | TLP:AMBER |
| `marking-definition--5e57c739-391a-4eb3-b6be-7d15ca92d5ed` | TLP:RED |

---

### 4. `language-content`

**Description:** Provides translations of string properties of another STIX object into different languages. Allows internationalisation without modifying the original object.

| Property | Type | Required | Notes |
|---|---|---|---|
| `type` | string | Required | MUST be `"language-content"` |
| `spec_version` | string | Required | MUST be `"2.1"` |
| `id` | identifier | Required | `language-content--<UUIDv4>` |
| `created` | timestamp | Required | Creation time |
| `modified` | timestamp | Required | Last modification time |
| `created_by_ref` | identifier | Optional | Reference to an `identity` |
| `object_marking_refs` | list | Optional | Marking references |
| `granular_markings` | list | Optional | Property-level markings |
| `object_ref` | identifier | Required | Reference to the STIX object being translated |
| `object_modified` | timestamp | Required | The `modified` timestamp of the referenced object |
| `contents` | dictionary | Required | Map of BCP 47 language tag → property translation map (e.g., `{"fr": {"name": "Logiciel malveillant"}}`) |

---

### 5. `extension-definition`

**Description:** Defines a schema extension to STIX 2.1. Allows producers to add new properties or new object types in a structured way.

| Property | Type | Required | Notes |
|---|---|---|---|
| `type` | string | Required | MUST be `"extension-definition"` |
| `spec_version` | string | Required | MUST be `"2.1"` |
| `id` | identifier | Required | `extension-definition--<UUIDv4>` |
| `created` | timestamp | Required | Creation time |
| `modified` | timestamp | Required | Last modification time |
| `created_by_ref` | identifier | Optional | Reference to an `identity` |
| `name` | string | Required | Name of the extension |
| `description` | string | Optional | Human-readable description |
| `schema` | string | Required | URL to or content of the JSON Schema definition |
| `version` | string | Required | Version of the extension schema |
| `extension_types` | list of enum | Required | How this extension is used: `"new-sdo"`, `"new-sco"`, `"new-sro"`, `"property-extension"`, `"toplevel-property-extension"` |
| `extension_properties` | list of strings | Optional | Names of new top-level properties added (used with `toplevel-property-extension`) |

---

## STIX Bundle

**Description:** A collection of arbitrary STIX objects. Bundles are the primary transport container for STIX content. A bundle is NOT a STIX Core Object — it has no `modified` property and cannot be referenced by `id`.

| Property | Type | Required | Notes |
|---|---|---|---|
| `type` | string | Required | MUST be `"bundle"` |
| `id` | identifier | Required | `bundle--<UUIDv4>` |
| `objects` | list of STIX objects | Optional | Any STIX objects (SDOs, SCOs, SROs, meta-objects) |

> Note: Bundles do not have `spec_version`, `created`, or `modified`.

---

## STIX 2.1 Defined Relationship Types

The table below lists all relationship types defined in the STIX 2.1 specification (Section 7). Each row shows a valid `source_type → relationship_type → target_type` triple.

### `attack-pattern` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `attack-pattern` | `delivers` | `malware` |
| `attack-pattern` | `targets` | `identity` |
| `attack-pattern` | `targets` | `location` |
| `attack-pattern` | `targets` | `vulnerability` |
| `attack-pattern` | `uses` | `malware` |
| `attack-pattern` | `uses` | `tool` |
| `attack-pattern` | `mitigates` | `attack-pattern` |

### `campaign` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `campaign` | `attributed-to` | `intrusion-set` |
| `campaign` | `attributed-to` | `threat-actor` |
| `campaign` | `compromises` | `infrastructure` |
| `campaign` | `originates-from` | `location` |
| `campaign` | `targets` | `identity` |
| `campaign` | `targets` | `location` |
| `campaign` | `targets` | `vulnerability` |
| `campaign` | `uses` | `attack-pattern` |
| `campaign` | `uses` | `infrastructure` |
| `campaign` | `uses` | `malware` |
| `campaign` | `uses` | `tool` |

### `course-of-action` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `course-of-action` | `investigates` | `indicator` |
| `course-of-action` | `mitigates` | `attack-pattern` |
| `course-of-action` | `mitigates` | `indicator` |
| `course-of-action` | `mitigates` | `malware` |
| `course-of-action` | `mitigates` | `tool` |
| `course-of-action` | `mitigates` | `vulnerability` |
| `course-of-action` | `remediates` | `malware` |
| `course-of-action` | `remediates` | `vulnerability` |

### `identity` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `identity` | `located-at` | `location` |
| `identity` | `part-of` | `identity` |

### `indicator` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `indicator` | `based-on` | `observed-data` |
| `indicator` | `indicates` | `attack-pattern` |
| `indicator` | `indicates` | `campaign` |
| `indicator` | `indicates` | `infrastructure` |
| `indicator` | `indicates` | `intrusion-set` |
| `indicator` | `indicates` | `malware` |
| `indicator` | `indicates` | `threat-actor` |
| `indicator` | `indicates` | `tool` |

### `infrastructure` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `infrastructure` | `communicates-with` | `infrastructure` |
| `infrastructure` | `communicates-with` | `ipv4-addr` |
| `infrastructure` | `communicates-with` | `ipv6-addr` |
| `infrastructure` | `communicates-with` | `domain-name` |
| `infrastructure` | `communicates-with` | `url` |
| `infrastructure` | `consists-of` | `infrastructure` |
| `infrastructure` | `consists-of` | `ipv4-addr` |
| `infrastructure` | `consists-of` | `ipv6-addr` |
| `infrastructure` | `consists-of` | `domain-name` |
| `infrastructure` | `consists-of` | `url` |
| `infrastructure` | `consists-of` | `artifact` |
| `infrastructure` | `consists-of` | `network-traffic` |
| `infrastructure` | `consists-of` | `software` |
| `infrastructure` | `controls` | `infrastructure` |
| `infrastructure` | `controls` | `malware` |
| `infrastructure` | `delivers` | `malware` |
| `infrastructure` | `has` | `vulnerability` |
| `infrastructure` | `hosts` | `tool` |
| `infrastructure` | `hosts` | `malware` |
| `infrastructure` | `located-at` | `location` |
| `infrastructure` | `uses` | `infrastructure` |

### `intrusion-set` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `intrusion-set` | `attributed-to` | `threat-actor` |
| `intrusion-set` | `compromises` | `infrastructure` |
| `intrusion-set` | `hosts` | `infrastructure` |
| `intrusion-set` | `originates-from` | `location` |
| `intrusion-set` | `owns` | `infrastructure` |
| `intrusion-set` | `targets` | `identity` |
| `intrusion-set` | `targets` | `location` |
| `intrusion-set` | `targets` | `vulnerability` |
| `intrusion-set` | `uses` | `attack-pattern` |
| `intrusion-set` | `uses` | `infrastructure` |
| `intrusion-set` | `uses` | `malware` |
| `intrusion-set` | `uses` | `tool` |

### `malware` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `malware` | `authored-by` | `threat-actor` |
| `malware` | `authored-by` | `intrusion-set` |
| `malware` | `beacons-to` | `infrastructure` |
| `malware` | `communicates-with` | `ipv4-addr` |
| `malware` | `communicates-with` | `ipv6-addr` |
| `malware` | `communicates-with` | `domain-name` |
| `malware` | `communicates-with` | `url` |
| `malware` | `controls` | `malware` |
| `malware` | `downloads` | `malware` |
| `malware` | `downloads` | `tool` |
| `malware` | `downloads` | `file` |
| `malware` | `drops` | `malware` |
| `malware` | `drops` | `tool` |
| `malware` | `drops` | `file` |
| `malware` | `exploits` | `vulnerability` |
| `malware` | `originates-from` | `location` |
| `malware` | `targets` | `identity` |
| `malware` | `targets` | `infrastructure` |
| `malware` | `targets` | `location` |
| `malware` | `targets` | `vulnerability` |
| `malware` | `uses` | `attack-pattern` |
| `malware` | `uses` | `infrastructure` |
| `malware` | `uses` | `malware` |
| `malware` | `uses` | `tool` |
| `malware` | `variant-of` | `malware` |

### `malware-analysis` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `malware-analysis` | `characterizes` | `malware` |
| `malware-analysis` | `analysis-of` | `malware` |
| `malware-analysis` | `static-analysis-of` | `malware` |
| `malware-analysis` | `dynamic-analysis-of` | `malware` |

### `threat-actor` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `threat-actor` | `attributed-to` | `identity` |
| `threat-actor` | `compromises` | `infrastructure` |
| `threat-actor` | `hosts` | `infrastructure` |
| `threat-actor` | `owns` | `infrastructure` |
| `threat-actor` | `impersonates` | `identity` |
| `threat-actor` | `located-at` | `location` |
| `threat-actor` | `targets` | `identity` |
| `threat-actor` | `targets` | `location` |
| `threat-actor` | `targets` | `vulnerability` |
| `threat-actor` | `uses` | `attack-pattern` |
| `threat-actor` | `uses` | `infrastructure` |
| `threat-actor` | `uses` | `malware` |
| `threat-actor` | `uses` | `tool` |

### `tool` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `tool` | `delivers` | `malware` |
| `tool` | `drops` | `malware` |
| `tool` | `has` | `vulnerability` |
| `tool` | `targets` | `identity` |
| `tool` | `targets` | `infrastructure` |
| `tool` | `targets` | `location` |
| `tool` | `targets` | `vulnerability` |
| `tool` | `uses` | `infrastructure` |

### `vulnerability` relationships

| Source | Relationship Type | Target |
|---|---|---|
| `vulnerability` | `has` | `vulnerability` |

### `sighting` — implicit relationships

A `sighting` SRO records that `sighting_of_ref` was seen. The following object types are the primary targets for sightings:

- `indicator`
- `malware`
- `campaign`
- `threat-actor`
- `intrusion-set`
- `tool`
- `attack-pattern`
- `infrastructure`

---

## Summary of All Relationship Type Values

The following are all named `relationship_type` values defined in STIX 2.1:

| Relationship Type | Typical Usage |
|---|---|
| `analysis-of` | malware-analysis → malware |
| `attributed-to` | campaign/intrusion-set/threat-actor → threat-actor/identity |
| `authored-by` | malware → threat-actor/intrusion-set |
| `based-on` | indicator → observed-data |
| `beacons-to` | malware → infrastructure |
| `characterizes` | malware-analysis → malware |
| `communicates-with` | infrastructure/malware → SCO/infrastructure |
| `compromises` | campaign/intrusion-set/threat-actor → infrastructure |
| `consists-of` | infrastructure → infrastructure/SCO |
| `controls` | infrastructure/malware → infrastructure/malware |
| `delivers` | attack-pattern/infrastructure/tool → malware |
| `downloads` | malware → malware/tool/file |
| `drops` | malware/tool → malware/tool/file |
| `dynamic-analysis-of` | malware-analysis → malware |
| `exploits` | malware → vulnerability |
| `has` | infrastructure/tool/vulnerability → vulnerability |
| `hosts` | infrastructure/intrusion-set/threat-actor → infrastructure/malware/tool |
| `impersonates` | threat-actor → identity |
| `indicates` | indicator → attack-pattern/campaign/infrastructure/intrusion-set/malware/threat-actor/tool |
| `investigates` | course-of-action → indicator |
| `located-at` | identity/infrastructure/malware/threat-actor → location |
| `mitigates` | attack-pattern/course-of-action → attack-pattern/indicator/malware/tool/vulnerability |
| `originates-from` | campaign/intrusion-set/malware/threat-actor → location |
| `owns` | intrusion-set/threat-actor → infrastructure |
| `part-of` | identity → identity |
| `remediates` | course-of-action → malware/vulnerability |
| `static-analysis-of` | malware-analysis → malware |
| `targets` | attack-pattern/campaign/intrusion-set/malware/threat-actor/tool → identity/infrastructure/location/vulnerability |
| `uses` | campaign/intrusion-set/malware/threat-actor/tool → attack-pattern/infrastructure/malware/tool |
| `variant-of` | malware → malware |
