# Third-party components

Satsuma's own code is MIT licensed. It uses the following components. Licences are quoted from memory
of each project's published terms - **check each project before redistributing a bundle**.

| Component | Used for | Licence (as published by the project) |
|---|---|---|
| Pillow | images | HPND (permissive) |
| pypdfium2 / PDFium | PDF rendering, text | Apache-2.0 / BSD-3 |
| pikepdf / qpdf | PDF editing | MPL-2.0 / Apache-2.0 |
| pdfplumber / pdfminer.six | PDF text layout | MIT |
| python-docx, lxml | Word files | MIT, BSD |
| fontTools | fonts | MIT |
| PySide6 (Qt) | window | LGPL-3.0 - keep Qt as dynamic libraries when you distribute |
| FFmpeg (external program) | audio / video | LGPL-2.1+ or GPL depending on the build you install |
| Tesseract + tessdata (external) | OCR | Apache-2.0 |
| LibreOffice (external) | Office conversion | MPL-2.0 |
| Pandoc (external) | Markdown / EPUB | GPL-2.0+ (run as a separate program) |
| 7-Zip (external) | RAR and more | LGPL + unRAR restriction |
| Calibre (external) | e-books | GPL-3.0 (run as a separate program) |

## Optional Python packages (not required, not bundled by default)

| Package | Licence note |
|---|---|
| pdf2docx (+ PyMuPDF) | GPL / AGPL. If you build and distribute a program that **includes** them, that program must follow the AGPL. Satsuma runs fine without them (it has its own PDF-to-Word converter). |
| rembg, onnxruntime, faster-whisper | permissive licences; the AI **models** they download have their own terms |
| rawpy (LibRaw), pillow-heif (libheif), psd-tools, py7zr, brotli, trimesh | see each project |

External programs are started as separate processes; Satsuma does not link against them.
