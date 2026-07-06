# Lab 1: Dictionary Methods & Sentiment Analysis

## Overview

In this lab, you will use dictionary-based NLP methods to study emotions and sentiment in Reddit discussions related to gun politics. You will work with the simplified GoEmotions Reddit dataset. The goal of this lab is substantive analysis: using dictionary methods to answer a social science research question.

The guiding research question is:

```text
What emotions dominate gun politics discussions?
```

You will construct a topic-specific corpus by selecting comments from three gun-politics-related subreddits:

```text
gunpolitics
progun
liberalgunowners
```

You will then apply two dictionary-based methods:

- VADER sentiment analysis
- NRC Emotion Lexicon analysis

This lab introduces a common approach in computational social science: using pre-existing dictionaries to measure sentiment and emotion in text. You will also reflect on the strengths and limitations of dictionary methods.

## Learning Objectives

By the end of this lab, you should be able to:

- Construct a topic-specific corpus using subreddit metadata.
- Apply VADER sentiment analysis to social media text.
- Interpret VADER positive, negative, neutral, and compound scores.
- Use the NRC Emotion Lexicon to measure emotion categories in text.
- Count dictionary-based emotion matches at the post level.
- Aggregate emotion scores across a corpus.
- Compare emotion patterns across subreddits.
- Visualize sentiment and emotion results.
- Critically evaluate dictionary-based methods for social science research.

## Dataset

You will use:

```text
goemotions_raw_train_simplified.csv
```

The data file will be provided separately. Place it in the same folder as your
lab script, or place it one folder above the lab script folder. The provided
starter script checks both locations.

The dataset contains Reddit comments and metadata. The available columns include:

```text
text
id
author
subreddit
link_id
parent_id
created_utc
rater_id
```

For this lab, you will use:

- `text` for sentiment and emotion analysis.
- `id` for duplicate removal.
- `subreddit` for constructing the gun-politics corpus.

## Setup

Before running the lab script, make sure your folder contains:

```text
lab1_dictionary_sentiment.py
goemotions_raw_train_simplified.csv
lab1_resources/
```

The `lab1_resources/` folder should include the NRC Emotion Lexicon file and
the local NLTK VADER resource provided with the lab materials.

This lab requires Python 3 and the following packages:

```text
pandas
matplotlib
nltk
```

If needed, install them with:

```bash
pip install pandas matplotlib nltk
```

Then run the script from a terminal or command prompt:

```bash
python lab1_dictionary_sentiment.py
```

The script will print progress messages and save output files in:

```text
lab1_dictionary_sentiment_outputs/
```

## Corpus Definition

For this assignment, define the gun-politics corpus as comments from the following subreddits:

```text
gunpolitics
progun
liberalgunowners
```

This design choice avoids relying on keyword search alone and provides a clearer sampling rule for the lab. However, it is still important to recognize that subreddit-based sampling has limitations. Not every comment in these subreddits will discuss gun politics directly, and relevant gun-politics discussions may appear in other subreddits.

## Preprocessing Strategy

Preprocessing should be treated as a method-dependent research decision. In this lab, you will use two dictionary-based tools, but they do not require exactly the same preprocessing.

For VADER sentiment analysis, use the original Reddit text as much as possible. VADER was designed for social media text and can use information from capitalization, punctuation, intensifiers, negation, and informal language. For example, punctuation such as `!!!`, capitalization such as `VERY`, and phrases such as `not good` can affect VADER scores. If you remove too much punctuation or normalize the text too aggressively before applying VADER, you may remove information that the method is designed to use.

For NRC Emotion Lexicon analysis, use lightly preprocessed and tokenized text. NRC works by matching words in a text to emotion-word dictionaries. Therefore, it is useful to lowercase the text, remove punctuation, and tokenize each comment into words before matching tokens to the NRC lexicon. This helps ensure that words such as `Fear`, `fear,`, and `fear.` are treated as the same dictionary term.

In short, this lab uses:

```text
Raw text -> VADER sentiment analysis
Lightly preprocessed tokens -> NRC emotion analysis
```

This distinction illustrates an important principle in NLP for social science research: preprocessing is not a fixed recipe. It should depend on the research question, the data source, and the method being used.

## Tasks

### Part A: Corpus Construction

Load the dataset and construct the target corpus.

You should:

- Import the CSV file.
- Print the number of rows and columns.
- Remove duplicate comments using the `id` column.
- Filter the dataset to include only:

```text
gunpolitics
progun
liberalgunowners
```

- Report the number of comments in the final corpus.
- Report how many comments come from each subreddit.
- Display several example comments for a qualitative check.

In your report, explain why you used subreddit membership to define the corpus.

### Part B: VADER Sentiment Analysis

Use VADER to calculate sentiment scores for each post.

For each comment, compute:

```text
vader_positive
vader_negative
vader_neutral
vader_compound
```

The compound score is a normalized summary score. It is commonly interpreted as:

- closer to `1`: more positive
- close to `0`: more neutral or mixed
- closer to `-1`: more negative

You should:

- Apply VADER to each comment.
- Use the original `text` column rather than heavily cleaned text.
- Store the four VADER scores as new columns.
- Produce summary statistics for the VADER scores.
- Create a histogram of compound sentiment scores.

