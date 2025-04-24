# json-cnl
Reduces token amount required for communication

# JSON-CNL
Quick standard for efficient AI communication, minimizing token usage.

---

## Overview
**JSON-CNL** (Controlled Natural Language for JSON) is a minimalist schema that:

- Eliminates filler and redundancies.
- Uses active voice and compact notation (ISO dates, >=, etc.).
- Applies fixed labels per section.
- Ensures automatic parsing and validation.
- Saves up to 30% of tokens versus pretty JSON.

Ideal for sharing context between AI sessions securely and quickly.

---

## Structure
```json
{
  "title": "<Short text>",           // summary <=15 words
  "summary": "<Concise phrase>",        
  "content": [                       
    "[SECTION]: compact item; another",
    "..."
  ],
  "metadata": {                      
    "output_format": "JSON-CNL",
    "version": "<x.y>",
    "creation_date": "YYYY-MM-DD",
    "importance_level": "high|medium|low"
  }
}
```

### Premises (rules)
1. **[REMOVE]**: remove filler and redundancies.
2. **[ACTIVE_VOICE]**: use simple sentences.
3. **[ACRONYMS]**: define then reuse.
4. **[LABELS]**: prefix sections with `[SECTION]:`.
5. **[LISTS]**: bullets max 20 words.
6. **[SYMBOLS]**: use `>=`, ISO dates, abbreviated units.
7. **[GROUP]**: merge related items with `;` or `,`.
8. **[LANGUAGE]**: limited CNL vocabulary.
9. **[DATA]**: use concise numbers and dates.
10. **[PLACEHOLDERS]**: use variables for repetition.
11. **[NO_REPEAT]**: avoid contextual redundancy.
12. **[LENGTH]**: summary <=15 words, section <=50 tokens.
13. **[VALIDATE]**: enforce schema before use.

---

## JSON Schema
Save as `json-cnl.schema.json`:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["title","summary","content","metadata"],
  "properties": {
    "title": {"type":"string","maxLength":50},
    "summary": {"type":"string","maxLength":100},
    "content": {
      "type":"array","minItems":1,
      "items":{"type":"string","maxLength":200}
    },
    "metadata": {
      "type":"object","required":["output_format","version","creation_date","importance_level"],
      "properties":{
        "output_format":{"type":"string","enum":["JSON-CNL"]},
        "version":{"type":"string"},
        "creation_date":{"type":"string","format":"date"},
        "importance_level":{"type":"string","enum":["high","medium","low"]}
      }
    }
  }
}
```

---

## Usage
1. **Create** your JSON-CNL in an editor.
2. **Validate**: `ajv validate -s json-cnl.schema.json -d my.json`.
3. **Combine modules**:
   ```bash
   # Bash example: merge base + profile
   jq -s 'add' base.json profile.json > context.json
   ```
4. **Inject** into AI:
   ```
   Here is my context: <COPY PASTE context.json>
   ```

---

## Examples
- `examples/user_evaluation.json` — user evaluation.
- `examples/roadmap.json` — DVE roadmap.
- `examples/opsec.json` — NDA and OPSEC templates.

---

## Contributing
1. **Fork** this repo.
2. Add rules or examples.
3. Submit a pull request.

---

## License
MIT © 2025

