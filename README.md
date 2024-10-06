# Machine Translation Decoders

A Python implementation of statistical machine translation decoders with support for monotone and non-monotonic decoding, including beam search with future cost estimation.

## Table of Contents

- [Motivation](#motivation)
- [Algorithm Details](#algorithm-details)
  - [Monotone Decoding (`decode-baseline`)](#monotone-decoding-decode-baseline)
  - [Non-Monotonic Decoding (`decode`)](#non-monotonic-decoding-decode)
  - [Beam Search with Future Cost Estimation (`decode-ext`)](#beam-search-with-future-cost-estimation-decode-ext)
- [Requirements](#requirements)
- [Directory Structure](#directory-structure)
- [How to Run](#how-to-run)
  - [Decoding](#decoding)
  - [Evaluating Translation Quality](#evaluating-translation-quality)
- [Results](#results)
- [Contributing](#contributing)
- [License](#license)
- [Copyright](#copyright)

## Motivation

This project implements a phrase-based statistical machine translation (SMT) decoder. It explores different decoding algorithms to improve translation performance, including monotone decoding, non-monotonic decoding with phrase reordering, and beam search with future cost estimation.

The goal is to compare these algorithms in terms of translation quality and computational efficiency, providing insights into their practical applications in SMT.

## Algorithm Details

### Monotone Decoding (`decode-baseline`)

A basic decoder that translates phrases in the source language in the same order (monotonic translation). It does not perform any reordering of phrases, resulting in straightforward but potentially less fluent translations.

### Non-Monotonic Decoding (`decode`)

An enhanced decoder that allows for phrase reordering, enabling non-monotonic translation. It explores different phrase segmentations and reorderings to find translations with higher likelihood according to the translation and language models.

Key features:

- **Phrase Reordering**: Permits non-monotonic alignment of source and target phrases.
- **Stack-Based Beam Search**: Uses stacks to manage hypotheses based on the number of source words translated.

### Beam Search with Future Cost Estimation (`decode-ext`)

An advanced decoder that implements beam search with future cost estimation to improve translation quality while maintaining computational efficiency.

Key features:

- **Future Cost Estimation**: Incorporates heuristic estimates of the future cost (the best possible translation score for untranslated portions), guiding the search towards more promising hypotheses.
- **Beam Search Optimization**: Prioritizes hypotheses based on estimated total cost (current log probability plus future cost estimate).

## Requirements

- Python 3.x

## Directory Structure

```
├── data/
│   ├── input       # Source language sentences to translate
│   ├── lm          # Language model file (ARPA format)
│   └── tm          # Translation model file
├── decode          # Non-monotonic decoder script
├── decode-baseline # Monotone decoder script
├── decode-ext      # Decoder with beam search and future cost estimation
├── compute-model-score # Script to compute translation quality
├── LICENSE         # MIT License
└── README.md       # Project documentation
```

## How to Run

### Decoding

1. **Prepare the Environment**

   Ensure all scripts have execution permissions:

   ```bash
   chmod +x decode decode-baseline decode-ext compute-model-score
   ```

2. **Run the Monotone Decoder**

   ```bash
   ./decode-baseline > translations_baseline
   ```

3. **Run the Non-Monotonic Decoder**

   ```bash
   ./decode -s 1000 -k 5 > translations_nonmonotonic
   ```

4. **Run the Decoder with Beam Search and Future Cost Estimation**

   Adjust parameters `-s` (stack size), `-k` (translations per phrase), and `max_phrase_length` in the script as needed.

   ```bash
   ./decode-ext -s 1000 -k 5 > translations_beam_search
   ```

   For example, to use `k=10`, `s=5000`, and `max_phrase_length=7`:

   ```bash
   ./decode-ext -s 5000 -k 10 > translations_beam_search
   ```

   **Note**: Modify `max_phrase_length` directly in the `decode-ext` script.

### Evaluating Translation Quality

Use the `compute-model-score` script to evaluate the translations:

```bash
./compute-model-score < translations_baseline
```

Replace `translations_baseline` with the appropriate translation output file.

## Results

The following are the results obtained from different decoding strategies:

### Monotone Decoding (`decode-baseline`)

- **Time to Decode**: 1.6 seconds
- **Total Corpus Log Probability (LM+TM)**: -1439.873990

### Non-Monotonic Decoding (`decode`)

- **Parameters**: `-s 1000 -k 5`, `max_phrase_length = 5`
  - **Time**: 4 minutes 33 seconds
  - **Log Probability**: -1248.087999

- **Parameters**: `-s 1000 -k 5`, `max_phrase_length = 7`
  - **Time**: 5 minutes 8 seconds
  - **Log Probability**: -1247.564523

- **Parameters**: `-s 1000 -k 5`, `max_phrase_length = 3`
  - **Time**: 4 minutes 23 seconds
  - **Log Probability**: -1268.181645

### Beam Search with Future Cost Estimation (`decode-ext`)

- **Parameters**: `-s 1000 -k 5`, `max_phrase_length = 5`
  - **Time**: 6 minutes 32 seconds
  - **Log Probability**: -1245.607535

- **Parameters**: `-s 5000 -k 10`, `max_phrase_length = 7`
  - **Time**: 58 minutes 17 seconds
  - **Log Probability**: -1226.093660

- **Parameters**: `-s 1000 -k 5`, `max_phrase_length = 7`
  - **Time**: 6 minutes 34 seconds
  - **Log Probability**: -1245.084060

- **Parameters**: `-s 1000 -k 10`, `max_phrase_length = 7`
  - **Time**: 10 minutes 26 seconds
  - **Log Probability**: -1239.005381

**Interpretation**:

- **Higher Log Probabilities(Lower Absolute Log Probabilities)**: Indicate better translation quality according to the models.
- **Computational Trade-offs**: Increasing `k`, `s`, and `max_phrase_length` improves quality but increases decoding time.

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss improvements, bug fixes, or new features.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.


## Copyright
© 2024 Mohammad Junayed Hasan. All rights reserved.


---

**Disclaimer**: The language model and translation model files provided are for educational purposes. Ensure you have the rights to use any language or translation models included in the `data` directory.
