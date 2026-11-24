# sat: a Tesseract model for Santali (Ol Chiki) documents

`sat.traineddata` is a Tesseract LSTM model for Santali text in the Ol Chiki script. It is the first LSTM model for Ol Chiki.

## Training

The model is fine-tuned from `Latin.traineddata` (tessdata_best) in one run of 30,000 iterations. Ol Chiki is an alphabet, so Latin is a closer starting point than an abugida such as Odia.

It trained on 700 line images:

| Type | Lines |
|------|-------|
| Book scans: 9 pages from 5 publications | 221 |
| The Santali Wikisource article on Pandit Raghunath Murmu, printed and scanned on a flatbed: 5 pages | 139 |
| Synthetic: Noto Sans Ol Chiki (Regular, Bold) and Guru Gomke (Regular, Italic) | 240 |
| Synthetic with blur, noise, contrast changes and slight rotation | 100 |

The book pages are from *ᱜᱟᱱᱟᱝ ᱟᱨ ᱪᱤᱴᱷᱤ ᱥᱟᱠᱟᱢ (Gānāṁ ār ciṭhi sākām)* (2008) by Hajam Baskey, *Guru Gomke* (2004), *Ajay Gada* (2022), *Rar Sanes* and *Sahayinon* (2021). The ground truth was transcribed by hand.

The training repository has the corpus, fonts, line images, scripts and test set, and rebuilds this model from a clean clone: https://github.com/ofdn/tesseract-training-santali

## Results

The test set is 142 lines from 5 pages that weren't trained on. These are one page each of *Gānāṁ ār ciṭhi sākām*, *Guru Gomke*, *Ajay Gada* and *Rar Sanes*, and page 6 of the Raghunath Murmu article. No synthetic line shares a run of four words with them. Each line was read with `--psm 13`. The legacy `sat` model from [Indic OCR](https://github.com/indic-ocr/tessdata) by RKVS Raman is shown for reference.

| Model | CER | WER | Spaces found | CER without spaces |
|-------|-----|-----|--------------|--------------------|
| sat (this model) | 13.8% | 32.6% | 98.9% | 15.7% |
| Indic OCR (OEM 0) | 28.8% | 99.9% | 0.0% | 16.2% |

The earlier version of this model wrote no spaces and was trained on these pages, so it isn't compared here.

## Usage

Copy `best/sat.traineddata` into your tessdata directory. Then run:

```bash
tesseract scan.tif output -l sat --psm 6
```

## Credits

Subhashish Panigrahi trained the model. Ramjit Tudu guided the choice of Santali Wikisource books and shared background on earlier Santali OCR work. Prasanta Hembram tested interim models and advised during training.

Text sources, all CC BY-SA 4.0:

- [Santali Wikisource](https://wikisource.org/wiki/Category:%E1%B1%A5%E1%B1%9F%E1%B1%B1%E1%B1%9B%E1%B1%9F%E1%B1%B2%E1%B1%A4) books
- Chapter 1 of [OpenSpeaks](https://en.wikiversity.org/wiki/OpenSpeaks/sat) by Subhashish Panigrahi, translated into Santali by R Ashwani Banjan Murmu, Fagu Baskey and Joy Sagar Murmu
- Santali Wikisource article on [Pandit Raghunath Murmu](https://w.wiki/D$AN)
- [Community Language Documentation & Archiving Training](https://w.wiki/ReCS), translated by Bodi Baski

Fonts: Noto Sans Ol Chiki and [Guru Gomke](https://github.com/GuruGomke) by Pooja Saxena, both under the SIL Open Font License.

## Licence

Apache 2.0, the same as Tesseract.
