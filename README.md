<!--
<meta property="og:title" content="gutenberg-vocab-a-tale-of-two-cities" />
<meta property="og:description" content="Vocabulary frequency data extracted from Project Gutenberg's 'A Tale of Two Cities' (Gutenberg ID: 98). JS array of 9,838 unique words sorted by frequency." />
<meta property="og:type" content="website" />
<meta property="og:url" content="https://github.com/<your-username>/gutenberg-vocab-a-tale-of-two-cities" />
<meta property="og:image" content="https://raw.githubusercontent.com/<your-username>/gutenberg-vocab-a-tale-of-two-cities/main/og-image.png" />
<meta property="og:site_name" content="gutenberg-vocab-a-tale-of-two-cities" />
-->

# gutenberg-vocab-a-tale-of-two-cities

[![unique words](https://img.shields.io/badge/unique_words-9838-blue.svg)](./vocabulary.js)
[![Gutenberg ID](https://img.shields.io/badge/Gutenberg-ID%2098-orange.svg)](https://www.gutenberg.org/ebooks/98)
[![top word](https://img.shields.io/badge/top_word-the%20(8055)-brightgreen.svg)](./vocabulary.js)
[![license](https://img.shields.io/badge/license-Public%20Domain-lightgrey.svg)](https://www.gutenberg.org)

Vocabulary frequency data extracted from Project Gutenberg's "A Tale of Two Cities" by Charles Dickens (Gutenberg ID: 98). This repository provides a single JavaScript data file (vocabulary.js) containing an array of word-frequency entries sorted by frequency (highest first).

- Total unique words: 9,838
- Top 5 words:
  1. the — 8055
  2. and — 4999
  3. of — 4016
  4. to — 3571
  5. in — 2603

## Contents

- vocabulary.js — JavaScript file exporting the vocabulary array (sorted by frequency descending).
- README.md — this file.

## Data structure

vocabulary.js exports a single array. Each entry is an object with these fields:

- word: the canonical lowercase form (string)
- frequency: number of occurrences in the text (integer)
- variants: array of capitalization variants observed in the source (array of strings)

Example entry (the top-most entry):

```js
{
  word: "the",
  frequency: 8055,
  variants: ["the", "The"]
}
```

Notes:
- The array is sorted by frequency descending (most frequent words first).
- Variants capture capitalization forms seen in the original text (helpful for case-sensitive matching or reconstruction).

## Usage examples

Assuming vocabulary.js exports the array as the default export or as a named export `vocabulary`.

Node (CommonJS):

```js
// If vocabulary.js uses module.exports = vocabulary;
const vocabulary = require('./vocabulary.js');

// Top N words
function topN(n) {
  return vocabulary.slice(0, n);
}
console.log(topN(5));

// Find a word's entry (case-insensitive)
function findWord(w) {
  const key = w.toLowerCase();
  return vocabulary.find(e => e.word === key) || null;
}
console.log(findWord('The')); // -> { word: "the", frequency: 8055, variants: [...] }

// Sum of all token occurrences (total tokens in text)
const totalTokens = vocabulary.reduce((sum, e) => sum + e.frequency, 0);
console.log('Total tokens:', totalTokens);
```

ES Modules:

```js
import vocabulary from './vocabulary.js';

const top5 = vocabulary.slice(0, 5);
console.log(top5);
```

Browser (include vocabulary.js via script tag that sets a global `vocabulary` variable or as an ES module):

```html
<!-- If vocabulary.js defines window.vocabulary = [...] -->
<script src="vocabulary.js"></script>
<script>
  console.log(vocabulary.slice(0, 5));
</script>
```

Common tasks:

- Get frequency for a word:
  - Case-insensitive: lookup by word.toLowerCase()
  - If you want to match a specific capitalization, check the `variants` array.
- Get the vocabulary size: `vocabulary.length` (== 9838).
- Compute relative frequencies: divide each `frequency` by total token count (sum of all frequencies).

## Example: get relative frequency of "the"

```js
function relativeFrequency(word) {
  const entry = vocabulary.find(e => e.word === word.toLowerCase());
  if (!entry) return 0;
  const totalTokens = vocabulary.reduce((s, e) => s + e.frequency, 0);
  return entry.frequency / totalTokens;
}
console.log(relativeFrequency('the')); // ~ proportion of tokens that are "the"
```

## Source & License

- Source text: "A Tale of Two Cities" by Charles Dickens, Project Gutenberg (Gutenberg ID: 98). The original text is in the public domain.
- This dataset is derived from the Project Gutenberg text (public domain). The dataset in this repository is provided for research, education, and development purposes. If you intend to redistribute or publish derived works, please credit Project Gutenberg and verify any applicable terms.

## How it was generated (summary)

- Text downloaded from Project Gutenberg (ID 98).
- Tokenized to words; capitalization variants were recorded.
- Words were normalized to lowercase for the canonical `word` field.
- Frequencies counted and entries sorted by frequency descending.
- The result exported as a single JavaScript array in vocabulary.js.

## Contributing

- Corrections, issues, and pull requests are welcome — e.g., to improve tokenization or add alternate exports (JSON, CSV).
- Please include a short description of changes and verify consistency of sorting and total counts.

## Citation / Acknowledgements

- Charles Dickens — A Tale of Two Cities (Project Gutenberg ID: 98)
- Project Gutenberg — https://www.gutenberg.org

If you have questions or want a different export (CSV, JSON, Python-ready format), open an issue or submit a pull request.