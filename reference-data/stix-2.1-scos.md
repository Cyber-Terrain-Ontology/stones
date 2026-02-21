# STIX 2.1 Cyber Observable Objects (SCOs) — Complete Reference

Source: STIX Version 2.1 OS — https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.html

All SCOs inherit these common SCO properties (omitted from per-object tables below):
`type`, `id`, `spec_version`, `object_marking_refs`, `granular_markings`, `defanged`, `extensions`

---

## 1. `artifact`

**Description:** Represents a sequence of bytes, such as the content of a file, a raw stream of network traffic, or a data blob captured from memory. The content can be embedded directly via `payload_bin` or referenced via `url`; these two are mutually exclusive.

| Property | Type | Required | Notes |
|---|---|---|---|
| `mime_type` | string | Optional | MIME type of the artifact content (e.g., `"application/pdf"`) |
| `payload_bin` | binary (base64) | Optional | The artifact bytes, encoded as base64. Mutually exclusive with `url`. |
| `url` | string | Optional | URL where the artifact content can be fetched. Mutually exclusive with `payload_bin`. |
| `hashes` | hashes dictionary | Optional | Cryptographic hashes of the artifact content |
| `encryption_algorithm` | open-vocab | Optional | Encryption algorithm used (e.g., `"AES-256-GCM"`); required if `decryption_key` is present |
| `decryption_key` | string | Optional | Decryption key for the artifact if it is encrypted |

---

## 2. `autonomous-system`

**Description:** Represents an Autonomous System (AS) — a collection of IP routing prefixes under the control of a single administrative entity.

| Property | Type | Required | Notes |
|---|---|---|---|
| `number` | integer | Required | The AS number (e.g., `15169`) |
| `name` | string | Optional | Name of the autonomous system (e.g., `"GOOGLE"`) |
| `rir` | string | Optional | Regional Internet Registry that assigned the number (e.g., `"ARIN"`) |

---

## 3. `directory`

**Description:** Represents a file system directory, conveying its path and timestamps. May reference the files and sub-directories it contains.

| Property | Type | Required | Notes |
|---|---|---|---|
| `path` | string | Required | Absolute path to the directory (e.g., `"C:\\Windows\\System32"`) |
| `path_enc` | string | Optional | Character encoding for the `path` property if non-ASCII |
| `ctime` | timestamp | Optional | Directory creation time |
| `mtime` | timestamp | Optional | Directory last-modified time |
| `atime` | timestamp | Optional | Directory last-accessed time |
| `contains_refs` | list of identifiers | Optional | References to `file` or `directory` SCOs contained within this directory |

---

## 4. `domain-name`

**Description:** Represents a network domain name. Can reference the IP addresses it resolves to.

| Property | Type | Required | Notes |
|---|---|---|---|
| `value` | string | Required | The domain name (e.g., `"example.com"`) |
| `resolves_to_refs` | list of identifiers | Optional | References to `ipv4-addr`, `ipv6-addr`, or `domain-name` SCOs this domain resolves to |

---

## 5. `email-addr`

**Description:** Represents a single email address, optionally associating it with a display name.

| Property | Type | Required | Notes |
|---|---|---|---|
| `value` | string | Required | The email address (e.g., `"jane@example.com"`) |
| `display_name` | string | Optional | Display name associated with the address |
| `belongs_to_ref` | identifier | Optional | Reference to a `user-account` SCO that owns this address |

---

## 6. `email-message`

**Description:** Represents an email message, including its headers, body, and any MIME parts.

| Property | Type | Required | Notes |
|---|---|---|---|
| `is_multipart` | boolean | Required | `true` if `body_multipart` is populated; `false` if `body` is used |
| `date` | timestamp | Optional | Date the email was sent |
| `content_type` | string | Optional | Value of the `Content-Type:` header |
| `from_ref` | identifier | Optional | `email-addr` in the `From:` header |
| `sender_ref` | identifier | Optional | `email-addr` in the `Sender:` header |
| `to_refs` | list of identifiers | Optional | `email-addr` SCOs in the `To:` header |
| `cc_refs` | list of identifiers | Optional | `email-addr` SCOs in the `Cc:` header |
| `bcc_refs` | list of identifiers | Optional | `email-addr` SCOs in the `Bcc:` header |
| `message_id` | string | Optional | Value of the `Message-ID:` header |
| `subject` | string | Optional | Subject line |
| `received_lines` | list of strings | Optional | `Received:` header values |
| `additional_header_fields` | dictionary | Optional | Additional non-standard headers |
| `body` | string | Optional | Plain-text body (when `is_multipart` is `false`) |
| `body_multipart` | list of email-mime-part-type | Optional | MIME parts (when `is_multipart` is `true`) |
| `raw_email_ref` | identifier | Optional | Reference to an `artifact` SCO with the raw email bytes |

