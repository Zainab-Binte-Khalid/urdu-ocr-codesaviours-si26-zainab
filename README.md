Urdu OCR Project | Code Saviours SI-26 | Zainab Binte Khalid
# urdu-ocr-codesaviours-si26-zainab
# Week 1 Research Task

## 1. What is OCR (Optical Character Recognition)?

Optical Character Recognition (OCR) is a technology that converts text from images, scanned documents, or photographs into editable and searchable digital text. It helps computers recognize printed or handwritten characters without typing them manually. OCR is widely used to digitize books, forms, receipts, and historical documents.

## 2. Why is Urdu OCR harder than English OCR?

Urdu OCR is more difficult than English OCR because Urdu is written in a connected script, where the shape of letters changes depending on their position in a word. Urdu also contains many dots and special marks that can be difficult for a computer to identify accurately. Different fonts, handwriting styles, and image quality make Urdu OCR even more challenging.

## 3. What are two real-world situations where Urdu OCR would be useful?

One use of Urdu OCR is converting printed Urdu books, newspapers, and old documents into editable digital text for preservation and searching. Another use is reading information from government forms, educational documents, or business records written in Urdu so the information can be stored and processed electronically.





## Why We Need a Better Model

### Week 2 Gap Analysis — Tesseract OCR on Urdu Text

#### Image 1 — synthetic_01.png
**Actual text:** پاکستان زندہ باد
**Tesseract output:** پاکستان زندہ باد
**What went wrong:** Nothing — perfect match. Clean, computer-generated text with high contrast is the ideal case for Tesseract.

#### Image 2 — handwritten_01.png
**Actual text:** ہوٹل کا عملہ نہایت دوستانہ ہے عملہ نے
**Tesseract output:** ہو ٹل کا شف خہایے دوستام ہےے۔ علرے
**What went wrong:** Severely garbled. Tesseract broke single words into fragments (ہوٹل → "ہو ٹل"), substituted several letters with visually similar ones with different dot placement (عملہ → "شف", نہایت → "خہایے"), and lost the correct word order and boundaries entirely. This shows Tesseract cannot reliably parse cursive, connected handwriting strokes, even with darkened, well-cropped preprocessing.

#### Image 3 — newspaper_01.png
**Actual text:** وینزویلا میں پانچ روز قبل یکے بعد دیگرے آنے والے شدید زلزلوں میں مرنے والوں کی تعداد 1400 سے بڑھ گئی ہے... [full paragraph]
**Tesseract output:** ویئزویلا میں پانچ روز قیل یکے بعد دچگرے آئے والے شدید زلزلوں میں مرئے والوں کی تعداد 0 سے بڑھ گئی ہے... [similar structure, scattered errors]
**What went wrong:** Mostly readable and structurally intact, but consistently misreads dot placement on similar-shaped letters (قبل→قیل, دیگرے→دچگرے, مرنے→مرئے), and completely dropped the number "1400," outputting just "0" instead. Errors are localized to individual characters rather than whole-word failures, unlike the handwritten sample.

#### Image 4 — signboard_01.png (Faizabad Station sign)
**Actual text:** فیض آباد اسٹیشن Faizabad Station ELEVATOR
**Tesseract output:** (empty — no output produced)
**What went wrong:** Complete failure despite significant preprocessing improvements (cropped to remove background clutter, contrast-enhanced). This image mixes Urdu and English text with icons/pictograms on a real photographed metal sign — Tesseract could not extract any text at all, suggesting the mixed-script layout and non-standard sign formatting confuse its text detection entirely.

#### Image 5 — signboard_04.png (Village name sign)
**Actual text:** گاؤں کمالپورہ تحصیل و ضلع قصور
**Tesseract output:** (empty — no output produced)
**What went wrong:** Complete failure on a relatively simple, short phrase. Even after cropping out excess background, Tesseract could not recognize this real-world photographed signage at all — reinforcing that the issue isn't just image quality, but Tesseract's fundamental inability to generalize beyond clean, standard printed document layouts.

#### Summary

Tesseract fails on Urdu because it was primarily trained on clean, standardized printed text, and its accuracy collapses as image conditions move away from that ideal — regardless of preprocessing quality. It performs perfectly on synthetic, computer-rendered text (100% accuracy), reasonably on printed newspaper text (structurally correct but riddled with character-level dot/nuqta confusion), poorly on handwritten text (severely garbled, word boundaries lost), and completely fails (0% output) on real-world photographed signboards even after significant preprocessing improvements like background cropping and contrast enhancement. This shows Tesseract has no ability to adapt to cursive handwriting, mixed-script layouts, or real-world photography conditions — exactly the real-world use cases a genuinely useful Urdu OCR tool needs to handle, which is the gap this project aims to close.