In your report, describe whether the corpus appears generally positive, negative, neutral, or mixed based on the VADER results.

### Part C: NRC Emotion Lexicon Analysis

Use the NRC Emotion Lexicon to measure emotion categories in each comment.

For this lab, focus on the following emotions:

```text
anger
anticipation
disgust
fear
joy
sadness
surprise
trust
```

The NRC lexicon also includes positive and negative sentiment categories. This
lab does not use those categories because sentiment polarity is measured
separately using VADER.

You should:

- Load the NRC Emotion Lexicon.
- Lightly preprocess each Reddit comment by lowercasing text and removing punctuation.
- Tokenize each preprocessed Reddit comment.
- Count how many words in each comment match each target NRC emotion category.
- Create post-level columns such as:

```text
nrc_anger
nrc_anticipation
nrc_disgust
nrc_fear
nrc_joy
nrc_sadness
nrc_surprise
nrc_trust
```

- Calculate normalized emotion rates by dividing emotion counts by token count.
  The main post-level output CSV should include these rate columns, such as
  `nrc_anger_rate` and `nrc_fear_rate`.
- Aggregate emotion counts across the full corpus.
- Aggregate emotion counts by subreddit.

In your report, identify which emotions dominate the corpus overall and whether the pattern differs across subreddits.

### Part D: Visualization

Create visualizations that summarize your results.

Your code should produce at least the following figures:

- A bar chart of total NRC emotion counts.
- A histogram of VADER compound sentiment scores.
- A grouped bar chart comparing NRC emotion counts across the three subreddits.

Each visualization should have:

- A clear title.
- Labeled axes.
- A readable layout.

In your report, include or describe the figures and interpret the patterns shown.

## Required Outputs

Your code should produce the following outputs:

- A CSV file containing post-level VADER and NRC results.
- A CSV file containing VADER summary statistics.
- A CSV file containing total NRC emotion counts.
- A CSV file containing normalized NRC emotion rates. In the starter script,
  this file reports corpus-level mean rates per token; post-level rates are
  also kept in the main post-level output CSV.
- A CSV file containing NRC emotion counts by subreddit.
- A bar chart of NRC emotion totals.
- A histogram of VADER compound scores.
- A grouped bar chart of NRC emotion counts by subreddit.

Suggested file names:

```text
goemotions_simplified_lab1_target_subreddits_sentiment.csv
goemotions_simplified_lab1_target_subreddits_vader_summary.csv
goemotions_simplified_lab1_target_subreddits_nrc_emotion_totals.csv
goemotions_simplified_lab1_target_subreddits_nrc_emotion_rates.csv
goemotions_simplified_lab1_target_subreddits_nrc_by_subreddit.csv
goemotions_simplified_lab1_target_subreddits_nrc_emotion_totals.png
goemotions_simplified_lab1_target_subreddits_vader_compound_histogram.png
goemotions_simplified_lab1_target_subreddits_nrc_by_subreddit.png
```

## Deliverables

Submit the following:

### 1. Code File

Submit one complete Python script or notebook.

Your code should:

- Load the dataset.
- Remove duplicate comments.
- Construct the target subreddit corpus.
- Apply VADER sentiment analysis.
- Apply NRC emotion analysis.
- Save all required CSV and figure outputs.
- Include comments explaining major steps.

Suggested file name:

```text
lab1_dictionary_sentiment.py
```

or

```text
lab1_dictionary_sentiment.ipynb
```

### 2. Results Report

Submit a short results report that include:

- A brief description of the research question.
- A description of how you constructed the gun-politics corpus.
- The number of comments in the final corpus.
- The number of comments from each selected subreddit.
- Summary statistics for VADER sentiment scores.
- A brief interpretation of the VADER compound score distribution.
- Total NRC counts for anger, anticipation, disgust, fear, joy, sadness,
  surprise, and trust.
- A comparison of NRC emotion patterns across subreddits.
- At least one visualization or a clear discussion of the generated visualizations.
- A short reflection on the limitations of dictionary methods.

## Suggested Report Structure

You may organize your report as follows:

1. Research Question and Corpus
2. VADER Sentiment Results
3. NRC Emotion Results
4. Subreddit Comparison
5. Visualization Interpretation
6. Limitations of Dictionary Methods

## Reflection Questions

Your report should address at least three of the following questions:

- What emotion appears most dominant in the gun-politics corpus?
- Does the most frequent NRC emotion differ across the three subreddits?
- Does VADER suggest that the corpus is mostly positive, negative, neutral, or mixed?
- What kinds of emotion or sentiment might dictionary methods miss?
- How might sarcasm, political language, or community-specific slang affect the results?
- How might the results change if the corpus were constructed using keywords instead of subreddit membership?

## Evaluation Criteria

Your submission will be evaluated based on:

- Correct construction of the target subreddit corpus.
- Correct implementation of VADER sentiment analysis.
- Correct implementation of NRC emotion analysis.
- Correct generation of required summary tables and visualizations.
- Clear interpretation of sentiment and emotion results.
- Critical reflection on dictionary-method limitations.
- Quality and clarity of code.
- Quality and clarity of the results report.