### Email MIME Part Type

| Property | Type | Required | Notes |
|---|---|---|---|
| `body` | string | Optional | Decoded body of this MIME part |
| `body_raw_ref` | identifier | Optional | Reference to an `artifact` or `file` SCO |
| `content_type` | string | Optional | `Content-Type:` header for this part |
| `content_disposition` | string | Optional | `Content-Disposition:` header for this part |

---

## 7. `file`

**Description:** Represents a file with its metadata and optional content reference.

| Property | Type | Required | Notes |
|---|---|---|---|
| `hashes` | hashes dictionary | Optional | Cryptographic hashes of the file content |
| `size` | integer | Optional | File size in bytes |
| `name` | string | Optional | Filename including extension |
| `name_enc` | string | Optional | Character encoding for the `name` value |
| `magic_number_hex` | hex | Optional | Magic bytes (file signature) as a hex string |
| `mime_type` | string | Optional | MIME type of the file |
| `ctime` | timestamp | Optional | File creation time |
| `mtime` | timestamp | Optional | File last-modified time |
| `atime` | timestamp | Optional | File last-accessed time |
| `parent_directory_ref` | identifier | Optional | Reference to a `directory` SCO |
| `contains_refs` | list of identifiers | Optional | References to `file`/`directory` SCOs embedded within (e.g., archive members) |
| `content_ref` | identifier | Optional | Reference to an `artifact` SCO holding the file content |

### Extension: `archive-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `contains_refs` | list of identifiers | Required | References to `file` SCOs for the archive contents |
| `comment` | string | Optional | Comment field in the archive |

### Extension: `ntfs-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `sid` | string | Optional | Security Identifier (SID) of the file owner |
| `alternate_data_streams` | list of alternate-data-stream-type | Optional | NTFS Alternate Data Streams |

**Alternate Data Stream Type:** `name` (required), `hashes` (optional), `size` (optional)

### Extension: `pdf-ext`

| Property | Type | Notes |
|---|---|---|
| `version` | string | PDF format version (e.g., `"1.7"`) |
| `is_optimized` | boolean | Whether the PDF is linearized |
| `document_info_dict` | dictionary | PDF document information dictionary |
| `pdfid0` | string | First PDF file identifier |
| `pdfid1` | string | Second PDF file identifier |

### Extension: `raster-image-ext`

| Property | Type | Notes |
|---|---|---|
| `image_height` | integer | Image height in pixels |
| `image_width` | integer | Image width in pixels |
| `bits_per_pixel` | integer | Colour depth |
| `exif_tags` | dictionary | EXIF tag names and values |

### Extension: `windows-pebinary-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `pe_type` | open-vocab | Required | `"exe"`, `"dll"`, or `"sys"` (`windows-pebinary-type-ov`) |
| `imphash` | string | Optional | Import hash |
| `machine_hex` | hex | Optional | Machine type from COFF header |
| `number_of_sections` | integer | Optional | Number of PE sections |
| `time_date_stamp` | timestamp | Optional | Timestamp from PE header |
| `pointer_to_symbol_table_hex` | hex | Optional | File offset of COFF symbol table |
| `number_of_symbols` | integer | Optional | Number of symbols |
| `size_of_optional_header` | integer | Optional | Size of optional header |
| `characteristics_hex` | hex | Optional | Characteristics flags |
| `file_header_hashes` | hashes dictionary | Optional | Hashes of the file header |
| `optional_header` | windows-pe-optional-header-type | Optional | Optional header contents |
| `sections` | list of windows-pe-section-type | Optional | PE section entries |

**Windows PE Section Type:** `name` (required string), `size` (optional integer), `entropy` (optional float), `hashes` (optional)

---

## 8. `ipv4-addr`

**Description:** Represents one or more IPv4 addresses using CIDR notation.

| Property | Type | Required | Notes |
|---|---|---|---|
| `value` | string | Required | IPv4 address or CIDR range (e.g., `"198.51.100.0/24"`) |
| `resolves_to_refs` | list of identifiers | Optional | References to `mac-addr` SCOs |
| `belongs_to_refs` | list of identifiers | Optional | References to `autonomous-system` SCOs |

---

## 9. `ipv6-addr`

