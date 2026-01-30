# Survey Schema Examples

This directory contains example Content Understanding analyzer schemas for different survey types.

## What are Schemas?

Schemas define the structure of your survey for Azure AI Content Understanding. They tell the analyzer:
- What fields to extract (checkboxes, text, ratings)
- Where to find them on the form
- What type of data each field contains

## Example Schema

See `example_survey_schema.json` for a complete example showing:
- **Selection marks** (checkboxes) - `"type": "selectionMark"`
- **Text extraction** (names, emails, open responses) - `"type": "string", "method": "extract"`
- **Generated fields** (ratings, categorizations) - `"type": "string", "method": "generate"`

## Creating Your Own Schema

1. **Analyze your survey**: List all questions and response types
2. **Map to field types**:
   - Checkboxes → `selectionMark`
   - Handwritten text → `string` with `method: "extract"`
   - Ratings/scales → `string` with `method: "generate"`
3. **Give descriptive names**: `q1_option1`, `respondent_name`, etc.
4. **Add descriptions**: Help the analyzer understand what to extract

## Field Types

### Selection Mark (Checkboxes)
```json
{
  "q1_option1": {
    "type": "selectionMark",
    "description": "First checkbox in Q1"
  }
}
```

### Text Extraction (Handwriting/OCR)
```json
{
  "respondent_name": {
    "type": "string",
    "method": "extract",
    "description": "Respondent's full name"
  }
}
```

### Generated Fields (AI-inferred)
```json
{
  "satisfaction_rating": {
    "type": "string",
    "method": "generate",
    "description": "Extract the rating (1-5) from the survey"
  }
}
```

## Using Your Schema

1. Upload schema to Azure AI Foundry Content Understanding
2. Train the analyzer with 5-10 sample survey images
3. Test and refine
4. Use the analyzer ID in `process_surveys.py`:

```bash
python process_surveys.py --input surveys_sample/ --analyzer your-analyzer-id
```

## Tips

- **Be specific in descriptions**: "First checkbox in Q1 (Strongly Agree)" is better than "Q1 option"
- **Use consistent naming**: `q1_option1`, `q1_option2`, etc. makes processing easier
- **Test with diverse samples**: Include different handwriting styles, marks (checks vs circles)
- **Iterate**: Add fields you missed, remove fields that don't work

## Resources

- [Content Understanding Documentation](https://learn.microsoft.com/azure/ai-services/content-understanding/)
- [Analyzer Schema Reference](https://learn.microsoft.com/azure/ai-services/content-understanding/schema-reference)
