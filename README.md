# Survey Image to Excel Processor

Convert paper survey images (photos/scans) into structured Excel workbooks using Azure AI Content Understanding and GPT-4o Vision.

## 🎯 Problem Solved

Paper surveys are great for broad distribution at events, but extracting data from hundreds of handwritten responses is tedious. This project demonstrates a pattern for automating survey data extraction while maintaining human oversight for quality.

## 🔧 How It Works

**Hybrid AI Approach:**

1. **Azure AI Content Understanding** - Custom-trained analyzer extracts form structure (checkboxes, text fields)
2. **GPT-4o Vision** - Validates extraction and flags low-confidence items (especially circled responses)
3. **Excel Output** - Structured workbook with confidence scores and review flags

## ✨ Key Features

- **Form-Specific Training** - Train custom Content Understanding analyzers for your survey layout
- **Checkbox Detection** - Handles both checkmarks and circles
- **Confidence Scoring** - Flags low-confidence fields for human review
- **Batch Processing** - Process hundreds of survey images automatically
- **Excel Output** - Formatted workbooks with color-coded flagged items

## 🚀 Quick Start

### Prerequisites

- Python 3.8+
- Azure AI Content Understanding resource
- Azure OpenAI resource (GPT-4o with vision)

### 1. Install Dependencies

```bash
pip install -r requirements.txt
```

### 2. Configure Environment

Create `.env` file:

```env
# Azure AI Content Understanding
CONTENT_UNDERSTANDING_ENDPOINT=https://your-resource.cognitiveservices.azure.com
CONTENT_UNDERSTANDING_KEY=your_key_here

# Azure OpenAI (for GPT-4o Vision validation)
AZURE_OPENAI_ENDPOINT=https://your-resource.cognitiveservices.azure.com/
AZURE_OPENAI_KEY=your_key_here
AZURE_OPENAI_DEPLOYMENT=gpt-4o
AZURE_OPENAI_API_VERSION=2024-05-01-preview
```

### 3. Train Custom Analyzer

Create a Content Understanding analyzer for your survey:

1. Go to Azure AI Foundry portal
2. Create new Content Understanding analyzer
3. Define schema (see `survey_schemas/` for examples)
4. Test with sample survey images

See detailed training guide: `TRAINING_GUIDE.md`

### 4. Process Surveys

```bash
# Process a folder of survey images
python process_surveys.py --input surveys_sample/ --analyzer your-analyzer-id --output results.xlsx
```

## 📁 Project Structure

```
survey-image-to-excel/
├── process_surveys.py              # Main processing script (hybrid approach)
├── survey_schemas/                 # Example Content Understanding schemas
│   ├── example_survey.json         # Sample schema definition
│   └── ...
├── surveys_sample/                 # Place your survey images here
├── requirements.txt
└── README.md
```

## 🤖 AI Pipeline

### Step 1: Content Understanding Extraction
```
Survey Image → Content Understanding Analyzer → Structured Fields
- Checkboxes detected as selected/unselected
- Text fields extracted via OCR
- Confidence scores for each field
```

### Step 2: GPT-4o Vision Validation
```
Low-confidence fields → GPT-4o Vision → Validation + Flags
- Re-check circled responses
- Validate ambiguous checkboxes
- Flag items needing human review
```

### Step 3: Excel Generation
```
Validated Data → Excel Workbook
- Color-coded flags (yellow = needs review)
- Confidence scores in metadata columns
- Direct links to source images
```

## 📊 Schema Definition

Content Understanding analyzers use JSON schemas to define survey structure:

```json
{
  "description": "Example survey analyzer",
  "fieldSchema": {
    "fields": {
      "q1_option1": {
        "type": "selectionMark",
        "description": "First checkbox in Q1"
      },
      "q1_option2": {
        "type": "selectionMark",
        "description": "Second checkbox in Q1"
      },
      "respondent_name": {
        "type": "string",
        "method": "extract",
        "description": "Handwritten name field"
      }
    }
  }
}
```

See `survey_schemas/` for complete examples.

## 🎓 Use Cases

- **Event Surveys** - Conference attendee feedback
- **Customer Feedback** - In-person survey collection
- **Research Studies** - Paper-based data collection
- **Assessments** - Form-based evaluations

## 🔐 Security & Privacy

- Never commit actual survey images (`.gitignore` configured)
- All data processing happens in your Azure resources
- No survey data stored in this repository
- Excel outputs should be treated as sensitive customer data

## 📝 Customization

### For Your Survey:

1. **Create schema** - Define your survey structure in `survey_schemas/`
2. **Train analyzer** - Upload 5-10 sample surveys to Content Understanding
3. **Configure thresholds** - Adjust confidence thresholds in processing script
4. **Customize Excel output** - Modify column names and formatting

## 🛠️ Development

### Testing with Sample Data

```bash
# Test with a single image
python process_surveys.py --input surveys_sample/sample1.jpg --analyzer your-analyzer-id
```

### Adjusting Confidence Thresholds

Edit `process_surveys.py`:

```python
# Lower threshold = more items flagged for review
CONFIDENCE_THRESHOLD = 0.70  # Default: 70%
```

## 📖 Documentation

- `TRAINING_GUIDE.md` - Step-by-step Content Understanding analyzer training
- `SCHEMA_REFERENCE.md` - Schema definition reference
- `survey_schemas/` - Example schemas for different survey types

## 🤝 Contributing

This is a template project demonstrating the pattern. Adapt it for your specific survey needs:

1. Define your survey schema
2. Train your Content Understanding analyzer
3. Customize the processing pipeline
4. Adjust confidence thresholds for your use case

## 📄 License

MIT License - Feel free to adapt for your own survey processing needs

---

## 💡 Pattern Summary

**The Core Pattern:**
- **Structured extraction** (Content Understanding) handles consistent form layout
- **Vision validation** (GPT-4o) catches edge cases (circles, ambiguous marks)
- **Human review** (flagged items) maintains quality without full manual transcription

**Result:** Reduces manual work from transcribing everything → reviewing only flagged items.

**Typical Accuracy:**
- Checkmarks: 95%+ detection
- Circles: 70-80% detection (flagged for review)
- Handwritten text: 80-90% accuracy (flagged if < 70% confidence)