**Description:** Represents one or more IPv6 addresses using CIDR notation.

| Property | Type | Required | Notes |
|---|---|---|---|
| `value` | string | Required | IPv6 address or CIDR range (e.g., `"2001:0db8::/32"`) |
| `resolves_to_refs` | list of identifiers | Optional | References to `mac-addr` SCOs |
| `belongs_to_refs` | list of identifiers | Optional | References to `autonomous-system` SCOs |

---

## 10. `mac-addr`

**Description:** Represents a single MAC address. MUST be in lowercase IEEE 802 colon-hexadecimal notation.

| Property | Type | Required | Notes |
|---|---|---|---|
| `value` | string | Required | MAC address (e.g., `"d2:fb:49:24:37:18"`) |

---

## 11. `mutex`

**Description:** Represents a named mutual-exclusion object, commonly used by malware.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the mutex |

---

## 12. `network-traffic`

**Description:** Represents an arbitrary network traffic flow between source and destination endpoints.

| Property | Type | Required | Notes |
|---|---|---|---|
| `protocols` | list of strings | Required | Ordered list of protocols, lowest to highest layer (e.g., `["ipv4", "tcp", "http"]`) |
| `src_ref` | identifier | Optional | Source address (`ipv4-addr`, `ipv6-addr`, `mac-addr`, `domain-name`) |
| `dst_ref` | identifier | Optional | Destination address |
| `src_port` | integer | Optional | Source port (0–65535) |
| `dst_port` | integer | Optional | Destination port (0–65535) |
| `start` | timestamp | Optional | When traffic started |
| `end` | timestamp | Optional | When traffic ended |
| `is_active` | boolean | Optional | Whether traffic was active when observed |
| `src_byte_count` | integer | Optional | Bytes from source to destination |
| `dst_byte_count` | integer | Optional | Bytes from destination to source |
| `src_packets` | integer | Optional | Packets from source |
| `dst_packets` | integer | Optional | Packets from destination |
| `ipfix` | dictionary | Optional | IPFIX fields as key-value pairs |
| `src_payload_ref` | identifier | Optional | `artifact` SCO with source payload |
| `dst_payload_ref` | identifier | Optional | `artifact` SCO with destination payload |
| `encapsulates_refs` | list of identifiers | Optional | `network-traffic` SCOs encapsulated by this flow |
| `encapsulated_by_ref` | identifier | Optional | `network-traffic` SCO that encapsulates this flow |

### Extension: `http-request-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `request_method` | string | Required | HTTP method (e.g., `"GET"`, `"POST"`) |
| `request_value` | string | Required | Request URI (e.g., `"/index.html"`) |
| `request_version` | string | Optional | HTTP version (e.g., `"HTTP/1.1"`) |
| `request_header` | dictionary | Optional | HTTP headers as key-value pairs |
| `message_body_length` | integer | Optional | Length of the message body |
| `message_body_ref` | identifier | Optional | `artifact` SCO with the message body |

### Extension: `icmp-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `icmp_type_hex` | hex | Required | ICMP type as hex |
| `icmp_code_hex` | hex | Required | ICMP code as hex |

### Extension: `socket-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `address_family` | enum | Required | `AF_UNSPEC`, `AF_INET`, `AF_IPX`, `AF_APPLETALK`, `AF_NETBIOS`, `AF_INET6`, `AF_IRDA`, `AF_BTH` |
| `is_blocking` | boolean | Optional | Whether the socket is blocking |
| `is_listening` | boolean | Optional | Whether the socket is listening |
| `options` | dictionary | Optional | Socket options |
| `socket_type` | enum | Optional | `SOCK_STREAM`, `SOCK_DGRAM`, `SOCK_RAW`, `SOCK_RDM`, `SOCK_SEQPACKET` |
| `socket_descriptor` | integer | Optional | Platform-specific socket descriptor |
| `socket_handle` | integer | Optional | Windows handle for the socket |

### Extension: `tcp-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `src_flags_hex` | hex | Optional | TCP flags set on packets from source (e.g., `"0x02"` for SYN) |
| `dst_flags_hex` | hex | Optional | TCP flags set on packets from destination |

---

## 13. `process`

**Description:** Represents a running or terminated process on a host.

