# Oshkaabewis

Lexical items from the glossaries of *Oshkaabewis Native Journal* (ONJ), volumes 1–8 (1990–2011), that are not already in `OPD/` or `HammerlyFieldwork/`. The directory takes its name from the journal. Its purpose is the FST's coverage of the ONJ texts in the Ojibwe Language Explorer: words the glossaries define but OjibweMorph could not analyse.

Rights: "We have the rights to work with anything from the ONJ" (Chris Hammerly).

The extraction, classification and checks were done by Claude (Anthropic) in Nov 2026, working in the Ojibwe Language Explorer repository. Every row here was reviewed by Chris Hammerly before it was merged. The scripts, intermediate files and full reports are in that repository under `audit/glossaries/`; this README summarises the method.

## Columns

Same as the other directories: Lemma, Stem, Paradigm, Class, Translation, Source.

- **Lemma** is the headword as printed, with three exceptions:
  - Proper names are capitalised, as in `OPD/` and as the texts print them. Nichols's 1990 glossary prints place names in lower case.
  - VAI+O verbs are cited with n, as in OPD (`agoon`, stem `agoo`), though Treuer cites them without it.
  - A word broken across two lines is joined. Treuer marks the break with `=`.
- **Stem** follows OjibweLexicon's conventions, as OPD's rows do. VTA stems carry `n1` and `s1` for the n~zh and s~sh alternations; noun stems carry `w2`, `y1` or `y2`, chosen from the plural. Adverbs and particles ending in a short vowel take `w2`.
- **Paradigm** and **Class** come from the paradigm maps in `resources/` (see *Classification* below).
- **Translation** is the glossary's gloss. Where the entry appears in a born-digital issue (vol. 7 no. 2 or vol. 8 no. 1), that issue's gloss is used; otherwise the gloss of the first attestation. Page furniture and editors' notes are cut, and a few typos are corrected (see *Glosses* below).
- **Source** is the first issue and printed page where the entry appears, written like `onj-v01n2-p158` (volume 1, number 2, page 158). Vol. 6 nos. 1–2 is a double issue, cited `onj-v06n1`. For a variant printed as *also* (below), it is the first issue that prints the variant.

## Sources

Sixteen glossaries in twelve issues. vol. 1 no. 1 and vol. 1 no. 3 have none.

