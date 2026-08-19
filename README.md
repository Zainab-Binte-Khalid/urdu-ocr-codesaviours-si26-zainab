# Urdu OCR — A Fine-Tuned Model That Reads Urdu Text From Images

Optical Character Recognition (OCR) for Urdu, built by fine-tuning Microsoft's TrOCR model on a custom-collected dataset of real and synthetic Urdu images.

## Why This Matters

Urdu is written in a cursive, right-to-left script where letter shapes change depending on their position in a word — this makes it far harder for computers to read than English. We tested this ourselves: a standard OCR tool (Tesseract) reads clean printed Urdu fine, but completely fails on real photos of signboards and badly garbles handwriting. Millions of Urdu documents, newspapers, and signs exist only as images with no searchable text. A working Urdu OCR tool helps digitize newspaper archives, process government and business forms automatically, and read signage for accessibility.

## Live Demo

**[Try it here](https://urdu-ocr-codesaviours-si26-zainab-mkk3ezqncmxhuol7nw84dk.streamlit.app)**


Upload an image containing Urdu text and the model will extract and display the text.

<img width="1600" height="786" alt="image" src="https://github.com/user-attachments/assets/00793d72-18b8-4afa-bd1e-93c038e528b1" />

> Note: HuggingFace Spaces currently requires a paid plan for Gradio/Docker-based apps, so this project is deployed on **Streamlit Community Cloud** instead — fully live and public, no login required.

**Demo video:** [Add your Drive demo video link here]

## How It Works

This project uses TrOCR, an AI model that reads an image and turns it into text. The starting version only knew English, so we retrained it — a process called fine-tuning — on thousands of labeled Urdu images instead, teaching it to recognize Urdu letters and words. We collected images from six different sources (printed text, handwriting, newspapers, signs, and more) so the model would see real-world variety, not just clean text. After training, we tested it on images it had never seen before to measure how well it actually learned.

## Results

- **Exact-match accuracy:** 24.3% (142/585 test images)
- **Character Error Rate (CER):** 16.4%

| Category | Exact-match | CER | Samples |
|---|---|---|---|
| Natural scene | 49.5% | 20.5% | 200 |
| Clean printed | 10.3% | 9.5% | 300 |
| Handwritten | 19.6% | 23.6% | 56 |
| Newspaper | 0.0% | 76.1% | 8 |
| Synthetic | 7.1% | 52.2% | 14 |
| Signboard | 0.0% | 160.4% | 7 |

**Training loss** dropped from 17.68 to 0.06 over 10 epochs, confirming the model was genuinely learning throughout training.

**Why accuracy is modest:** exact-match is a strict metric — one wrong character fails the whole prediction — so CER (16.4%) is a fairer picture of real performance. The dataset (2,922 images) is also small for teaching a model an entirely new script and reading direction from scratch; production OCR systems typically train on far more data. Performance varies sharply by category — strong on natural scene text, weak on newspaper and signboard images, which had the fewest training examples (under 45 each).

**With more time, we would:** collect substantially more real newspaper and signboard images (the clearest bottleneck), and try `trocr-base-handwritten` as a starting point to specifically improve handwritten text.

## How to Run It Locally

```bash
git clone https://github.com/Zainab-Binte-Khalid/urdu-ocr-codesaviours-si26-zainab.git
cd urdu-ocr-codesaviours-si26-zainab
pip install -r requirements.txt
streamlit run app.py
```

Then open the local URL shown in your terminal (usually `http://localhost:8501`). The model downloads automatically from HuggingFace Hub the first time you run it — no manual download needed.

## Dataset

- **Total images:** 2,922, across 6 categories (natural scene, clean printed, handwritten, newspaper, synthetic, signboard)
- **Split:** 2,337 training / 585 test
- Collected from real photographs, newspaper clippings, handwriting samples, and synthetically generated text — chosen to capture real-world variety in fonts, backgrounds, and image quality

---

<details>
<summary><b>Appendix: Week 1-2 Research (Tesseract Gap Analysis)</b></summary>

### What is OCR?

Optical Character Recognition (OCR) converts text from images or scanned documents into editable, searchable digital text.

### Why is Urdu OCR harder than English OCR?

Urdu uses a connected, cursive script where letter shapes change based on position in a word, plus dots and marks that are easy to misread — making it significantly harder than Latin-script OCR.

### Tesseract Gap Analysis Summary

We tested Tesseract (a standard OCR tool) on 5 sample images:
- **Synthetic, clean text:** perfect match (100% accuracy)
- **Newspaper paragraph:** structurally correct but riddled with character-level dot/nuqta confusion; dropped a number entirely
- **Handwritten text:** severely garbled, word boundaries lost
- **Signboards (2 samples):** complete failure — zero output on both, even after cropping and contrast enhancement

This confirmed Tesseract cannot generalize beyond clean, standardized printed text — exactly the gap this fine-tuned model was built to close.

</details>

---

**Built by:** Zainab Binte Khalid | Code Saviours SI-26 | 2026