| Property | Type | Required | Notes |
|---|---|---|---|
| `is_hidden` | boolean | Optional | Whether the process is hidden |
| `pid` | integer | Optional | Process ID |
| `created_time` | timestamp | Optional | When the process was created |
| `cwd` | string | Optional | Current working directory |
| `command_line` | string | Optional | Full command line |
| `environment_variables` | dictionary | Optional | Environment variables |
| `opened_connection_refs` | list of identifiers | Optional | `network-traffic` SCOs opened by this process |
| `creator_user_ref` | identifier | Optional | `user-account` SCO of the user who launched the process |
| `image_ref` | identifier | Optional | `file` SCO of the process executable |
| `parent_ref` | identifier | Optional | Parent `process` SCO |
| `child_refs` | list of identifiers | Optional | Child `process` SCOs |

### Extension: `windows-process-ext`

| Property | Type | Notes |
|---|---|---|
| `aslr_enabled` | boolean | Whether ASLR is enabled |
| `dep_enabled` | boolean | Whether DEP is enabled |
| `priority` | string | Scheduling priority |
| `owner_sid` | string | Security Identifier of the process owner |
| `window_title` | string | Title of the main window |
| `startup_info` | dictionary | Fields from Windows `STARTUPINFO` struct |
| `integrity_level` | open-vocab | Windows integrity level (`windows-integrity-level-enum`): `low`, `medium`, `high`, `system` |

### Extension: `windows-service-ext`

| Property | Type | Required | Notes |
|---|---|---|---|
| `service_name` | string | Required | Windows service name |
| `descriptions` | list of strings | Optional | Service description strings |
| `display_name` | string | Optional | Human-readable service display name |
| `group_name` | string | Optional | Load ordering group |
| `start_type` | open-vocab | Optional | `SERVICE_AUTO_START`, `SERVICE_BOOT_START`, `SERVICE_DEMAND_START`, `SERVICE_DISABLED`, `SERVICE_SYSTEM_ALERT` |
| `service_dll_refs` | list of identifiers | Optional | `file` SCOs of DLLs loaded by the service |
| `service_type` | open-vocab | Optional | `SERVICE_KERNEL_DRIVER`, `SERVICE_FILE_SYSTEM_DRIVER`, `SERVICE_WIN32_OWN_PROCESS`, `SERVICE_WIN32_SHARE_PROCESS` |
| `service_status` | open-vocab | Optional | `SERVICE_CONTINUE_PENDING`, `SERVICE_PAUSE_PENDING`, `SERVICE_PAUSED`, `SERVICE_RUNNING`, `SERVICE_START_PENDING`, `SERVICE_STOP_PENDING`, `SERVICE_STOPPED` |

---

## 14. `software`

**Description:** Represents a software product including commercial applications, OSes, libraries, and utilities.

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Name of the software |
| `cpe` | string | Optional | NVD CPE 2.3 URI binding identifier |
| `swid` | string | Optional | SWID tag identifier |
| `languages` | list of strings | Optional | BCP 47 language tags |
| `vendor` | string | Optional | Name of the vendor |
| `version` | string | Optional | Version string |

---

## 15. `url`

**Description:** Represents a Uniform Resource Locator conforming to RFC 3986.

| Property | Type | Required | Notes |
|---|---|---|---|
| `value` | string | Required | The full URL (e.g., `"https://example.com/path?q=1"`) |

---

## 16. `user-account`

**Description:** Represents a user account on a system or service, including local OS accounts, cloud accounts, and social media accounts.

| Property | Type | Required | Notes |
|---|---|---|---|
| `user_id` | string | Optional* | Identifier of the account (SHOULD be provided) |
| `credential` | string | Optional | Clear-text credential (use with caution) |
| `account_login` | string | Optional | Login name if different from `user_id` |
| `account_type` | open-vocab | Optional | Type of account (`account-type-ov`): `facebook`, `ldap`, `nis`, `openid`, `radius`, `skype`, `tacacs`, `twitter`, `unix`, `windows-domain`, `windows-local` |
| `display_name` | string | Optional | Human-readable display name |
| `is_service_account` | boolean | Optional | Whether this is a service/system account |
| `is_privileged` | boolean | Optional | Whether the account has elevated privileges |
| `can_escalate_privs` | boolean | Optional | Whether the account can escalate privileges |
| `is_disabled` | boolean | Optional | Whether the account is disabled |
| `account_created` | timestamp | Optional | When the account was created |
| `account_expires` | timestamp | Optional | When the account expires |
| `credential_last_changed` | timestamp | Optional | When credentials were last changed |
| `account_first_login` | timestamp | Optional | First login time |
| `account_last_login` | timestamp | Optional | Most recent login time |

### Extension: `unix-account-ext`

