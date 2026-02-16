# Google-Vision-OCR

This project is a Python CLI tool that performs batch OCR on a folder of screenshots/images using the Google Cloud Vision API (DOCUMENT_TEXT_DETECTION) and produces two outputs: 

1. **RAW JSON:** Verbatim OCR text with per-paragraph confidence, explicit error/warning reporting, and block-level geometry only (bounding boxes) to preserve layout context without inferring table structure or diagram semantics. 

2. **INDEXED JSON:** A smaller retrieval-optimised file containing globally numbered pages, chunked verbatim text (headings, paragraphs, lists), deterministic chunk IDs, and a lightweight retrieval index (topics, terms, title-case phrases) that references the raw file by filename + SHA256.

The design prioritises traceability, zero paraphrasing, and safe retrieval across large document collections.
