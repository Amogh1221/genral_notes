# 📘 NLP Complete Exam Notes
### Covers Full ETE Syllabus | Formulas + Explanations + Numericals

---

## TABLE OF CONTENTS
1. [NLP Introduction & Phases](#1-nlp-introduction--phases)
2. [Semantics & Pragmatics](#2-semantics--pragmatics)
3. [Ambiguity in NLP](#3-ambiguity-in-nlp)
4. [Morphology & Text Preprocessing](#4-morphology--text-preprocessing)
5. [Corpus & Corpus Linguistics](#5-corpus--corpus-linguistics)
6. [Edit Distance — Levenshtein Algorithm](#6-edit-distance--levenshtein-algorithm)
7. [N-gram Language Models](#7-n-gram-language-models)
8. [Smoothing Techniques](#8-smoothing-techniques)
9. [Entropy, Cross-Entropy & Perplexity](#9-entropy-cross-entropy--perplexity)
10. [Bag of Words & TF-IDF](#10-bag-of-words-bow--tf-idf)
11. [Parts of Speech Tagging](#11-parts-of-speech-pos-tagging)
12. [Word Sense Disambiguation](#12-word-sense-disambiguation-wsd)
13. [HMM & Viterbi Algorithm](#13-hidden-markov-model-hmm--viterbi)
14. [Parsing — CFG, CNF, CYK, PCFG](#14-parsing--cfg-cnf-cyk-pcfg)
15. [Deep Learning — CNN, RNN, LSTM](#15-deep-learning-in-nlp--cnn-rnn-lstm)
16. [Real-World NLP Applications](#16-real-world-nlp-applications)
17. [Quick Revision Cheat Sheet](#17-quick-revision-cheat-sheet)

---

# 1. NLP Introduction & Phases

## What is NLP?
Natural Language Processing (NLP) is a **subfield of Artificial Intelligence** and an **interdisciplinary subject**. The core aim is to build intelligent computers that can interact with human beings like a human being.

> **Definition:** NLP is a theoretically motivated range of computational techniques for analyzing and representing naturally occurring text/speech at one or more levels of linguistic analysis, for the purpose of achieving human-like language processing for a range of tasks or applications.

In simpler terms: **NLP is the ability of a computer to understand what a human is saying to it.**

## Two Core Components

| Component | Full Form | Direction | What it does |
|-----------|-----------|-----------|--------------|
| NLU | Natural Language Understanding | Text/Speech → Meaning | Computer reads and understands |
| NLG | Natural Language Generation | Meaning/Data → Text/Speech | Computer generates human output |

## Why NLP?
Applications that process large amounts of text require NLP expertise. Without NLP, computers can only work with structured data (numbers, tables). With NLP, computers can work with books, articles, emails, voice commands, medical records, and more.

## Phases / Stages of NLP

NLP processes language in multiple layers, from word-level to intent-level:

```
Raw Text
   ↓
1. Morphological Analysis   → internal word structure
   ↓
2. Lexical Analysis         → word-level processing & tokenization
   ↓
3. Syntactic Analysis       → grammar & sentence structure (parse tree)
   ↓
4. Semantic Analysis        → literal meaning
   ↓
5. Discourse Analysis       → meaning across multiple sentences
   ↓
6. Pragmatic Analysis       → speaker's actual intent in context
   ↓
Final Understanding
```

### Each Phase Explained:

**1. Morphological Analysis**
- **Focus:** Internal structure of words
- **What it does:** Breaks words into morphemes (smallest meaningful units). Identifies roots, prefixes, suffixes, tense, number, gender.
- **Example:** "unhappiness" → un- + happy + -ness | "Students" → student + -s (plural)
- **Importance:** Critical for Indian languages like Hindi and Sanskrit which are morphologically rich.

**2. Lexical Analysis**
- **Focus:** Basic text processing at the word level
- **What it does:** Converts raw text into meaningful tokens. Performs tokenization, normalization, stop-word removal, stemming/lemmatization.
- **Example:** Sentence: "The students are playing." → Output: ["student", "play"] (after stemming and stop-word removal)
- **Purpose:** To prepare raw text for further analysis by breaking it into smaller processable units.

**3. Syntactic Analysis (Parsing)**
- **Focus:** Grammatical structure and relationships between words
- **What it does:** Checks if the sentence follows grammar rules. Constructs a **parse tree** showing hierarchical structure. Identifies POS of each word.
- **Example (from PPT):** "The quick brown fox jumps over the lazy dog."
  - Tokens: "The", "quick", "brown", "fox", "jumps", "over", "the", "lazy", "dog"
  - POS: DET, ADJ, ADJ, NOUN, VERB, PREP, DET, ADJ, NOUN
  - Parse tree: S → NP(The quick brown fox) + VP(jumps over the lazy dog)
- **Purpose:** To ensure the sentence follows grammar and derive a hierarchical structure for subsequent phases.

**4. Semantic Analysis**
- **Focus:** Understanding meaning and context
- **What it does:** Understands the meaning of words and sentences. Resolves ambiguity. Extracts semantic roles (who did what to whom).
- **Example (from PPT):** "John gave the book to Mary."
  - John = agent (the one who performs the action)
  - the book = theme (the object transferred)
  - Mary = recipient (the one who receives)
  - Logical form: give(John, book, Mary)
- **Example of ambiguity:** "He saw the bank." → Is it a river bank or financial institution? Semantic analysis determines based on context.
- **Purpose:** To extract meaning and ensure logical and contextual coherence.

**5. Discourse Analysis**
- **Focus:** Meaning across multiple sentences
- **What it does:** Resolves pronoun references across sentence boundaries. Maintains coherence and cohesion.
- **Example:** "Ram went to the store. He bought milk." → "He" refers to Ram.

**6. Pragmatic Analysis**
- **Focus:** Context and implied meanings beyond literal interpretation
- **What it does:** Understands the speaker's actual intent — not just the literal words. Considers social context, politeness, indirect speech acts.
- **Detailed Example (from PPT):**
  - Speaker A: "It's getting cold in here."
  - Speaker B: "I'll close the window."
  - Step 1 (Surface meaning): A observes temperature drop; B intends to close window.
  - Step 2 (Implied meaning): A is not just observing — A is indirectly requesting B to do something about the cold.
  - Step 3 (Social context): B interprets the implied request and acts cooperatively. If B had said "Yes, it is" without acting, it would show a lack of social understanding.
  - Step 4 (Politeness): A uses indirect language to be polite rather than saying "Please close the window." This softens the request.
- **Conclusion:** Pragmatic analysis uncovers unspoken social cues and implied meanings that are vital to everyday communication.

---

# 2. Semantics & Pragmatics

## Semantics
Semantics deals with the **literal meaning** of words, phrases, and sentences — what the words actually say.

**Types:**
- **Lexical Semantics** — meaning of individual words ("bank" = financial or river?)
- **Compositional Semantics** — meaning of phrases/sentences built from word meanings

**How semantic analysis works step-by-step (from PPT):**
Consider: "John gave the book to Mary."
1. Identify semantic roles: John (agent), book (theme), Mary (recipient)
2. Disambiguate meaning: "gave" = physical transfer of possession
3. Logical form: give(John, book, Mary)
4. Contextual understanding: Was it a gift or a loan? How does it affect John and Mary's relationship?

**Key Point:** Semantics goes beyond identifying words and POS tags — it seeks to understand the **relationships and meaning between entities** in the sentence.

## Pragmatics
Pragmatics goes **beyond literal meaning** — it figures out what the speaker actually intends to communicate in a given situation.

**Key factors in pragmatic analysis:**
- Speaker's intention
- Listener's interpretation
- Situational and social context
- Politeness strategies and indirect speech

**Examples:**
| Sentence | Literal Meaning | Pragmatic Meaning |
|----------|----------------|-------------------|
| "Can you open the window?" | Are you physically capable? | Please open it |
| "It's cold in here." | Temperature observation | Turn up the heat / close the window |
| "Nice haircut!" (sarcastic tone) | Compliment | Insult |

## Key Difference:
- **Semantics** = What does the sentence literally mean?
- **Pragmatics** = What does the speaker mean by saying it **right now in this context?**

## Challenges for NLP Systems
- Sarcasm, irony, and metaphor are very hard to detect computationally
- Context-dependence is difficult to model — same words, different intent in different situations
- World knowledge (common sense) is needed for pragmatic understanding — computers lack this naturally

---

# 3. Ambiguity in NLP

**Definition:** Ambiguity means a word, phrase, or sentence can be understood in **more than one way**. This is the **biggest challenge in NLP** — humans resolve ambiguity effortlessly using context and world knowledge, but computers struggle badly.

## Types of Ambiguity

### 3.1 Lexical Ambiguity
A **single word** has multiple possible meanings or can function as multiple parts of speech.

**Sub-types:**

**a) Homonymy** — same spelling/sound, completely different unrelated meanings:
- "Bank" = river bank OR financial institution
- "Bat" = cricket bat OR flying animal

**b) Polysemy** — same word, multiple related meanings:
- "Head" = body part / head of department / face of a coin

**c) Part-of-Speech ambiguity** — same word, different grammatical roles:
- "Silver" as **noun**: "She bagged two silver medals."
- "Silver" as **adjective**: "She made a silver speech."
- "Silver" as **verb**: "His worries had silvered his hair."

**How to resolve:** POS Tagging (for category ambiguity), Word Sense Disambiguation (for meaning ambiguity)

---

### 3.2 Syntactic Ambiguity (Structural Ambiguity)
The **grammatical structure** of a sentence is unclear — the sentence has multiple valid parse trees.

**Two sub-types:**

**a) Attachment Ambiguity** — unclear which element a phrase modifies:
> "The man saw the girl with the telescope."
- Reading 1: The man used a telescope to see the girl (PP attaches to verb "saw")
- Reading 2: The girl had a telescope (PP attaches to noun "girl")

> "Buy books for children"
- "for children" can modify the verb "buy" (buy for them) OR the noun "books" (books meant for children)

**b) Scope Ambiguity** — unclear how far a modifier/quantifier applies:
> "Old men and women were taken to safety."
- Reading 1: (Old men) and (women) — only men are old
- Reading 2: Old (men and women) — both groups are old

> "Every man loves a woman."
- Reading 1: For every man, there exists some woman he loves (different women)
- Reading 2: There is one particular woman that every man loves (same woman)

**How to resolve:** Parsing, Probabilistic Grammar (PCFG selects the most probable parse)

---

### 3.3 Semantic Ambiguity
Even after correctly resolving syntax and word meanings, the **overall sentence meaning** remains ambiguous.

> "Seema loves her mother and Sriya does too."
- Reading 1: Sriya loves Seema's mother
- Reading 2: Sriya loves her own mother

> "The car hit the pole while it was moving."
- "it" could refer to the car (logical) OR the pole (grammatically valid but illogical)
- **Key insight from PPT:** Humans prefer the first reading because we have a model of the world that helps us distinguish what is logical from what is not. Giving a computer such a model of the world is not easy.

> "We saw his duck."
- "duck" = his pet bird OR a motion he made (ducking down)

---

### 3.4 Anaphoric Ambiguity (Referential Ambiguity)
A pronoun or referential expression could point to **more than one previously mentioned noun**.

> "The horse ran up the hill. It was very steep. It soon got tired."
- First "It" = the hill (steep applies to surfaces — world knowledge)
- Second "It" = the horse (tired applies to animate beings — world knowledge)

The anaphoric reference of "it" in both situations causes ambiguity. Both readings are grammatically valid — only world knowledge resolves it.

**How to resolve:** Coreference Resolution, Discourse Analysis

---

### 3.5 Pragmatic Ambiguity
The **speaker's intended meaning** is unclear even after all other levels of analysis.

> "Can you close the door?" — Is this a genuine question about physical ability, or a polite request?

---

**Summary Table:**
| Type | Level | Example | How to Resolve |
|------|-------|---------|----------------|
| Lexical | Word | "bank" | WSD, POS tagging |
| Syntactic | Sentence structure | "man with telescope" | Parsing, PCFG |
| Semantic | Overall meaning | "Sriya loves her mother" | World knowledge |
| Anaphoric | Pronoun reference | "It was tired" | Coreference resolution |
| Pragmatic | Speaker intent | "Can you pass salt?" | Context modeling |

---

# 4. Morphology & Text Preprocessing

## 4.1 Morphology

**Morphology** = the study of the internal structure and formation of words in a language.

**Morpheme** = the smallest unit of a language that carries meaning. Words are built from morphemes.

**Example:** "unhappily" → un- (prefix, reverses meaning) + happy (root/free morpheme) + -ly (suffix, makes it adverb)

### Why Morphological Analysis is Important in NLP

**1. Information Retrieval:**
- User searches: "running shoes"
- **Without morphological analysis:** search engine only finds "running shoes" exactly — misses "run" or "runner"
- **With morphological analysis:** understands "running" derives from "run", retrieves documents with "run", "runner", "runs" too → broader, more relevant results

**2. Machine Translation:**
- Sentence: "She is studying."
- **Without morphological analysis:** translator may produce wrong tense
- **With morphological analysis:** identifies "studying" = present continuous form of "study" → correctly translates to "Ella está estudiando." in Spanish

**3. Sentiment Analysis:**
- Sentence: "This movie was disappointing."
- **Without morphological analysis:** might miss that "dis-" is a negative prefix
- **With morphological analysis:** identifies "dis-" + "appoint" → negative sentiment → correctly classifies sentence as negative

---

### Types of Morphemes:
| Type | Description | Example |
|------|-------------|---------|
| Free morpheme | Can stand alone as a full word | "dog", "run", "yes", "walk", "berry" |
| Bound morpheme | Cannot stand alone — must attach to another morpheme | "-s" in dogs, "de-" in detoxify, "-ness" in happiness, "cran-" in cranberry |

---

### Types of Morphology:

**1. Inflectional Morphology**
Changes the **grammatical form** of a word without changing its core meaning or part of speech. Used to express tense, number, gender, case, degree.

| Original | Inflected Form | What changed |
|----------|---------------|--------------|
| cat | cats | plural (noun stays noun) |
| walk | walked | past tense (verb stays verb) |
| run | runs | 3rd person singular present |
| fast | faster | comparative (adjective stays adjective) |
| mouse | mice | irregular plural |
| catch | caught | irregular past tense |
| eat | ate, eaten | irregular verb forms |

**Key rule:** Part of speech stays the same. Core meaning stays the same. Only grammatical nuance changes.

**2. Derivational Morphology**
Creates **entirely new words** from existing ones, often changing the part of speech or core meaning. Done by adding prefixes or suffixes.

| Original | Derived | Change |
|----------|---------|--------|
| teach (verb) | teacher (noun) | POS changes: verb → noun |
| happy (adj) | unhappy (adj) | Meaning changes: POS same |
| beauty (noun) | beautiful (adj) | POS changes: noun → adj |
| create (verb) | creative (adj) | POS changes |
| compute (verb) | computational (adj) | POS changes |
| kill (verb) | killer (noun) | POS changes |
| clue (noun) | clueless (adj) | POS changes |

**Key difference from inflection:**
- Inflection: dog → dogs (still "dog", just plural)
- Derivation: teach → teacher (new concept — a person who teaches, not just "teach" with a suffix)

**3. Compounding**
Combining two or more **independent (free) morphemes** to form a new word with its own distinct meaning.
- tooth + brush → toothbrush
- sun + flower → sunflower
- black + board → blackboard
- head + ache → headache

**Challenge:** "New York" = New + York — but these are two separate words compounded as a proper noun. Compounding rules vary across languages.

**4. Reduplication** (common in non-English languages)
Repeating a word or part of it to add intensity, emphasis, or frequency.
- Hindi: धीरे → धीरे-धीरे (slow → slowly/gradually)
- Hindi: रात → रात-रात (night → every night)
- Indonesian: orang → orang-orang (person → people)

**5. Cliticization**
Attaching short functional words (clitics) to a host word to express grammatical features.
- I + am → I'm (negation/contraction)
- they + will → they'll
- John + 's → John's (possession)

**6. Suppletion**
An irregular form completely **replaces** the expected inflected form — no predictable pattern.
- go → went (past tense — not "goed")
- good → better → best (comparative/superlative — not "gooder, goodest")

---

### Morphologically Rich vs Poor Languages:

| Feature | Rich Language (e.g., Hindi) | Poor Language (e.g., English) |
|---------|---------------------------|------------------------------|
| Word forms | Many (gender, number, case all encoded in word) | Few (mostly word order) |
| Example | जा → जाता हूँ / जाती हूँ / गए / जाएगी (many forms) | go → goes → went (few forms) |
| NLP challenge | Data sparsity, huge vocabulary, ambiguity | Simpler for NLP models |
| Word order | Flexible (grammar is in morphology, not position) | Fixed (grammar depends on word order) |

**Why morphologically rich languages are difficult for NLP:**
- One root word appears in many inflected forms → machine sees too many different "words"
- Models need far more training data to learn all forms
- Grammatical information is embedded inside words and word order is flexible → more ambiguity and model complexity

---

## 4.2 Text Preprocessing Steps

### Step 1: Tokenization
Splitting a text (character sequence) into smaller units called **tokens**. Tokens are the basic units for NLP processing — usually words, but can be subwords or characters.

**Purpose:** Tokenization and normalization together help machines process text by breaking it into manageable pieces and standardizing its format. This improves performance on all downstream NLP tasks.

**Example:**
- Input: "I love NLP!"
- Output tokens: ["I", "love", "NLP", "!"]

**Challenges:**
- "New York" → 1 token or 2? (Named entity)
- "don't" → "do" + "n't" or keep as one?
- Abbreviations: "Dr. Smith" — the period here is NOT a sentence boundary
- Punctuation: keep as separate token or discard?

**Types:**
- Word tokenization: split by spaces/punctuation
- Sentence tokenization: split text into individual sentences
- Subword tokenization: used in modern transformer models (BPE — handles unknown words)

---

### Step 2: Text Normalization
Converting text into a **standardized, consistent format** to reduce variability and improve downstream processing.

**Why:** Real-world text (especially from social media) has misspellings, abbreviations, inconsistent capitalization, and noise. Normalization cleans it up so models can handle it effectively. For example, normalizing "USA", "U.S.A.", and "United States" to the same form ensures all variations are recognized as the same entity.

**Key operations:**

**a) Lowercasing:** "The Cat Sat on the MAT" → "the cat sat on the mat"
Ensures "Cat" and "cat" are treated as the same word.

**b) Removing Punctuation:** "Hello, world!" → "Hello world"
Focuses processing on core words.

**c) Expanding Contractions:** "don't" → "do not", "they'll" → "they will"

**d) Removing Special Characters:** "@#$%" → removed

**e) Number Normalization:** "100" → "one hundred" (for speech tasks) or vice versa

---

### Step 3: Stopword Removal
Remove **very common words** that appear in almost all documents and carry little discriminating meaning.

**Common stopwords:** "is", "the", "a", "and", "of", "in", "to", "was", "it", "that", ...

**Example:**
- Input: "The cat is sitting on the mat"
- After removal: ["cat", "sitting", "mat"]

**When to use:** Information retrieval, text classification, topic modeling.

**When NOT to use:** Sentiment analysis (removing "not" from "not good" destroys the meaning), question answering.

---

### Step 4: Stemming
Reducing a word to its **root/stem** by mechanically chopping off suffixes using rules. The result may NOT be a real dictionary word — stemming doesn't care about correctness, just consistency.

**Most common algorithm: Porter Stemmer**

**Examples:**
| Word | Stem |
|------|------|
| running | run |
| happiness | happi |
| studies | studi |
| foxes | fox |
| boxing | box |
| troubling | troubl |
| cats | cat |

**Use cases:** Search engines, information retrieval — where exact word form doesn't matter as long as similar words match.

**Advantages:**
- Very fast (simple rule-based)
- Reduces vocabulary size significantly
- Good for search/retrieval tasks

**Disadvantages:**
- Produces non-real words ("happi", "studi")
- **Over-stemming:** different words get the same stem (e.g., "university" and "universe" → both "univers" — but they mean different things)
- **Under-stemming:** same word's forms not collapsed (some inflections missed)

---

### Step 5: Lemmatization
Reducing a word to its **proper base/dictionary form (lemma)** using vocabulary and morphological analysis. Always returns a real, valid word.

**Examples:**
| Word | Lemma |
|------|-------|
| running | run |
| better | good |
| studies | study |
| went | go |
| am / is / are | be |
| caught | catch |
| mice | mouse |

**How it works:** Uses a dictionary (lexicon) combined with POS information and morphological rules. For example, to lemmatize "better", it needs to know it's an adjective — otherwise it might lemmatize it incorrectly.

**Advantages:**
- Always returns a real, valid word
- More linguistically accurate
- Better for tasks where exact meaning matters (WSD, translation, QA)

**Disadvantages:**
- Slower than stemming (needs dictionary lookup)
- Requires language-specific lexical resources

### Stemming vs Lemmatization Comparison:
| Feature | Stemming | Lemmatization |
|---------|----------|---------------|
| Speed | Fast | Slower |
| Accuracy | Lower | Higher |
| Output | May be non-word | Always a real word |
| Method | Rule-based chopping | Dictionary + morphology |
| "studies" | "studi" | "study" |
| "went" | "went" | "go" |
| Best for | Search engines, IR | WSD, MT, QA |

---

# 5. Corpus & Corpus Linguistics

## What is a Corpus?
A **corpus** (plural: corpora) is essentially a **large, structured collection of written or spoken material in digital form**. It can include books, articles, transcripts, social media posts, and more. Texts are often annotated with additional linguistic information.

**Importance of Corpora in NLP:**
1. **Training Data:** NLP models require large text to learn patterns, word associations, and language structure. A well-constructed corpus provides this.
2. **Evaluation and Testing:** After training, models are tested on a separate held-out corpus to evaluate performance and ensure generalization.
3. **Language Research:** Linguists use corpora to study word frequencies, grammar, and language change over time.
4. **Resource for Annotation:** Annotated corpora are essential for supervised learning — the model learns to predict annotations based on input text.

## Corpus Design
Corpus Design means **planning and building a text dataset in a structured, systematic way** so that it accurately represents real language usage.

**Key questions a corpus designer must answer:**
- What language and domain?
- How much data? (Small for testing, large for deep learning)
- Written or spoken text?
- Annotated or raw?

**Key Factors:**

**1. Purpose** — Why are you building it?
- Linguistic research, machine translation, chatbots, sentiment analysis, etc.

**2. Size** — Depends on the task:
- Small: thousands of sentences (testing, small research)
- Large: millions of words (training deep learning models)

**3. Balance** — Include variety:
- News, blogs, books, social media, technical documents
- Avoid overrepresentation of one style → prevents bias

**4. Representativeness** — Must reflect real-world language use. Don't use only literary text if you want to model everyday English.

**5. Language & Domain** — Monolingual vs multilingual; medical/legal/general/social media.

**6. Format** — Written text, spoken transcripts, parallel (same text in multiple languages).

**7. Ethical Issues** — Ensure consent for using text/audio, anonymize personal data, respect copyright laws.

## Types of Corpora

**1. Monolingual Corpora** — Single language.
- Used for: language modeling, word frequency, sentiment analysis
- Examples: English Wikipedia, BooksCorpus
- Features: Language-specific, diverse text sources, can be millions to billions of words

**2. Multilingual / Parallel Corpora** — Multiple languages, often sentence-aligned (each sentence in one language matches a sentence in another).
- Used for: machine translation, cross-linguistic studies
- Examples: Europarl Corpus (EU Parliament proceedings), OpenSubtitles (movie subtitles)
- Challenge: High-quality parallel data is scarce for most language pairs. Most corpora are biased toward specific domains (legal, governmental) and may not generalize well.

**3. Annotated Corpora** — Raw text + linguistic labels. Essential for supervised machine learning.
- Types of annotation:
  - POS Tags: "She/PRP runs/VBZ fast/RB"
  - Parse Trees: hierarchical syntactic structure
  - Named Entities: [Delhi]LOCATION, [Apple]ORG
  - Sentiment Labels: positive/negative/neutral
  - Coreference: "Riya said she is tired." → she = Riya
- Examples: Penn Treebank (POS + syntax trees), CoNLL-2003 (NER), WordNet (semantic relations)

**4. Specialized Corpora** — Domain-specific text.
- Medical: MedLine (research articles, clinical reports) — used for biomedical NER, clinical decision support
- Legal: ECJ Corpus (court rulings) — legal document classification
- Social Media: Twitter corpora — sentiment analysis, opinion mining
- Financial: Reports for market prediction, risk assessment

## Corpus Annotation
**Corpus Annotation** = Adding linguistic labels or tags to transform raw text into structured linguistic data.

**Types of Annotation:**
| Type | Description | Example |
|------|-------------|---------|
| POS Tagging | Grammatical category of each word | run → Verb |
| Lemmatization | Base form of words | running → run |
| Syntactic Parsing | Sentence structure | Subject-Verb-Object |
| Named Entity Recognition | Entity categories | Delhi → Location |
| Semantic Tagging | Meaning categories | emotion, intent |
| Coreference | Who does a pronoun refer to? | "she" → Riya |

**Tools for annotation:** NLTK (Python, beginner-friendly), SpaCy (fast, industrial), Stanford NLP (accurate, multilingual), Brat (web GUI for NER), Prodigy (commercial, large-scale with active learning).

**When annotation IS needed:** Training supervised ML/DL models, WSD, information extraction, high-accuracy NLP systems.

**When annotation may NOT be needed:** Basic frequency counts, unsupervised learning, large pretraining on raw text (e.g., GPT).

## Collocations

**Collocation** = a pair or group of words that **frequently occur together more often than by chance**, where the combined meaning feels natural and fixed.

**Examples:**
- "strong tea" ✓ (NOT "powerful tea" — same meaning but sounds wrong)
- "make a decision" ✓ (NOT "do a decision")
- "heavy rain" ✓ (NOT "strong rain")
- "artificial intelligence" ✓ (occurs 1500 times in a corpus → genuine collocation)

**Why collocations matter in NLP:**
- **Language Understanding:** Helps machines recognize idiomatic expressions
- **Text Generation:** Ensures models produce natural-sounding text
- **Machine Translation:** Preserves correct word combinations in translations
- **Information Retrieval:** Improves search by recognizing meaningful word pairs

### How to Identify Collocations:

**Method 1: Frequency-Based**
Simply count how often word pairs appear together. High frequency → possible collocation. Problem: common words like "the" and "of" always appear together but aren't meaningful collocations.

**Method 2: Pointwise Mutual Information (PMI)**
Measures how much more often two words appear together than random chance would predict.

```
PMI(x, y) = log₂ [ P(x,y) / (P(x) × P(y)) ]
```

- PMI > 0 → words appear together more than chance → likely collocation
- PMI = 0 → statistically independent (no preference to co-occur)
- PMI < 0 → appear together less than chance

**Numerical Example:**
- P("strong") = 0.01, P("tea") = 0.005, P("strong tea") = 0.002
```
PMI = log₂(0.002 / (0.01 × 0.005))
    = log₂(0.002 / 0.00005)
    = log₂(40) ≈ 5.32
```
High PMI (5.32) → "strong tea" is a genuine collocation.

**Method 3: t-test**
A statistical significance test that measures whether the co-occurrence frequency is significantly higher than what chance would predict. Widely used in computational linguistics for reliable collocation detection.

---

# 6. Edit Distance — Levenshtein Algorithm

## What is Edit Distance?
The **minimum number of single-character edit operations** needed to transform one string into another. Also called **Minimum Edit Distance** or **Levenshtein Distance**.

**Applications in NLP:**
- **Spell correction:** User typed "graffe" — which word is it closest to? "graf"(2), "graft"(2), "giraffe"(1) → choose "giraffe"
- **Machine Translation evaluation:** How different is the system translation from the reference?
- **Computational Biology:** Aligning DNA sequences — AGGCTAT... vs TAGCTAT...
- **Named Entity Coreference:** Is "IBM Inc." the same entity as "IBM"?
- **Speech Recognition:** Evaluating accuracy of transcription

**The three edit operations and their costs:**
| Operation | Cost | Example |
|-----------|------|---------|
| Insertion | 1 | "" → "a" |
| Deletion | 1 | "a" → "" |
| Substitution | 1 | "a" → "b" |

**Simple examples (from PPT):**
- Edit("Happy", "Hilly") = 3
  - 'a'→'i' (sub) → Hippy; 'p'→'l' (sub) → Hilpy; 'p'→'l' (sub) → Hilly

- Edit("Banana", "Car") = 5
  - Delete B → anana; Delete a → nana; Delete n → naa; n→C (sub) → Caa; a→r (sub) → Car

## Algorithm — Dynamic Programming

**Core idea:** Build a 2D table where dp[i][j] = minimum edit distance between the first i characters of string 1 and first j characters of string 2. Each cell is computed from previously computed cells — we never repeat work.

**Formula:**
```
Base cases:
  dp[i][0] = i    (delete all i chars of s1 to reach empty string)
  dp[0][j] = j    (insert all j chars to reach s2 from empty string)

Recursive case:
  If s1[i] == s2[j]:
      dp[i][j] = dp[i-1][j-1]      (characters match — no operation needed)
  Else:
      dp[i][j] = 1 + min(
          dp[i-1][j],              (deletion from s1)
          dp[i][j-1],              (insertion into s1)
          dp[i-1][j-1]             (substitution)
      )
```

---

## ★ Numerical Example 1: "kitten" → "sitting"

|   |   | **s** | **i** | **t** | **t** | **i** | **n** | **g** |
|---|---|---|---|---|---|---|---|---|
|   | **0** | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| **k** | 1 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| **i** | 2 | 2 | 1 | 2 | 3 | 4 | 5 | 6 |
| **t** | 3 | 3 | 2 | 1 | 2 | 3 | 4 | 5 |
| **t** | 4 | 4 | 3 | 2 | 1 | 2 | 3 | 4 |
| **e** | 5 | 5 | 4 | 3 | 2 | 2 | 3 | 4 |
| **n** | 6 | 6 | 5 | 4 | 3 | 3 | 2 | 3 |

**Edit Distance = 3** (bottom-right cell)

Operations: k→s (substitution), e→i (substitution), insert g at end.

---

## ★ Numerical Example 2: "SUNDAY" → "SATURDAY"

|   |   | **S** | **A** | **T** | **U** | **R** | **D** | **A** | **Y** |
|---|---|---|---|---|---|---|---|---|---|
|   | **0** | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| **S** | 1 | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| **U** | 2 | 1 | 1 | 2 | 2 | 3 | 4 | 5 | 6 |
| **N** | 3 | 2 | 2 | 2 | 3 | 3 | 4 | 5 | 6 |
| **D** | 4 | 3 | 3 | 3 | 3 | 4 | 3 | 4 | 5 |
| **A** | 5 | 4 | 3 | 4 | 4 | 4 | 4 | 3 | 4 |
| **Y** | 6 | 5 | 4 | 4 | 5 | 5 | 5 | 4 | 3 |

**Edit Distance = 3**

---

# 7. N-gram Language Models

## What is a Language Model?
A **language model (LM)** is a model that **assigns a probability to a sequence of words**. It tries to predict how likely a sentence is, or what the next word should be.

**Why language models are the foundation of NLP:**

1. **Predicting and Understanding Language:** If someone says "Ram is going to the ___", the model should predict "school", "market", "temple" with higher probability than "banana". This helps machines understand context and meaning.

2. **Speech Recognition:** Converts audio to text. LM helps choose between sentences that sound the same. Example: "recognize speech" vs "wreck a nice beach" — same sounds, different words. LM picks the more probable sentence.

3. **Machine Translation:** Ensures the translated output is grammatically correct and meaningful.

4. **Spelling & Grammar Correction:** Suggests corrections based on likely word sequences.

5. **Chatbots & Virtual Assistants:** Generates human-like responses.

6. **Text Prediction:** Like predictive text on phones or Gmail's Smart Compose.

**Without a language model:** AI responses would be random words. **With LM:** AI produces coherent, natural, context-aware sentences.

## Chain Rule of Probability
```
P(w₁w₂...wₙ) = P(w₁) × P(w₂|w₁) × P(w₃|w₁w₂) × ... × P(wₙ|w₁...wₙ₋₁)
```
**Problem:** As sentences get longer, we never see the full history in training data — too sparse.

## Markov Assumption
Approximate by only looking at the last N-1 words (not the full history):
```
P(wₙ | w₁...wₙ₋₁) ≈ P(wₙ | wₙ₋ₙ₊₁...wₙ₋₁)
```
This makes the model computationally feasible.

## Types of N-gram Models

| Model | N | Context used | Formula |
|-------|---|--------------|---------|
| Unigram | 1 | None — each word independent | P(w) |
| Bigram | 2 | Previous 1 word | P(w₂\|w₁) |
| Trigram | 3 | Previous 2 words | P(w₃\|w₁w₂) |

**Trigrams give better predictions** (more context) but need more training data and suffer more from the sparse data problem (many trigrams never seen in training).

## MLE — Maximum Likelihood Estimation

**Bigram:**
```
P(wₙ | wₙ₋₁) = Count(wₙ₋₁, wₙ) / Count(wₙ₋₁)
```

**Unigram:**
```
P(w) = Count(w) / Total words in corpus
```

**Intuition:** Estimate probability by counting how often things occur in training data. The more times you see "love" follow "I", the higher P(love|I).

## Sentence Probability
Add `<s>` (sentence start) and `</s>` (sentence end) markers:
```
P(<s> w₁ w₂ ... wₙ </s>) = P(w₁|<s>) × P(w₂|w₁) × ... × P(</s>|wₙ)
```

---

## ★ Full Numerical Example

**Corpus:**
```
"I love NLP"
"I love AI"
"I study NLP"
```

### Step 1: Add markers → Extract bigrams
```
<s> I love NLP </s>
<s> I love AI  </s>
<s> I study NLP </s>
```

**Bigram Counts:**
| Bigram | Count |
|--------|-------|
| (<s>, I) | 3 |
| (I, love) | 2 |
| (I, study) | 1 |
| (love, NLP) | 1 |
| (love, AI) | 1 |
| (study, NLP) | 1 |
| (NLP, </s>) | 2 |
| (AI, </s>) | 1 |

**Unigram Counts:** `<s>`=3, I=3, love=2, study=1, NLP=2, AI=1

### Step 2: Bigram Probabilities (MLE)
```
P(I | <s>)     = 3/3 = 1.0
P(love | I)    = 2/3 ≈ 0.67
P(study | I)   = 1/3 ≈ 0.33
P(NLP | love)  = 1/2 = 0.5
P(AI | love)   = 1/2 = 0.5
P(NLP | study) = 1/1 = 1.0
P(</s> | NLP)  = 2/2 = 1.0
P(</s> | AI)   = 1/1 = 1.0
```

### Step 3: P("I love NLP")
```
= P(I|<s>) × P(love|I) × P(NLP|love) × P(</s>|NLP)
= 1.0 × (2/3) × (1/2) × 1.0
= 0.335
```

### Step 4: P("I study NLP")
```
= P(I|<s>) × P(study|I) × P(NLP|study) × P(</s>|NLP)
= 1.0 × (1/3) × 1.0 × 1.0
= 0.333
```

## N-gram vs LLM Comparison

| Feature | N-gram Model | LLM (e.g., GPT) |
|---------|-------------|-----------------|
| Context window | Fixed & short (last N-1 words) | Very long (hundreds/thousands of words) |
| Knowledge | Explicit frequency count tables | Compressed into billions of neural weights |
| Unseen words | Assigns 0 probability (needs smoothing) | Handles via subword tokenization |
| Older models | N-gram, HMM | Older |
| Evolution | N-gram → RNN → LSTM → Transformer → LLM |

---

# 8. Smoothing Techniques

## Why Smoothing?
N-gram models assign **zero probability** to any word sequence not seen in training data.

**The problem:** If even a single word pair has P=0, the entire sentence probability = 0 (anything × 0 = 0). This makes N-gram models fail on any slightly novel text.

**Example:** If "I eat sushi" never appeared in training → P("I eat sushi") = 0, even though this is a perfectly valid sentence.

**Solution:** Smoothing = redistribute a small amount of probability mass from seen events to unseen ones. "Borrow" from the rich (frequent bigrams) and give to the poor (unseen bigrams).

---

## 8.1 Add-One Smoothing (Laplace Smoothing)

**Idea:** Add 1 to every count — including bigrams that were never seen (count of 0 becomes 1).

**Formula:**
```
P_Laplace(wₙ | wₙ₋₁) = (C(wₙ₋₁, wₙ) + 1) / (C(wₙ₋₁) + V)
```
Where **V = vocabulary size** (total unique words in corpus).

**Why add V to denominator?** Because we added 1 to all V possible words that could follow this context, so the total count increases by exactly V.

**Example:**
- Corpus: "I love NLP", "I love AI", "I study NLP"
- V = 7 words
- C(I, love) = 2, C(I) = 3

```
P_Laplace(love | I) = (2 + 1) / (3 + 7) = 3/10 = 0.3
MLE (no smoothing):  P(love | I) = 2/3 = 0.667
```

Laplace pushes the probability down from 0.667 to 0.3 — redistributing some mass to unseen events.

**Advantages:**
- Simple and easy to implement
- Guaranteed: no zero probabilities

**Disadvantages:**
- **Over-smooths** — takes too much probability from frequent events and gives too much to rare/unseen events
- For large vocabularies, severely distorts probabilities of common bigrams
- Not the most accurate method in practice for NLP

---

## 8.2 Good-Turing Smoothing

**Core idea:** Use the count of words/bigrams seen **exactly once** (called singletons or hapax legomena) to estimate the probability of things **never seen before**.

**Intuition (from naturalist analogy):** If last week you saw 10 species of birds you'd never seen before, you might expect to see a few new species next week too. The number of "new" things you encounter is related to how many "singleton" things you saw.

**Key Formula:**
```
c* = (c + 1) × N(c+1) / N(c)
```

Where:
- **c** = original raw count of a bigram
- **c*** = Good-Turing adjusted (smoothed) count
- **N(c)** = number of distinct bigrams that appear exactly **c** times

**For unseen bigrams (c=0):**
```
P*(unseen bigram) = N(1) / N_total
```
Where N_total = total number of bigram tokens in corpus.

---

### ★ Numerical Example:

**Given frequency-of-frequency table:**
| Count c | N(c) — how many bigrams have this count |
|---------|------------------------------------------|
| 1 | 10 |
| 2 | 5 |
| 3 | 4 |
| 4 | 2 |

**Compute adjusted counts c*:**

For bigrams seen once (c=1):
```
c* = (1+1) × N(2) / N(1) = 2 × 5 / 10 = 1.0
```

For bigrams seen twice (c=2):
```
c* = (2+1) × N(3) / N(2) = 3 × 4 / 5 = 2.4
```

For bigrams seen 3 times (c=3):
```
c* = (3+1) × N(4) / N(3) = 4 × 2 / 4 = 2.0
```

**Interpretation:** All counts are adjusted downward (c* < c in most cases). The freed probability mass is allocated to unseen events. This is more principled than add-one.

**Advantages:**
- More statistically principled than Add-one
- Based on actual data statistics (how many singletons, doubletons, etc.)

**Disadvantages:**
- More complex to implement
- Unreliable when N(c+1) = 0 (no bigrams exist with count c+1)
- Becomes noisy for large counts c

---

## 8.3 Comparison of Smoothing Methods

| Method | Formula | Advantage | Disadvantage |
|--------|---------|-----------|--------------|
| No smoothing (MLE) | C(w,c)/C(c) | Simple, accurate for seen events | Zero probabilities for unseen |
| Add-one (Laplace) | (C+1)/(N+V) | Simple, no zeros | Over-smooths, distorts frequent probs |
| Good-Turing | c* = (c+1)N(c+1)/N(c) | More accurate redistribution | Complex, unstable for large c |
| Kneser-Ney | Advanced backoff with absolute discounting | Best real-world performance | Most complex |

---

# 9. Entropy, Cross-Entropy & Perplexity

## 9.1 Entropy

**Entropy** = a measure of **uncertainty or unpredictability** in a probability distribution. It describes how unpredictable the language itself is — independent of any specific model.

**Key intuition:**
- Entropy is HIGH when all outcomes are equally likely → very uncertain → hard to guess
- Entropy is LOW when one outcome dominates → more predictable → easy to guess

**Formula:**
```
H(X) = -∑ P(xᵢ) × log₂ P(xᵢ)
```
**Units:** bits (using log base 2)

**Entropy tells us how uncertain or complex the language is, based on real data.**

**Context examples (from PPT):**
- "I drink hot ___" → very predictable (tea, coffee) → LOW entropy
- "I saw a ___" → many options → HIGH entropy

---

### ★ Entropy Example 1: Fair Coin
P(H) = P(T) = 0.5
```
H = -(0.5 × log₂0.5 + 0.5 × log₂0.5)
  = -(0.5 × (-1) + 0.5 × (-1))
  = 1 bit
```

### ★ Entropy Example 2: Biased Coin
P(H) = 0.9, P(T) = 0.1
```
H = -(0.9 × log₂0.9 + 0.1 × log₂0.1)
  = -(0.9 × (-0.152) + 0.1 × (-3.322))
  = -(−0.137 − 0.332) = 0.469 bits
```
Less uncertain → lower entropy ✓

### ★ Context-Specific Entropy (from PPT):

**Context 1:** "I like to drink hot ___"
P(tea)=0.7, P(coffee)=0.25, P(juice)=0.03, P(water)=0.02
```
H = -(0.7 × log₂0.7 + 0.25 × log₂0.25 + 0.03 × log₂0.03 + 0.02 × log₂0.02)
  ≈ 1.125 bits  (low — "tea" dominates → predictable context)
```

**Context 2:** "At the party they served ___"
P(tea) = P(coffee) = P(juice) = P(water) = 0.25 (uniform distribution)
```
H = -(4 × 0.25 × log₂0.25)
  = -(4 × 0.25 × (-2))
  = 2 bits  (higher — all options equally likely → unpredictable context)
```

**Key point:** Entropy is about the **language/data itself**, not about any model.

## 9.2 Cross-Entropy

**Cross-Entropy** measures how well a **model (Q)** approximates the **true distribution (P)** of the language.

**Simple explanation:**
- **Entropy** = "How unpredictable is the real language?" (inherent difficulty)
- **Cross-Entropy** = "How confused is my model when predicting the real data?" (model quality)
- **Cross-entropy = Entropy + Extra confusion caused by model's mistakes**

**Formula:**
```
H(P, Q) = -∑ P(x) × log₂ Q(x)
```
Where P = true distribution (from real data), Q = model's predicted distribution.

**Key properties:**
- Cross-entropy ≥ Entropy always (model can't do better than the true distribution)
- If Q = P exactly: H(P,Q) = H(P) → perfect model
- If model is wrong: H(P,Q) > H(P) → model adds extra surprise/confusion
- **Lower cross-entropy = better model**

**For a language model evaluated on test text:**
```
H(P, Q) = -(1/N) × ∑ log₂ Q(wᵢ | wᵢ₋₁)
```
Where N = total words in test corpus.

## 9.3 Perplexity

**Perplexity** = the most widely used metric to **evaluate language models**. It is the exponential of cross-entropy.

**Intuition:** If a model has perplexity K, it is as confused as if it were randomly choosing between **K equally likely words** at each position.

- PP = 2 → model is choosing between 2 equally likely words per step (very confident)
- PP = 100 → model is confused among 100 choices per step (very uncertain)
- **Lower perplexity = better model**

**Formula:**
```
PP(W) = 2^H(P,Q)
```

Or equivalently for a test sequence W = w₁w₂...wₙ:
```
PP(W) = P(w₁w₂...wₙ)^(-1/N)
```

Or using bigram probabilities:
```
PP(W) = [∏ 1/P(wᵢ|wᵢ₋₁)]^(1/N)
```

**Key relation chain:** Lower entropy → Lower cross-entropy → Lower perplexity → Better model

---

### ★ Full Numerical Example (from PPT):

**Sentence:** "I am happy" (N = 3 words)

**LM predicted probabilities for each word:**
| Word | P(word) |
|------|---------|
| I | 0.3 |
| am | 0.5 |
| happy | 0.4 |

**Step 1: Compute negative log probabilities:**
```
-log₂(0.3) = 1.737
-log₂(0.5) = 1.0
-log₂(0.4) = 1.322
```

**Step 2: Cross-entropy (average negative log probability per word):**
```
H = (1/3) × (1.737 + 1.0 + 1.322)
  = (1/3) × 4.059
  = 1.353 bits/word
```

**Step 3: Perplexity:**
```
PP = 2^1.353 ≈ 2.55
```

**Interpretation:** The model is, on average, confused among approximately **2.55 equally probable word choices** per word. Note: PP=2.55 doesn't mean literally between 2 and 3 words — it means the model's uncertainty is equivalent to that level of confusion.

---

### Entropy vs Perplexity Summary:
| Aspect | Entropy | Perplexity |
|--------|---------|-----------|
| What it measures | Uncertainty (bits) | Effective number of word choices |
| Scale | Logarithmic | Linear (2^entropy) |
| Interpretation | Abstract (bits) | Intuitive (number of choices) |
| Formula | -∑P log₂P | 2^H |

### Typical Perplexity Values:
| Model | Perplexity |
|-------|-----------|
| Random (V=10,000) | 10,000 |
| Unigram LM | ~1000 |
| Bigram LM | ~200 |
| Trigram LM | ~100 |
| LSTM / Transformer | ~20–50 |

---

# 10. Bag of Words (BoW) & TF-IDF

## 10.1 Bag of Words (BoW)

**Idea:** Represent each document as a **vector of word counts**, completely ignoring word order. The document is treated as a "bag" — we only care what words are in it, not their order.

**Why needed:** Machine learning models need numerical input. BoW is the simplest way to convert text → numbers.

**Steps:**
1. Build a vocabulary from all documents (all unique words)
2. For each document, count how many times each vocabulary word appears
3. Represent the document as that count vector

**Example:**
- Doc1: "I love NLP"
- Doc2: "I love AI"
- Doc3: "NLP is amazing"

Vocabulary: [I, love, NLP, AI, is, amazing] → V = 6

| Document | I | love | NLP | AI | is | amazing |
|----------|---|------|-----|----|----|---------|
| Doc1 | 1 | 1 | 1 | 0 | 0 | 0 |
| Doc2 | 1 | 1 | 0 | 1 | 0 | 0 |
| Doc3 | 0 | 0 | 1 | 0 | 1 | 1 |

**Advantages:**
- Very simple to implement
- Works reasonably well for text classification tasks

**Limitations:**
- **Ignores word order:** "dog bites man" = "man bites dog" (same BoW vector, different meaning!)
- **High dimensionality:** vocabularies can be 50,000+ words → very large sparse vectors
- **No word importance:** "the" and "NLP" treated equally
- Common words dominate the representation

---

## 10.2 TF-IDF (Term Frequency — Inverse Document Frequency)

**Core concept:** A word that appears in ALL documents tells us nothing specific about any single document. A word that appears in only ONE document is highly characteristic of that document. TF-IDF captures this intuition.

### Term Frequency (TF):
Measures how often a term appears in a document (normalized by document length):
```
TF(t, d) = count of term t in document d / total terms in document d
```

### Inverse Document Frequency (IDF):
Measures how rare a term is across all documents:
```
IDF(t) = log(N / df(t))
```
Where N = total documents, df(t) = number of documents containing term t.

- Word in ALL docs → IDF = log(N/N) = log(1) = 0 → no discriminating power
- Word in 1 doc → IDF = log(N/1) = log(N) → highly distinctive

### TF-IDF Score:
```
TF-IDF(t, d) = TF(t, d) × IDF(t)
```

**Result:** Common words like "the", "is" get TF-IDF ≈ 0. Important topic words get high TF-IDF.

**Can also be applied to bigrams/trigrams** (n-grams) — TF-IDF vectorizer finds relevant word combinations.

---

### ★ Full Numerical Example:

**Documents:**
- Doc1: "the cat sat on the mat" (6 words)
- Doc2: "the cat sat" (3 words)
- Doc3: "the dog barked" (3 words)

N = 3 documents

**Step 1: TF**
```
TF(cat, Doc1) = 1/6 = 0.167
TF(cat, Doc2) = 1/3 = 0.333
TF(cat, Doc3) = 0/3 = 0
TF(the, Doc1) = 2/6 = 0.333
TF(dog, Doc3) = 1/3 = 0.333
```

**Step 2: IDF**
```
"cat" in Doc1 + Doc2 → df=2 → IDF(cat) = log(3/2) = 0.176
"the" in all 3 docs → df=3 → IDF(the) = log(3/3) = log(1) = 0
"dog" only in Doc3 → df=1 → IDF(dog) = log(3/1) = log(3) = 0.477
```

**Step 3: TF-IDF**
```
TF-IDF(cat, Doc1) = 0.167 × 0.176 = 0.029
TF-IDF(cat, Doc2) = 0.333 × 0.176 = 0.059
TF-IDF(dog, Doc3) = 0.333 × 0.477 = 0.159  ← highest — very distinctive
TF-IDF(the, Doc1) = 0.333 × 0    = 0       ← zero — useless for any doc
```

**Conclusion:** "dog" is the most distinctive word in Doc3. "the" is meaningless since it appears everywhere.

**Advantages over BoW:**
- Automatically down-weights common stop words
- Captures word importance and discriminating power
- Works well for information retrieval (search engines use TF-IDF variants)

**Limitations:**
- Still ignores word order
- Cannot capture semantic similarity ("car" and "automobile" treated as completely different)

---

# 11. Parts of Speech (POS) Tagging

## What is POS Tagging?
POS Tagging is the process of **assigning a grammatical category (tag) to each word** in a sentence so that machines can analyze structure and infer meaning.

- **Linguistics definition:** Words classified by their grammatical role and function.
- **NLP definition:** Computational process of labeling each word with its grammatical category.

## Why POS is Important in NLP

**1. Syntactic Parsing — Building Sentence Structure**
POS tags allow parsers to build parse trees.
- "The boy is eating an apple." → boy=Noun, eating=Verb
- Parser identifies: NP(The boy) + VP(is eating an apple)
- Without POS → sentence structure cannot be correctly formed.

**2. Named Entity Recognition (NER)**
POS distinguishes proper names from common words.
- "Ram lives in Delhi." → Ram=Proper Noun → Person, Delhi=Proper Noun → Location
- Without POS → system would have much higher false detection rate.

**3. Machine Translation**
POS ensures correct word order, tense, and grammatical agreement in the target language.
- "She is eating." → "is eating" = present continuous (Verb)
- Correct Hindi: "वह खा रही है।" (POS helps decide gender agreement and auxiliary verbs)

**4. Sentiment Analysis**
Sentiment is mostly carried by adjectives and adverbs.
- "The movie is extremely good." → good=Adjective (positive), extremely=Adverb (intensifier)
- POS helps focus the sentiment system on opinion-bearing words.

**5. Information Extraction**
POS helps extract useful facts from text.
- "Apple launched a new phone." → Apple=Noun(Org), launched=Verb(action), phone=Noun(product)
- Helps identify: who did what to whom.

**6. Question Answering**
POS reveals what type of answer is expected.
- "Who invented the telephone?" → "Who" → expects a Person noun
- System searches for proper nouns matching the expected answer type.

## POS Tags — Penn Treebank Standard (most widely used for English):

| Tag | Meaning | Example |
|-----|---------|---------|
| NN | Noun, singular | book, data |
| NNS | Noun, plural | books, dogs |
| NNP | Proper noun, singular | Delhi, Microsoft |
| VB | Verb, base form | eat, run |
| VBD | Verb, past tense | ate, ran |
| VBZ | Verb, 3rd person singular present | eats, runs |
| VBG | Verb, gerund/present participle | eating, running |
| JJ | Adjective | good, complex |
| RB | Adverb | quickly, efficiently |
| DT | Determiner | the, a, an |
| PRP | Personal pronoun | I, he, she, they |
| IN | Preposition or subordinating conjunction | in, on, at, of |
| CC | Coordinating conjunction | and, but, or |

**Example:** "She reads books."
- She → PRP (personal pronoun)
- reads → VBZ (verb, 3rd person singular)
- books → NNS (noun, plural)

## Why POS Tagging is Hard — The Ambiguity Problem
Many words legitimately belong to multiple POS categories:
- "book" → NN (read a book) or VB (book a ticket)
- "can" → NN (tin can), VB (I can do it), MD (modal: can run fast)
- "fast" → JJ (fast car), RB (runs fast), VB (to fast = not eat)

## Approaches to POS Tagging

**1. Rule-Based:**
- Hand-written linguistic rules: "If word ends in '-ing' and preceded by auxiliary verb → tag VBG"
- Accurate for covered cases but requires extensive expert knowledge and doesn't handle new patterns

**2. Statistical (HMM-based):**
- Uses probability: P(tag|word) × P(tag|previous tag)
- Learns from annotated training corpus automatically
- Viterbi algorithm finds the best tag sequence (covered in Section 13)

**3. Deep Learning:**
- RNN/LSTM/Transformer-based taggers
- Best accuracy currently — learn complex features automatically

## Phrase Structure
Sentences are made of phrases, each serving a grammatical role:

| Phrase | Abbreviation | Example |
|--------|-------------|---------|
| Noun Phrase | NP | "the big dog", "a fast car" |
| Verb Phrase | VP | "runs quickly", "is eating" |
| Prepositional Phrase | PP | "in the park", "on the table" |
| Adjective Phrase | AdjP | "very happy", "extremely tall" |
| Sentence | S | NP + VP |

---

# 12. Word Sense Disambiguation (WSD)

## What is WSD?
WSD is the task of **automatically determining which sense (meaning) of an ambiguous word is intended** in a given context.

**The core problem:** Many words have multiple meanings (polysemy). Computers must figure out the right one.

**Example:**
> "I went to the bank."
- Sense 1: river bank (geographical feature)
- Sense 2: financial institution

**Applications:** Machine Translation (must translate to correct word), Information Retrieval, Question Answering, Text Summarization — all require correct word meaning.

**WordNet:** A widely-used lexical database that organizes words into synonym sets (synsets) connected by semantic relations (hypernyms = more general, hyponyms = more specific, antonyms, etc.). Used by most WSD systems.

## Types of WSD Approaches

### A. Knowledge-Based (Dictionary-Based)
Uses dictionaries or knowledge bases — **no training data needed**.

### B. Supervised WSD
Uses labeled data (word + context + correct sense). High accuracy but needs large annotated corpus.

### C. Unsupervised WSD
Clusters word usages without labels. No labeled data but lower accuracy.

---

## 12.1 Lesk Algorithm (Dictionary-Based WSD)

**Basic idea:** Find the sense whose **dictionary definition (gloss)** has the **maximum word overlap** with the surrounding context words.

**Core formula:**
```
Sense = argmax_s |Context ∩ Gloss(s)|
```
Choose the sense whose definition shares the most words with the context.

**Step-by-step process:**
1. Identify the ambiguous target word
2. Retrieve all possible senses from a dictionary (e.g., WordNet)
3. Collect context words (words around the target in the sentence)
4. For each sense, count how many words its definition shares with the context
5. Choose the sense with the highest overlap

---

### ★ Numerical Example 1: "bank" in "I went to the bank to deposit money"

**Context words:** {went, deposit, money}

**WordNet definitions:**
- **Sense 1 (financial bank):** "a financial institution that accepts **deposits** and channels the **money** into lending activities"
  - Gloss words: {financial, institution, accepts, deposits, channels, money, lending, activities}
- **Sense 2 (river bank):** "sloping land especially the slope beside a body of water"
  - Gloss words: {sloping, land, especially, slope, beside, body, water}

**Overlap:**
- Sense 1 ∩ Context = {money, deposits} → overlap = 2
- Sense 2 ∩ Context = {} → overlap = 0

**Winner: Sense 1 (financial institution) ✓**

---

### ★ Numerical Example 2: "bank" in "The fisherman sat on the bank of the river"

**Context words:** {fisherman, sat, river}

**Overlap:**
- Sense 1 (financial) ∩ Context = {} → overlap = 0
- Sense 2 (river bank) — gloss contains "water" which relates to "river" → overlap = 1

**Winner: Sense 2 (river bank) ✓**

---

### Advantages of Lesk:
- Simple and intuitive
- No training data needed — only a dictionary
- Easy to implement

### Limitations of Lesk:
- **Short gloss problem:** Dictionary definitions are often too short, providing too few words for meaningful overlap.
  - Example: "bass" as fish: "any of various North American freshwater fish" — context words like "caught" and "lake" don't appear in this short gloss → incorrect or zero overlap
- Only uses exact word matches — misses synonyms and semantically related words
- Highly dependent on the quality of the dictionary
- Struggles with polysemous words where glosses aren't well differentiated

---

## 12.2 Advanced (Extended) Lesk Algorithm

**Problem with Basic Lesk:** Glosses are too short to provide enough overlap. The basic algorithm often fails.

**Solution:** Extend the gloss by including definitions of **semantically related words** from WordNet (hypernyms, hyponyms, synonyms). Also compare definitions of neighboring context words with each other.

**Key enhancements:**
1. **Extended Gloss Overlap:** Use the word's definition + definitions of its hypernyms (more general) and hyponyms (more specific) → much larger pool of words
2. **Semantic Similarity Measures:** Instead of exact word matching, use distance in WordNet's hierarchy
3. **Contextual Expansion:** Use the entire sentence or paragraph as context, not just a small fixed window
4. **Smoothing:** Give partial credit for related words that aren't exact matches

---

### ★ Numerical Example: "bass" in "He likes to play the bass guitar"

**Context words:** [He, likes, to, play, the, bass, guitar]

**Sense 1 (musical — lowest singing voice):**
- Basic gloss: "the lowest part of the musical range"
- **Extended gloss** (adding hypernym "voice" + hyponym "baritone"):
  - {lowest, part, musical, range, voice, sound, vibration, vocal, cords, baritone, second, lowest, adult, male, singing, ...}
- Overlap with context: {musical} (and "play" relates to musical performance) → overlap = 1–2

**Sense 2 (freshwater fish):**
- Basic gloss: "any of various North American freshwater fish"
- **Extended gloss** (adding hypernym "fish" + hyponym "smallmouth bass"):
  - {various, North, American, freshwater, fish, cold-blooded, vertebrates, water, smallmouth, small, mouth, ...}
- Overlap with context: {} → overlap = 0

**Winner: Sense 1 (musical bass) ✓**

The Advanced Lesk correctly identifies "bass guitar" context as musical, even though the basic gloss alone might not have enough words to distinguish it clearly.

---

## 12.3 Supervised WSD

**Approach:** Train a machine learning classifier on labeled examples where each word occurrence is annotated with its correct sense.

**Features used:**
- Surrounding words in a window (±2–3 words)
- POS tags of surrounding words
- Collocations (specific word pairs near the target)
- Syntactic dependency relations

**Algorithms:** Naive Bayes, SVM, Neural Networks, pre-trained models like BERT

**Advantage:** High accuracy when sufficient labeled data is available.
**Disadvantage:** The **"knowledge acquisition bottleneck"** — creating large sense-annotated corpora for every word is extremely time-consuming and expensive.

---

## 12.4 Unsupervised WSD (Word Sense Induction)

**Approach:** Automatically cluster occurrences of a word into groups based on contextual similarity — without any labeled data.

**Algorithm:**
1. Collect all sentences containing the target word
2. Represent each occurrence as a context vector (surrounding word counts or embeddings)
3. Cluster the occurrences (K-means, etc.)
4. Each cluster is treated as one discovered sense

**Advantage:** No labeled data needed — fully automatic, can discover new senses.
**Disadvantage:** Discovered clusters may not align with standard dictionary senses; harder to evaluate.

---

## 12.5 WSD Summary Table

| Approach | Data Needed | Accuracy | Key Method |
|----------|-------------|----------|------------|
| Dictionary (Lesk) | Dictionary only | Medium | Gloss overlap |
| Advanced Lesk | Dictionary + WordNet | Medium-High | Extended gloss |
| Supervised | Large sense-annotated corpus | High | SVM, BERT |
| Unsupervised | Only unlabeled text | Lower | Clustering |

---

# 13. Hidden Markov Model (HMM) & Viterbi

## What is an HMM?

A **Hidden Markov Model (HMM)** is a probabilistic model used for systems where:
- The system is always in one of several **hidden states** (not directly observable)
- Each hidden state **emits an observation** with a certain probability
- States **transition** to other states over time

**In NLP:** The hidden states are things we want to predict (POS tags, named entity labels), and the observations are the words we can see.

**Used in NLP for:** POS Tagging, Speech Recognition, Named Entity Recognition

## HMM Components: λ = (A, B, π)

| Symbol | Name | Description | NLP Example |
|--------|------|-------------|-------------|
| Q | States | Hidden states we want to find | POS tags: N, V, ADJ |
| O | Observations | What we can directly see | Words: "dog", "runs" |
| A | Transition Matrix | P(next state \| current state) | P(Verb \| Noun) = 0.7 |
| B | Emission Matrix | P(observation \| state) | P("dog" \| Noun) = 0.3 |
| π | Initial Probabilities | P(starting in each state) | P(Noun at start) = 0.6 |

## Three Problems in HMM

| Problem | Question | Algorithm |
|---------|----------|-----------|
| Evaluation | What is P(observations \| model)? | Forward Algorithm |
| Decoding | What is the best hidden state sequence? | **Viterbi Algorithm** |
| Learning | How to learn A, B, π from data? | Baum-Welch (EM) |

---

## Viterbi Algorithm

**Goal:** Given a sequence of observations, find the **most probable sequence of hidden states**.

**Why dynamic programming?** Brute force would check every possible state sequence — exponentially costly. DP avoids recomputing overlapping subproblems → polynomial time.

### Algorithm Steps:

**Step 1: Initialization (t=1)**
```
v₁(j) = π(j) × b(j, o₁)
```
Probability of starting in state j AND emitting the first observation o₁.

**Step 2: Recursion (t > 1)**
```
vₜ(j) = max_i [vₜ₋₁(i) × a(i,j)] × b(j, oₜ)
```
Best probability of being in state j at time t = (best path to any previous state i × transition i→j) × probability of emitting oₜ from state j.

**Step 3: Termination**
```
Best final state = argmax_j [vT(j)]
```
Then **backtrack** through the stored best previous states to recover the full sequence.

---

## ★ Numerical Example 1: Weather HMM (from your handwritten notes)

**Setup:**
- Hidden States: Sunny (S), Cloudy (C), Rainy (R)
- Observations: Happy (H), Sad (Sa)

**Transition Matrix:**
| From\To | Sunny | Cloudy | Rainy |
|---------|-------|--------|-------|
| Sunny | 0.33 | 0.67 | 0 |
| Rainy | 0 | 0.33 | 0.67 |
| Cloudy | 0.67 | 0 | 0.33 |

**Emission Matrix:**
| State\Obs | Happy | Sad |
|-----------|-------|-----|
| Sunny | 0.5 | 0.5 |
| Cloudy | 0.67 | 0.33 |
| Rainy | 0.33 | 0.67 |

**Initial Probabilities:** π(Sunny)=0.4, π(Cloudy)=0.3, π(Rainy)=0.3

**Observation sequence:** [Happy, Sad]

---

### Step 1: Initialization (t=1, O₁=Happy)
```
v₁(Sunny) = 0.4 × 0.5   = 0.200
v₁(Rainy) = 0.3 × 0.33  = 0.099
v₁(Cloudy)= 0.3 × 0.67  = 0.201
```

### Step 2: Recursion (t=2, O₂=Sad)

**For Sunny at t=2:**
```
Candidates:
  v₁(Sunny)×P(S→S) = 0.200×0.33 = 0.066
  v₁(Rainy)×P(R→S) = 0.099×0    = 0
  v₁(Cloudy)×P(C→S)= 0.201×0.67 = 0.13467  ← max

v₂(Sunny) = 0.13467 × P(Sad|Sunny) = 0.13467 × 0.5 = 0.067335
            (came from Cloudy)
```

**For Rainy at t=2:**
```
Candidates:
  v₁(Rainy)×P(R→R) = 0.099×0.33 = 0.03267
  v₁(Sunny)×P(S→R) = 0.200×0.67 = 0.134    ← max
  v₁(Cloudy)×P(C→R)= 0.201×0    = 0

v₂(Rainy) = 0.134 × P(Sad|Rainy) = 0.134 × 0.67 = 0.08978
            (came from Sunny)
```

**For Cloudy at t=2:**
```
Candidates:
  v₁(Cloudy)×P(C→C)= 0.201×0.33 = 0.06633  ← max
  v₁(Sunny)×P(S→C) = 0.200×0    = 0
  v₁(Rainy)×P(R→C) = 0.099×0.67 = 0.06603

v₂(Cloudy) = 0.06633 × P(Sad|Cloudy) = 0.06633 × 0.33 = 0.021889
             (came from Cloudy)
```

### Step 3: Termination
```
max[v₂(Sunny)=0.067335, v₂(Rainy)=0.08978, v₂(Cloudy)=0.021889]
= 0.08978 → Best state at t=2 = Rainy
```

### Backtrack:
- t=2: Rainy (came from Sunny)
- t=1: max[v₁(S)=0.200, v₁(R)=0.099, v₁(C)=0.201] = 0.201 → Cloudy

**Most Likely State Sequence: Cloudy → Rainy**
```
t=1: Cloudy (observed: Happy)
t=2: Rainy  (observed: Sad)
```

---

## ★ Numerical Example 2: POS Tagging (from your handwritten notes)

**States:** Noun (N), Verb (V)  
**Observations:** ["fish", "sleep"]

**Given:**
```
Initial: P(N)=0.6, P(V)=0.4
Transition: P(N→N)=0.3, P(N→V)=0.7, P(V→N)=0.8, P(V→V)=0.2
Emission:   P(fish|N)=0.6, P(fish|V)=0.4
            P(sleep|N)=0.4, P(sleep|V)=0.6
```

### Step 1: Initialization (t=1, word="fish")
```
v₁(N) = P(N) × P(fish|N) = 0.6 × 0.6 = 0.36
v₁(V) = P(V) × P(fish|V) = 0.4 × 0.4 = 0.16
```

### Step 2: Recursion (t=2, word="sleep")

**For Noun:**
```
v₂(N) = max[v₁(N)×P(N→N), v₁(V)×P(V→N)] × P(sleep|N)
       = max[0.36×0.3, 0.16×0.8] × 0.4
       = max[0.108, 0.128] × 0.4
       = 0.128 × 0.4 = 0.0512   (came from V)
```

**For Verb:**
```
v₂(V) = max[v₁(N)×P(N→V), v₁(V)×P(V→V)] × P(sleep|V)
       = max[0.36×0.7, 0.16×0.2] × 0.6
       = max[0.252, 0.032] × 0.6
       = 0.252 × 0.6 = 0.1512   (came from N)
```

### Step 3: Termination
```
max[0.0512, 0.1512] = 0.1512 → t=2 state = Verb
Backtrack: Verb at t=2 came from Noun at t=1
```

**Most Likely POS Sequence: [Noun, Verb]**
```
fish → Noun
sleep → Verb
```

---

# 14. Parsing — CFG, CNF, CYK, PCFG

## What is Parsing?
Parsing = the process of **discovering the grammatical structure (derivation) of a sentence** according to a grammar. It produces a **parse tree** showing how the sentence was generated from grammar rules.

**Why parsing is important:**
- Reveals grammatical relationships: subject, verb, object
- Required for machine translation, QA, information extraction
- Identifies ambiguities in sentence structure

**Context-Free Grammar (CFG) expresses context-free syntax.** The process of parsing = discovering a derivation for some sentence using CFG rules.

## Types of Parsing

| Type | Direction | Method | Characteristic |
|------|-----------|--------|----------------|
| Top-Down | Root → Leaves | Start with S, expand | May backtrack, can loop on left-recursive grammars |
| Bottom-Up | Leaves → Root | Start with words, reduce to S | Better for ambiguous grammars |
| Chart (CYK) | Both | Dynamic programming table | Guaranteed polynomial time |

---

## 14.1 Top-Down Parsing (Recursive Descent)

**Process:**
1. Construct the root with the starting symbol S
2. Select a production rule with S on the left-hand side
3. For each symbol on the right-hand side, construct the appropriate child
4. When a terminal is added to the fringe and **doesn't match the input** → **backtrack**
5. Continue until the parse tree's leaves match the input words exactly

**Problem — Left Recursion:**
A grammar is **left-recursive** if a non-terminal A can derive a sequence beginning with itself (A → Aα).
Example: Expr → Expr + Term | Term

This causes recursive-descent parsers to go into **infinite loops** — they keep expanding Expr into Expr + Term, Expr + Term + Term, forever.

**Wrong choices waste time:** If the parser picks the wrong production rule, it must backtrack and try again. This makes top-down parsing potentially slow.

**Solution — Eliminate Left Recursion:**
Replace: A → Aα | β
With: A → βA' and A' → αA' | ε

**Example (from PPT):**
```
Sum → Sum + number | number
Becomes:
Sum → number Sum'
Sum' → + number Sum' | ε
```

**Predictive Parsing (LL(1)):**
If for every production A → α | β, FIRST(α) ∩ FIRST(β) = ∅, then we can always pick the right rule with just ONE symbol of lookahead → no backtracking needed!

---

## 14.2 Context-Free Grammar (CFG)

**Definition:** A CFG defines the syntax of a language using rewriting rules.

**Components G = (N, Σ, R, S):**
| Symbol | Name | Description |
|--------|------|-------------|
| N | Non-terminals | Syntactic categories: S, NP, VP, NN, VB... |
| Σ | Terminals | Actual words: "dog", "runs", "the"... |
| R | Production Rules | N → sequence of N and Σ |
| S | Start Symbol | Usually S (Sentence) |

**Example Grammar:**
```
S  → NP VP
NP → DT NN | DT JJ NN | NNP
VP → VB NP | VBZ | VB PP
PP → IN NP
DT → "the" | "a"
NN → "dog" | "cat" | "ball"
NNP → "John" | "London"
VB → "chases" | "sees"
VBZ → "runs"
IN → "in" | "on"
JJ → "big" | "quick"
```

**Parse tree for "The dog chases a cat":**
```
         S
        / \
      NP   VP
     / \   / \
   DT  NN VB  NP
   |   |  |  / \
  the dog  |  DT NN
        chases |  |
              a  cat
```

---

## 14.3 CFG to Chomsky Normal Form (CNF)

**CNF definition:** Every production must be EITHER:
- **A → B C** (exactly two non-terminals), OR
- **A → a** (exactly one terminal)

**Why CNF is needed:** The CYK algorithm requires all rules to be in CNF. Every CFG can be converted to an equivalent CNF.

### Conversion Steps (applied in order):

**Step 1: Add new start symbol** (if original S appears on right-hand side)
```
S' → S
```

**Step 2: Eliminate ε-productions (A → ε)**
- Find all nullable symbols (symbols that can derive ε)
- For every rule using nullable symbols, add alternatives without them
- Remove the ε rules

**Step 3: Eliminate unit productions (A → B)**
- Find all unit production chains: A → B → CD
- Replace directly: A → CD
- Remove the intermediate chain

**Step 4: Fix long productions (A → B C D...)**
- Break into binary using new non-terminals:
- A → B C D becomes: A → B X, X → C D

**Step 5: Fix mixed rules (A → a B)**
- Create a new non-terminal for the terminal:
- Yₐ → a, then A → Yₐ B

**Example:**
```
Original: S → ABC
CNF: S → AX, X → BC

Original: A → aB (terminal mixed with non-terminal)
CNF: Yₐ → a, A → Yₐ B
```

---

## 14.4 CYK Algorithm (Cocke-Younger-Kasami)

**Purpose:** Checks if a sentence can be generated by a CNF grammar, and if so, finds its parse tree(s).

**Type:** Bottom-up, dynamic programming.

**Time complexity:** O(n³ × |G|) — polynomial! (n = sentence length, |G| = grammar size)

### CYK Table:
A triangular table where **Table[i][j]** = set of non-terminals that can derive the substring from word i to word j.

**Base case:**
```
Table[i][i] = {A | A → wᵢ ∈ Grammar}   (single words)
```

**Inductive step:**
```
Table[i][j] = {A | A → BC ∈ Grammar,
               B ∈ Table[i][k],
               C ∈ Table[k+1][j],
               for some split point k: i ≤ k < j}
```

**Success condition:** S ∈ Table[1][n] → sentence is grammatically valid!

---

### ★ Full CYK Numerical Example:

**Grammar (in CNF):**
```
S  → NP VP
NP → Det N
VP → V NP | V
Det → "the"
N  → "cat" | "dog"
V  → "chased"
```

**Sentence:** "the cat chased the dog"
w₁=the, w₂=cat, w₃=chased, w₄=the, w₅=dog

**Step 1: Diagonal (single words)**
```
Table[1][1] = {Det}     "the" → Det
Table[2][2] = {N}       "cat" → N
Table[3][3] = {V}       "chased" → V
Table[4][4] = {Det}     "the" → Det
Table[5][5] = {N}       "dog" → N
```

**Step 2: Length-2 spans**
```
Table[1][2]: Det N → NP? YES (NP → Det N) → {NP}
Table[2][3]: N V → nothing → {}
Table[3][4]: V Det → nothing → {}
Table[4][5]: Det N → NP? YES (NP → Det N) → {NP}
```

**Step 3: Length-3 spans**
```
Table[3][5]: k=3: V(T[3][3]) NP(T[4][5]) → VP? YES (VP → V NP) → {VP}
Table[1][3]: No useful combination → {}
Table[2][4]: No useful combination → {}
```

**Step 4: Length-4 spans → all empty**

**Step 5: Full sentence [1][5]**
```
k=2: NP(T[1][2]) VP(T[3][5]) → S? YES (S → NP VP) → {S}
```

**S ∈ Table[1][5] → Sentence is VALID! ✓**

**Parse Tree:**
```
         S
        / \
      NP   VP
     / \   / \
   Det  N  V  NP
   |    |  |  / \
  the  cat | Det  N
       chased |   |
             the dog
```

---

## 14.5 PCFG — Probabilistic Context-Free Grammar

**Idea:** Attach **probabilities** to grammar rules. When a sentence has multiple valid parse trees (ambiguity), PCFG selects the most probable one.

**Constraint:** All rules for a given non-terminal must sum to 1.

**Example:**
```
S  → NP VP     [1.0]    (S always becomes NP VP)
NP → Det N     [0.6]
NP → NNP       [0.4]    (0.6 + 0.4 = 1.0 ✓)
VP → V NP      [0.7]
VP → V         [0.3]    (0.7 + 0.3 = 1.0 ✓)
```

**Parse tree probability:**
```
P(tree) = ∏ P(each rule used in the tree)
```

**Example:**
```
P = P(S→NP VP) × P(NP→Det N) × P(VP→V NP) × P(NP→Det N)
  = 1.0 × 0.6 × 0.7 × 0.6
  = 0.252
```

If a competing parse tree gives probability 0.1, PCFG selects the 0.252 tree as the preferred interpretation.

**Why PCFG matters:** Natural language is deeply ambiguous. A single sentence can have thousands of valid parse trees. Without probabilities, parsers can't choose between them. PCFG provides a principled way to select the most plausible parse.

---

# 15. Deep Learning in NLP — CNN, RNN, LSTM

## 15.1 CNN for NLP

**Originally designed for images**, CNN was adapted for text classification tasks.

**How CNN works for NLP:**
1. Words are represented as dense vectors (word embeddings)
2. Convolutional filters (of different sizes) slide over the sequence of word vectors
3. Each filter captures **local n-gram patterns:**
   - Filter size 2 → detects bigram features ("very good", "not bad")
   - Filter size 3 → detects trigram features
4. Max-pooling selects the most salient feature from each filter
5. Pooled features fed into a classifier

**Use cases:** Text classification, sentiment analysis, spam detection.

**Advantages:**
- Fast — convolutions are highly parallelizable (unlike sequential RNNs)
- Captures local n-gram patterns well
- Fixed-size output regardless of input length (after pooling)

**Disadvantages:**
- **Fixed window size** — cannot capture dependencies longer than the filter size
- Doesn't model sequential order or long-range context well
- Less suitable for tasks requiring full-sentence understanding (translation, QA)

**CNNs vs RNNs:**
- CNNs handle **spatial/local patterns** well → used for spatial data (images) and local text features
- RNNs handle **temporal/sequential dependencies** → better for language where word order and long-range context matter
- CNN takes fixed-size inputs; RNN handles arbitrary-length sequences

---

## 15.2 RNN — Recurrent Neural Network

### Motivation
Language is inherently **sequential** — the meaning of a word depends on what came before it. Regular (feed-forward) neural networks:
- Cannot handle sequences of variable length
- Consider only the current input, not past context
- Cannot memorize previous inputs

**The key insight:** A simple feed-forward NN sees each word independently. But "I love NLP" is not three independent words — the meaning of "love" here depends on "I" coming before it, and "NLP" is what's being loved.

**RNN solution:** Maintain a **hidden state** that acts as memory. At each time step, the hidden state is updated based on both the current input and the previous hidden state — capturing sequential context.

**Feature Sharing:** The same weights (Wₓₕ, Wₕₕ, Wₕᵧ) are used at every time step. This reduces parameters, prevents overfitting, and allows the model to generalize across different positions in the sequence.

### RNN Equations

**Recurrence formula:**
```
hₜ = tanh(Wₓₕ·xₜ + Wₕₕ·hₜ₋₁)     ← current hidden state
yₜ = Wₕᵧ·hₜ                          ← output at time t
```

Where:
- **xₜ** = input vector at time t (current word's embedding/one-hot)
- **hₜ₋₁** = hidden state from previous time step (what the RNN "remembers")
- **hₜ** = new hidden state (updated memory incorporating current input)
- **yₜ** = output (e.g., next word probability distribution)
- **Wₓₕ** = input weight matrix (input → hidden)
- **Wₕₕ** = recurrent weight matrix (hidden → hidden) — **shared across all time steps**
- **Wₕᵧ** = output weight matrix (hidden → output)

**Unrolled RNN visualization:**
```
x₀→[A]→h₀    x₁→[A]→h₁    x₂→[A]→h₂    xₜ→[A]→hₜ
     ↓              ↓              ↓              ↓
    y₀             y₁             y₂             yₜ
```
Each box "A" represents the same weights. The hidden state carries forward information from all previous steps.

### RNN Types:
| Type | Structure | Typical Use |
|------|-----------|-------------|
| One-to-one | 1 input → 1 output | Image classification |
| One-to-many | 1 input → sequence | Image captioning |
| Many-to-one | Sequence → 1 output | Sentiment analysis |
| Many-to-many | Sequence → sequence | Machine translation, POS tagging |

### Training: Backpropagation Through Time (BPTT)

**Steps:**
1. **Feedforward:** Supply input at each time step, compute hidden states, compute output
2. **Compute error:** E = y - d (predicted minus desired output) using cross-entropy
3. **Backpropagate:** Unroll the network for all time steps, propagate error backward through each time step
4. **Combine gradients:** Since same weights used at all time steps, gradients from all steps are summed
5. **Update weights:** Wₓₕ, Wₕₕ, Wₕᵧ are updated

---

## ★ RNN Numerical Example (from your PPT)

**Task:** Next character prediction — train on "hello"
**Vocabulary:** {h, e, l, o} → one-hot encoded (size 4)
**Architecture:** 4 inputs, 3 hidden neurons, 4 outputs

**Given weight matrices (randomly initialized):**
```
Wₓₕ (3×4):             Wₕₕ (scalar):    Wₕᵧ (4×3):
[0.287027  0.84606  0.572392  0.486813]   [0.427043]   [0.37168  0.974829  0.830035]
[0.902874  0.871522 0.691079  0.18998 ]               [0.39141  0.282586  0.659836]
[0.537524  0.09224  0.558159  0.491528]               [0.64985  0.098216  0.334287]
                                                        [0.91266  0.325816  0.144630]
```

**Input h = [1, 0, 0, 0]ᵀ (one-hot for 'h')**

**Step 1:** Wₓₕ × x_h = first column of Wₓₕ = [0.287027, 0.902874, 0.537524]ᵀ

**Step 2:** Wₕₕ × hₜ₋₁ + bias (first step: hₜ₋₁=0, bias=0.567):
= [0.567001, 0.567001, 0.567001]ᵀ

**Step 3:** hₜ = tanh(step1 + step2):
```
hₜ = tanh([0.287027+0.567001, 0.902874+0.567001, 0.537524+0.567001])
   = tanh([0.854028, 1.469875, 1.104525])
   = [0.693168, 0.899554, 0.802118]
```

**Step 4:** Output yₜ = Wₕᵧ × hₜ = [1.797, 1.049, 0.801, 1.041]ᵀ

**Step 5:** Softmax(yₜ) gives probabilities over {h, e, l, o} → predict next character.

**Step 6:** Continue for letter 'e' — the previous hₜ (from 'h') becomes hₜ₋₁ for the next step.

---

### Limitations of Simple (Vanilla) RNN

**1. Vanishing Gradient Problem:**
During BPTT, gradients are multiplied together repeatedly as they flow backward. If the weight matrix has values < 1, gradients shrink exponentially with each time step → gradients from early time steps become practically zero → **the model cannot learn long-range dependencies** (it "forgets" what happened far back).

Effect: Performance of simple RNN degrades significantly for longer sequences — it has only short-term memory.

**2. Exploding Gradient Problem:**
If weights > 1, gradients grow exponentially → training becomes unstable (NaN values, model diverges).

**3. Short-Term Memory:**
Due to vanishing gradients, Vanilla RNN can only effectively use context from a few recent time steps.

---

## 15.3 LSTM — Long Short-Term Memory

### Motivation
Consider these two sentences:

**Short context:** "The sky is ___." → RNN can predict "blue" (recent context sufficient)

**Long context:** "There is fire in the forests of Cardiff and it's snowing ash everywhere making the sky ___."
→ RNN fails! The relevant clue ("fire", "ash") is many steps back → vanishing gradient → RNN forgets → predicts "blue" instead of "grey/red/dark"

**LSTM fixes this** by maintaining a separate **cell state (Cₜ)** — a "highway" that allows information to flow across many time steps with minimal modification, solving the vanishing gradient problem.

### LSTM: Two States

| State | Type | Purpose |
|-------|------|---------|
| **Cell state (Cₜ)** | Long-term memory | Stores important information across long distances |
| **Hidden state (hₜ)** | Short-term memory | Current output used for predictions |

### LSTM Cell: Three Gates

**Gate 1 — Forget Gate: What to discard from memory?**
```
fₜ = σ(Wf · [hₜ₋₁, xₜ] + bf)
```
- Sigmoid output: 0 (forget completely) to 1 (keep completely)
- Applies element-wise to the cell state: fₜ ⊙ Cₜ₋₁

**Gate 2 — Input Gate: What new information to add?**
```
iₜ = σ(Wᵢ · [hₜ₋₁, xₜ] + bᵢ)      ← how much to update (sigmoid: 0 to 1)
C̃ₜ = tanh(Wc · [hₜ₋₁, xₜ] + bc)   ← candidate values to add (tanh: -1 to 1)
```
- Sigmoid (iₜ): decides which values to update
- Tanh (C̃ₜ): gives the actual new values (weighted importance)

**Cell State Update:**
```
Cₜ = fₜ ⊙ Cₜ₋₁ + iₜ ⊙ C̃ₜ
```
= (Keep some of old memory) + (Add new relevant information)

**Gate 3 — Output Gate: What to output?**
```
oₜ = σ(Wo · [hₜ₋₁, xₜ] + bo)      ← which part of cell state to output
hₜ = oₜ ⊙ tanh(Cₜ)                 ← final hidden state
```
- Sigmoid decides which part of the cell state is relevant right now
- tanh squashes values to range [-1, 1]

### Complete LSTM Equations:
```
fₜ = σ(Wf · [hₜ₋₁, xₜ] + bf)      Forget gate
iₜ = σ(Wᵢ · [hₜ₋₁, xₜ] + bᵢ)      Input gate (sigmoid)
C̃ₜ = tanh(Wc · [hₜ₋₁, xₜ] + bc)   Input gate (tanh/candidates)
Cₜ = fₜ ⊙ Cₜ₋₁ + iₜ ⊙ C̃ₜ          Cell state update
oₜ = σ(Wo · [hₜ₋₁, xₜ] + bo)      Output gate
hₜ = oₜ ⊙ tanh(Cₜ)                 Hidden state output
```
(σ = sigmoid function, ⊙ = element-wise multiplication)

### LSTM Example Intuition (from PPT):

**Example 1 (remember the right thing):**
"Sengar likes to eat **samosas** on every Sunday, which is a popular **cuisine** in -------."
- LSTM uses input gate to store "samosas" (keyword) in cell state
- Forget gate discards irrelevant "Sunday", "every"
- Output gate produces "India" (because samosa = Indian food)

**Example 2 (forget old, remember new):**
"Sengar likes to eat **samosas** — famous in **India**. His friend Roy likes to eat **pasta** and **cheese**, which is famous in -------."
- LSTM's forget gate removes "samosas/India" when new context arrives
- Input gate stores "pasta" and "cheese" as new relevant keywords
- Predicts: **Italy** ✓

### LSTM vs Simple RNN Comparison:
| Feature | Simple RNN | LSTM |
|---------|-----------|------|
| Memory type | Short-term only | Short-term (hₜ) + Long-term (Cₜ) |
| Vanishing gradient | Severe problem | Mostly solved via cell state highway |
| Gates | None | 3 gates (Forget, Input, Output) |
| States | 1 (hₜ) | 2 (Cₜ and hₜ) |
| Long sequences | Poor performance | Good performance |
| Complexity | Simple | More complex |
| Training | Faster (simpler) | Slower (more parameters) |

---

# 16. Real-World NLP Applications

| Application | Description | Key Techniques |
|-------------|-------------|----------------|
| **Text Processing** | Detect language, writing quality, entity mentions, POS tags, dates, locations, sentiment | Tokenization, POS tagging, NER |
| **Morph Analyzer** | Analyzes morphology of input words. Detects morphemes of any text. Works at word and phrase levels. | Morphological rules, finite-state transducers |
| **POS Tagger** | Software that reads text and assigns grammatical category to each word (noun, verb, adjective, etc.). Often uses fine-grained tags like "noun-plural". | HMM, Viterbi, neural taggers |
| **Parsing** | Receives input text and breaks it into grammatical parts (nouns/objects, verbs/methods, attributes) that can be managed by other programs. | CFG, PCFG, CYK |
| **Machine Translation** | Translates text automatically with no human involvement. Uses combination of language rules, grammar, and dictionaries for common words. | Seq2Seq, Transformer |
| **Speech Processing** | Study of speech signals and their processing methods. Signals usually processed in digital form — NLP for audio. | HMM, RNN, CTC |
| **Text-to-Speech (TTS)** | Converts normal language text into speech. Synthesized speech created by concatenating pieces of recorded speech stored in a database. | Concatenative synthesis, Neural TTS |
| **Sentiment Analysis** | Determine opinion (positive/negative/neutral) from text. | BoW, CNN, LSTM, BERT |
| **NER (Named Entity Recognition)** | Find and classify names, places, dates, organizations. | HMM, CRF, BERT |
| **Question Answering** | Answer questions from text. | BERT, Transformers |
| **Spell/Grammar Checking** | Correct errors based on language model probabilities and edit distance. | Edit Distance, N-gram LM |
| **Summarization** | Condense long documents to key points. | Extractive, Abstractive |
| **Chatbots / Dialogue** | Conversational AI systems. | Transformers, RLHF (ChatGPT style) |
| **Information Retrieval** | Search engines — find relevant documents for a query. | TF-IDF, BM25, Dense Retrieval |

---

# 17. Quick Revision Cheat Sheet

## All Key Formulas

```
═══════════════════════════════════════════════════════════
LANGUAGE MODELS (N-GRAM)
═══════════════════════════════════════════════════════════
BIGRAM MLE:     P(w | prev) = C(prev, w) / C(prev)
UNIGRAM MLE:    P(w) = C(w) / N_total

ADD-ONE:        P(w | prev) = (C(prev,w) + 1) / (C(prev) + V)

GOOD-TURING:    c* = (c+1) × N(c+1) / N(c)
                P*(unseen) = N(1) / N_total

SENTENCE PROB:  P(<s> w₁ w₂ ... wₙ </s>)
                = P(w₁|<s>) × P(w₂|w₁) × ... × P(</s>|wₙ)

═══════════════════════════════════════════════════════════
INFORMATION THEORY
═══════════════════════════════════════════════════════════
ENTROPY:        H = -∑ P(x) log₂P(x)
CROSS-ENTROPY:  H(P,Q) = -∑ P(x) log₂Q(x)
                = -(1/N) × ∑ log₂Q(wᵢ|wᵢ₋₁)
PERPLEXITY:     PP = 2^H  OR  P(W)^(-1/N)
Relation:       lower PP = lower H = better model

═══════════════════════════════════════════════════════════
TEXT REPRESENTATION
═══════════════════════════════════════════════════════════
TF:             TF(t,d) = count(t,d) / total_words(d)
IDF:            IDF(t) = log(N / df(t))
TF-IDF:         TF-IDF(t,d) = TF(t,d) × IDF(t)

COLLOCATION:    PMI(x,y) = log₂[P(x,y) / (P(x) × P(y))]

═══════════════════════════════════════════════════════════
EDIT DISTANCE
═══════════════════════════════════════════════════════════
dp[i][0] = i,  dp[0][j] = j
If s1[i] == s2[j]: dp[i][j] = dp[i-1][j-1]
Else: dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])

═══════════════════════════════════════════════════════════
WSD — LESK ALGORITHM
═══════════════════════════════════════════════════════════
Basic Lesk:     Sense = argmax_s |Context ∩ Gloss(s)|
Advanced Lesk:  Extend Gloss with hypernyms, hyponyms, synonyms

═══════════════════════════════════════════════════════════
VITERBI (HMM DECODING)
═══════════════════════════════════════════════════════════
INIT:   v₁(j) = π(j) × b(j, o₁)
REC:    vₜ(j) = max_i[vₜ₋₁(i) × a(i,j)] × b(j, oₜ)
TERM:   best = argmax_j[vT(j)]  then backtrack

═══════════════════════════════════════════════════════════
CYK PARSING
═══════════════════════════════════════════════════════════
Table[i][i] = {A | A → wᵢ}
Table[i][j] = {A | A→BC, B∈Table[i][k], C∈Table[k+1][j]}
Valid if: S ∈ Table[1][n]

PCFG: P(tree) = ∏ P(each rule used)

═══════════════════════════════════════════════════════════
RNN
═══════════════════════════════════════════════════════════
HIDDEN:  hₜ = tanh(Wₓₕ·xₜ + Wₕₕ·hₜ₋₁)
OUTPUT:  yₜ = Wₕᵧ·hₜ

═══════════════════════════════════════════════════════════
LSTM GATES
═══════════════════════════════════════════════════════════
FORGET:  fₜ = σ(Wf·[hₜ₋₁,xₜ] + bf)         → what to forget
INPUT:   iₜ = σ(Wᵢ·[hₜ₋₁,xₜ] + bᵢ)          → how much to update
CAND:    C̃ₜ = tanh(Wc·[hₜ₋₁,xₜ] + bc)        → new candidate values
CELL:    Cₜ = fₜ ⊙ Cₜ₋₁ + iₜ ⊙ C̃ₜ            → update long-term memory
OUTPUT:  oₜ = σ(Wo·[hₜ₋₁,xₜ] + bo)          → what to output
HIDDEN:  hₜ = oₜ ⊙ tanh(Cₜ)                  → short-term memory output
═══════════════════════════════════════════════════════════
```

## Key One-Line Definitions

| Term | Definition |
|------|------------|
| NLP | AI subfield enabling computers to understand/generate human language |
| Morpheme | Smallest meaningful unit of language |
| Inflection | Word form change without changing POS or core meaning (dog→dogs) |
| Derivation | Creating new words by adding affixes, often changing POS (teach→teacher) |
| Stemming | Chop suffix to get root form — fast but may give non-real words |
| Lemmatization | Get dictionary base form — slower but always returns real word |
| Tokenization | Split text into smaller units (words, subwords, or characters) |
| Corpus | Large structured collection of text for NLP training and research |
| Collocation | Word pair appearing together more often than chance |
| PMI | Measure of how much more often two words co-occur than by chance |
| N-gram | Sequence of N consecutive words |
| MLE | Estimate probability by counting occurrences in training data |
| Smoothing | Redistribute probability from seen to unseen events (fix zero probs) |
| Entropy | Measure of uncertainty in a probability distribution (bits) |
| Cross-Entropy | How confused is the model predicting real data (entropy + model error) |
| Perplexity | Effective number of word choices per step; lower = better model |
| BoW | Represent document as word count vector, ignoring order |
| TF-IDF | Weight words by frequency in document × rarity across all documents |
| POS Tagging | Assign grammatical category (noun, verb, adj...) to each word |
| WSD | Determine which sense of an ambiguous word is used in context |
| Lesk Algorithm | WSD: pick sense whose gloss has max overlap with context |
| Advanced Lesk | Extend gloss with WordNet hypernyms/hyponyms for better coverage |
| HMM | Probabilistic model with hidden states emitting observable outputs |
| Viterbi | DP algorithm to find the most likely hidden state sequence in HMM |
| CFG | Grammar rules: non-terminals produce sequences of symbols |
| CNF | CFG where every rule is A→BC or A→a (required for CYK) |
| CYK | Bottom-up DP parsing algorithm that works on CNF grammars |
| PCFG | CFG with probabilities on rules; resolves parse tree ambiguity |
| CNN (NLP) | Uses convolution filters to capture local n-gram text features |
| RNN | Neural net with recurrent connections for sequential/temporal data |
| LSTM | RNN with forget/input/output gates; handles long-range dependencies |
| Edit Distance | Min operations (insert/delete/substitute) to transform one string to another |
| Lexical Ambiguity | Single word has multiple meanings or POS categories |
| Syntactic Ambiguity | Sentence has multiple valid grammatical structures |
| Semantic Ambiguity | Sentence meaning unclear even after syntax and words resolved |
| Anaphoric Ambiguity | Pronoun could refer to more than one earlier noun |
| Pragmatic Ambiguity | Speaker's intent is unclear from the sentence alone |

---

*End of NLP Complete Exam Notes*
*All 16 ETE Syllabus Topics Covered — with Formulas, Descriptions, Examples, and Numericals*