| Property | Type | Notes |
|---|---|---|
| `uid` | integer | Numeric user ID (UID) |
| `gid` | integer | Primary numeric group ID (GID) |
| `groups` | list of strings | Group names the account belongs to |
| `home_dir` | string | Home directory path |
| `shell` | string | Login shell path |

---

## 17. `windows-registry-key`

**Description:** Represents a Windows Registry key with its path, values, and metadata.

| Property | Type | Required | Notes |
|---|---|---|---|
| `key` | string | Optional* | Full registry key path (SHOULD be provided) |
| `values` | list of windows-registry-value-type | Optional | Registry values under this key |
| `modified_time` | timestamp | Optional | Last modification time |
| `creator_user_ref` | identifier | Optional | `user-account` SCO of the creator |
| `number_of_subkeys` | integer | Optional | Number of subkeys |

### Windows Registry Value Type

| Property | Type | Required | Notes |
|---|---|---|---|
| `name` | string | Required | Value name (`""` for the default value) |
| `data` | string | Optional | Data stored in the value |
| `data_type` | open-vocab | Optional | Data type (`windows-registry-datatype-enum`): `REG_NONE`, `REG_SZ`, `REG_EXPAND_SZ`, `REG_BINARY`, `REG_DWORD`, `REG_DWORD_BIG_ENDIAN`, `REG_DWORD_LITTLE_ENDIAN`, `REG_LINK`, `REG_MULTI_SZ`, `REG_RESOURCE_LIST`, `REG_FULL_RESOURCE_DESCRIPTION`, `REG_RESOURCE_REQUIREMENTS_LIST`, `REG_QWORD`, `REG_INVALID_TYPE` |

---

## 18. `x509-certificate`

**Description:** Represents an X.509 digital certificate, capturing its subject, issuer, validity window, and cryptographic details.

| Property | Type | Required | Notes |
|---|---|---|---|
| `is_self_signed` | boolean | Optional | Whether self-signed |
| `hashes` | hashes dictionary | Optional | Hashes of the raw certificate bytes |
| `version` | string | Optional | X.509 version (e.g., `"3"`) |
| `serial_number` | string | Optional | Serial number assigned by issuer |
| `signature_algorithm` | string | Optional | Algorithm used to sign (e.g., `"sha256WithRSAEncryption"`) |
| `issuer` | string | Optional | Distinguished Name of the issuer |
| `validity_not_before` | timestamp | Optional | Start of validity period |
| `validity_not_after` | timestamp | Optional | End of validity period |
| `subject` | string | Optional | Distinguished Name of the subject |
| `subject_public_key_algorithm` | string | Optional | Algorithm of the subject's public key |
| `subject_public_key_modulus` | string | Optional | RSA public key modulus (hex) |
| `subject_public_key_exponent` | integer | Optional | RSA public key exponent |
| `x509_v3_extensions` | x509-v3-extensions-type | Optional | X.509 v3 extensions |

### X.509 v3 Extensions Type (all optional)

`basic_constraints`, `name_constraints`, `policy_constraints`, `key_usage`, `extended_key_usage`, `subject_key_identifier`, `authority_key_identifier`, `subject_alternative_name`, `issuer_alternative_name`, `subject_directory_attributes`, `crl_distribution_points`, `inhibit_any_policy`, `private_key_usage_period_not_before` (timestamp), `private_key_usage_period_not_after` (timestamp), `certificate_policies`, `policy_mappings`, `authority_info_access`

---

## Summary Table

| # | Type | Key Required Property | Extensions |
|---|---|---|---|
| 1 | `artifact` | — | — |
| 2 | `autonomous-system` | `number` | — |
| 3 | `directory` | `path` | — |
| 4 | `domain-name` | `value` | — |
| 5 | `email-addr` | `value` | — |
| 6 | `email-message` | `is_multipart` | — |
| 7 | `file` | — | `archive-ext`, `ntfs-ext`, `pdf-ext`, `raster-image-ext`, `windows-pebinary-ext` |
| 8 | `ipv4-addr` | `value` | — |
| 9 | `ipv6-addr` | `value` | — |
| 10 | `mac-addr` | `value` | — |
| 11 | `mutex` | `name` | — |
| 12 | `network-traffic` | `protocols` | `http-request-ext`, `icmp-ext`, `socket-ext`, `tcp-ext` |
| 13 | `process` | — | `windows-process-ext`, `windows-service-ext` |
| 14 | `software` | `name` | — |
| 15 | `url` | `value` | — |
| 16 | `user-account` | — | `unix-account-ext` |
| 17 | `windows-registry-key` | — | — |
| 18 | `x509-certificate` | — | — |
