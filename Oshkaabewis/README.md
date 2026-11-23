# Oshkaabewis

Lexical items from the glossaries of *Oshkaabewis Native Journal* (ONJ), volumes 1–8, not already in `OPD/` or `HammerlyFieldwork/`. The name is the journal's.

Sixteen glossaries in twelve issues were read: John Nichols's glossaries to Maude Kegg's texts (vol. 1 no. 2) and to the stories in vol. 2 no. 1, vol. 3 no. 1 and vol. 4 no. 1; Anton Treuer's Main Glossary (vol. 2 no. 1 to vol. 8 no. 1, cumulative); and Rand Valentine's glossary in vol. 4 no. 1. Rights: "We have the rights to work with anything from the ONJ" (Chris Hammerly).

## Columns

Same as the other directories: Lemma, Stem, Paradigm, Class, Translation, Source.

- **Translation** is the glossary's gloss, from the latest issue that prints the entry (vol. 7 no. 2 and vol. 8 no. 1 are born-digital, so their text is exact). Page furniture that the extraction caught (running heads, the Bemidji State University footer) and notes (*Also: ...*, *Reduplicated form*) are cut; a few English typos in the print are corrected (each listed in the Explorer's `audit/glossaries/release-fixes.csv`).
- **Lemma** is the headword as printed. Proper names are capitalised, as in `OPD/` and as the texts print them (Nichols's vol. 1 no. 2 glossary prints place names in lower case). VAI+O verbs are cited with n, as in OPD (`agoon`, stem `agoo`).
- **Source** is the first issue and printed page where the entry appears, as `onj-v01n2-p158` (volume 1, number 2, page 158). Vol. 6 nos. 1–2 is a double issue, cited `onj-v06n1`.
- **Paradigm** and **Class** come from the glossary's code and stem, run through the paradigm maps in `resources/` the same way as OPD. Where the glossary leaves the class open (no stem printed, no plural printed), the class was tested against the ONJ texts with the FST; where the texts don't decide, the class is OPD's majority class for that shape.

## What is here and what is not

967 rows (779 verbs, 95 nouns, 81 proper names, 12 preverbs, preadverbs and prenouns; the 5 preverbs are also listed as preadverbs, as in `OPD/`). 198 rows are not here yet, among them every particle and adverb: they wait on Chris Hammerly's review (particle kinds guessed from the gloss, stem classes the texts don't settle, words the FST already analyses another way, and a few entries odd in the print). Proper names are kept; participles are recorded separately and not added as lemmas.

## Using it in the FST

Add the directory to `LEMMAS_DIR` when building OjibweMorph:

```
make all OJIBWE_LEXICON=<path to OjibweLexicon> LEMMAS_DIR=<path>/OPD,<path>/HammerlyFieldwork,<path>/Oshkaabewis
```

Without it, the FST is built exactly as before. With it, on the ONJ texts in the Ojibwe Language Explorer, 1,028 more tokens are analysed (7,710 unanalysed tokens drop to 6,682), and 804 of the 3,722 words on the Explorer's list of words the FST misses are analysed.

## How it was made

Extraction, checks and scripts are in the Ojibwe Language Explorer repository, `audit/glossaries/` (start with its `README.md`; the report on the new items is `NEW_ITEMS.md`). Headwords were accepted when the PDF's text layer and an independent OCR agree, when they come from a born-digital issue, or when checked by eye against the page.
