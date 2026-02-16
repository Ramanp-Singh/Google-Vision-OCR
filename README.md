# Google Vision OCR → Raw + Indexed JSON

A Python CLI tool that performs batch OCR on a folder of images using the Google Cloud Vision API and produces:

1. **RAW JSON:** Verbatim OCR text with per-paragraph confidence, explicit error/warning reporting, and block-level geometry only (bounding boxes) to preserve layout context without inferring table structure or diagram semantics. 

2. **INDEXED JSON:** A smaller retrieval-optimised file containing globally numbered pages, chunked verbatim text (headings, paragraphs, lists), deterministic chunk IDs, and a lightweight retrieval index (topics, terms, title-case phrases) that references the raw file by filename + SHA256.

The tool is designed for traceability, zero paraphrasing, and audit-safe text extraction from screenshots or document images.


<br> 

# Overview

This script processes a folder of images in a single batch request to the Google Vision API using DOCUMENT_TEXT_DETECTION.

It then performs a second stage of processing to create a structured, retrieval-ready index without modifying or paraphrasing the original OCR text.

The architecture separates:

- Stage 1: OCR extraction → RAW JSON 

- Stage 2: Chunking and indexing → INDEXED JSON 

This ensures:

- Full traceability to original OCR output

- Lightweight indexed files for fast retrieval

- Clean separation between raw data and search structures






# Features
### OCR processing

- Batch OCR of multiple images in one API request
- Verbatim text extraction (no paraphrasing or correction)
- Confidence scoring for each text segment
- Explicit reporting of:
- API errors
- Unreadable files
- Empty pages
- Low-confidence segments




### Geometry preservation
- Stores block-level bounding boxes
- Detects:
  - Tables
  - Diagrams (picture blocks)
- Does not infer table cells or diagram relationships

### Retrieval-optimised indexing
- Global page numbering across all images
- Chunked text (headings, lists, paragraphs)
- Deterministic chunk IDs
- Lightweight retrieval index:
  - Topics (headings)
  - Terms (tokenised keywords)
  - Title-case phrases

### Traceability
- Raw and indexed files are linked via:
  - Raw filename
  - SHA256 hash
- No loss of original OCR data


# Usage: 

**Example:**
python code_file_name.py --topic-id "topic_name" --image-folder "directory_path_of_images" --output-raw-json "directory_path_for_raw_output_file" ----output-indexed-json "directory_path_for_indexed_output_file" 



### Input batching

- The script was tested with batches of ~20 images per run.
- This helps:
  - Keep API requests manageable
  - Improve stability
  - Simplify output review


### Dynamic input and output paths
- Input and output paths are not hard-coded.
- Paths must be supplied each time the script runs.
- This allows:
  - Flexible file naming
  - Different output folders per run
  - Easy automation in pipelines

### Google credentials
- A valid Google service account JSON key is required.
- Must be configured before running the script.

### Performance considerations
- Processing time depends on:
- Number of images
- Image resolution
- Network latency
- API quotas

### API costs
- Google Vision API is a paid service.
- Charges apply per image processed.
- Check Google Cloud pricing before large runs.

