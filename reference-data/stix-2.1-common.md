# STIX 2.1 Common Properties & Object Types Overview

Source: STIX Version 2.1 OS — https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.html

---

## Object Type Hierarchy

```
STIX Object
├── STIX Core Object
│   ├── STIX Domain Object (SDO)          — 18 types (attack-pattern, campaign, …)
│   ├── STIX Cyber Observable Object (SCO)— 18 types (artifact, autonomous-system, …)
│   └── STIX Relationship Object (SRO)   — 2 types (relationship, sighting)
├── STIX Meta Object                      — 3 types (marking-definition, language-content, extension-definition)
└── STIX Bundle                           — 1 type (bundle)
```

---

## Common Properties (All STIX Objects)

| Property | Type | Required | Notes |
|---|---|---|---|
| `type` | string | Required | Object type identifier (e.g., `"attack-pattern"`) |
| `spec_version` | string | Required | MUST be `"2.1"` |
| `id` | identifier | Required | `<type>--<UUIDv4>` |
| `created` | timestamp | Required | Creation time (millisecond precision) |
| `modified` | timestamp | Required | Last modification time (≥ `created`) |
| `created_by_ref` | identifier | Optional | Reference to an `identity` object |
| `revoked` | boolean | Optional | Whether the object has been revoked |
| `labels` | list of strings | Optional | Set of terms applying to this object |
| `confidence` | integer (0–100) | Optional | Producer's confidence in the accuracy |
| `lang` | string | Optional | BCP 47 language tag for object content |
| `external_references` | list of external-reference | Optional | References to non-STIX information (CVEs, URLs, etc.) |
| `object_marking_refs` | list of identifiers | Optional | References to `marking-definition` objects |
| `granular_markings` | list of granular-marking | Optional | Property-level marking definitions |
| `extensions` | dictionary | Optional | Extension objects keyed by extension type |

### External Reference Sub-type

| Property | Type | Required | Notes |
|---|---|---|---|
| `source_name` | string | Required | Name of the source (e.g., `"mitre-attack"`, `"cve"`) |
| `description` | string | Optional | Human-readable description |
| `url` | string | Optional | URL to the referenced resource |
| `hashes` | hashes dictionary | Optional | Hashes of the referenced content |
| `external_id` | string | Optional | External identifier (e.g., `"T1059"`, `"CVE-2021-44228"`) |

### Kill Chain Phase Sub-type

| Property | Type | Required | Notes |
|---|---|---|---|
| `kill_chain_name` | string | Required | Name of the kill chain (e.g., `"mitre-attack"`, `"lockheed-martin-cyber-kill-chain"`) |
| `phase_name` | string | Required | Phase within the kill chain (e.g., `"execution"`, `"persistence"`) |

### Granular Marking Sub-type

| Property | Type | Required | Notes |
|---|---|---|---|
| `marking_ref` | identifier | Required | Reference to a `marking-definition` |
| `selectors` | list of strings | Required | JSON property selectors (e.g., `["name", "description"]`) |
| `lang` | string | Optional | Language tag for this marking |

---

## Hashes Dictionary

A hashes dictionary maps hash algorithm names to their hex-encoded values.

| Key (algorithm) | Example value |
|---|---|
| `MD5` | `"d41d8cd98f00b204e9800998ecf8427e"` |
| `SHA-1` | `"da39a3ee5e6b4b0d3255bfef95601890afd80709"` |
| `SHA-256` | `"e3b0c44298fc1c149afb…"` |
| `SHA-512` | `"cf83e1357eefb8bdf…"` |
| `SSDEEP` | `"3::"`|
| `TLSH` | `"T1…"` |

Vocabulary: `hash-algorithm-ov` (see vocabularies.ttl).

---

## Identifier Format

All STIX identifiers follow the pattern: `<type>--<UUIDv4>`

Examples:
- `attack-pattern--0c7b5b88-8ff7-4a4d-aa9d-feb398cd0061`
- `identity--f165d4a4-a686-4e40-9985-f2e16a0d5a08`

---

## STIX Domain Objects (SDOs) — Summary

| Type | Description |
|---|---|
| `attack-pattern` | A technique used by adversaries to achieve goals (often maps to MITRE ATT&CK) |
| `campaign` | A grouping of adversary behaviours over time with a shared objective |
| `course-of-action` | Recommended action to prevent or remediate an attack |
| `grouping` | A collection of STIX objects sharing a context without implying a report |
| `identity` | Individuals, organisations, or groups — victims or actors |
| `indicator` | A pattern that detects suspicious or malicious activity |
| `infrastructure` | Systems, services, or resources used by adversaries or defenders |
| `intrusion-set` | A clustered set of adversary behaviours attributed to a single actor |
| `location` | A geographic region, country, or precise lat/lon |
| `malware` | Malicious code — either a family or a specific instance |
| `malware-analysis` | The results of a malware analysis process |
| `note` | Commentary on other STIX objects |
| `observed-data` | Raw cybersecurity observations (wraps SCO references) |
| `opinion` | An assessment of the correctness of other STIX objects |
| `report` | A collection of threat intelligence on a topic |
| `threat-actor` | Individuals, groups, or organisations believed to be operating with malicious intent |
| `tool` | Legitimate software used for malicious purposes |
| `vulnerability` | A flaw that can be exploited, typically identified by CVE |

## STIX Cyber Observable Objects (SCOs) — Summary

| Type | Description |
|---|---|
| `artifact` | A byte sequence (file content, network capture, memory dump) |
| `autonomous-system` | An Autonomous System identified by AS number |
| `directory` | A file system directory |
| `domain-name` | A network domain name |
| `email-addr` | An email address |
| `email-message` | An email message with headers and body |
| `file` | A file with metadata and optional content |
| `ipv4-addr` | An IPv4 address or CIDR range |
| `ipv6-addr` | An IPv6 address or CIDR range |
| `mac-addr` | A MAC address |
| `mutex` | A named mutex object |
| `network-traffic` | A network traffic flow between endpoints |
| `process` | A running or terminated process |
| `software` | A software product |
| `url` | A URL |
| `user-account` | A user account on a system or service |
| `windows-registry-key` | A Windows Registry key |
| `x509-certificate` | An X.509 digital certificate |

## STIX Relationship Objects (SROs) — Summary

| Type | Description |
|---|---|
| `relationship` | A directed link between two STIX objects |
| `sighting` | Records that a STIX object was seen |

## STIX Meta Objects — Summary

| Type | Description |
|---|---|
| `marking-definition` | Defines a data handling marking (e.g., TLP) |
| `language-content` | Provides translations of STIX object properties |
| `extension-definition` | Defines a schema extension to STIX |

---

## TLP Marking Definition Values (Built-in)

| Identifier | TLP Level | Meaning |
|---|---|---|
| `marking-definition--613f2e26-407d-48c7-9eca-b8e91df99dc9` | TLP:WHITE | Unlimited disclosure |
| `marking-definition--34098fce-860f-48ae-8e50-ebd3cc5e41da` | TLP:GREEN | Community disclosure |
| `marking-definition--f88d31f6-486f-44da-b317-01333bde0b82` | TLP:AMBER | Limited disclosure |
| `marking-definition--5e57c739-391a-4eb3-b6be-7d15ca92d5ed` | TLP:RED | Personal/named recipients only |
