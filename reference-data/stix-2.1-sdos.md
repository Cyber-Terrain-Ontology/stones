# STIX 2.1 Domain Objects (SDOs) — Complete Reference

Source: STIX Version 2.1 OS — https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.html

All SDOs inherit the **Common Properties** (`type`, `spec_version`, `id`, `created`, `modified`, `created_by_ref`, `revoked`, `labels`, `confidence`, `lang`, `external_references`, `object_marking_refs`, `granular_markings`, `extensions`). Only type-specific properties are listed here.

---

## 1. `attack-pattern`

**Description:** A type of Tactics, Techniques, and Procedures (TTP) that describes ways that adversaries attempt to compromise targets. Often maps to MITRE ATT&CK techniques.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the attack pattern |
| `description` | string | Optional | Human-readable description |
| `aliases` | list of strings | Optional | Alternative names |
| `kill_chain_phases` | list of kill-chain-phase | Optional | Kill chain phases this technique appears in |

---

## 2. `campaign`

**Description:** A grouping of adversary behaviours and resources with a common objective over a period of time. Campaigns may be attributed to intrusion sets or threat actors.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the campaign |
| `description` | string | Optional | Human-readable description |
| `aliases` | list of strings | Optional | Alternative names |
| `first_seen` | timestamp | Optional | When the campaign was first observed |
| `last_seen` | timestamp | Optional | When the campaign was last observed |
| `objective` | string | Optional | Primary goal of the campaign |

---

## 3. `course-of-action`

**Description:** A recommendation for a specific measure to be taken in response to an attack. May describe automated responses, playbook steps, or manual mitigations.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the course of action |
| `description` | string | Optional | Human-readable description |

---

## 4. `grouping`

**Description:** Explicitly asserts that a set of STIX objects share a common context without implying the existence of a threat report. Useful for linking related objects without a formal report.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Optional | Name of the grouping |
| `description` | string | Optional | Human-readable description |
| `context` | open-vocab | Required | Shared context (`grouping-context-ov`): `suspicious-activity`, `malware-analysis`, `unspecified` |
| `object_refs` | list of identifiers | Required | References to STIX objects in this grouping |

---

## 5. `identity`

**Description:** Actual individuals, organisations, or groups, as well as classes of individuals, organisations, systems, or groups. Used to represent victims, producers, or any other real-world actor.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the identity |
| `description` | string | Optional | Human-readable description |
| `roles` | list of strings | Optional | Roles this identity plays (e.g., `"analyst"`) |
| `identity_class` | open-vocab | Optional | Type of entity (`identity-class-ov`): `individual`, `group`, `system`, `organization`, `class`, `unknown` |
| `sectors` | list of open-vocab | Optional | Industry sectors (`industry-sector-ov`) |
| `contact_information` | string | Optional | Contact details (email, phone, etc.) |

---

## 6. `indicator`

**Description:** Contains a pattern that can be used to detect suspicious or malicious cyber activity. Patterns are expressed using the STIX Patterning Language or another supported language.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Optional | Name of the indicator |
| `description` | string | Optional | Human-readable description |
| `indicator_types` | list of open-vocab | Required | Types (`indicator-type-ov`): `anomalous-activity`, `anonymization`, `benign`, `compromised`, `malicious-activity`, `attribution`, `unknown` |
| `pattern` | string | Required | Detection pattern (e.g., STIX pattern expression) |
| `pattern_type` | open-vocab | Required | Language of the pattern (`pattern-type-ov`): `stix`, `pcre`, `sigma`, `snort`, `suricata`, `yara` |
| `pattern_version` | string | Optional | Version of the pattern language |
| `valid_from` | timestamp | Required | When the indicator is valid from |
| `valid_until` | timestamp | Optional | When the indicator is no longer valid |
| `kill_chain_phases` | list of kill-chain-phase | Optional | Kill chain phases this indicator detects |

---

## 7. `infrastructure`

**Description:** Represents systems, software services, or any group of resources used to conduct cyber operations. Infrastructure is used by threat actors and intrusion sets both offensively and defensively.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the infrastructure |
| `description` | string | Optional | Human-readable description |
| `infrastructure_types` | list of open-vocab | Required | Types (`infrastructure-type-ov`): `amplification`, `anonymization`, `botnet`, `command-and-control`, `exfiltration`, `firewall`, `hosting-malware`, `hosting-target-lists`, `phishing`, `reconnaissance`, `routers-switches`, `staging`, `workstation`, `unknown` |
| `aliases` | list of strings | Optional | Alternative names |
| `kill_chain_phases` | list of kill-chain-phase | Optional | Kill chain phases |
| `first_seen` | timestamp | Optional | When this infrastructure was first observed |
| `last_seen` | timestamp | Optional | When this infrastructure was last observed |

---

## 8. `intrusion-set`

