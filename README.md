# OjibweLexicon

This repository houses public-facing lexical items for the OjibweMorph FST, including processed versions of the OPD data.

### Citation

To cite this work or the contents of the repository in an academic work, please use the following:

> [Hammerly, C., Livesay, N., Arppe A., Stacey, A., & Silfverberg, M. (Submitted) OjibweMorph: An approachable morphological parser for Ojibwe](https://christopherhammerly.com/publication/ojibwemorph/OjibweMorph.pdf)

### Inflectional class mappings (`resources/`)
*Paradigm* refers to a categorization of words in Ojibwe more specific than *part of speech* (e.g., verbs, nouns, etc.) -- for example, the paradigm `VAI` refers to **v**erbs that are **a**nimate and **i**ntransitive.  A full list of paradigms is available in the XSpreadsheets/ subdirectories of [OjibweMorph](https://github.com/ELF-Lab/OjibweMorph/).  Inflectional *class* provides further categorization within each paradigm.  For example, `VAI_m` forms are VAI words whose stem ends in *m*, while `VAI_n` forms are VAI words whose stem ends in *n*.  Inflectional classes are important because they determine the endings that lexemes take when inflected.

In order to get data from the [Ojibwe People's Dictionary (OPD)](https://ojibwe.lib.umn.edu) into the expected form for use in the FST, we had to convert to our labels for paradigm and class.  To do so, we look at the OPD categorization, stem, and lemma of a given lexeme, and use that information to determine its paradigm and class.

To perform this classification, we use **paradigm maps** implemented as CSV files (located in the `resources/` directory here).  There is one map file for each part of speech. Each row in the paradigm map corresponds to a single inflectional class.  For each OPD lexeme, we go row-by-row through the paradigm map. We find the *first* row where all tests pass, categorizing the lexeme into the class (and paradigm) specified by that row.

As an example, here is the paradigm map for verbs:

```
Paradigm,Class,OPDClass,StemPattern,LemmaPattern
VAI,VAI_rcp,vai,NONE,^.*diwag$
VAI,VAI_rfx,vai,^.*dizo$,NONE
VAIPL,VAIPL_VV,vai,^.*(aa|ii|oo|e)$,^.*wag$
VAIPL,VAIPL_V,vai,^.*[^iou][iou]$,^.*wag$
VAIPL,VAIPL_n,vai,^.*n$,^.*oog$
VAIPL,VAIPL_am,vai2,^.*am$,^.*oog$
VAI,VAI_V,vai,NONE,^.*[^aiou][aiou]$
VAI,VAI_VV,vai,NONE,^.*(aa|ii|oo|e)$
VAI,VAI_am,vai2,NONE,^.*am$
VAI,VAI_m,vai,NONE,^.*m$
VAI,VAI_n,vai,NONE,^.*n$
VAIO,VAIO,vai + o,NONE,^.*$
VIIPL,VIIPL_VV,vii,^.*(aa|ii|oo|e)$,^.*wan$
VIIPL,VIIPL_d,vii,^.*d$,^.*oon$
VII,VII_VV,vii,^.*(aa|ii|oo|e)$,NONE
VII,VII_V,vii,^.*[^iou][iou]$,NONE
VII,VII_d,vii,NONE,^.*d$
VII,VII_n,vii,NONE,^.*n$
VTA,VTA_C,vta,^.*[bcdfghjklmnpstz']$,NONE
VTA,VTA_C,vta,^.*[^bcdfghjklmnpstzwa']w$,NONE
VTA,VTA_Cw,vta,^.*[bcdfghjklmnpstwz']w$,NONE
VTA,VTA_aw,vta,^.*aw$,NONE
VTA,VTA_n,vta,^.*n1$,NONE
VTA,VTA_s,vta,^.*s1$,NONE
VTI,VTI_am,vti,NONE,^.*an$
VTI,VTI_i,vti3,^.*i$,NONE
VTI,VTI_oo,vti2,NONE,^.*oon$
VTI,VTI_aa,vti4,NONE,^.*aan$
```

The columns are as follows:
- **Paradigm**: the paradigm (more general than the class; typically several rows per paradigm).
- **Class**: the class that this row uniquely identifies.
- **OPDClass**: the category assigned to lexemes of this class by the OPD. The lexeme's OPD category must match to pass all tests for this row.
- **StemPattern**: the regular expression pattern that identifies stems of this class.  The lexeme's stem must match to pass all tests for this row.  If `NONE`, any stem passes.
- **LemmaPattern**: the regular expression pattern that identifies lemmas of this class.  The lexeme's lemma must match to pass all tests for this row.  If `NONE`, any lemma passes.

As an illustrative example, consider the verb lexeme with lemma *abaabas*, stem *abaabasw*, and OPD category `vta`.  
The inflectional class will be `VTA_Cw` because the lexeme matches this row's tests:
```
VTA,VTA_Cw,vta,^.*[bcdfghjklmnpstwz']w$,NONE
```
... because the OPD category is `vta` and the stem matches the regular expression `^.*[bcdfghjklmnpstwz']w$` (and the lemma matches `NONE`, like all lemmas).  No previous rows match, so this class is assigned.

Some parts of speech require more information to be categorized.  For example, for nouns, we may also need to examine the plural or locative form to determine the inflectional class.  In this case, extra columns are used, adding to the tests that a lexeme must pass to match with a particular row:
- **Tag1** (and Tag2, Tag3, ...): the OPD label for identifying the form that must match the regex in Tag1Pattern.  `NONE` or missing if there is no such test.
- **Tag1Pattern** (and Tag2Pattern, Tag3Pattern, ...): the regex pattern that identifies inflected forms as specified by Tag1 of this class.  `NONE` or missing if there is no such test.

For example, here's a row from the paradigm map for nouns:
```
Paradigm,Class,OPDClass,StemPattern,LemmaPattern,Tag1,Tag1Pattern,Tag2,Tag2Pattern
NA,NA_VVw,na,^.*(aa|ii|oo|e)w$,NONE,pl,^.*wag$,loc,NONE
```
This means that to be categorized as the class `NA_VVw`, the plural (`pl`) form of this lexeme must match the regex `^.*wag$`, while there are no requirements for the locative (`loc`) form.

### Lexical data to exclude (`other/`)
This CSV specifies forms in the lemma spreadsheets that should *not* be included in the FST.  Each row specifies one form or group of forms.  The following information must be specified for each row:
- **Directory**: The directory here in OjibweLexicon that this exclusion applies to.  For example, you may want to exclude pronouns from the `OPD` directory but include pronouns from the `HammerlyFieldwork` directory.  If you want an exclusion to apply to all directories, you must repeat it with one row specifying each directory.
- **Field**: The field being used to identify the form(s) to exclude.  For example, if you want to exclude all members of a particular class, this should be set to `Class`.  Options are: Class, Lemma, Paradigm, Stem.
- **Value**: The value of the field that should be excluded.  For example, this could be the paradigm `PRONDem` or the lemma `dagonigan`.

If the CSV specifies forms that are already not included in the FST (e.g., some forms in OjibweLexicon/OPD when the user has specified in OjibweMorph not to use the CSVs in that directory at all), nothing will happen.
