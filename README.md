# Buryat morphological analyzer
This is a rule-based morphological analyzer for Buryat (``bua``; Mongolic). It is based on a formalized description of literary Buryat morphology, which uses [uniparser-morph](https://github.com/timarkh/uniparser-morph) for parsing. It performs full morphological analysis of Buryat words (lemmatization, POS tagging, grammatical tagging, glossing).

NB: The analyzer is still under construction. Right now, a number of entries in the dictionary have wrong POS tags or paradigms. Use with caution.

## How to use
### Python package
The analyzer is available as a Python package. If you want to analyze Buryat texts in Python, install the module:

```
pip3 install uniparser-buryat
```

Import the module and create an instance of ``BuryatAnalyzer`` class. Set ``mode='strict'`` if you are going to process text in standard orthography, or ``mode='nodiacritics'`` if you expect some words to lack the diacritics (which often happens in social media). After that, you can either parse tokens or lists of tokens with ``analyze_words()``, or parse a frequency list with ``analyze_wordlist()``. Here is a simple example:

```python
from uniparser_buryat import BuryatAnalyzer
a = BuryatAnalyzer(mode='strict')

analyses = a.analyze_words('Морфологи')
# The parser is initialized before first use, so expect
# some delay here (usually several seconds)

# You will get a list of Wordform objects
# The analysis attributes are stored in its properties
# as string values, e.g.:
for ana in analyses:
        print(ana.wf, ana.lemma, ana.gramm, ana.gloss)

# You can also pass lists (even nested lists) and specify
# output format ('xml' or 'json')
# If you pass a list, you will get a list of analyses
# with the same structure
analyses = a.analyze_words([['А'], ['Би', 'шамдаа', 'дуратайб', '.']],
	                       format='xml')
analyses = a.analyze_words(['Морфологи', [['А'], ['Би', 'шамдаа', 'дуратайб', '.']]],
	                       format='json')
```

Refer to the [uniparser-morph documentation](https://uniparser-morph.readthedocs.io/en/latest/) for the full list of options.

### Word lists


## Description format
The description is carried out in the ``uniparser-morph`` format and involves a description of the inflection (paradigms.txt), a grammatical dictionary (bua_lexemes_XXX.txt files), and a short list of analyses that should be avoided (bad_analyses.txt). The dictionary contains descriptions of individual lexemes, each of which is accompanied by information about its stem, its part-of-speech tag and some other grammatical/borrowing information, its inflectional type (paradigm), and, for some, Russian translation. See more about the format [in the uniparser-morph documentation](https://uniparser-morph.readthedocs.io/en/latest/format.html).
