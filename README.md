# Satsuma

**An offline, open-source file converter and toolbox for Windows 10/11** (also runs on Linux/macOS).
Drop files in, pick what you want, get the result next to the original. **Nothing is uploaded anywhere.**

[العربية ↓](#الدليل-بالعربية)

## What it does

| Area | Features |
|---|---|
| **Images** | JPG, PNG, WebP, AVIF, TIFF, BMP, GIF (animated too), ICO, PDF; HEIC/RAW/PSD with optional packages; resize; remove background; enlarge; **exact size preview** |
| **Make it smaller** | "At most 2 MB" for images (quality search, then shrinking), **PDF** (recompress images, last resort: rasterise), **video** (two-pass at a computed bitrate, auto-lower resolution), **audio** |
| **PDF** | to Word / text / images; merge, split, extract & reorder pages, rotate, remove password; images → one PDF |
| **Word / Office / e-books** | DOCX, ODT, RTF, XLSX, ODS, CSV, PPTX, HTML, SVG, Markdown, EPUB, MOBI via LibreOffice / Pandoc / Calibre |
| **OCR** | Images and scanned PDFs → text, Word (right-to-left aware) or **searchable PDF**; Arabic, English and ~100 languages (Tesseract) |
| **Audio / video** | MP3, M4A, WAV, FLAC, OGG, OPUS, WMA, MP4, MKV, WebM, MOV, AVI, GIF (FFmpeg); speech → text/SRT/VTT (optional) |
| **Privacy** | Remove EXIF/GPS, PDF author/XMP, Office authors, media tags — with a **report of what was found** |
| **Workflow** | Presets (WhatsApp photo, "upload ≤ 1 MB", scan → searchable PDF…), custom presets, **watch a folder**, batch, **history + safe undo**, right-click menu, command line |
| **Other** | Archives (zip, tar.gz, 7z; safe extraction), fonts (TTF/OTF ↔ WOFF/WOFF2), 3D models (experimental) |

Safety rules that are enforced for every operation (and covered by tests): the original is **never modified**; an existing
file is **never overwritten** (`a.png`, `a (1).png`…); a failed or cancelled job **leaves no partial files**; undo only
removes files Satsuma itself created and skips any you edited afterwards; only `satsuma/net.py` may touch the network and it
is used **only** to download OCR language data when you ask for it.

## Install

### Windows (installer / portable)
Download `Satsuma-Setup-x.y.z.exe` from the CI artifacts or Releases (or build it: `packaging\build_windows.ps1`).
Then add the helper programs you need — one-time, free, and Satsuma tells you which are missing (**Components** tab or `satsuma doctor`):

```powershell
winget install Gyan.FFmpeg                       # audio / video
winget install UB-Mannheim.TesseractOCR          # OCR
winget install TheDocumentFoundation.LibreOffice # Word/Excel/PowerPoint conversion
winget install JohnMacFarlane.Pandoc             # Markdown / EPUB
winget install 7zip.7zip                         # RAR
```
Prefer not to install? Put portable copies in the `tools` folder next to the program (see `tools/README.md`).

### From source
```
python -m pip install -r requirements.txt        # Python 3.10+
python -m satsuma gui                            # window
python -m satsuma --help                         # command line
python -m pip install -r requirements-optional.txt   # optional extras (HEIC, RAW, AI ...)
```

## Using it

**Window:** drop files → choose *what to do* (or a preset) → *Start*. Results appear in the table with **Show in folder**, **Undo**
and **Privacy report**. The language switch (English / العربية) flips the whole layout right-to-left.

**Command line** (PowerShell/cmd/Task Scheduler friendly, exit code 0 = all OK):
```
satsuma convert report.pdf --to docx
satsuma compress scan.pdf --size 2MB
satsuma convert photo.heic --to jpg --max-side 1600 --strip
satsuma ocr invoice.pdf --to pdf --lang ara+eng --download-langs
satsuma pdf merge a.pdf b.pdf          satsuma pdf extract book.pdf --pages 3-5,1
satsuma clean *.jpg                    satsuma preset upload_1mb form.pdf
satsuma watch C:\Users\me\Downloads --preset whatsapp_photo
satsuma formats file.docx              satsuma doctor
satsuma history / report 12 / undo 12  satsuma shell register   # right-click menu
```
Windows 11 shows classic right-click entries under **Show more options** (Shift+F10).

## Quality, honestly

* **Tested here:** 100+ automated tests on Linux (Python 3.12, FFmpeg 6.1, Tesseract 5.3 *English data only*, LibreOffice 24.2,
  Pandoc 3.1) pass, including Arabic file names, size targets, cancel/rollback, zip-slip, undo and a "no network" test.
* **Not run by the author:** Windows itself, the real Qt window, the PyInstaller/Inno build, Arabic OCR and real-world Arabic
  PDFs, HEIC/RAW/PSD, rembg/whisper, the `winget` IDs, the OCR-data download over the internet, `pdf2docx`.
  The included GitHub Actions workflow runs the whole suite on **Windows (3.10 & 3.12)**, drives the real window headless,
  builds the `.exe`, smoke-tests it and builds the installer — **trust that result, not this README**.
* **Known limits:** PDF → Word is best-effort (text, headings, bullets, tables and pictures are rebuilt; multi-column or
  highly designed pages will not be pixel-perfect — use *layout* mode for the look, or `pdf2docx` for better results).
  PDFs with Arabic text often store letters in visual order, so *auto* mode re-reads them with OCR when Tesseract + Arabic data
  are present. Size limits need a lossy/shrinkable format (not BMP/WAV/FLAC) and are refused, not faked, when impossible.
  Excel → CSV exports the first sheet. Password-protected ZIPs with AES need 7-Zip. Colour management for CMYK is basic.
  Background removal without `rembg` is a basic fallback. Very large images are decoded in memory.

## Development
```
pip install -r requirements-dev.txt
python -m unittest discover -s tests -v
```
Layout: `satsuma/core.py` (actions, chains, safe finalising) · `satsuma/convert/*` (back-ends) · `satsuma/gui/app.py`
(window) · `satsuma/cli.py` · `tests/` · `packaging/` (PyInstaller + Inno Setup) · `.github/workflows/ci.yml`.
License: MIT (see `THIRD_PARTY_NOTICES.md` for the components it uses — note pdf2docx/PyMuPDF are AGPL/GPL if you bundle them).

---

# الدليل بالعربية

**ساتسوما**: برنامج مفتوح المصدر لتحويل الملفات ومعالجتها على **ويندوز 10 و11**، يعمل بالكامل على جهازك — **لا يرفع ملفاتك إلى أي موقع**.

**ماذا يفعل؟** تحويل الصور (JPG/PNG/WebP/AVIF/GIF/ICO…) والصوت والفيديو؛ **تصغير الملف إلى حجم محدد** (مثل: "أقل من 2 ميغابايت" للصور وPDF والفيديو والصوت)؛
**PDF إلى Word** ونص وصور؛ دمج وتقسيم واستخراج وتدوير صفحات PDF وإزالة كلمة سرّه؛ **التعرّف على النص العربي (OCR)** وإخراجه نصًا أو Word (باتجاه من اليمين لليسار) أو PDF قابلًا للبحث؛
إزالة البيانات الخفية (الموقع GPS، اسم المؤلف، الكاميرا) مع **تقرير بما وُجد**؛ إعدادات جاهزة (واتساب، نماذج التقديم…)، **مراقبة مجلد**، **سجل مع تراجع آمن**، قائمة النقر الأيمن، وسطر أوامر.

**التثبيت:** ثبّت البرنامج، ثم شغّل الأوامر أعلاه (`winget install …`) للمكوّنات التي تحتاجها (FFmpeg للفيديو والصوت، Tesseract للـOCR، LibreOffice لملفات Office).
تبويب **المكوّنات** يُظهر ما ينقص. لتحميل بيانات اللغة العربية للـOCR (مرة واحدة، تحتاج إنترنت): تبويب المكوّنات ← اللغة ← ⬇، أو `satsuma ocr-lang ara`.

**قواعد الأمان:** لا يعدّل الملف الأصلي أبدًا، ولا يستبدل ملفًا موجودًا، ولا يترك ملفات ناقصة عند الفشل أو الإلغاء، والتراجع يحذف فقط ما أنشأه البرنامج.

**بصراحة عن الجودة:** الاختبارات الآلية نجحت على لينكس. لم أشغّل البرنامج على ويندوز ولا النافذة الحقيقية ولا بناء الـ`.exe` ولا الـOCR العربي على ملفات حقيقية؛
ملف GitHub Actions المرفق يفعل ذلك كله على ويندوز — **اعتمد على نتيجته**. تحويل PDF إلى Word جهد أفضل وليس مطابقة تامة للتصميم المعقّد.
