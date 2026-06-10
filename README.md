# knn-language-classifier

A K-Nearest Neighbors (KNN) classifier that detects programming languages from code snippets using Levenshtein distance and feature extraction.

## Overview

This project implements a language detection system from scratch (no ML libraries for the core algorithm) that can identify programming languages like Python, C++, HTML, and others from raw code snippets. It combines string distance metrics with language-specific features for improved accuracy.

## Features

- **Custom KNN Implementation** - Built from scratch without scikit-learn
- **Levenshtein Distance** - Normalized edit distance for code similarity
- **Feature Extraction** - Detects language-specific keywords and patterns
- **Weighted Voting** - Nearest neighbors contribute based on distance
- **Interactive Demo** - Test with your own code snippets
- **Comprehensive Metrics** - Accuracy, precision, recall, F1-score, confusion matrix
- **Automatic K Selection** - Finds optimal K value (1-15, odd numbers)

## Requirements

```bash
pandas
matplotlib (optional, for visualizations)
```

## Install with
```
pip install pandas
```

Note: The core KNN and distance calculations use only Python standard library.

## Dataseset Format
Your CSV file should have two columns:
- code
- language
### Example
```
code,language
"def hello():\n    print('hi')",Python
"cout << 'hello' << endl;",Cpp
"<div class='box'>Content</div>",HTML
```

## Run
use the colab to run the script

The script will:

1. Prompt for dataset upload (or load local dataset.csv)

2. Split data 80/20 train/test

3. Find optimal K value automatically

4. Display evaluation metrics

5. Start interactive demo for custom code

## Interactive Demo Mode
After training, you can test any code snippet:

```
Enter a code snippet (or 'quit' to exit):
> def calculate_sum(numbers):
>     total = 0
>     for n in numbers:
>         total += n
>     return total

Predicted language: Python

Nearest matches in training data:
  1. Python (distance=0.234)
     Code: def add(a, b): return a + b...
  2. Python (distance=0.287)
     Code: def multiply(x, y): result = x*y...
  3. Python (distance=0.312)
     Code: def subtract(a, b): return a-b...
```

## How It Works
1. Preprocessing
- Converts code to lowercase

- Strips whitespace

- Extracts language features (def, class, cout, <html>, etc.)

2. Distance Calculation
- Normalized Levenshtein distance: distance / max(len(a), len(b))

- Feature matching reduces distance by 0.05 per matching feature

3. KNN Classification
- Finds K nearest neighbors in training data

- Weighted voting: weight = 1 / (distance + 0.1)

- Returns language with highest total weight

4. Evaluation
- Confusion matrix for all language pairs

- Per-language precision, recall, F1-score

- Overall accuracy

## File Structure

```
code-language-detector/
├── code_language_detector.py   # Main application
├── dataset.csv                 # Training data (you provide)
├── README.md                   # This file
└── .gitignore                  # Python ignore patterns
```

## Configuration
Modify these parameters in the code:

```
# In CodeLanguageDetector.__init__()
self.k = 7  # Default K value (overridden during auto-selection)

# In split_dataset()
train_ratio = 0.8  # Train/test split ratio

# In K selection
k_values = range(1, 16, 2)  # K values to test (1,3,5,...,15)
```
## Performance Tips
- Dataset Size: More samples = better accuracy (aim for 1000+ per language)

- Code Length: Longer snippets (50+ chars) work better than very short ones

- Feature Extraction: Add language-specific patterns to extract_features()

- Normalization: Distance normalization prevents bias toward shorter strings

## Extending for New Languages
Add new language patterns in extract_features():

```
def extract_features(code):
    features = {
        # Existing features...
        'has_rust': 'fn ' in code and 'let ' in code,
        'has_go': 'func ' in code and ':= ' in code,
        'has_ruby': 'def ' in code and 'end' in code,
    }
    return features
```

## Acknowledgments
- Levenshtein distance algorithm from Vladimir Levenshtein (1965)

- K-Nearest Neighbors concept from Fix & Hodges (1951)

