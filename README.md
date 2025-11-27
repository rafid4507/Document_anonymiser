# Document Anonymiser Project Plan

This document outlines the core steps and dependencies required to build the document anonymiser using LLM and OpenCV.

# Dependencies
Before running the application, install the following core Python libraries in doc_anonymiser Conda environment:

### Install core libraries
pip install opencv-python pillow

### Install PDF handling (requires Poppler installed on your system)
pip install pdf2image

### Install Tesseract wrapper (requires Tesseract-OCR installed on your system)
pip install pytesseract

### Install Google GenAI SDK for LLM interaction
pip install google-genai


Note: pdf2image requires the external utility Poppler, and pytesseract requires the external utility Tesseract-OCR. These usually need to be installed separately based on your operating system (Windows, macOS, or Linux).

# Core Application Workflow


| Step | Component(s) | Function |
|------|--------------|----------|
| 1 | Load & Convert<br>pdf2image, PIL | Loads the input file. If PDF, converts pages to images. |
| 2 | OCR Extraction<br>pytesseract | Extracts text and, crucially, the bounding box coordinates for every word on the image. |
| 3 | LLM Analysis<br>google-genai (Gemini API) | Sends the extracted text to the LLM with a System Instruction to identify PII. The LLM must return a structured JSON list of the PII words/phrases. |
| 4 | Redaction<br>opencv-python | Uses the LLM's PII list to find the corresponding bounding boxes (from Step 2) and draws solid black rectangles over those areas. |
| 5 | Save Output<br>PIL, pdf2image | Saves the redacted image or converts the set of redacted images back into a new PDF. |


# Next Steps
1. Set up the Conda environment (as described above).
2. Install the required Python dependencies.
3. Write the core logic following the structure in anonymizer.py.
4. Implement the function that handles the API call and forces structured JSON output.