Urdu OCR Project | Code Saviours SI-26 | Zainab Binte Khalid
# Urdu OCR — A fine-tuned TrOCR model for extracting text from Urdu images

## What problem this solves and why it matters

Optical Character Recognition (OCR) for Urdu is significantly harder than for Latin-script
languages. Urdu is written in a cursive, right-to-left script where letter shapes change
depending on their position in a word, and dots/marks (nuqta) are easy to misread. Our
own gap analysis (see Week 2 Research below) confirmed this: Tesseract, a standard OCR
tool, performs perfectly on clean synthetic text but completely fails (0% output) on
real-world photographed signboards, and severely garbles handwritten Urdu.

This project fine-tunes a pretrained OCR model specifically on Urdu images so it can
reliably read text from photos of newspapers, signboards, handwritten notes, and printed
documents — closing the gap that tools like Tesseract can't. Real-world use case:
digitizing Urdu newspaper archives, or reading Urdu text from government forms and
signage for accessibility and record-keeping.

## How it works

This project uses **TrOCR**, a model built by Microsoft that combines two parts:
a **vision encoder** (reads the image) and a **text decoder** (outputs the characters).

The starting model (`microsoft/trocr-base-printed`) was originally trained to read
**English** printed text. Through **fine-tuning**, we continued training this model on
thousands of labeled Urdu images instead, so it learned to recognize Urdu characters
and words. Our dataset had 2,922 labeled Urdu images across six categories (see Dataset
section below). We trained for 10 epochs, saving a checkpoint after each one, and
evaluated performance on a held-out test set the model never saw during training.

## Live demo

Try it here: [Urdu OCR — Live Demo](https://urdu-ocr-codesaviours-si26-zainab-mkk3ezqncmxhuol7nw84dk.streamlit.app)

Upload an image containing Urdu text and the model will extract and display the text.

<img width="1600" height="786" alt="image" src="https://github.com/user-attachments/assets/00793d72-18b8-4afa-bd1e-93c038e528b1" />

## How to run it locally

1. Clone this repository:
```bash
   git clone https://github.com/Zainab-Binte-Khalid/urdu-ocr-codesaviours-si26-zainab.git
   cd urdu-ocr-codesaviours-si26-zainab
```
2. Install dependencies:
```bash
   pip install -r requirements.txt
```
3. Run the app:
```bash
   streamlit run app.py
```
4. Open the local URL shown in your terminal (usually `http://localhost:8501`).

The model is hosted on Hugging Face Hub and downloads automatically the first time the
app runs — no manual model download needed.

## Dataset details

- **Total images:** 2,922
- **Categories:** natural scene text, clean printed text, handwritten text, newspaper
  headlines, synthetic text, and signboards
- **Split:**  2,337 training / 585 test
- **Sources:** newspaper clippings, handwritten samples, synthetically generated Urdu
  text, and photographed signboards — collected to capture variety in fonts, backgrounds,
  and image quality (clean scans to real-world photos)

## Results

After 10 epochs of fine-tuning:

- **Exact-match accuracy:** 24.3% (142/585 test samples)
- **Character Error Rate (CER):** 16.4%

| Category | Exact-match | CER | Samples |
|---|---|---|---|
| Natural scene | 49.5% | 20.5% | 200 |
| Clean printed | 10.3% | 9.5% | 300 |
| Handwritten | 19.6% | 23.6% | 56 |
| Newspaper | 0.0% | 76.1% | 8 |
| Synthetic | 7.1% | 52.2% | 14 |
| Signboard | 0.0% | 160.4% | 7 |

**Training progress:**

| Epoch | Exact-match | CER |
|---|---|---|
| 1 | 0.9% | 79.2% |
| 5 | 11.3% | 30.3% |
| 10 | 24.3% | 16.4% |

Accuracy improved steadily across all 10 epochs, with most gains in the first 6-7 epochs
before flattening — suggesting the model was close to convergence by the end.

  **Why accuracy is relatively low:**
24.3% exact-match is a modest score, and a few factors explain this. First, exact-match
is a strict metric — the model's Urdu output must match the reference text character-for
-character to count as correct, so even a single misplaced dot or extra space counts as
a full miss. Character Error Rate (16.4%) gives a fairer picture, since it measures how
close the output is rather than requiring a perfect match. Second, our dataset (2,922
images) is small for fine-tuning a model on a completely different script and script
direction (right-to-left cursive Urdu vs. the model's original left-to-right English
training) — deep learning models generally need thousands more examples per category to
generalize well. Third, performance varies sharply by category (49.5% on natural scene
text vs. 0% on signboards), showing the model learned some patterns well while others
(irregular fonts, real-world photo conditions) remain genuinely hard with this amount of
training data.

**What we'd do differently with more time:**
- `newspaper`, `synthetic`, and `signboard` categories have very few test samples (7-14
  each), making those scores noisy — more data in these categories is needed before
  trusting the numbers.
- High CER on `signboard` (160%) suggests the model struggles with irregular fonts/layouts
  on signs, consistent with our Week 2 finding that Tesseract failed completely here too
  — targeted augmentation for this category could help.
- Trying `trocr-base-handwritten` as the starting checkpoint (instead of `-printed`) could
  improve handwritten-category performance specifically.
## Credit

Built by Zainab Binte Khalid during the Code Saviours ML/AI Internship — Batch SI-26.

---
## Appendix: Week 1-2 Research
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