**Description:** A grouped set of adversarial behaviours and resources with common properties, believed to be orchestrated by a single organisation. Often represents a named APT group.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the intrusion set |
| `description` | string | Optional | Human-readable description |
| `aliases` | list of strings | Optional | Alternative names |
| `first_seen` | timestamp | Optional | When first observed |
| `last_seen` | timestamp | Optional | When last observed |
| `goals` | list of strings | Optional | High-level goals (e.g., `"financial gain"`) |
| `resource_level` | open-vocab | Optional | Resource level (`attack-resource-level-ov`): `individual`, `club`, `contest`, `team`, `organization`, `government` |
| `primary_motivation` | open-vocab | Optional | Primary motivation (`attack-motivation-ov`) |
| `secondary_motivations` | list of open-vocab | Optional | Secondary motivations (`attack-motivation-ov`) |

---

## 9. `location`

**Description:** Represents a geographic location. May be as specific as a latitude/longitude coordinate or as broad as a continent. At least one of `region`, `country`, or `latitude`+`longitude` MUST be provided.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Optional | Name of the location |
| `description` | string | Optional | Human-readable description |
| `latitude` | float | Optional | Latitude in decimal degrees (−90 to 90). Required with `longitude`. |
| `longitude` | float | Optional | Longitude in decimal degrees (−180 to 180). Required with `latitude`. |
| `precision` | float | Optional | Radius of accuracy in metres around lat/lon |
| `region` | open-vocab | Optional | UN M.49 region code (`region-ov`) |
| `country` | string | Optional | ISO 3166-1 alpha-2 country code (e.g., `"US"`) |
| `administrative_area` | string | Optional | State, province, or region within a country |
| `city` | string | Optional | City name |
| `street_address` | string | Optional | Street address |
| `postal_code` | string | Optional | Postal/ZIP code |

---

## 10. `malware`

**Description:** A type of TTP that represents malicious code. Captures both malware families (when `is_family` is `true`) and specific instances or samples.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Optional | Name of the malware (SHOULD be provided) |
| `description` | string | Optional | Human-readable description |
| `malware_types` | list of open-vocab | Required | Types (`malware-type-ov`): `adware`, `backdoor`, `bot`, `bootkit`, `ddos`, `downloader`, `dropper`, `exploit-kit`, `keylogger`, `ransomware`, `remote-access-trojan`, `resource-exploitation`, `rogue-security-software`, `rootkit`, `screen-capture`, `spyware`, `trojan`, `unknown`, `virus`, `webshell`, `wiper`, `worm` |
| `is_family` | boolean | Required | `true` if this is a malware family, `false` for a specific instance |
| `aliases` | list of strings | Optional | Alternative names |
| `kill_chain_phases` | list of kill-chain-phase | Optional | Kill chain phases |
| `first_seen` | timestamp | Optional | When first observed |
| `last_seen` | timestamp | Optional | When last observed |
| `operating_system_refs` | list of identifiers | Optional | References to `software` SCOs (OS platforms) |
| `architecture_execution_envs` | list of open-vocab | Optional | Processor architectures (`processor-architecture-ov`) |
| `implementation_languages` | list of open-vocab | Optional | Implementation languages (`implementation-language-ov`) |
| `capabilities` | list of open-vocab | Optional | Capabilities (`malware-capabilities-ov`) |
| `sample_refs` | list of identifiers | Optional | References to `file` or `artifact` SCOs with samples |

---

## 11. `malware-analysis`

**Description:** Captures the results of a malware analysis process, whether static or dynamic. Represents the output of a specific analysis run on a specific sample.

| Property | Type | Required | Notes |
|---|---|---|---|
| `product` | string | Required | Name of the analysis tool (e.g., `"Cuckoo"`, `"VirusTotal"`) |
| `version` | string | Optional | Version of the analysis tool |
| `host_vm_ref` | identifier | Optional | Reference to a `software` SCO for the analysis VM |
| `operating_system_ref` | identifier | Optional | Reference to a `software` SCO for the OS |
| `installed_software_refs` | list of identifiers | Optional | References to `software` SCOs installed during analysis |
| `configuration_version` | string | Optional | Version of the analysis configuration |
| `modules` | list of strings | Optional | Modules or plugins used in the analysis |
| `analysis_engine_version` | string | Optional | Version of the analysis engine |
| `analysis_definition_version` | string | Optional | Version of the analysis definitions/signatures |
| `submitted` | timestamp | Optional | When the sample was submitted for analysis |
| `analysis_started` | timestamp | Optional | When analysis started |
| `analysis_ended` | timestamp | Optional | When analysis ended |
| `av_result` | string | Optional | Classification string from AV engine (e.g., `"Trojan.GenericKD"`) |
| `analysis_sco_refs` | list of identifiers | Optional | References to SCOs generated during analysis |
| `result_name` | string | Optional | Classification name from the analysis product |
| `result` | open-vocab | Optional | Analysis verdict (`malware-result-ov`): `malicious`, `suspicious`, `benign`, `unknown` |

---

