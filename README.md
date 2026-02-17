# WF Manifest Schema

**WF Manifest** is a **JSON Schema** for representing the output of the 
PID-LAND _special-pid_ like wf-search and wf-select in **RO-Crate / manifest format**. It is used to organize waveform metadata into a structured dataset with proper provenance and references.

---

## Purpose

- Standardize **RO-Crate / manifest representation** of waveform datasets.
- Enable **automatic validation** via JSON Schema.
- Support **FAIR principles**: Findable, Accessible, Interoperable, Reusable.
- Integration with other workflow components: `wf-handle` and `wf-provenance`.

---

## Format and Structure

- **Main type**: `object`
- **Required fields**: `@context`, `@graph`
- **Type checks**: string, array, object
- **Graph items**: can be a **Dataset**, **CreativeWork**, or **MediaObject**
- **Strict field consistency**: `additionalProperties: false` ensures no unexpected fields.

---

## Key Properties

| Field | Type | Description |
|-------|------|-------------|
| `@context` | string (URI) | Reference to JSON-LD context, e.g., RO-Crate context |
| `@graph` | array | List of entities in the manifest |
| `@id` | string | Unique identifier (URI) for the entity |
| `@type` | string | Entity type (`Dataset`, `CreativeWork`, `MediaObject`) |
| `name` | string | Human-readable name of the entity |
| `hasPart` | array | References to constituent parts of a dataset (for `Dataset`) |
| `about` | object | Indicates what the `CreativeWork` describes |
| `encodingFormat` | string | MIME type of the media object (for `MediaObject`) |

## Validation

- **JSON Schema**: ensures manifest structure is correct.
- **SHACL**: RDF-level validation of relationships.
- **Checks include**:
  - Dataset must have at least one `hasPart`
  - CreativeWork must have an `about` property

---

## Example JSON

```json
{
  "@context": "https://w3id.org/ro/crate/1.1/context",
  "@graph": [
    {
      "@id": "ro-crate-metadata.json",
      "@type": "CreativeWork",
      "about": {"@id": "./"}
    },
    {
      "@id": "./",
      "@type": "Dataset",
      "name": "INGV mSEED Waveform Collection",
      "hasPart": [
        {"@id": "https://hdl.handle.net/11099/file1"},
        {"@id": "https://hdl.handle.net/11099/file2"}
      ]
    },
    {
      "@id": "https://hdl.handle.net/11099/file1",
      "@type": "MediaObject",
      "encodingFormat": "application/vnd.fdsn.mseed"
    },
    {
      "@id": "https://hdl.handle.net/11099/file2",
      "@type": "MediaObject",
      "encodingFormat": "application/vnd.fdsn.mseed"
    }
  ]
}
```

Notes

The schema is designed to link with [WF Handle](https://github.com/INGV/wf-handle) and [WF Provenance](https://github.com/INGV/wf-provenance)  outputs.

Together they implement a [PID-centric](https://github.com/INGV/pid-land), [information-centric](https://github.com/INGV/rum-framework) metadata model
supporting FAIR data management, reproducible science, and long-term preservation.