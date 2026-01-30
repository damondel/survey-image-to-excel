# Sample Survey Images

Place your survey images (photos or scans) in this directory for processing.

## Supported Formats
- JPEG (.jpg, .jpeg)
- PNG (.png)
- PDF (.pdf)

## Best Practices

### Image Quality
- **Resolution**: Minimum 1500x2000 pixels for clear text recognition
- **Lighting**: Even lighting, avoid shadows
- **Focus**: Sharp focus on text and checkboxes
- **Orientation**: Upright (not rotated)

### File Naming
Use consistent naming for easier tracking:
```
survey_001.jpg
survey_002.jpg
survey_003.jpg
```

Or with event/date context:
```
conference_2025_001.jpg
conference_2025_002.jpg
```

## Privacy Note

**Never commit actual survey images to version control.**

This directory is excluded via `.gitignore` to prevent accidental commits of customer data. Survey images should be:
- Stored locally only
- Processed and then archived securely
- Treated as sensitive customer data

## Processing

Once you have survey images in this directory:

```bash
python process_surveys.py --input surveys_sample/ --analyzer your-analyzer-id --output results.xlsx
```

Results will include:
- Extracted data in Excel format
- Confidence scores for each field
- Flags for items needing human review
