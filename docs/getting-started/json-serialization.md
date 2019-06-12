---
layout: doc
subpage: Documentation
title: JSON Serialization
description: Serialization of Events and associated data to a JSON interchange format.
---
## Serializing to json

To serialize NEWT events to json use the ``nlpnewt-serializer`` processor
found in nlpnewt.io.serialization.

## Schema

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://nlpie.github.io/event.schema.json",
  "title": "Event",
  "description": "An event that has associated documents and labels.",
  "type": "object",
  "properties": {
    "event_id": {
      "description": "The global unique identifier for the event.",
      "type": "string"
    },
    "metadata": {
      "description": "Event related metadata.",
      "type": "object",
      "patternProperties": {
        ".*": {
          "type": [
            "string",
            "boolean",
            "number",
            "null"
          ]
        }
      }
    },
    "documents": {
      "type": "object",
      "description": "Documents keyed by their document names.",
      "patternProperties": {
        ".*": {
          "$ref": "#/definitions/document"
        }
      }
    }
  },
  "definitions": {
    "document": {
      "type": "object",
      "description": "A document: text associated with labels.",
      "properties": {
        "text": {
          "type": "string",
          "description": "The document's text."
        },
        "label_indices": {
          "type": "object",
          "description": "Label indices keyed based on their index name",
          "patternProperties": {
            ".*": {
              "$ref": "#/definitions/label_index"
            }
          }
        }
      }
    },
    "label": {
      "type": "object",
      "properties": {
        "start_index": {
          "type": "number",
          "description": "The start index of any covered text."
        },
        "end_index": {
          "type": "number",
          "description": "The end index of any covered text."
        }
      },
      "patternProperties": {
        ".*": {
          "type": [
            "string",
            "boolean",
            "number",
            "null",
            "array",
            "object"
          ]
        }
      }
    },
    "label_index": {
      "type": "object",
      "description": "A label index.",
      "properties": {
        "distinct": {
          "type": "boolean",
          "description": "Whether the labels are distinct/non-overlapping."
        },
        "json_labels": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/label"
          }
        }
      }
    }
  }
}
```

## Example:

```json

{
  "event_id": "c2d14372-7641-41dd-92ef-13e54978deed",
  "metadata": {
    "file_name": "abc.txt",
    "mrn": "12345678"
  },
  "documents": {
    "plaintext": {
      "text": "ADENOIDECTOMY\n\nPROCEDURE: The patient was brought into the operating room suite, anesthesia administered via endotracheal tube. Following this the patient was draped in standard fashion. The Crowe-Davis mouth gag was inserted in the oral cavity. The palate and tonsils were inspected, the palate was suspended with a red rubber catheter passed through the right nostril. Following this, the mirror was used to visualize the adenoid pad and an adenoid curet was seated against the vomer. The adenoid pad was removed without difficulty. The nasopharynx was packed. Following this, the nasopharynx was unpacked, several discrete bleeding sites were gently coagulated with electrocautery and the nasopharynx and oral cavity were irrigated. The Crowe-Davis was released.\n\nThe patient tolerated the procedure without difficulty and was in stable condition on transfer to recovery. ",
      "label_indices": {
        "sentences": {
          "json_labels": [
            {
              "start_index": 0,
              "end_index": 13
            },
            {
              "start_index": 15,
              "end_index": 25
            },
            {
              "start_index": 26,
              "end_index": 127
            },
            {
              "start_index": 128,
              "end_index": 186
            },
            {
              "start_index": 187,
              "end_index": 245
            },
            {
              "start_index": 246,
              "end_index": 370
            }
          ],
          "distinct": true
        }
      }
    }
  }
}
```