## 12. `note`

**Description:** Conveys informational text to provide further context and/or to assess the accuracy of existing STIX information. Similar to a comment or annotation.

| Property | Type | Required | Notes |
|---|---|---|---|
| `abstract` | string | Optional | Summary of the note |
| `content` | string | Required | Body of the note |
| `authors` | list of strings | Optional | Names of the note authors |
| `object_refs` | list of identifiers | Required | STIX objects this note applies to |

---

## 13. `observed-data`

**Description:** Conveys information observed on systems and networks using the SCO format. Wraps one or more SCO references together with observation metadata.

| Property | Type | Required | Notes |
|---|---|---|---|
| `first_observed` | timestamp | Required | Earliest time this data was observed |
| `last_observed` | timestamp | Required | Latest time this data was observed |
| `number_observed` | integer (≥1) | Required | Number of times the observation occurred |
| `object_refs` | list of identifiers | Required | References to SCO objects observed (replaces deprecated `objects` dictionary) |

---

## 14. `opinion`

**Description:** An assessment of the correctness of information in a STIX object. Allows analysts to express agreement/disagreement with existing intelligence.

| Property | Type | Required | Notes |
|---|---|---|---|
| `explanation` | string | Optional | Explanation of the opinion |
| `authors` | list of strings | Optional | Names of the opinion authors |
| `opinion` | enum | Required | Level of agreement (`opinion-enum`): `strongly-disagree`, `disagree`, `neutral`, `agree`, `strongly-agree` |
| `object_refs` | list of identifiers | Required | STIX objects this opinion applies to |

---

## 15. `report`

**Description:** Collections of threat intelligence focused on one or more topics, such as a description of a threat actor, malware, or attack technique. Used to package related STIX content.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the report |
| `description` | string | Optional | Human-readable description |
| `report_types` | list of open-vocab | Required | Types (`report-type-ov`): `attack-pattern`, `campaign`, `identity`, `indicator`, `intrusion-set`, `malware`, `observed-data`, `threat-actor`, `threat-report`, `tool`, `vulnerability` |
| `published` | timestamp | Required | Date and time the report was published |
| `object_refs` | list of identifiers | Required | References to STIX objects included in the report |

---

## 16. `threat-actor`

**Description:** Actual individuals, groups, or organisations believed to be operating with malicious intent. Unlike `identity` (which can be neutral), a `threat-actor` has adversarial connotations.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the threat actor |
| `description` | string | Optional | Human-readable description |
| `threat_actor_types` | list of open-vocab | Required | Types (`threat-actor-type-ov`): `activist`, `competitor`, `crime-syndicate`, `criminal`, `hacker`, `insider-accidental`, `insider-disgruntled`, `nation-state`, `sensationalist`, `spy`, `terrorist`, `unknown` |
| `aliases` | list of strings | Optional | Alternative names |
| `first_seen` | timestamp | Optional | When first observed |
| `last_seen` | timestamp | Optional | When last observed |
| `roles` | list of open-vocab | Optional | Roles (`threat-actor-role-ov`): `agent`, `director`, `independent`, `infrastructure-architect`, `infrastructure-operator`, `malware-author`, `sponsor` |
| `goals` | list of strings | Optional | High-level goals |
| `sophistication` | open-vocab | Optional | Sophistication level (`threat-actor-sophistication-ov`): `none`, `minimal`, `intermediate`, `advanced`, `expert`, `innovator`, `strategic` |
| `resource_level` | open-vocab | Optional | Resource level (`attack-resource-level-ov`) |
| `primary_motivation` | open-vocab | Optional | Primary motivation (`attack-motivation-ov`) |
| `secondary_motivations` | list of open-vocab | Optional | Secondary motivations (`attack-motivation-ov`) |
| `personal_motivations` | list of open-vocab | Optional | Personal motivations (`attack-motivation-ov`) |

---

## 17. `tool`

**Description:** Legitimate software that can be used by threat actors to perform attacks. Distinguishable from malware by being designed for benign purposes but used maliciously.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the tool |
| `description` | string | Optional | Human-readable description |
| `tool_types` | list of open-vocab | Required | Types (`tool-type-ov`): `denial-of-service`, `exploitation`, `information-gathering`, `network-capture`, `credential-exploitation`, `remote-access`, `vulnerability-scanning`, `unknown` |
| `aliases` | list of strings | Optional | Alternative names |
| `kill_chain_phases` | list of kill-chain-phase | Optional | Kill chain phases |
| `tool_version` | string | Optional | Version of the tool |

---

## 18. `vulnerability`

**Description:** A mistake in software that can be directly used by a threat actor to gain access to a system or network. Typically identified by a CVE number.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the vulnerability (e.g., `"CVE-2021-44228"`) |
| `description` | string | Optional | Human-readable description |

> Vulnerability objects reference CVEs via `external_references` with `source_name: "cve"` and `external_id: "CVE-YYYY-NNNNN"`.
