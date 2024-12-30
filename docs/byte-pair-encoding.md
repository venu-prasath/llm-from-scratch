# Byte Pair Encoding (BPE)

Byte Pair Encoding (BPE) is a simple and efficient algorithm widely used in natural language processing for tokenizing text into subwords or word pieces. It is particularly popular in neural network models for language processing, such as GPT and BERT, due to its ability to strike a balance between character-level and word-level tokenization.

---

## Key Features

- **Data-Driven**: BPE learns subword units from the text data.
- **Compact Vocabulary**: Reduces the size of the vocabulary while maintaining flexibility for representing unknown or rare words.
- **Versatile**: Works well for morphologically rich languages.

---

## How It Works

BPE operates iteratively to merge the most frequent pairs of symbols (characters or subwords) into new symbols. Below are the main steps:

### Step 1: Initialize

1. Start with a sequence of text where each character is treated as a separate token.

   - Example:
     ```
     Text: `hellohello`
     Tokens: `h e l l o h e l l o`
     ```

2. Create a vocabulary of all unique characters and their frequencies.

### Step 2: Identify Frequent Pairs

1. Count the frequency of all adjacent pairs of symbols (bigrams).
2. Identify the pair with the highest frequency.
   - Example:
     ```
     Pairs: {'h e': 2, 'e l': 2, 'l l': 2, 'l o': 2, 'o h': 1}
     Most Frequent: `l l`
     ```

### Step 3: Merge the Pair

1. Replace all occurrences of the most frequent pair with a new token.

   - Example:
     ```
     Before: `h e l l o h e l l o`
     After: `h e ll o h e ll o`
     ```

2. Add the new token (`ll`) to the vocabulary.

### Step 4: Repeat

Repeat Steps 2 and 3 for a predefined number of iterations or until the desired vocabulary size is reached.

---

## Example Walkthrough

Given the word "banana":

1. **Initialization**:

   ```
   Tokens: `b a n a n a`
   ```

2. **Iteration 1**:

   - Count pairs: `{'b a': 1, 'a n': 2, 'n a': 2}`
   - Most frequent: `a n`
   - Merge: `b an an a`

3. **Iteration 2**:

   - Count pairs: `{'b an': 1, 'an an': 1, 'an a': 1}`
   - Most frequent: `an`
   - Merge: `b an an`

4. **Iteration 3**:
   - Count pairs: `{'b an': 1, 'an an': 1}`
   - Most frequent: `an`
   - Merge: `ban an`

Vocabulary after merging: `{'b', 'a', 'n', 'an'}`.

---

## Advantages of BPE

1. **Handles Rare Words**: Breaks rare words into meaningful subwords, reducing the number of out-of-vocabulary (OOV) tokens.
2. **Compression**: Represents text more compactly by combining frequently co-occurring symbols.
3. **Language Agnostic**: Applicable to a wide range of languages.

---

## Applications

- Tokenization in NLP models (e.g., GPT, BERT).
- Text compression.
- Morphological analysis of languages.

---

## Example Code (Python)

Here is a simplified implementation of BPE:

```python
from collections import Counter, defaultdict

def byte_pair_encoding(text, num_merges):
    # Initialize vocabulary with individual characters
    vocab = [' '.join(word) + ' </w>' for word in text]
    vocab = Counter(vocab)

    for _ in range(num_merges):
        # Find the most frequent pair
        pairs = defaultdict(int)
        for word, freq in vocab.items():
            symbols = word.split()
            for i in range(len(symbols) - 1):
                pairs[symbols[i], symbols[i + 1]] += freq

        if not pairs:
            break

        best_pair = max(pairs, key=pairs.get)

        # Merge the best pair
        new_vocab = {}
        for word, freq in vocab.items():
            new_word = word.replace(' '.join(best_pair), ''.join(best_pair))
            new_vocab[new_word] = freq

        vocab = new_vocab

    return vocab

# Example usage
text = ["banana", "bandana"]
result = byte_pair_encoding(text, num_merges=5)
print(result)
```

---

## Limitations

1. **Fixed Vocabulary**: Once the vocabulary is generated, it cannot adapt to new data.
2. **Context Ignorance**: Merges are based solely on frequency, not context.

---

Byte Pair Encoding has proven to be a cornerstone in modern NLP, enabling efficient tokenization and representation of text for various downstream tasks.
