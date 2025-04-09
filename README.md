# OjibweLexicon

This repository houses public-facing lexical items for the OjibweMorph FST, including processed versions of the OPD data.

### Inflectional class mappings (`resources/`)
The data from the [Ojibwe People's Dictionary (OPD)](https://ojibwe.lib.umn.edu) (stored in `OPD/` in this repo) has had its part of speech (POS) tags (e.g., *vta*) converted into inflectional classes (e.g., *VTA_s*) used in the paradigm spreadsheets in [OjibweMorph](https://github.com/ELF-Lab/OjibweMorph/) (in the XSpreadsheets subdirectories).

The mappings between the two systems are realized by CSV files, located here in the `resources/` directory.  As an example, here is [the mapping](https://github.com/ELF-Lab/OjibweLexicon/blob/main/resources/VERBS_paradigm_map.csv) which translates OPD POS tags to inflectional classes for verbs for the Border Lakes Ojibwe FST:

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

Each row defines a test which determines the inflectional class. The test is based on matching the OPD POS tag, and matching the lemma/stem with the given regular expresion.

The columns are as follows:
1. The intended paradigm (more general than the class)
1. The intended inflectional class in the paradigm spreadsheets
2. The OPD POS tag in the original scraped data
3. The regular expression pattern the stem must match (if `NONE`, any stem passes)
4. The regular expression pattern the lemma must match (if `NONE`, any lemma passes)

Each test is run in order (i.e., from *VAI_rcp* to *VTI_aa*) until a matching one is found.

E.g. consider the row (lemma,stem,POS):
```
abaabas,abaabasw,vta,
```
The inflectional class will be *VTA_Cw* because this lexeme matches the test:
```
VTA,VTA_Cw,vta,^.*[bcdfghjklmnpstwz']w$,NONE
```
because the POS is *vta* and the stem matches the regular expression `^.*[bcdfghjklmnpstwz']w$`.  No previous rows match, so this class is assigned.