| Glossary | Issue(s) | Printed pages | Notes |
|---|---|---|---|
| John Nichols, glossary to Maude Kegg's *Nookomis Gaa-inaajimotawid* | vol. 1 no. 2 (1990) | 157–185 | Codes in capitals in brackets, `(VAI+O)`, `(VTI2)`; plural endings after the headword. Preceded by notes on prefixes and endings (pp. 133–156). |
| Nichols, story glossaries | vol. 2 no. 1, vol. 3 no. 1, vol. 4 no. 1 | 66–67, 34–36, 70–72 | Only words absent from, or differing from, Nichols and Nyholm's *Concise Dictionary of Minnesota Ojibwe* (1995). Verb subclasses marked. |
| Rand Valentine, glossary to "Sun and Moon" | vol. 4 no. 1 (1997) | 111–119 | Archaic vocabulary. Words marked with a dagger (Valentine's uncertain glosses) were left out. |
| Anton Treuer, Main Glossary | vol. 2 no. 1 to vol. 8 no. 1 (1995–2011), every issue | about 40 pages per issue | Cumulative: about 600 headwords in 1995, 2,425 in 2011. Lower-case codes; plurals and alternative spellings after `; pl` and `; also`; a stem between slashes when the headword is not itself the stem, with N for n~zh from vol. 5 no. 2 on. Pv, vti and vai subclasses are not distinguished. |

About 380 of Treuer's headwords occur only in issues before vol. 7 no. 2, so every issue was read, not just the last. In all, 18,803 entries were read, one per printed attestation.

## Method

### 1. Extraction

- The words on each glossary page were taken from the PDF's text layer with their positions (`pdftotext -bbox`).
- Each page was split into columns at the gutter and grouped into lines. A line at the column margin starts an entry; an indented line continues it, across columns and pages. Running heads, page numbers and letter heads were dropped.
- Each entry was parsed by its glossary's notation into headword, code(s), stem, plural, *also* forms, gloss and notes. Nichols's `Stem:`, `Also:`, `locative:` and `Reduplicated form of` notes were parsed too.
- Two OCR habits were corrected:
  - **Misread codes**, such as `via` for `vta`, `vat` for `vai`, and `m` for `na` or `ni` in vol. 5 no. 2. These were mapped back and flagged; for `m`, the plural decides between `na` and `ni`.
  - **A space beside an apostrophe** in the scanned issues' text layer (`bawa 'iminaan`). It was removed in headwords, since no Ojibwe word has one there.

### 2. Checking each reading

vol. 7 no. 2 and vol. 8 no. 1 are born-digital, so their text is exact. The other issues are scans with an OCR text layer. Every glossary page of those issues was read a second time with Tesseract 5, one column at a time. A headword was **accepted** on any of three grounds:

- the text layer and Tesseract read it identically (95% of scanned entries);
- it comes from a born-digital issue;
- it was checked by eye against a crop of the printed page. 190 readings that nothing else confirmed were checked this way, each reading written down before the key was opened.

A reading not accepted that is within two letters of an accepted headword of the same class was treated as an OCR misreading of it, and dropped. As a check on the check, Tesseract alone reads 98.4% of headwords exactly on the born-digital pages.

### 3. Merging, and matching against the lexicon

Attestations were merged by headword and word class into about 4,000 headwords. Each was matched by lemma and paradigm against `OPD/`, `HammerlyFieldwork/` and `PortageLake/`. These were **not** added:

- headwords already in the lexicon, about 2,240. These became the blind test below.
- forms the FST already analyses in the same class as an inflected or derived form: `-magad` forms, locatives, preverb compounds.
- changed and reduplicated forms (`ezhi`, `endazhi`), and possessed nouns glossed "my …" (`nookomis`).
- pronouns, which OjibweMorph takes by full tag; dependent nouns, which Treuer cites possessed and OPD without the prefix; and multiword phrases (`gemaa gaye`).
- participles (`na-pt`, `vii-prt` …). They are recorded in the Explorer (`participles.csv`) for later FST work on lexicalised participles.
- Valentine's dagger-marked words.

Personal names (`name`) are kept, as `NamePerson`; place names (`place`) as `NamePlace`.

### 4. Classification

Each new entry's glossary code was turned into OPD's class code (`vti2`, `vai2` …), and its stem was built from the headword, the printed stem and the plural. The paradigm maps in `resources/` then assign Paradigm and Class, exactly as for OPD. On OPD itself the maps reproduce the classes of all 39,456 verbs and 6,333 of 6,552 nouns.

**Verbs:**

- **VTA.** The printed stem gives n~zh as `n1` (Treuer's N, or n after a zh-final headword) and s~sh as `s1`. Lemmas in `-shi`/`-zhi` match OPD's without the i (*mawadish*). With no printed stem, a z-, s- or p-final VTA defaults to `VTA_Cw` (OPD's majority for those finals), and `'`, zh and sh finals were tested against the texts (step 5).
- **VTI.** `-an` → VTI (`VTI_am`), `-oon` → VTI2, `-in`/`-en` → VTI3, `ayaan` → VTI4.
- **VAI.** Short `-am` → VAI2 (`VAI_am`). `-aam` → `VAI_m`, as OPD has all 30 of its `-aam` verbs.
- **VII.** An n-final VII is `VII_n`, except that the short-vowel finals `-amon`, `-pon`, `-gamin` and `-mon` were tested for `VII_V` (OPD's *dagon*, *zoogipon*).

**Nouns:** the plural decides the class and stem marker, as OPD writes them:

- `-wag`/`-wan` and `-oog`/`-oon` → `w2`;
- `-iig`/`-iin` → `y2`;
- `-yag` after a long vowel → `y1`.

With no plural in any glossary, every class the final sound allows was tested against the texts (step 5).

**Adverbs and particles:** the kind (locative, temporal, quantity, degree, manner, interjection, otherwise discourse) is proposed from the gloss. This is the weakest step, and every particle and adverb was held for review.

**Blind test.** The roughly 2,240 headwords already in the lexicon were classified by the same rules without looking, then compared with the lexicon:

| Word class | Class agrees | Stem agrees |
|---|---|---|
| Verbs | 99.3% | 99.2% |
| Nouns | 94.5% | 89.9% |
| Particles | 57.5% | 99.6% |

The nouns' disagreements are mostly nouns printed without a plural.

### 5. Open stem classes tested against the texts

107 new entries had a class the glossary does not settle (no stem printed, no plural printed). For each, the candidate classes were built into separate test FSTs. Every token of the ONJ texts was analysed with each, and a class counted as **supported** when it alone analyses attested forms, such as a plural, a locative or an inflected form.

- **38 were settled by the texts, 5 of them changing class.** Example: `wanak` 'tree top' is `NI_Cw`, from *wanakong*.
- **4 were ambiguous.**
- **65 had no deciding form**, and keep OPD's majority class for their shape.

The unsettled ones were reviewed by Chris Hammerly.

### 6. Glosses

The headword checks do not cover glosses, so every gloss in this directory was read once more before release. Cut:

- running heads and the Bemidji State University page footer;
- notes (*Also: …*, *Reduplicated form*);
- speaker codes printed after a gloss.

Corrected: English typos and OCR errors in the print (*suprised*, *camoflauge*, *pericxlically*), and one factual error (Grantsburg is in Wisconsin; the print says Minnesota). Each change is listed with its reason in the Explorer's `audit/glossaries/release-fixes.csv`.

### 7. Review, and what was held

The draft had 1,166 rows. Chris Hammerly reviewed the 975 here (25 Nov 2026), along with the effect of the whole directory on the FST (below). 198 rows are not here yet and wait on a second review: every particle and adverb except the *also* variants below, stem classes the texts do not settle, entries that add readings to words the FST already analyses, and a few entries odd in the print, such as an apostrophe after a consonant in `bebaapim'`.

## What is here

975 rows:

| File | Rows |
|---|---|
| VERBS.csv | 779 |
| NOUNS.csv | 97 |
| PROPER_NOUNS.csv | 81 |
| ADVERBS.csv | 4 |
| LEXICAL_PREVERBS.csv, LEXICAL_PREADVERBS.csv | 5 each (the preverbs are also listed as preadverbs, as in `OPD/`) |
| LEXICAL_PRENOUNS.csv | 2 |
| NUMERALS.csv | 1 |
| PARTICLES.csv | 1 |

## Variants the glossaries print as *also*

Treuer often prints a second spelling after *also* (`gichimookomaan na white man; pl gichimookomaanag; also chimookomaan`). Eight such variants that occur among the ONJ words the FST did not analyse are added as rows of their own. Each takes the class of its headword in `OPD/`:

| Variant | Headword | Printed note |
|---|---|---|
| `chimookomaan` | *gichimookomaan* | |
| `ige` | *gaye* | |
| `inakeyi'ii` | *akeyaa* | |
| `keyi'ii` | *akeyaa* | |
| `niibiyo` | *niibowa* | "Rainy Lake dialects" |
| `abezhig` | *bezhig* | |
| `hai'` | *hay'* | |
| `zaaga'egan` | *zaaga'igan* | "Wisconsin" |

Each was checked with its own impact report before review (below). A variant is an ordinary row: nothing in the columns links it to its headword. The link is kept here and in the Explorer's `audit/glossaries/lexicon.csv` (column `also`).

## Effect on the FST

Measured in the Ojibwe Language Explorer (`audit/coverage/impact/`). Each comparison used OjibweMorph's own Makefile at the same commit, once without and once with this directory, and looked up every word of the Explorer's corpus (the 14 ONJ issues and the OPD example sentences, 81,895 tokens) with both FSTs. It also ran OjibweMorph's own tests on both.

- **Coverage.** The 967 glossary rows bring 1,031 more tokens (851 word types) under analysis: unanalysed tokens fall from 5,715 to 4,684. No word loses its analysis, and no reading is removed. The eight variants add 234 tokens more: `chimookomaan` and its inflected forms 139, the other seven 95.
- **Overgeneration.** Two words already analysed gain a reading from the 967 rows. `chimookomaan` gives four possessed readings to *gichimookomaan* and three of its forms (*gi-* + *chimookomaan* 'your white man').
- **OjibweMorph's tests.** Recall is unchanged everywhere, and so is the error rate of the corpus test on OPD's example sentences (0.56%). Precision drops slightly, because some test forms gain a second analysis: OPD verb tests 67.81% → 67.77%, OPD noun tests 80.09% → 80.02%. 29 lemmas cause this. About half are the glossary's own spelling of an OPD lemma (`wezhim` beside OPD's *wayezhim*, `mamaaji` beside *mamaajii*); the rest are chance homographs. They were accepted on review, the extra readings being left to disambiguation.

## Using it in the FST

Add the directory to `LEMMAS_DIR` when building OjibweMorph:

```
make all OJIBWE_LEXICON=<path to OjibweLexicon> LEMMAS_DIR=<path>/OPD,<path>/HammerlyFieldwork,<path>/Oshkaabewis
```

Without it, the FST is built exactly as before.

## Known limitations

- **Single printings.** Of the draft's 1,166 rows, about 310 rest on one scanned issue, mostly vol. 5 no. 1 (which later glossaries dropped) and the story glossaries. Their spelling is OCR-checked but not cross-checked against a second printing.
- **Glossary-only words.** About 145 of the draft's rows occur on the glossary pages but in no ONJ text, so their class could not be checked against usage.
- **Short-vowel VIIs.** n-final VIIs may be `VII_V` in some cases the glossaries do not mark and the texts do not settle.
- **Variants.** Variants are not linked to their headwords (above), and neither are the glossary's own spellings of OPD lemmas among the 967.

## Reproducing it

In the Ojibwe Language Explorer repository, `audit/glossaries/` (start with its `README.md`). The scripts run in this order:

1. inventory and OCR (`inventory.py`, `ocr_pages.py`);
2. extraction (`extract.py`);
3. the agreement check (`agree.py`);
4. crops for the eye check (`crops.py`);
5. merging and classification (`build_lexicon.py`);
6. test FSTs and comparison (`fst_compare.py`);
7. the stem-class tests (`scrutinize.py`);
8. the review queue (`build_queue.py`);
9. the release split (`split_release.py`).

The reports are:

- `REPORT.md`: the glossaries and their notations;
- `EXTRACTION.md`: extraction, checks and the blind test;
- `NEW_ITEMS.md`: the new items and the stem-class tests;
- `audit/coverage/impact/2026-11-25_oshkaabewis-branch/`: the effect on the FST.
