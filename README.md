# Document Anonymiser Project Plan: Hybrid Workflow

This document outlines the core steps and dependencies required to build a robust document anonymiser that can handle both native (text-copyable) and scanned (image-only) PDF files.

---

## Project Directory Structure

```bash
doc_anonymiser/
├── setup.py
├── requirement.txt
├── README.md
├── anonymizer.py # The main application logic
└── redacted_output/ # Output folder for final PDFs and temp images
```


### Dependencies

To support the conditional hybrid workflow (using PyMuPDF for native PDFs and OCR for scanned PDFs), installing the following Python libraries in `doc_anonymiser` Conda environment:

### PDF and Image Handling

```bash
pip install PyMuPDF         # For native PDF text/coordinate extraction and final PDF redaction
pip install pdf2image       # For converting scanned PDF pages to images
pip install pillow          # Python Imaging Library dependency (required by pdf2image)

### OCR and reflection
pip install pytesseract     # For OCR and coordinate extraction on scanned images
pip install opencv-python   # For drawing redaction boxes on scanned images

### LLM Interaction
pip install google-genai    # For communication with the Gemini API

# System Requirements
The hybrid workflow requires the following external command-line utilities:
- Poppler — required by pdf2image to convert PDF pages to images
- Tesseract-OCR — required by pytesseract to perform OCR with bounding boxes

```

### Core Application Workflow

| Step | Component(s) | Function  |
| ------- | ------- | -------- |
| **1. Load & Check Type**  | PyMuPDF | Loads the PDF and checks if word-level text extraction is possible.   |
| **2. Coordinate & Text Extraction** | **IF Native:** PyMuPDF<br>**IF Scanned:** pdf2image, pytesseract | Native: Extracts word-level text + bounding boxes. Scanned: Converts PDF→image and performs OCR to get text + coordinates. |
| **3. LLM Analysis**   | google-genai (Gemini API)  | Sends extracted text to LLM with System Instruction + JSON Schema to identify PII.  |
| **4. Redaction** | **IF Native:** PyMuPDF<br>**IF Scanned:** opencv-python  | Native: Applies redaction boxes directly to PDF. Scanned: Draws black rectangles on images.  |
| **5. Save Output**  | PyMuPDF, PIL | Saves final redacted PDF (native or reassembled from images). |




This document outlines the core steps and dependencies required to build the document anonymiser using LLM and OpenCV