# Aleppo Domari Glossing Toolkit

This project provides a set of scripts and dictionaries for automatic morphological segmentation and interlinear glossing of Aleppo Domari language texts. The workflow is implemented in a Google Colab notebook and organized into five clear steps.

---

## Input Format

The input file should consist of unsegmented Aleppo Domari sentences,  
each followed by an English translation enclosed in quotation marks (e.g., `'`).  
Each sentence pair should appear on two consecutive lines, followed by a blank line to separate entries.  
Lines starting with a quotation mark will be preserved throughout all processing steps.

---

## Step 1a: Morphological Segmentation (Preprocessing + Word Replacement)

Morphological boundaries are inserted into words in two steps:

1. **Preprocessing**:  
   This step adds a space or hyphen (`-`) after specific punctuation marks or endings.  
   For example, the clitic `šii` (meaning 'also') is separated from the preceding word,  
   as it typically follows an inflected phrase and should be treated as an independent morpheme.  
   This prevents segmentation errors in the next step.

2. **Word replacement using a segmentation list**:  
   Words are checked for known endings and replaced with their segmented equivalents.  
   These replacements are defined in a text file (`morpheme_boundary_dic.txt`)  
   where each line lists an original word ending and its segmented version (with hyphens).  
   Longer patterns are applied first to avoid partial matches overriding full ones.

Lines starting with a quotation mark (e.g., translations) are skipped and preserved as-is.

**Example**:
- `saaʕidkarrisaa.` → `saaʕid- -kar-r-is-aa .`

---

## Step 1b: Correcting Over-segmentation

This step applies additional adjustments to morpheme segmentation using a list of replacement patterns.  
It is used to correct cases where automatic segmentation may have over-applied boundary rules.

The script reads a replacement dictionary (`adjust_morpheme_boundary.txt`),  
where each line lists an original word and its adjusted version.

Replacements are applied from longest to shortest match to avoid partial overlap issues.

**Example**:
- `ḍ -o-m` → `ḍom`

---

## Step 2: Splitting into One Word Per Line

This step converts each sentence into a list of individual words, with one word per line.

This step is applied to segmented text.  
Lines that start with a quotation mark (e.g., translations) are preserved as they are.

After this step, **download the output file and manually check whether each word is correctly segmented** into morphemes.  
If over-segmentation or incorrect boundaries are found, correct them manually before proceeding to the next step.

---

## Step 3: Interlinear Glossing

In this step, each word is matched against a predefined gloss dictionary and assigned a gloss if available.

The script reads a dictionary file (`aleppo_domari_dic_updated.txt`) from the `dictionaries/` folder.  
Each line in the file lists a word and its corresponding gloss, separated by a tab character.

The script then reads the list of segmented words from `output_texts/processed_words.txt`,  
and for each word:
- If a gloss is found, the word and its gloss are written side by side (tab-separated).
- If no gloss is found, the word is written alone to indicate a missing entry.

**Example**:
- `ḍom` → `ḍom	Dom/people`

The glossed output is saved as `output_texts/output_texts_glossed.txt`.

---

## Step 4: Formatting for Interlinear Glossing

This step reformats the glossed text into a standard three-line interlinear glossing format.

The input file (`output_texts/glossed.txt`) contains one word per line, optionally followed by a gloss separated by a tab.  
Each sentence block is separated by an empty line and may include a free translation enclosed in quotation marks (e.g., `'` or `"`).

For each block, the script outputs:

1. A segmented sentence 
2. A gloss line
3. A free translation line

If a gloss is missing:
- `*` is inserted for general words
- `-*` is inserted if the word begins with a hyphen (e.g., for suffixes)

After formatting, an additional cleaning step is applied to remove formatting artifacts such as:
- Extra spaces before punctuation (e.g., `' , '` → `','`)
- Double asterisks (`**`) from unglossed items
- Redundant or broken hyphenation (e.g., `- -` → `-`)

**Example**:
- `ḥasanee laavtiiy-ee ,`
- `* girl-COP.PRES/2SG.PRES/PL`
- `'the daughter of ḥasanee,'`

---

## Step 5: Gloss Disambiguation

This step performs **gloss disambiguation**—it replaces multiple gloss entries with more specific or context-appropriate ones.

The input file (`output_texts/cleaned_formatted_glossing.txt`) may contain glosses with multiple possible analyses.  
To improve clarity, a set of replacement rules is applied using a dictionary file (`gloss_replacement.txt`),  
where each line lists an original gloss string and its disambiguated version, separated by a tab.

**Example**:
- `d-išt-ir-r-ee`  
- `give-COP/PROG-3SG-2SG-PRES` → `give-PROG-3SG-2SG-PRES`

The disambiguated and finalized output is saved as `output_texts/formatted_glossing_final.txt`.

This automatic disambiguation process works for glosses that can be clarified based on surrounding morphemes.  
However, certain glosses—such as those involving lexemes with multiple context-dependent meanings (e.g., `qay-` glossed as `eat/vomit`)—cannot be resolved by this process.  
Such cases should be manually reviewed and resolved after this step.

---

## License

- Code and notebooks: [MIT License](./LICENSE)  
- Dictionaries and linguistic data (`dictionaries/`, `input_texts/`, `output_texts/`): [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)


---

## Citation

If you use this toolkit in your research, please cite it as:

Kitamura, Moe. *Aleppo Domari Glossing Toolkit*. GitHub repository. https://github.com/moekitamura/aleppo-domari-glossing
---

For questions or suggestions, feel free to open an issue or contact the maintainer.

