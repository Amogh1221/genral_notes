# Information Retrieval (CSE4144) — Midterm Notes
### Covers Lectures 1–22

---

# UNIT 1: Introduction, Boolean Retrieval, Inverted Index, Document Processing, Tolerant Retrieval

## Lecture 1–2: Introduction to IR and Basic Concepts

### What is Information Retrieval?
Information Retrieval (IR) deals with the **organization, storage, retrieval and evaluation** of information relevant to a user's need (query). The query is written in natural language, and the retrieval system responds by retrieving **documents** that seem relevant to the query.

> **Important nuance:** An IR system does **not** "inform" (change the knowledge of) the user about the subject of their inquiry directly. It merely informs on the **existence (or non-existence) and whereabouts** of documents relating to the request. IR traditionally returns *documents containing the information*, not the actual information/answer itself.

### What is a Document?
A document is any unit of information that can be retrieved by an IR system.
- Examples: PDF files, Word documents, emails, research papers, news articles, web pages, social media posts.

**Example:** Search → "Machine Learning". Retrieved documents: ML Tutorial PDF, Wikipedia page, Research paper, Blog article.

### Document Collection (Corpus)
A **collection/corpus** is a set of documents stored together for searching.
- Google Index → Billions of Web Pages
- Library Database → Books
- Research Repository → Research Papers
- Company Database → Reports

### What is a Query?
A **query** is the information need expressed by the user.

**Example:** User wants "Best restaurants in Jaipur" → User enters "restaurants Jaipur" → IR system interprets and returns relevant documents.

**Types of Queries:**
1. Keyword Query
2. Natural Language Query
3. Boolean Query — e.g., `AI AND Healthcare NOT Sports`
4. Phrase Query — e.g., `"Machine Learning"`

### The IR Process (pipeline)
1. User enters a query through the user interface.
2. The query is processed and refined.
3. Documents in the database are preprocessed and indexed.
4. The system matches the processed query with indexed documents.
5. Relevant documents are ranked based on their relevance.
6. The ranked results are displayed to the user.

### IR vs Databases: Structured vs Unstructured Data

**Structured data** — refers to information organized in tables (rows/columns). Typically allows numerical range and exact-match queries.
- Example: `Salary < 60000 AND Manager = Smith` on an Employee table.

**Unstructured data** — refers to free text.
- Allows keyword queries (including operators) and more sophisticated "concept" queries, e.g., "find all web pages dealing with drug abuse."
- This is the classic model for searching text documents.

**Semi-structured data** — contains both structured and unstructured information; doesn't follow a fixed table format but uses tags/labels/metadata for organization. Easier to search than plain text because structure helps identify important fields.
- Characteristics: flexible schema, contains metadata (tags/attributes), hierarchical organization, supports field-specific search.
- Examples: XML, JSON, HTML web pages, medical records.

| Structured | Semi-Structured | Unstructured |
|---|---|---|
| Database records | XML | Text |
| Tables | HTML | PDF |
| CSV | JSON | Images |
| Excel | Metadata | Web pages |

**Key point:** Most IR systems deal with **unstructured documents**.

### Motivating Example for IR
*"Which plays of Shakespeare contain the words Brutus AND Caesar but NOT Calpurnia?"*

- Simplest approach: `grep` all of Shakespeare's plays for Brutus and Caesar, then strip out lines containing Calpurnia.
- Problems:
  - **Slow** for large corpora (linear scan).
  - `NOT Calpurnia` is non-trivial to handle via grep.
  - Other operations (e.g., find "Romans" NEAR "countrymen") are not feasible with grep.
  - Doesn't support **ranked retrieval** (returning the *best* documents).

### How to Avoid Linear Scanning
**Index the documents in advance.**

**Indexing** = the process of transforming document text into some representation of it. The most commonly used index data structure in IR is the **inverted index**.

### IR Models
An **IR model** is a pattern that defines:
1. How documents and queries are represented.
2. How the system retrieves relevant documents according to the query.
3. How retrieved documents are ranked.

An IR model consists of:
- A model for documents
- A model for queries
- A matching function comparing queries to documents
- A ranking function

### Classical IR Models
Based on mathematical foundations that are simple, well understood, efficient, and easy to implement. The three classical models are:
1. **Boolean model**
2. **Vector model**
3. **Probabilistic model**

In general, the actual text isn't used directly for matching — instead documents are represented (commonly as a **set of index terms/keywords**), and this representation is matched against the query representation.

---

## Lecture 3–4: Boolean Retrieval Model

### Boolean Model — Overview
- The **oldest** of the three classical models.
- Based on **Boolean logic** and **classical set theory**.
- Represents documents as a set of keywords, usually stored in an **inverted file**.
- Users express queries as Boolean expressions of keywords connected by **AND, OR, NOT**.
- Retrieval is performed strictly based on whether a document contains the query terms (**exact match**).

### Formal Definition
Given:
- A finite set of index terms `T = {t1, t2, ..., ti, ..., tm}`
- A finite set of documents `D = {d1, d2, ..., dj, ..., dn}`
- A Boolean expression (in normal form) representing query `Q`

Processing:
1. For each term `ti`, obtain the set `Ri` of documents that do/do not contain `ti`.
2. Use **set operations** (∩, ∪, complement) on the `Ri` sets to answer `Q`.

### Worked Example: Shakespeare's Plays
**Query:** Which plays contain **Brutus AND Caesar** but **NOT Calpurnia**?

**Binary term–document incidence matrix:** 1 if the play contains the word, 0 otherwise (one row per term, one column per play).

We get a 0/1 vector per term. To answer the query: take vectors for Brutus, Caesar, and Calpurnia (complemented), and take the **bitwise AND**.

`110100 AND 110111 AND 101111 = 100100`

**Answers found:** *Antony and Cleopatra* (Act III, Scene ii) and *Hamlet* (Act III, Scene ii).

### Full Step-by-Step Query Processing Example
Query: `Q = (Antony ∧ Caesar) ∧ ¬Brutus`

**Step 1: Antony ∧ Caesar**

| Document | Antony | Caesar | Antony ∧ Caesar |
|---|---|---|---|
| Antony & Cleopatra | 1 | 1 | 1 |
| Julius Caesar | 1 | 1 | 1 |
| The Tempest | 0 | 0 | 0 |
| Hamlet | 0 | 1 | 0 |
| Othello | 0 | 1 | 0 |
| Macbeth | 1 | 1 | 1 |

**Step 2: ¬Brutus**

| Document | Brutus | ¬Brutus |
|---|---|---|
| Antony & Cleopatra | 1 | 0 |
| Julius Caesar | 1 | 0 |
| The Tempest | 0 | 1 |
| Hamlet | 1 | 0 |
| Othello | 0 | 1 |
| Macbeth | 0 | 1 |

**Final Result:**

| Document | Antony ∧ Caesar | ¬Brutus | Result |
|---|---|---|---|
| Antony & Cleopatra | 1 | 0 | 0 |
| Julius Caesar | 1 | 0 | 0 |
| The Tempest | 0 | 1 | 0 |
| Hamlet | 0 | 0 | 0 |
| Othello | 0 | 1 | 0 |
| Macbeth | 1 | 1 | **1** |

**Retrieved Document: Macbeth**

> Boolean retrieval answers any query in the form of a Boolean expression of terms; such queries view documents as a **set of words**.

### Problem With the Matrix Approach: Scaling
Consider **N = 1,000,000 documents**, each with about 1000 terms, avg 6 bytes/term (incl. punctuation) → **6 GB of data**. Say there are **m = 500,000** distinct terms.

**Can't build the full matrix:**
- A 500K × 1M matrix has half-a-trillion 0's and 1's.
- But it has no more than **one billion 1's** → the matrix is **extremely sparse**.
- Better representation: **only record the 1 positions** (this motivates the inverted index).

### Limitations of Naive Boolean/Matrix Retrieval
- Sequential scanning of all documents.
- Redundant processing — documents without any query terms are still examined.
- High search time: **O(N × M)**.
- Complex Boolean expressions become computationally expensive.

> To overcome redundant processing and reduce search time → the concept of the **INVERTED INDEX**.

---

## Lecture 5–6: Inverted Index

### Motivation
For each term T, we must store a list of all documents that contain T.

Example (Brutus, Calpurnia, Caesar → lists of docIDs). We could use an array or a list. Question raised: *What happens if the word Caesar is added to document 14?* (Motivates using linked lists instead of arrays for postings.)

### Structure of an Inverted Index
- **Linked lists are generally preferred over arrays** for postings because:
  - Dynamic space allocation.
  - Insertion of new documents/terms is easy.
  - (Trade-off: space overhead of pointers.)
- **Dictionary**: sorted list of terms (the vocabulary).
- **Postings list**: for each term, a list of documents (docIDs) containing it, sorted by docID. Each entry in a postings list is called a **posting**.

### Inverted Index Construction Pipeline
```
Documents → Tokenizer → Token stream → Linguistic modules → Modified tokens → Indexer → Inverted Index
```
Example: "Friends Romans Countrymen" → tokens `Friends, Romans, Countrymen` → normalized to `friend, roman, countryman` → indexer builds postings, e.g., `friend → [2,4]`, `roman → [1,2]`, `countryman → [16,13]`.

### Indexer Steps (detailed)
1. **Sequence of (Modified token, Document ID) pairs** is generated.
   - Doc 1: "I did enact Julius Caesar I was killed i' the Capitol; Brutus killed me."
   - Doc 2: "So let it be with Caesar. The noble Brutus hath told you Caesar was ambitious."
2. **Sort by terms** (the core indexing step) — sorted primarily by term, secondarily by docID.
3. **Merge** multiple entries of the same term in a single document; **add frequency information**.
4. **Split the result** into a **Dictionary file** and a **Postings file**.
   - Dictionary: terms + pointers (+ document frequency).
   - Postings: list of docIDs (+ term frequency) per term.

### Query Processing: AND (Merge Algorithm)
Consider query: **Brutus AND Caesar**
1. Locate Brutus in the Dictionary; retrieve its postings.
2. Locate Caesar in the Dictionary; retrieve its postings.
3. **"Merge" (intersect)** the two postings lists.

**The Merge:** Walk through both postings lists simultaneously (both sorted by docID), in time **linear** in the total number of postings entries. If list lengths are `x` and `y`, the merge takes **O(x + y)** operations. This is only correct because postings are **sorted by docID**.

**Intersection Algorithm — `INTERSECT(p, q)`:**
```
1. Start
2. Ans ← ()
3. While p ≠ nil and q ≠ nil do
      if p.docID = q.docID
          then ADD(answer, p.docID)   // add to result and advance both pointers
               p ← p.next
               q ← q.next
      else if p.docID < q.docID
          then p ← p.next
          else q ← q.next
4. End
```

**Union Algorithm — `UNION_MERGE(P1, P2)`:**
```
1. While both lists are not empty
      If P1 = P2
          Output P1; Move both pointers
      Else if P1 < P2
          Output P1; Move P1
      Else
          Output P2; Move P2
2. Output remaining elements of P1.
3. Output remaining elements of P2.
End
```

### Why Boolean Queries Still Matter
- Boolean queries are an **exact match**: a document either matches the condition or not.
- Boolean retrieval was the **primary commercial retrieval tool for 3 decades**.
- Professional searchers (e.g., lawyers) still like Boolean queries.
- **Example: WestLaw** (http://www.westlaw.com/) — largest commercial legal search service (started 1975; ranking added 1992), tens of terabytes of data, 700,000 users, and the **majority of users still use Boolean queries**.

### Query Optimization
**Goal:** organize query processing to minimize total work.

For an AND query of *t* terms: get postings for each term, then intersect them all — but the **order matters**.

**Rule:** Process terms in order of **increasing document frequency (freq)** — start with the smallest postings list, then keep intersecting with progressively larger ones. This is why document frequency is kept in the dictionary.

**Example:** Given postings sizes: Brutus (7 items), Calpurnia (8 items), Caesar (2 items) → query `Brutus AND Calpurnia AND Caesar` should be executed as **(Caesar AND Brutus) AND Calpurnia** (smallest lists first).

**More general optimization** — e.g., `(madding OR crowd) AND (ignoble OR strife)`:
1. Get document frequencies for all terms.
2. Estimate the size of each OR sub-expression by the **sum of its frequencies** (a conservative/upper-bound estimate).
3. Process in **increasing order of OR sizes**.

### Beyond Simple Term Search
- **Phrases** — e.g., "Indian Institute of Information Technology."
- **Proximity** — e.g., find `Murty NEAR Infosys`. Requires the index to capture **position information**.
- Combined structured + text queries, e.g., `(author = Zufrasky) AND (text contains Retrieval)`.

### Proximity Search
The **proximity operator** retrieves documents where two or more terms occur close together within a specified distance — improves accuracy by considering word distance, not just presence.

**Syntax varies by system:**
- `NEAR/n`: two words must appear within *n* words of each other.
- `WITHIN n`: similar, specifying maximum allowed distance.
- Example: `"machine NEAR/3 learning"`.

**Proximity list / Positional index:** instead of storing only document IDs, store `Term → (Document ID, [Positions])`.

**Boolean AND vs Proximity Operator:**

| Boolean AND | Proximity Operator |
|---|---|
| Retrieves documents containing both terms anywhere | Retrieves documents where terms occur within a specified distance |
| Ignores word order and distance | Considers distance (and sometimes order) |
| May return many irrelevant documents | Produces more relevant, context-specific results |

### Beyond Presence/Absence
- Consider: 1 vs 0 occurrences of a term, 2 vs 1, 3 vs 2, etc. — usually "more seems better."
- This requires **term frequency information** in documents (a stepping stone toward ranked retrieval, covered later with TF-IDF).

### Ranking, Clustering, and Beyond
- Boolean queries only give inclusion/exclusion of documents and require precise query-expression skills (vs. free text). Often we instead want to **rank/group** results.
- **Clustering/classification:** Given a set of docs, group into clusters based on content; given topics + a new doc D, decide which topic(s) D belongs to.
- **The Web** brings unusual/diverse documents, users, and needs — beyond terms, using ideas like **link analysis**, **clickstreams**, etc.
- More sophisticated IR: cross-language IR, question answering, summarization, text mining.

---

## Lecture 7–9: Document Processing (Term Vocabulary & Postings Lists)

### Parsing a Document
Before indexing, we must determine:
- What **format** is it in? (pdf/word/excel/html?)
- What **language** is it in?
- What **character set** is in use?

Each is essentially a classification problem, but is often handled **heuristically**.

**Complications — Format/Language:**
- A collection may include documents in many different languages; a single index may need to contain terms from several languages.
- A single document (or its components) can contain multiple languages/formats, e.g., a **French email with a German PDF attachment**.
- What counts as a "unit document"? Is a file always one document?

### Tokenization
**Tokenization** converts a character stream into a stream of tokens (candidate index terms).

**Example:** Input `"Books, Chapters and Lecture"` → Output tokens: `Books`, `Chapters`, `Lecture`.

**Issues in Tokenization:**
- `India's capital` → tokenize as `India`? `Indias`? `India's`?
- `Hewlett-Packard` → should this be split into `Hewlett` and `Packard` as two tokens? (State-of-the-art: break up hyphenated sequences, e.g., `co-education`.) It can help to let the **user** put in possible hyphens.
- `New Delhi` → one token or two? How do you decide it's a single token?

**Numbers:**
- Examples: `18/1/07`, `Jan. 18, 2007`, `1400 B.C.`, `B-52`, an authentication key like `324a3df234cb23e`, an IP `100.2.86.144`.
- Often numbers are **not** indexed as plain text, but they can be very useful (e.g., looking up error codes) — **n-grams** can help here.
- **Metadata** (creation date, format, etc.) is often indexed separately.

**Language Issues:**
- French: `L'ensemble` — one token or two? `L`? `L'`? `Le`? We want `l'ensemble` to match `un ensemble`.
- Chinese/Japanese have **no spaces between words** (e.g., 莎拉波娃现在居住在美国东南部的佛罗里达). Tokenization is not always unique. Japanese is further complicated by multiple intermingled alphabets, and dates/amounts appear in multiple formats.
- Urdu/Arabic/Hebrew: written right-to-left, but items like numbers are written left-to-right. Words are separated, but letters within a word form complex **ligatures**. Example: *"استقلت الجزائر في سنة 1962 بعد 132 عاما من الاحتلال الفرنسي"* → "Algeria achieved its independence in 1962 after 132 years of French occupation."

### Normalization
We need to **normalize** terms in both indexed text and queries into the same form so they match — e.g., we want `U.S.A.` to match `USA`.

- Most commonly, we implicitly define **equivalence classes** of terms (e.g., by deleting periods in a term).
- **Alternative — asymmetric expansion:**
  - Enter: `window` → Search: `window, windows`
  - Enter: `windows` → Search: `Windows, windows`
  - Enter: `Windows` → Search: `Windows`

**Normalization in other languages:**
- Accents: `résumé` vs `resume`. Most important criterion: **how do your users actually type their queries?** Even in languages with accents, users often omit them when typing.

### Case Folding
- Reduce all letters to **lower case**.
- Exception: upper case in mid-sentence might matter (e.g., "General Motors", "SAIL" vs "sail").
- In practice, it's often best to lowercase everything, since users type lowercase regardless of "correct" capitalization.

### Stop Words
A **stop list** excludes the commonest words entirely from the dictionary.
- **Intuition for removing them:** they take a lot of space (~30% of postings are for the top 30 words); they carry little semantic content (`the`, `a`, `and`, `to`, `be`).
- **But the trend is away from removing them**, because you need stop words for:
  - **Phrase queries:** e.g., "President of India".
  - Song/movie titles etc.: "Let it be", "To be or not to be".
  - **"Relational" queries:** e.g., "flights to Bangalore".

### Thesauri and Handling Synonyms
Handle synonyms and homonyms via **hand-constructed equivalence classes**, e.g., `car = automobile`. Also consider polysemous words like *saw*, *bank*.
- **Rewrite** to form equivalence classes — index such equivalences (when a document contains "automobile", index it under "car" as well, usually vice versa too).
- **Or expand the query:** when the query contains "automobile", also look under "car".

### Soundex (brief intro here; detailed later in Lecture 11–12)
- Traditional class of heuristics to expand a query into **phonetic equivalents**.
- Language-specific — mainly for names.
- Example: `chebyshev → tchebycheff`.

### Lemmatization
**Reduce inflectional/variant forms to a base (dictionary) form.**
- Examples: `am, are, is → be`; `car, cars, car's, cars' → car`.
- Sentence example: *"the boy's cars are different colors"* → *"the boy car be different color"*.
- Lemmatization implies doing "proper" reduction to the dictionary headword form (requires understanding of morphology/POS).

### Stemming
**Reduce terms to their "roots" before indexing.** Stemming is generally cruder than lemmatization — it suggests crude **affix chopping**, and is language dependent.
- Example: `automate(s), automatic, automation` → all reduced to `automat`.
- Example: "for example compressed and compression are both accepted as equivalent to compress" → stemmed to something like "for exampl compress and compress ar both accept as equival to compress."

### Porter's Algorithm (Porter Stemmer)
- The most widely known algorithm for **stemming English**.
- Results are at least as good as other stemming options.
- Structure: a set of **conventions** plus **5 phases (steps)** of reductions, applied **sequentially**; each phase/step consists of a set of commands/rules.
- **Sample convention:** among the rules in a compound command, select the one that applies to the **longest suffix**.

**Typical rules (general form: `old_suffix → new_suffix`):**
- If a word ends in `"ies"` but not `"eies"` or `"aies"`, then `ies → y`.
- `sses → ss`
- `ies → i`
- `ational → ate`
- `tional → tion`
- Weighted (measure-sensitive) rule: `(m>1) ation → NULL` → e.g., `excitation → excit`, but `nation → nation` (unchanged, since condition fails).

**Porter Stemmer structure:** Consists of **condition/action rules**. A condition may be based on:
- Stems
- Suffix
- Rules (whether a previous rule was used)

**Stem Conditions (used in rule conditions):**
1. **Measure (m)** = number of VC (vowel-consonant) sequences in the stem. `m=0` (e.g., *tree*), `m=1` (e.g., *trees, oak*), `m=2` (e.g., *private*).
2. `*<X>` — the stem ends with a given letter X.
3. `*v*` — the stem contains a vowel.
4. `*d` — the stem ends in a double consonant.
5. `Suffix:` current suffix matches a pattern.
6. `Rule:` a specified rule was used previously.

**Action rules** rewrite `old_suffix → new_suffix`. Rules are divided into steps; within a step, rules are examined in sequence and **only one rule per step can apply**.

- **Step 1a rules:** `ssess → ss`, `ies → i`, `ss → ss`, `s → NULL`.
- **Step 1b rules:** `(m>0) eed → ee` (e.g., *feed→feed*, *agreed→agree*); `(*v*) ed → NULL` (e.g., *plastered→plaster*); `(*v*) ing → NULL` (e.g., *motoring→motor*, *sing→sing*); `at → ate` (e.g., *conflat(ed)→conflate*).
- **Step 1c:** `(*v*) y → i` (e.g., *happy→happi*, *sky→ski*).
- **Step 2 rules:** `(m>0) ational → ate` (e.g., *relational→relate*).
- **Step 3 rules:** `(m>0) cate → ic`; `(m>0) ative → NULL`.

**Stemmers are not perfect** — example over-stemming errors:
- `Organization → organ`
- `University → universe`
- `Policy → police`

### Other Stemmers
- **Lovins stemmer** — single-pass, longest suffix removal (about 250 rules), motivated by linguistics as well as IR.
- Full morphological analysis gives at most **modest benefits** for retrieval.
- Do stemming/normalization help? Results are **often mixed** — can really help **recall** for some queries but harm **precision** on others.

### Language-Specificity
Many of the above normalization features embody transformations that are **language-specific** and often **application-specific**. These are typically implemented as "plug-in" addenda to the indexing process (both open-source and commercial plug-ins exist).

---

### Faster Postings Merges: Skip Pointers

**Recall basic merge:** Walking through two postings lists simultaneously takes **O(m+n)** for list lengths m and n. Can we do better?
- **Yes, if the index isn't changing too fast** — by using **skip pointers**.

**Idea:** Augment postings with skip pointers at **indexing time**. A skip pointer lets the merge algorithm jump ahead past postings that cannot be part of the answer, avoiding unnecessary comparisons.

**Example (from slides):**
- Top list (with skips): `2, 4, 8(→skip to 16), 16, 32, 64, 128`
- Bottom list (with skips): `1, 2, 3, 5, 8(→skip to 31), 17, 21, 31`
- When we reach **16** on the top list, its successor is **32**.
- Meanwhile, we've stepped through both lists to **8** on each. The skip successor of 8 on the bottom list is **31** — so once we know 16 (top) won't match anything before 31 (bottom), we can **skip ahead past the intervening postings** on the bottom list instead of checking them one by one.

**Placing Skip Pointers — Tradeoffs:**
- **More skips** → shorter skip spans → more likely to successfully skip, but more comparisons against skip pointers (overhead).
- **Fewer skips** → fewer pointer comparisons, but longer skip spans → fewer *successful* skips.
- **Simple heuristic:** for a postings list of length **L**, use **√L evenly-spaced skip pointers**. (Note: this simple heuristic ignores the actual distribution of query terms.)
- Easy to compute if the index is relatively static; harder if L keeps changing due to updates.
- **Caveat (modern hardware):** This technique definitely used to help; with modern hardware it may not, because the cost of loading a bigger postings list can outweigh the gain from faster in-memory merging (Bahle et al. 2002).

---

## Lecture 10: Dictionary Data Structures (Search Structures for the Dictionary)

*(Context needed for wildcard/tolerant retrieval — the dictionary of terms must be stored in a structure that supports fast lookup.)*

Two broad choices for organizing the term dictionary:
- **Hash tables:** Each vocabulary term is hashed to an integer. Lookup of an exact term is **O(1)** on average — very fast for exact-match queries. **Drawback:** cannot easily support prefix/range/wildcard queries (e.g., "all terms starting with *mon*") because hashing destroys lexicographic order; also, no easy way to find minor variants of a query term (e.g., a misspelling), and hash tables must be resized as vocabulary grows.
- **Search Trees / B-Trees:** Terms are stored in **lexicographic (alphabetical) order**, typically in a balanced tree such as a B-tree. This supports:
  - Exact-match lookup in O(log M) for M terms.
  - **Range/prefix queries** efficiently (e.g., all terms between `hot` and `hyphen`), because lexicographically adjacent terms are stored near each other.
  - This ordered structure is exactly what is exploited by **wildcard queries** in Lecture 11–12 below.

This sets up the next topic — how B-trees are used to answer wildcard queries efficiently.

---

## Lecture 11–12: Tolerant Retrieval

### Part 1: Wild-card Queries

**1. Trailing Wildcard Queries** — e.g., `fan*` (matches *fanatic, fancy, fantasy*, etc.)
- Use a **B-tree** data structure on the dictionary.
- Walk down the tree following `f, a, n`.
- Retrieve all words `w` such that `fan ≤ w < fao` (i.e., all words with prefix "fan"). Call this set `W`.
- Use the inverted index to retrieve documents containing terms in `W`.

**2. Leading Wildcard Queries** — e.g., `*tic` (find words ending in "tic") — **harder**, because a normal B-tree orders by the *beginning* of the word.
- Solution: maintain an **additional B-tree for terms stored backwards (reversed)**.
- Can then retrieve all words in range `cit ≤ w < ciu` (i.e., a range on the *reversed* dictionary).
- Once we have all matching terms in the dictionary, we look up postings for each enumerated term.

**Why prefix queries are easy but suffix queries are hard — Example:**
Suppose dictionary contains (alphabetical order): `apple, application, banana, computer, computing, connection, reading, running, walking`.
- **Prefix query `comp*`:** matches `computer, computing` — these occur **next to each other** in lexicographic order, so the B-tree finds the starting point and scans a **contiguous range** → efficient.
- **Suffix query `*ing`:** matches `computing, reading, running, walking` — these are **scattered** throughout the alphabetical order (a normal B-tree orders by beginning, not ending), so there is **no single contiguous range** for `*ing` → difficult with a plain B-tree.

**3. General Wildcard Queries (`*` in the middle, possibly multiple `*`s)** — e.g., `se*tic` (matches *semantic, semiotic, semitic*, etc.)
- Use the (forward) B-tree to get the set of terms `W` with prefix "se".
- Use the reverse B-tree to get the set of terms `R` with suffix "tic".
- Compute `S = W ∩ R` (words with prefix "se" AND suffix "tic").
- Use the inverted index to retrieve documents containing terms in `S`.
- B-trees handle `*`'s **at the end** of a query term well (prefix search); this is the key limitation motivating the Permuterm Index.

**General processing steps for wildcard queries:**
1. Express wildcard query `w` as a Boolean query on a specially constructed index → obtain a **superset** of dictionary terms matching `w`.
2. **Post-filter** to discard dictionary terms that don't actually match `w`.
3. Use the standard inverted index to retrieve documents for the surviving terms.

**Motivation for the Permuterm Index:**
- Problem: a normal B-tree is good for prefix queries but poor for suffix/infix queries.
- Idea: **transform** wildcard queries so the required pattern becomes a **prefix** — then a B-tree can search the transformed terms efficiently.

**Two Techniques for handling `*` anywhere in the query term (especially multiple `*`s):**
- **Solution 1 — Permuterm Index:** transform every wildcard query so the `*` occurs at the end.
- **Solution 2 — K-gram (bigram) Indexes.**

### Solution 1: Permuterm Index
In a permuterm index, the dictionary consists of **all rotations** of each term (with `$` marking the end of the word). The postings of each rotation consist of all dictionary terms containing that rotation.

**Example:** for the term `hello`, index under: `hello$`, `ello$h`, `llo$he`, `lo$hel`, `o$hell`.

In the B-tree, **all rotations of a term point back to the original lexicon term**.

**Permuterm query processing:** rotate the query's wildcard to the **right**, then use B-tree lookup as before.
- **Permuterm problem:** the index size approximately **quadruples**.
- Example: query `hel*o` → written as `hel*o$` → rotate → `o$hel*` → traverse B-tree seeking prefix `o$hel`.

**Worked Example — Query `*cat`:**
- `*cat → *cat$ → cat$*` (rotate wildcard to the right).
- **Step 1–3:** build all rotations of dictionary terms (e.g., of `copycat` and `bobcat`) and create a B-tree of these rotations.
- Matches found via prefix `cat$`: `cat$copy` and `cat$bob` → these correspond to the original terms **copycat** and **bobcat**.

**Worked Example — Query `*at`:**
- `*at → *at$ → at$*`
- Matches: `at$copyc` and `at$bobc` → original terms **copycat** and **bobcat**.

**Advantages of Permuterm:**
- Supports prefix, suffix, and infix wildcard queries.
- Converts wildcard matching into a **prefix search**.
- Can use efficient B-tree searching.

**Disadvantage:**
- Large index size — a term of length `m` generates `m+1` rotations.

### Solution 2: K-gram (Bigram) Indexes
Enumerate all **k-grams** (sequences of k characters) occurring in any term. `$` is used as a special word-boundary symbol.

**Example:** from "bigram index" we get the 2-grams (bigrams): `$b, bi, ig, gr, ra, am, m$, $i, in, nd, de, ex, x$`.

Maintain an **inverted index from bigrams to dictionary terms** that contain each bigram.
- `$b → bag, big, bigram`
- `gr → grass, group, bigram`

A **k-gram index** is thus an index whose dictionary consists of all k-grams occurring in the lexicon, where each postings list points from a k-gram to all lexicon words containing that k-gram.

**Processing n-gram wildcard queries:**
- Query `pri*` can be run as a **Boolean AND of its k-grams**: `$p AND pr AND ri`.
- Fast and space-efficient. Matches words like *prince, pride, prior, price, priest*.
- **Caveat:** this can also match words that merely *contain* those k-grams but don't actually match the pattern, e.g., *proprietary* also contains `$p`, `pr`, `ri` (as substrings) but isn't a real prefix match. So we **must post-filter** these enumerated terms against the actual wildcard pattern before use.
- Surviving (post-filtered) terms are then looked up in the term–document inverted index.

**Wildcard queries in practice:** Executing a Boolean query for each enumerated, filtered term can be **expensive** — this is why search engines often hide wildcard features behind an "Advanced Search" option.

**Permuterm vs K-gram Index — Comparison:**

| Permuterm Index | K-gram Index |
|---|---|
| Stores all rotations of terms | Stores character sequences (k-grams) |
| Directly transforms wildcard queries into prefix searches | Generates candidate terms, then post-filters them |
| Larger index | Usually less storage than Permuterm |

---

### Part 2: Spelling Correction

**Two principal uses:**
1. Correcting document(s) being indexed.
2. Retrieving matching documents when a query contains a spelling error.

**Two main flavors:**
1. **Isolated word correction** — check each word on its own for misspelling. Will **not** catch typos that result in another correctly spelled word (e.g., "from" typed as "form").
2. **Context-sensitive correction** — looks at surrounding words, e.g., "I flew **form** ..." → catches errors that isolated-word checking misses.

### Document Correction
- Primarily used for **OCR'ed documents**; correction algorithms are tuned for this scenario.
- Goal: the index (dictionary) should contain fewer OCR-induced misspellings.
- Can use **domain-specific knowledge** — e.g., OCR is more likely to confuse **O and D** than O and I (based on shape similarity), or confuse **e and c**, **r and n**, etc. (Note: adjacency on a QWERTY keyboard is more relevant to *typing* errors than OCR errors — OCR confusions are based on visual similarity.)

### Query Mis-spellings
We can either:
- Retrieve documents indexed under the correct spelling, OR
- Return several suggested alternative queries with correct spelling ("**Did you mean...?**")

### Isolated Word Correction
Makes use of a **lexicon** from which correct spellings come. Two basic choices:
1. A standard lexicon (e.g., Webster's English Dictionary), or an "industry-specific" hand-maintained lexicon.
2. The lexicon of the **indexed corpus itself** (e.g., all words on the web — including names, acronyms, and even misspellings that occur frequently).

**Given a lexicon and a character sequence Q, return the words in the lexicon closest to Q.** "Closest" can mean:
- **Edit distance**
- **Weighted edit distance**
- **n-gram overlap**

### Edit Distance
Given two strings S1 and S2, the **edit distance** is the minimum number of basic (character-level) operations needed to transform one into the other. Basic operations:
- **Insert**
- **Delete**
- **Replace**

Example: the edit distance from `cat` to `dog` is **3**. Generally computed via **dynamic programming**.

Also called **Levenshtein distance**.

**Example alignment (tutor vs tumour):**
```
t   u   t   o   -   r
t   u   m   o   u   r
```
This alignment gives an edit distance of **2**.

Another possible (worse) alignment:
```
t   u   t   -   o   -   r
t   u   -   m   o   u   r
```
This gives an edit distance of **3**. The **best possible alignment corresponds to the minimum edit distance**.

### Weighted Edit Distance
- Like edit distance, but the **weight/cost of an operation depends on the character(s) involved**.
- Meant to capture keyboard errors — e.g., `m` is more likely to be mistyped as `n` than as `q`, so replacing `m→n` should have a **smaller cost** than replacing `m→q`.
- (Similar ideas apply to OCR, but with different weight matrices.)
- Requires a **weight matrix** as input.

### Minimum Edit Distance — Dynamic Programming
Implemented via an **edit distance matrix**: one row per character of the source string, one column per character of the target string.
- Cell `(i, j)` = the distance between the first `i` characters of the source and first `j` characters of the target.
- Each cell's value is computed from three possible paths (insert, delete, substitute).
- The **substitution cost is 0** if the i-th source character matches the j-th target character.

**Algorithm:**
```
Input: Two strings X and Y
Output: minimum edit distance between X and Y
m ← length(X);  n ← length(Y)
for i = 0 to m:  dist[i,0] ← i
for j = 0 to n:  dist[0,j] ← j
for i = 1 to m:
  for j = 1 to n:
    dist[i,j] = min{ dist[i-1,j] + insert_cost,
                      dist[i-1,j-1] + subst_cost(Xi,Yj),
                      dist[i,j-1] + delete_cost }
```

**Worked Example — edit distance matrix for `tutor` → `tumour`:**

|   | # | t | u | m | o | u | r |
|---|---|---|---|---|---|---|---|
| **#** | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
| **t** | 1 | 0 | 1 | 2 | 3 | 4 | 5 |
| **u** | 2 | 1 | 0 | 1 | 2 | 3 | 4 |
| **t** | 3 | 2 | 1 | 1 | 2 | 3 | 4 |
| **o** | 4 | 3 | 2 | 2 | 1 | 2 | 3 |
| **r** | 5 | 4 | 3 | 3 | 2 | 2 | 2 |

(Bottom-right cell gives the minimum edit distance = **2**.)

### Using Edit Distances for Spelling Correction
Given a query, first enumerate all dictionary terms within a preset (weighted) edit distance.
- To reduce search complexity, heuristics are used:
  - Consider only dictionary terms beginning with the same letter.
  - Use a **permutation index** leaving out the end-of-string symbol.
  - Omit a suffix of length *l* before performing rotation.
- Then look up the enumerated dictionary terms in the term–document inverted index.

**Problem:** computing edit distance to *every* dictionary term is **expensive and slow**. How do we cut down candidate terms? → Use **n-gram overlap**.

### n-gram Overlap
- Enumerate all n-grams in the query string as well as in the lexicon.
- Use an n-gram index to retrieve all lexicon terms matching any of the query's n-grams.
- **Threshold** by the number of matching n-grams.

**Example with trigrams:**
- Text: `november` → trigrams: `nov, ove, vem, emb, mbe, ber`.
- Query: `december` → trigrams: `dec, ece, cem, emb, mbe, ber`.
- **3 trigrams overlap** (out of 6 in each term: `emb, mbe, ber`).
- Question raised: how to turn this into a **normalized measure of overlap**? → Jaccard coefficient.

### Jaccard Coefficient (for spelling correction / general overlap)
A commonly used measure of overlap between two sets X and Y:

`Jaccard(X, Y) = |X ∩ Y| / |X ∪ Y|`

- Equals **1** when X and Y have the same elements, and **0** when they are disjoint.
- X and Y don't need to be the same size; always returns a number between 0 and 1.
- We then **threshold** to decide if there's a match, e.g., if J.C. > 0.8, declare a match.

**Worked Example — matching trigrams for query `lord`:**
We want words matching **2 of its 3 bigrams** (`lo, or, rd`):
- `lo` → alone, lord, sloth
- `or` → border, lord, morbid
- `rd` → border, card, ardent

A standard postings "merge" enumerates candidates across these bigram postings lists and counts overlaps (e.g., "lord" appears under both `lo` and `or`; "border" appears under both `or` and `rd`).

### Context-Sensitive Spelling Correction
Example: Text says *"I flew from Heathrow to Narita."* Consider the phrase query `"flew form Heathrow"` — we'd like to respond **"Did you mean 'flew from Heathrow'?"** because no documents matched the query phrase as typed.

- Need **surrounding context** to catch this kind of error; full NLP is considered **too heavyweight** for this task.
- **First idea:** retrieve dictionary terms close (in weighted edit distance) to *each* query term, then try all possible resulting phrases with **one word "fixed" at a time**:
  - `flew from heathrow`
  - `fled form heathrow`
  - `flea form heathrow`
  - etc.
  - Suggest the alternative phrase that has the most hits in the corpus.

**Exercise given in slides:** Suppose for `"flew form Heathrow"` we have 7 alternatives for "flew", 19 for "form", and 3 for "heathrow" (illustrating the combinatorial explosion of trying all combinations).

**Another approach — biword-based correction:**
- Break the phrase query into a conjunction of **biwords** (adjacent word pairs).
- Look for biwords that need only **one term corrected**.
- Enumerate the resulting phrase matches and **rank** them.

**General issue in spell correction:** Multiple "Did you mean" alternatives may be generated — need to determine which one (or small number) to present to the user.

**Computational cost:** Spell-correction is computationally expensive; typically avoided on every single query. A common strategy: **run spell-correction only on queries that matched few documents**.

---

### Part 3: Soundex (Phonetic Correction)

**Soundex** is a class of heuristics used to expand a query into **phonetic equivalents**. It is **language-specific**, mainly used for **names**.
- Example: `chebyshev → tchebycheff`.

**Soundex — typical algorithm:**
1. Turn every token to be indexed into a **4-character reduced form**.
2. Do the same with query terms.
3. Build and search an index on the reduced forms (when the query calls for a soundex match).

**Soundex Coding Rules:**
1. **Keep the first letter constant.**
2. Code the rest of the letters into digits using the table below.
3. **Ignore adjacent letters with the same Soundex digit** (collapse consecutive duplicate digits into one).
4. **Eliminate all zeros.**
5. **Truncate or pad with zeros** to produce exactly: 1 initial letter + 3 digits.

**Soundex Phonetic Code Table:**

| Letters | Code |
|---|---|
| a, e, h, i, o, u, w, y | 0 |
| b, f, p, v | 1 |
| c, g, j, k, q, s, x, z | 2 |
| d, t | 3 |
| l | 4 |
| m, n | 5 |
| r | 6 |

**Worked Example: Mickson, Mikson, Mixon → all yield Soundex code `M250`**

**1. Mickson:**
- Keep 1st letter: `M`
- Encode rest (`i, c, k, s, o, n`): `i→0, c→2, k→2, s→2, o→0, n→5`
- Raw sequence: `M - 0 - 2 - 2 - 2 - 0 - 5`
- Ignore adjacent same digits: consecutive `2`'s collapse into a single `2`: `M - 0 - 2 - 0 - 5`
- Eliminate zeros: `M - 2 - 5`
- Pad with zero to reach 1 letter + 3 digits: **`M250`**

**2. Mikson:**
- Keep 1st letter: `M`
- Encode rest (`i, k, s, o, n`): `i→0, k→2, s→2, o→0, n→5`
- Raw sequence: `M - 0 - 2 - 2 - 0 - 5`
- Consecutive `2`'s collapse into a single `2`: `M - 0 - 2 - 0 - 5`
- Eliminate zeros: `M - 2 - 5`
- Pad with zero: **`M250`**

**3. Mixon:**
- Keep 1st letter: `M`
- Encode rest (`i, x, o, n`): `i→0, x→2, o→0, n→5`
- Raw sequence: `M - 0 - 2 - 0 - 5` (no adjacent duplicates present)
- Eliminate zeros: `M - 2 - 5`
- Pad with zero: **`M250`**

| Name | Step-by-Step | Final Soundex Code |
|---|---|---|
| Mickson | M → c(2), k(2) collapse → 2, s(2) → collapse → 2, n(5) → pad | **M250** |
| Mikson | M → k(2), s(2) collapse → 2, n(5) → pad | **M250** |
| Mixon | M → x(2), n(5) → pad | **M250** |

**Notice: all three phonetically similar names yield the exact same Soundex code `M250`.**

**History & limitations:**
- Developed by **Odell and Russell in 1918**, used in the **US census** to match American English names.
- **Soundex fails** on two names with **different initials** even if they sound similar (e.g., Karlson vs Carlson).
- Also fails in other cases (e.g., Rodgers vs Rogers).
- The coding scheme is based on the observation that **vowels are interchangeable** and **consonants with similar sounds are grouped into equivalence classes**.

---

# UNIT 2: Index Construction, Scoring & Vector Space Model

## Lecture 13–16: Index Construction

### Why Hardware Fundamentals Matter in IR
- All IR algorithms are constrained by the underlying **hardware**.
- Access to data in **memory** is much faster than access to data on **disk**.
- **Key bottleneck:** massive speed gap between RAM and Disk.
  - Accessing RAM: ~5–10 nanoseconds.
  - Accessing Disk: requires physical movement of mechanical parts.
  - **Disk is approximately 1,000,000× slower than RAM.**
- We therefore want to keep as much frequently-used data as possible in memory — this technique is called **Caching**.

### The Memory Storage Hierarchy
1. **CPU Registers** — fastest, extremely small, most expensive.
2. **Cache Memory (SRAM)** — very fast, limited capacity.
3. **Main Memory (DRAM)** — ~10–200 ns, capacity of several GBs.
4. **Secondary Storage (Magnetic Disks / SSDs)** — ~10 ms, capacity of terabytes.
5. **Tertiary Storage (Magnetic Tape)** — very slow, massive capacity (archival).

**Principle:** faster storage costs exponentially more and has lower capacity. **Goal of index construction:** maximize use of RAM; when RAM is full, use disk cleverly.

### Hardware Parameters (as given in slides, ~2007-era numbers — gap ratio still similar today)
- **Average Seek Time (s)** = 5 ms = 5×10⁻³ s — time to move the read/write head to the correct track; **no data transferred during this time** (pure overhead).
- **Transfer Time per Byte (b)** = 0.02 μs = 2×10⁻⁸ s — speed of data transfer once positioned correctly.
- **Low-level CPU operation (p)** = 0.01 μs = 10⁻⁸ s — time for a simple compare-and-swap/arithmetic operation.
- Main Memory: several GB. Available Disk Space: 1 TB or more.

### The Cost of Disk Seeks
`Total Time = Seek Time + Rotational Latency + Transfer Time`
- **Seek time dominates** for small random reads.
- **Rotational latency:** time for the platter to spin the right sector under the head (average = half a rotation).

**Real-world example — transferring a 10 MB file:**
- **Case A — 1 contiguous chunk:** Transfer time = (10 MB × 0.02 μs) + 1 seek (5 ms) ≈ 0.2 + 0.005 = **0.205 seconds**.
- **Case B — 100 non-contiguous chunks:** Transfer time = (10 MB × 0.02 μs) + (100 seeks × 5 ms) ≈ 0.2 + 0.5 = **0.7 seconds**.
- **Result: non-contiguous storage is ~3× slower**, even for the same amount of data.

### The Block I/O Rule and Buffering
- Operating systems do **NOT** read/write single bytes — disk I/O happens in **fixed-size blocks** (e.g., 8 KB, 16 KB, 64 KB).
- **Implication:** reading just 1 byte costs the same as reading a full 64 KB block — so you should always **read large chunks sequentially**.
- **Buffer:** a reserved portion of main memory where incoming disk blocks are temporarily stored, letting the CPU process data while the next block is read.
- **Compression advantage:** storing compressed data on disk means reading the (smaller) compressed data + decompressing in RAM is usually **faster** than reading uncompressed data directly.

### The Reference Collection — Reuters RCV1
A standard academic IR benchmark:
- **N** (documents) = 800,000 (~1 year of Reuters newswire)
- **L_avg** (avg tokens/doc) ≈ 200 words
- **M** (distinct vocabulary terms) = 400,000
- **T** (total tokens in collection) = 100,000,000 (100 million)
- Raw text size ≈ **1 GB**

Even this "small" benchmark creates massive intermediate data during indexing.

### The Scaling Problem — Intermediate File Size
- To build an inverted index, we generate **(termID, docID) pairs**.
- Using 4 bytes for termID + 4 bytes for docID = **8 bytes per pair**.
- Total intermediate data = 100,000,000 tokens × 8 bytes = **800 MB** — exceeds RAM on many older systems.
- Real-world collections are often 100×–1000× larger than RCV1 → for a web-scale collection (100 billion tokens), intermediate data = **800 GB**.
- **Conclusion:** It is impossible to build an index using RAM alone; **disk-based external sorting is mandatory**.

### Introducing TermIDs — The Efficiency Trick
- **TermID** = a unique integer serial number assigned to each distinct term.
- Why use TermIDs instead of raw strings?
  - Strings are variable length (avg English word ≈ 7.5 bytes); integers are fixed length (4 bytes).
  - **Benefit 1 — Memory savings:** saves ~50% memory during sorting.
  - **Benefit 2 — Speed:** comparing two integers is a single CPU instruction; comparing strings requires iterating over characters.
- Record format for intermediate files: `[termID (4 bytes)][docID (4 bytes)]`.
- Requires building a mapping (hash table) from string → integer.

### The 4 Fundamental Steps of Index Construction
1. **Parse the documents** — tokenize, remove punctuation, case-fold, stem; generate a stream of `(termID, docID)` pairs.
2. **Sort the pairs** — dominant key: termID (lexicographic order); secondary key: docID (for merging duplicates).
3. **Invert the sorted pairs** — scan the sorted list, group all pairs with the same termID, create the postings list for each term.
4. **Write to disk** — store the final inverted index for query processing.

> For small collections, all steps fit in RAM. For large collections, we use **BSBI**.

### Blocked Sort-Based Indexing (BSBI)
- A classic **"Divide and Conquer"** approach for **external sorting**.
- **Core philosophy:** break the massive collection into smaller, memory-sized "chunks" (blocks); process each chunk entirely in RAM, then combine results.
- **Why divide?** Sorting 10 million items in RAM takes seconds; sorting 1 billion items takes hours and can crash the computer.
- **Principle of external sorting:** use RAM to sort, use disk to store intermediate results.
- BSBI specifically **minimizes random disk seeks**, relying heavily on sequential reads/writes.

**The BSBI Algorithm:**
```
BSBI(collection):
  n = 0                                  // block counter
  while documents remain:
      n = n + 1
      block = PARSENEXTBLOCK()
      BSBI-INVERT(block)                 // Sort in RAM and group by term
      WRITEBLOCKTODISK(block, f_n)       // Save to disk
  MERGEBLOCKS(f_1, f_2, ..., f_n; f_merged)
```
Each block-write produces a **complete inverted index for a subset of documents**. The final **merge step** combines the `n` partial indexes into one global index.

### Choosing the Right Block Size
- Block size must fit comfortably into available main memory. Recommended: leave **1–2 GB** for the OS, cache, and hash tables.
- **Example calculation:**
  - Total RAM = 8 GB; OS overhead = 2 GB → Available for block = 6 GB.
  - Each (termID, docID) pair = 8 bytes.
  - Max pairs per block = 6 GB / 8 bytes = **750 million pairs**.
  - For RCV1 (100M tokens), if we allocate 1 GB per block → 100M / (1GB/8) ≈ **10 blocks**.
- **Larger blocks → fewer merges (faster), but limited by RAM.**

### PARSENEXTBLOCK() — Detailed Parsing of a Block
```
PARSENEXTBLOCK():
  Initialize empty block array
  While block size < max_capacity:
      doc = get_next_document()
      raw_tokens = tokenize(doc.text)          // Split by spaces/punctuation
      For each raw_token in raw_tokens:
          clean_token = case_fold(raw_token)   // Convert to lowercase
          termID = getTermID(clean_token)      // Hash map lookup
          block.add( (termID, doc.id) )
  Return block
```
**Time dominant:** tokenization and `getTermID` (hash map lookups) are very CPU-intensive.

### BSBI-INVERT — The Inversion Process
Once the block is full and loaded in RAM:
- **Step A — Sort** the `(termID, docID)` pairs in memory (in-memory quicksort or mergesort).
- **Step B — Collect postings (grouping):** traverse the sorted array, identify contiguous ranges with the same termID, and for each range extract all docIDs into that term's posting list.

**Visual Example:**
- Input: `[(cat,1), (dog,1), (cat,2), (bird,2)]`
- Sorted: `[(bird,2), (cat,1), (cat,2), (dog,1)]`
- Inverted: `bird→[2], cat→[1,2], dog→[1]`

### Writing Intermediate Blocks to Disk
After inversion, the block's inverted index is serialized to disk. File naming convention: `f_1, f_2, ..., f_n`. Each file consists of:
- **Header:** number of terms in this block, block identifier.
- **Dictionary:** `(termID, offset_to_postings)`.
- **Postings:** flat array of docIDs for each term.

**DocIDs in postings are sorted ascending** — crucial because it speeds up later merging and compression.

### Merge Phase (K-Way Merge)
**Objective:** combine the `n` sorted blocks (`f_1` to `f_n`) into a single sorted inverted index.
- We **cannot load all blocks into RAM simultaneously** (though we do load small buffers).
- **Approach: simultaneous "K-way" merge** (K = number of blocks).
- **Core challenge:** efficiently selecting the smallest termID across `n` sources at each step.
- **Solution:** use a **Min-Heap (Priority Queue)**, which always holds the current smallest unprocessed termID from each block.

**Data Structures and Buffers for Merging:**
- Open all block files simultaneously (file descriptors are limited, usually up to 100–500 open files is fine).
- Maintain two types of buffers in RAM:
  - **Read buffers:** small chunks read from each block (e.g., 64 KB each) — prevents byte-by-byte disk reads.
  - **Write buffer:** accumulates final merged postings before flushing to disk.
- **Why buffers?** to convert random reads/seeks into large **sequential** reads. When a read buffer empties, refill it with the next large chunk from that block.

**The Min-Heap (Priority Queue) Logic:**
```
Initialization:
  For each block i: read first termID; push (termID, block_id) into min-heap.
Loop (while heap not empty):
  (term, block) = heap.pop_min()               // smallest termID
  For all blocks that have this same term:
      Retrieve the full postings list for this term from that block
      Merge (concatenate) the docID lists
      Write the merged list to the final output buffer
  For each processed block: read the next termID and push it back into the heap
```
**Complexity:** each heap operation is **O(log n)** due to push/pop.

**BSBI Merge Worked Example:**
- Block 1 (`f_1`): `bird → [2]`, `cat → [1,2]`, `dog → [1]`
- Block 2 (`f_2`): `bird → [3]`, `dog → [3]`

Execution:
1. Initialize heap: `(bird, B1), (bird, B2)`.
2. Pop "bird". Postings: `[2]` and `[3]` → merge = `[2,3]` → write `bird → [2,3]`.
3. Refill heap: `(cat, B1), (dog, B2)`.
4. Pop "cat". Postings: `[1,2]` → write `cat → [1,2]`.
5. Refill heap: `(dog, B1), (dog, B2)`.
6. Pop "dog". Merge = `[1] + [3] = [1,3]` → write `dog → [1,3]`.

**Final Index:** `bird→[2,3], cat→[1,2], dog→[1,3]`

### BSBI Time Complexity Analysis
- **Theoretical sort complexity:** Θ(T log T), where T = total tokens.
- **However**, this ignores disk I/O. In reality, the actual indexing time is usually **dominated by parsing time (PARSENEXTBLOCK)** and the **final merge time (MERGEBLOCKS)**, not by sorting.
- **Parsing is slow due to:** string manipulation and hash lookups; moving data from disk to RAM.
- **Merging is slow due to:** sequentially reading many open block files; writing the final index to disk.
- **Sorting is relatively fast** because it's purely in-memory integer sorting.

### Worked Numerical Problems (BSBI)

**Q1:** Collection like RCV1: 100 million tokens. Machine has 2 GB (2×10⁹ bytes) RAM. Each `(termID, docID)` pair = 8 bytes.
1. How many pairs fit in one block?
2. How many intermediate blocks are created?

**Solution:**
- Pairs per block = (2×10⁹) / 8 = **250,000,000 (250 million pairs)**.
- Since 100 million < 250 million, the whole collection fits in **a single block** in theory. (100M / 250M = 0.4 → rounds up to **1 block**.)
- **However**, if we strictly limit block size to **1 GB** (= 10 million pairs) to leave room for OS/hash tables, the number of blocks = 100M / 10M = **10 blocks**.

**Q2:** Assume BSBI creates 10 blocks, each with 10 million records. In-memory sorting speed = 1 million records/second.
1. How long to sort all 10 blocks?
2. If disk write speed is 50 MB/sec and each block is exactly 80 MB, how long to write all blocks?

**Solution:**
- **Sorting:** time per block = 10M / 1M = **10 seconds**; total = 10 blocks × 10 s = **100 seconds**.
- **Writing:** total data = 10 × 80 MB = 800 MB; time = 800 MB / 50 MB/s = **16 seconds**.

### SPIMI — Single-Pass In-Memory Indexing
**Problem with BSBI:** it requires storing the `(termID, docID)` pair for **every single token** in the block before sorting — memory overhead even before considering the postings themselves.

**SPIMI** eliminates the sorting step entirely — instead of collecting all pairs and sorting, it **builds postings lists on the fly**. This reduces memory usage significantly and achieves **linear time complexity Θ(T)**.

**Core Idea — Direct Insertion:**
- As SPIMI parses a token, it looks up the term in a hash map.
- If the term **exists**, it immediately **appends** the current docID to that term's dynamic postings list.
- If the term **doesn't exist**, it creates a new hash map entry with a new postings list.
- **Result:** no need to store `(termID, docID)` pairs for intermediate data → less wasted memory.
- **Why faster?** Sorting takes Θ(T log T) time; skipping it entirely means Θ(T) time.

**Dynamic Postings Lists:**
- Problem: when a term is first seen, we don't know how many documents it will appear in.
- Solution: allocate an initial small array (e.g., capacity = 4); when full, **double** its capacity and copy old elements.
- Average cost of this dynamic expansion is **O(1) amortized** per insertion.
- **Trade-off:** the doubling strategy may waste up to **50%** of allocated space on average — but since SPIMI doesn't store termIDs in intermediate structures, it still uses less memory overall than BSBI.

**SPIMI Algorithm:**
```
SPIMI-INVERT(token_stream):
  output_file = NEWFILE()
  dictionary = NEWHASH()
  while (free memory available):
      token = next(token_stream)
      if term(token) not in dictionary:
          postings_list = ADDTODICTIONARY(dictionary, term)
      else:
          postings_list = GETPOSTINGSLIST(dictionary, term)
      if full(postings_list):
          postings_list = DOUBLEPOSTINGSLIST(dictionary, term)
      ADDTOPOSTINGSLIST(postings_list, docID(token))
  sorted_terms = SORTTERMS(dictionary)              // sort terms for output order
  WRITEBLOCKTODISK(sorted_terms, dictionary, output_file)
  return output_file
```

### BSBI vs SPIMI — Comparison

| Aspect | BSBI | SPIMI |
|---|---|---|
| Sorting | Requires sorting all pairs | Does **not** sort pairs |
| Time Complexity | Θ(T log T) | Θ(T) (asymptotically faster) |
| Intermediate Storage | Stores `(termID, docID)` per occurrence | Stores postings lists directly (no termID per occurrence) |
| Term Mapping | Requires a global termID map fitting in memory | Can use strings directly as hash keys |
| Implementation | Simpler | Needs complex dynamic array management |

**When to use BSBI vs SPIMI:**
- **Choose BSBI if:** collection is moderately large; memory is predictable/sufficient for termID mapping; simplicity is desired.
- **Choose SPIMI if:** collection is massive (web-scale); vocabulary is extremely large and doesn't fit in RAM; fastest possible construction speed is needed.
- **Real-world insight:** many modern high-performance indexers (e.g., older versions of Lucene) are based on SPIMI principles.

### Worked Numerical Problems (SPIMI)

**Q1:** A BSBI block has 1,000,000 `(termID, docID)` pairs. In SPIMI (same block), there are 50,000 unique terms, each posting list averaging 20 documents. How much memory does SPIMI save vs BSBI? (BSBI: 8 bytes/pair; SPIMI stores only docID (4 bytes) per posting, no termID.)

**Solution:**
- **BSBI memory:** 1,000,000 pairs × 8 bytes = **8 MB**.
- **SPIMI memory:** only docIDs stored (termIDs are hash-map keys, not repeated per posting). Total postings = 1,000,000 → SPIMI memory = 1,000,000 × 4 bytes = **4 MB**.
- **Savings:** 8 MB − 4 MB = **4 MB (50% savings)**.

**Q2:** Collection has T = 10,000,000 tokens. BSBI time = T·log₂T; SPIMI time = T. Given log₂(10,000,000) ≈ 23. How many times faster is SPIMI?

**Solution:**
- BSBI time = 10,000,000 × 23 = **230,000,000 units**.
- SPIMI time = **10,000,000 units**.
- Speed-up = 230,000,000 / 10,000,000 = **23 times faster**.

### Dynamic Indexing — The Problem of Updates
So far we assumed a **static** collection. In reality, collections are **dynamic**: documents are added, deleted, and updated frequently.
- Example: web search (new pages every second), enterprise email (new emails every minute).
- **Operations:** Addition (index new docs), Deletion (remove docs), Update (re-index modified docs).
- **Naive solution:** rebuild the entire index from scratch periodically — works for weekly updates, **fails for real-time** needs.

### The Auxiliary Index Strategy
Maintain **two separate indexes**:
- **Main Index (Disk):** large, static index built from old documents.
- **Auxiliary Index (RAM):** small, dynamic index storing new/updated documents.

**Query execution:**
1. Search the Main Index.
2. Search the Auxiliary Index.
3. Merge results (union of docIDs).

**Deletions:** maintain an **"Invalidation Bit Vector"** marking deleted docIDs.
**Updates:** mark the old doc as deleted in the bit vector, and insert the updated document into the auxiliary index.

### The Cost of Merging Auxiliary into Main
- The auxiliary index cannot grow forever — it must eventually be merged into the main index.
- **Cost model:** merge whenever auxiliary size reaches `n` postings; total postings in final collection = `T`.
- Number of merge operations = `T / n`. Each merge processes `n` postings (auxiliary) + scans the affected part of the main index.
- On average, each posting gets processed `T/n` times.
- **Time Complexity: Θ(T² / n)** — this is **quadratic** and becomes disastrously slow for large T.
- **Conclusion:** the simple auxiliary-index approach **does not scale** to massive dynamic collections.

**Worked Numerical Example:**
T = 1000, n = 100. Complexity formula = T²/n.
- (1000 × 1000) / 100 = 1,000,000 / 100 = **10,000 operations**.

### The File System Challenge
Why not just append to the end of the main index?
- The main index is **sorted by termID** — we can't just append to a single file.
- **Option 1:** keep each postings list as a separate file — merge is easy (append to the specific file), but **most file systems choke on millions of small files** (too many inodes, slow lookups).
- **Option 2:** store the entire index as one huge concatenated file — merge is **hard**, requiring a rewrite of the entire file, touching every posting.
- We need a **smarter merging strategy** that avoids rewriting everything → **Logarithmic Merging**.

### Logarithmic Merging (The Lucene Way)
**Idea:** maintain multiple indexes of **exponentially increasing sizes**. If `n` is the size of the smallest (RAM-based) index:
- I₀ = size n
- I₁ = size 2n
- I₂ = size 4n
- Iₖ = size 2ᵏ × n

**Process (percolation / binomial-heap analogy):**
- New postings go into **Z₀** (in-memory). When Z₀ reaches size `n`:
  - If I₀ doesn't exist: Z₀ becomes I₀.
  - If I₀ exists: merge Z₀ and I₀ to create Z₁. If I₁ doesn't exist, Z₁ becomes I₁. Else merge Z₁ with I₁ to create Z₂, and so on (like carrying in binary addition).

**Algorithm:**
```
LOGARITHMICMERGE():
  Z₀ = ∅ (in-memory index)
  indexes = ∅
  while true:
      LMERGEADDTOKEN(indexes, Z₀, GETNEXTTOKEN())

LMERGEADDTOKEN(indexes, Z₀, token):
  Z₀ = MERGE(Z₀, {token})
  if |Z₀| == n:
      for i = 0 to ∞:
          if I_i exists:
              Z_{i+1} = MERGE(I_i, Z_i)
              remove I_i from indexes
          else:
              I_i = Z_i
              add I_i to indexes
              BREAK
      Z₀ = ∅
```

### Complexity of Logarithmic Merging
- Each posting is processed **only once per level** of the hierarchy.
- Total number of levels = **log₂(T / n)**.
- Total time complexity: **Θ(T log(T / n))**.

**Comparison:**
- Simple Auxiliary Merge: Θ(T² / n) — slow, quadratic.
- Logarithmic Merge: Θ(T log(T / n)) — fast, near-linear.

**Trade-off:** query speed degrades, because we must search across **all** existing indexes (I₀, I₁, ...). This trade-off is the **foundation of dynamic indexing in Apache Lucene and Elasticsearch**.

**Worked Numerical Example:** T = 1000, n = 100. Formula: T × log₂(T/n).
- T/n = 1000/100 = 10.
- log₂(10) ≈ 3.3 (since 2³=8, 2⁴=16).
- Total operations = 1000 × 3.3 = **3300 operations**.
- **Comparison:** Simple Auxiliary Merge = 10,000 ops (slow) vs Logarithmic Merging = 3,300 ops (**≈3× faster**).

### Log Merging Simulation Example (n = 2, i.e., Z₀ capacity = 2 tokens)
This mirrors how a **binary counter** works:
- **T=2:** Z₀ full → becomes I₀. State: I₀=✓, I₁=✗, I₂=✗
- **T=4:** Z₀ merges with I₀ → Z₁ (size 4). I₁ empty → Z₁ becomes I₁. State: I₀=✗, I₁=✓, I₂=✗
- **T=6:** Z₀ full → becomes I₀. State: I₀=✓, I₁=✓, I₂=✗
- **T=8:** Z₀ merges I₀ → Z₁ (size 4). I₁ exists, so Z₁ merges I₁ → Z₂ (size 8). I₂ empty → Z₂ becomes I₂. State: I₀=✗, I₁=✗, I₂=✓
- **T=10:** Z₀ full → becomes I₀. State: I₀=✓, I₁=✗, I₂=✓

### Query Processing in Logarithmic Merging
Since data is scattered across multiple indexes, querying is more complex. The query engine must:
1. Search Z₀ (in-memory, fastest).
2. Search all existing I₀, I₁, I₂, ... on disk.
3. Collect postings from all sources.
4. **Merge** (union) the retrieved postings lists into one final result list.

This is why query latency is slightly higher (must read from multiple files). However, the number of indexes is `log(T/n)`, usually **under 20**, so it remains manageable.

### MapReduce for Distributed Indexing
For **web-scale** collections (billions of pages), even BSBI/SPIMI on a single machine fails.
- **Solution: MapReduce** (Dean & Ghemawat, 2004) — used by Google, Hadoop.

**Map Phase (Parsers):**
- Input: web pages split into **64 MB chunks**.
- Output: `(termID, docID)` pairs.
- **Partitioning:** split output into key ranges (e.g., `a–f, g–p, q–z`).
- Write to local "segment files".

**Reduce Phase (Inverters):**
- Each inverter is assigned one key range (e.g., all terms `a–f`).
- It reads segment files from all parsers.
- Sorts the `(term, docID)` pairs and inverts them.
- Writes the final postings for that term range to disk.

**Fault Tolerance in MapReduce:**
- In large clusters, machines fail frequently. MapReduce handles this via:
  - **Reassignment:** if a parser dies, the master reassigns its split to another parser.
  - **Read-only operations:** segment files are immutable, so re-processing is safe.
  - **Checkpointing:** periodically saves the state of the map phase.
- This allows indexing of **petabytes** of data on cheap commodity hardware.

### Security and Access Control Lists (ACLs)
**Scenario:** Enterprise search — e.g., Managers can see "Salaries.pdf"; interns cannot.

**Solution:** build a **User–Document Matrix**.
- Rows = Users, Columns = Documents.
- Cell `(i,j) = 1` if user `i` has access to document `j`.

**Inverted ACL Index:** for each user, store a "postings list" of documents they can access.
- Example: `User_Alok → [doc5, doc7, doc9]`

**Query Processing with ACLs:**
1. **Text Search:** find candidate documents containing the query term, e.g., `[doc5, doc6, doc9]`.
2. **ACL Filter:** fetch the user's access list, e.g., `[doc5, doc7, doc9]`.
3. **Intersection:** compute the intersection of the two lists → Final Result = `[doc5, doc9]`.

**Challenges:**
- ACL lists for "Admin" can be incredibly large (millions of docs) → slow intersection.
- Maintaining ACLs dynamically is hard (the same dynamic-indexing problem as above).
- **Workaround:** permissions are often checked at the file-system level during the final retrieval step.

### The Complete Index Construction Pipeline (Summary)
1. **Document Collection** (RCV1: 800K docs; Web: billions).
2. **Parser** (Tokenization, TermID mapping, stemming).
3. **External Sorting (BSBI/SPIMI)** — chop into memory-sized blocks, sort, dump to disk.
4. **K-Way Merge (Min-Heap)** — combine sorted blocks into the final index.
5. **Final Inverted Index** (term → sorted docID list).
6. **Dynamic Updates (Logarithmic merging)** — add new documents over time.
7. **Query Processing** — search the index and return results.

### Algorithm Complexity Summary

| Algorithm | Complexity | Notes |
|---|---|---|
| In-Memory Sort (Simple) | Θ(T log T) | Only if it fits in RAM |
| BSBI | Θ(T log T) | I/O dominates, but scales |
| SPIMI | Θ(T) | Linear! Fastest, but complex |
| Simple Auxiliary Merge | Θ(T² / n) | Quadratic — avoid for large data |
| Logarithmic Merge | Θ(T log(T/n)) | The "sweet spot" for dynamic systems |
| MapReduce | Distributed | Parallel, fault-tolerant |

### Rules of Index Construction (Summary Takeaways)
1. **Respect the Hardware.** Disk seeks are the enemy — batch your reads.
2. **Divide and Conquer.** Sort blocks in memory, merge on disk.
3. **Avoid Sorting if Possible.** SPIMI saves time by building postings directly.
4. **Dynamic Updates are Hard.** Use Logarithmic Merging to avoid quadratic time penalties.
5. **Trade-offs Exist.** Faster indexing (Log Merge) often means slower querying (multiple indexes to search).

---

## Lecture 17: Index Compression (Recap Content)

*(Only a brief recap of this topic was available in the provided slides — introduced as "Recap of Lecture 5" at the start of the TF-IDF lecture deck. These are the key terms to know at a conceptual level for the midterm.)*

- **Heaps' Law** and **Zipf's Law** — statistical laws describing collection and vocabulary statistics (Heaps' Law predicts vocabulary size growth as collection size grows; Zipf's Law describes the frequency distribution of terms — a small number of terms occur very frequently while most terms occur rarely).
- **Dictionary compression** for Boolean indexes — techniques include storing the **dictionary as a single long string** (dictionary-as-a-string), using **blocking**, and **front coding** (exploiting shared prefixes between consecutive sorted terms).
- **Postings compression** — key ideas:
  - **Gap encoding:** instead of storing full docIDs, store the **gaps (differences)** between consecutive docIDs in a postings list (gaps are much smaller numbers, especially for frequent terms, and compress better).
  - **Prefix-unique / variable-length codes** for encoding these gaps.
  - **Variable-Byte (VB) codes** and **Gamma (γ) codes** — two standard compression schemes for encoding integers (gaps) using a variable number of bits/bytes, so that small numbers use fewer bits than large numbers.

**Illustrative compression figures given in the recap (for a reference collection), in MB:**

| Data | Size |
|---|---|
| Collection (text, XML markup, etc.) | 3,600.0 MB |
| Collection (text only) | 960.0 MB |
| Term–doc incidence matrix | 40,000.0 MB |
| Postings, uncompressed (32-bit words) | 400.0 MB |
| Postings, uncompressed (20 bits) | 250.0 MB |
| Postings, variable-byte encoded | 116.0 MB |
| Postings, γ-encoded | 101.0 MB |

*(Note: Lecture 18 was a Tutorial/Unit Review session with numerical problems and quiz — no new theoretical content.)*

---

## Lecture 19: Vector Space Model — Introduction, Ranked Retrieval & Scoring

### Ranked Retrieval — Motivation
So far, queries have all been **Boolean** — documents either match or don't.
- Good for **expert users** with a precise understanding of their needs and the collection.
- Good for **applications** that can easily consume thousands of results.
- **Not good** for the majority of users — most users can't (or won't) write Boolean queries, and don't want to wade through thousands of results. This is especially true for web search.

### Problem With Boolean Search: "Feast or Famine"
Boolean queries often result in **either too few (=0) or too many (1000s)** results.
- Query 1: `"standard user dlink 650"` → **200,000 hits**.
- Query 2: `"standard user dlink 650 no card found"` → **0 hits**.
- It takes a lot of skill to construct a query that produces a manageable number of hits: **AND gives too few; OR gives too many.**

### Ranked Retrieval Models
Rather than a set of documents satisfying a query expression, in **ranked retrieval** the system returns an **ordering** over the (top) documents for a query.
- **Free text queries:** instead of a query language of operators/expressions, the user's query is just one or more words in natural language.
- In principle these are two separate design choices, but in practice **ranked retrieval is normally associated with free-text queries** and vice versa.

**Feast or famine is not a problem in ranked retrieval:**
- When a system produces a ranked result set, **large result sets are not an issue** — the size of the result set doesn't matter.
- We just show the **top k (≈10)** results — we don't overwhelm the user.
- (Premise: the ranking algorithm actually works well.)

### Scoring as the Basis of Ranked Retrieval
We want to return, in order, the documents most likely to be useful to the searcher.
- **Assign a score** — say in `[0, 1]` — to each document, measuring how well the document and query "match."

### Query–Document Matching Scores
We need a way to assign a score to a query/document pair.
- Start with a **one-term query**: if the query term does not occur in the document, score should be **0**. The more frequent the query term in the document, the **higher** the score should be.

### Take 1: Jaccard Coefficient (for scoring)
`jaccard(A,B) = |A ∩ B| / |A ∪ B|`
- `jaccard(A,A) = 1`; `jaccard(A,B) = 0` if `A ∩ B = ∅`.
- A and B don't need to be the same size; always gives a number between 0 and 1.

**Scoring Example:**
- Query: `ides of march`
- Document 1: `caesar died in march`
- Document 2: `the long march`
(Students should compute the Jaccard score between the query set and each document's term set.)

**Issues with Jaccard for scoring:**
- Doesn't consider **term frequency** (how many times a term occurs in a document).
- Rare terms in a collection are **more informative** than frequent terms — Jaccard doesn't account for this.
- We need a more sophisticated way of **normalizing for length** — this motivates moving away from `|A∩B|/|A∪B|` toward weighted vector approaches (TF-IDF + cosine, covered next).

### Term–Document Matrices
- **Binary term–document incidence matrix (recap from Lecture 1):** each document is represented as a binary vector ∈ `{0,1}^|V|`.
- **Term–document count matrix:** consider the *number of occurrences* of a term in a document — each document becomes a **count vector** in `ℕ^V`.

### Bag of Words Model
- The vector representation **doesn't consider word order**.
- Example: *"John is quicker than Mary"* and *"Mary is quicker than John"* have the **same vector** representation.
- This is called the **bag of words model** — in a sense a step back, since the earlier **positional index** was able to distinguish these two sentences (positional info can be "recovered" later, e.g., for phrase queries).

### Term Frequency (tf)
The **term frequency `tf(t,d)`** of term `t` in document `d` = the number of times `t` occurs in `d`.
- We want to use `tf` when computing query–document match scores — but **raw term frequency is not ideal**:
  - A document with 10 occurrences of a term is more relevant than a document with 1 occurrence — **but not 10 times more relevant**.
  - **Relevance does not increase proportionally with term frequency.**
- (Note: in IR, "frequency" = "count", not a rate.)

---

## Lecture 20: Parametric and Zone Indexing

*(This lecture introduces three related techniques for handling structured/semi-structured aspects of documents, building on the earlier Boolean/vector ideas.)*

### Overview: Three Techniques, One Goal
Each technique helps a search engine find the right document — but they differ in **what** they search and **how** they rank results:
1. **Parametric Indexing** — filters documents using structured metadata like author, year, language.
2. **Zone Indexing** — searches specific regions of a document — title, author, abstract, or body.
3. **Weighted Zone Scoring** — assigns importance weights to zones and ranks documents by a combined score.

### Technique 1: Parametric Indexing
Uses **structured metadata fields** to filter documents. Instead of searching free text, it matches **exact field values** — like `Author = Shakespeare` or `Year = 1601`.
- Think of it like a **library catalog** — you filter by known attributes, not by reading every page.
- Common fields: Author, Year, Language, Doc Type.

**Worked Example — How the Index Works:**

| Doc ID | Author | Year | Language | Content |
|---|---|---|---|---|
| D1 | William Shakespeare | 1601 | English | "alas poor Yorick" |
| D2 | John Milton | 1667 | English | "paradise lost" |
| D3 | William Shakespeare | 1601 | English | "Hamlet" |

- **Author Index:** `Shakespeare → D1, D3` | `Milton → D2`
- **Year Index:** `1601 → D1, D3` | `1667 → D2`
- **Language Index:** `English → D1, D2, D3`

**Query:** `Author = Shakespeare AND Year = 1601` → **Result: D1, D3** (intersection of both parametric indexes).

### Technique 2: Zone Indexing
Divides a document into **named regions** — title, author, abstract, body — and indexes terms **within each zone**. A term appearing in the title is treated differently from the same term appearing in the body.
- Zone examples: **Title Zone** (document's name/heading), **Author Zone** (writer's name), **Body Zone** (main free-text content).

**Worked Example — Searching Within Zones:**

| Zone | Document D1 | Document D2 | Index Entry |
|---|---|---|---|
| Title | "Merchant of Venice" | "History of England" | `merchant.title → D1` |
| Author | "William Shakespeare" | "William Smith" | `william.author → D1, D2` |
| Body | "A famous play by Shakespeare" | "Shakespeare influenced English lit." | `shakespeare.body → D1, D2` |

**Query:** `merchant` in Title **AND** `william` in Author
- Step 1: `merchant.title → D1`
- Step 2: `william.author → D1, D2`
- **Result: Intersection → D1 only** ✓

### Technique 3: Weighted Zone Scoring
Not all zones are equally important — a match in the **title** often signals stronger relevance than a match in the **body**. Weighted Zone Scoring assigns a **numerical weight** to each zone and computes a combined relevance score.

**Formula:** `Score(q, d) = Σ (gᵢ × sᵢ)`
- `gᵢ` = zone weight
- `sᵢ` = 1 if the query matches in that zone, 0 if not
- **Weights must sum to 1.0**

**Example weight assignment:**
- Author — weight 0.2 (lowest importance)
- Title — weight 0.3 (medium importance)
- Body — weight 0.5 (highest importance)

**Worked Example — Calculating a Document Score:**
Query: `"Shakespeare"` | Document D1: Title = "Merchant of Venice", Author = "William Shakespeare", Body = "A famous play by Shakespeare"

| Zone | Weight (g) | Match? (s) | g × s |
|---|---|---|---|
| Author | 0.2 | Yes → 1 | 0.2 |
| Title | 0.3 | No → 0 | 0.0 |
| Body | 0.5 | Yes → 1 | 0.5 |

**Final Score for D1** = (0.2×1) + (0.3×0) + (0.5×1) = **0.7**

*(Note: the slide's own worked arithmetic states: Score = (0.2×0)+(0.3×1)+(0.5×1) = 0.8, matching "Author: No→0, Title: Yes→1, Body: Yes→1" — i.e., in the slide's actual example the match occurred in Title and Body, not Author. Either way, the method is the same: sum weight×match across zones.)*
- **D2 Score** = 0.2 (given as: match only in Author zone) → **D1 (0.8) ranks above D2 (0.2)**.

### Technique Comparison

| Feature | Parametric | Zone Indexing | Weighted Zone |
|---|---|---|---|
| Based On | Structured metadata | Document regions | Zone importance |
| Examples | Author, Year | Title, Body | Weighted zones |
| Content Type | Structured data | Free text | Boolean + weights |
| Purpose | Filter documents | Search regions | Rank documents |
| Example Query | `Year = 1601` | `"AI" in Title` | Title match = higher score |

> Each technique builds on the last — from simple filtering, to region-aware search, to scored ranking.

### Quick Self-Check (from slides)
1. Which technique uses Author and Year fields? → **Parametric Indexing**
2. Which technique searches Title and Body zones? → **Zone Indexing**
3. In weighted scoring, what does `s = 1` mean? → **Query matches that zone**
4. If Title weight = 0.4, Body weight = 0.6, and match occurs only in Title → Score = **0.4**
5. Parametric vs Zone Indexing — main distinction? → **Structured fields vs free-text zones**

---

## Lecture 21–22: Term Weighting, TF-IDF, and the Vector Space Model

### Log-Frequency Weighting
Since raw term frequency isn't ideal for scoring (see Lecture 19), we use a **dampened** version:

**Log-frequency weight** of term `t` in document `d`:
```
w(t,d) = 1 + log₁₀(tf(t,d))   if tf(t,d) > 0
w(t,d) = 0                     otherwise
```
Mapping examples: `0 → 0`, `1 → 1`, `2 → 1.3`, `10 → 2`, `1000 → 4`, etc.

**Score for a document–query pair** = sum over terms `t` in both `q` and `d` of `w(t,d)`. The score is **0** if none of the query terms is present in the document.

### Document Frequency (df)
- **Rare terms are more informative than frequent terms** (recall: stop words).
- Consider a rare query term (e.g., *arachnocentric*) — a document containing it is very likely to be relevant → we want a **high weight** for rare terms.
- Consider a frequent query term (e.g., *high, increase, line*) — a document containing it is *somewhat* more likely to be relevant, but it's not a sure indicator → we want positive weights for these too, but **lower** than for rare terms.
- We use **document frequency (df)** — the number of documents containing the term — to capture this.

### IDF Weight (Inverse Document Frequency)
- `df(t)` = document frequency of `t` = number of documents that contain `t`.
- `df(t)` is an **inverse measure** of the informativeness of `t` (and `df(t) ≤ N`).
- **IDF definition:**
```
idf(t) = log₁₀(N / df(t))
```
- We use `log(N/df)` instead of raw `N/df` to **dampen** the effect of idf. (The base of the log turns out to be immaterial for ranking purposes.)

**IDF Example (N = 1,000,000):**

| Term | df(t) | idf(t) |
|---|---|---|
| calpurnia | 1 | 6 |
| animal | 100 | 4 |
| sunday | 1,000 | 3 |
| fly | 10,000 | 2 |
| under | 100,000 | 1 |
| the | 1,000,000 | 0 |

*(There is one idf value per term in the collection.)*

### Effect of IDF on Ranking
- Does idf affect ranking for **one-term queries**, like `iPhone`? — **No.** IDF has **no effect on ranking for one-term queries** (since idf just scales all documents' scores by the same constant factor).
- IDF **does** affect ranking of documents for queries with **at least two terms**. E.g., for the query `capricious person`, idf weighting makes occurrences of the rarer word **"capricious"** count for **much more** in the final ranking than occurrences of the common word **"person"**.

### Collection Frequency vs Document Frequency
- **Collection frequency** of `t` = total number of occurrences of `t` in the whole collection (counting multiple occurrences per document).
- **Document frequency** of `t` = number of *distinct documents* containing `t`.

**Example — which word is a better search term (should get higher weight)?**

| Word | Collection frequency | Document frequency |
|---|---|---|
| insurance | 10,440 | 3,997 |
| try | 10,422 | 8,760 |

Even though both words have similar **collection** frequency, "insurance" appears in far **fewer documents** (df) than "try" — suggesting "insurance" is concentrated in fewer, more specific documents, making it the more discriminative/better search term. **This suggests df is the better statistic to use** (rather than raw collection frequency) for weighting.

### TF-IDF Weighting
The **tf-idf weight** of a term is the **product** of its tf weight and its idf weight:
```
tf-idf(t,d) = tf-weight(t,d) × idf(t)
```
- The **best-known weighting scheme** in Information Retrieval.
- Note: the "-" in "tf-idf" is a **hyphen**, not a minus sign. Alternative names: `tf.idf`, `tf × idf`.
- **Increases** with the number of occurrences within a document (tf).
- **Increases** with the rarity of the term in the collection (idf).

There are many variants of this formula: how "tf" is computed (with/without logs), and whether query terms are also weighted.

### Documents and Queries as Vectors
- Each document is represented by a **real-valued vector of tf-idf weights** ∈ `ℝ^|V|` (recall: Binary → Count → Weight matrix progression).
- We now have a **|V|-dimensional vector space**: terms are the **axes**, documents are **points/vectors** in this space.
- This space is **very high-dimensional** — tens of millions of dimensions for a web search engine — and vectors are **very sparse** (most entries are zero).

**Queries as vectors:**
- **Key idea 1:** represent the query as a vector in the same space, using the same weighting scheme.
- **Key idea 2:** rank documents according to their **proximity** to the query in this space.
  - `proximity ≈ similarity of vectors`
  - `proximity ≈ inverse of distance`
- This moves us away from the strict "in-or-out" Boolean model, toward ranking more relevant documents higher than less relevant ones.

### Formalizing Vector Space Proximity: Why NOT Euclidean Distance
- **First cut idea:** distance between two points (the endpoints of the two vectors) — i.e., **Euclidean distance**.
- **Euclidean distance is a bad idea**, because it is **large for vectors of different lengths**, even when they point in a very similar "direction."
- **Example:** the Euclidean distance between query `q` and document `d2` can be large even though the *distribution* of terms in `q` and `d2` is very similar (e.g., d2 might just be a much longer document repeating the same terms proportionally).

### Use Angle Instead of Distance
**Thought experiment:** take a document `d` and append it to itself, forming `d'`.
- "Semantically," `d` and `d'` have the same content (same term proportions).
- The **Euclidean distance** between `d` and `d'` can be quite large (since `d'` has twice as many term occurrences).
- The **angle** between `d` and `d'` is **0**, corresponding to maximal similarity.

**Key idea: rank documents according to the angle with the query** (smaller angle = more similar).

### From Angles to Cosines
Two equivalent notions:
- Rank documents in **decreasing order of the angle** between query and document.
- Rank documents in **increasing order of cosine(query, document)**... 

*(Correction/clarification from the material: since cosine is a monotonically **decreasing** function over `[0°, 180°]`, ranking by decreasing angle is equivalent to ranking by **increasing** cosine value being wrong — actually, smaller angle ⇒ **larger** cosine ⇒ **more similar**. So in practice: rank documents in **decreasing order of cosine similarity** to get documents in order of increasing angle / decreasing similarity-to-most-similar-first. The important operational takeaway: **higher cosine value = more similar = higher rank**.)*

### Length Normalization
A vector can be **length-normalized** by dividing each component by its length — using the **L2 norm**:
```
||v||₂ = sqrt(Σ vᵢ²)
```
- Dividing a vector by its L2 norm makes it a **unit vector** (lies on the surface of the unit hypersphere).
- **Effect on `d` and `d'`** (d appended to itself): after length normalization, they have **identical vectors**.
- Long and short documents now have **comparable weights** — length normalization removes the unwanted effect of document length on the raw distance/dot-product.

### Cosine Similarity Formula
```
cos(q, d) = (q · d) / (||q|| × ||d||)
```
where:
- `q · d` = dot product of the (raw) vectors.
- `qᵢ` = tf-idf weight of term `i` in the query; `dᵢ` = tf-idf weight of term `i` in the document.
- `cos(q,d)` is the **cosine similarity** — equivalently, the cosine of the angle between `q` and `d`.

**For length-normalized vectors**, cosine similarity reduces simply to the **dot product**:
```
cos(q, d) = q · d       (for q, d already length-normalized / unit vectors)
```

### Worked Example: Cosine Similarity Among 3 Documents
How similar are the novels **SaS** (Sense and Sensibility), **PaP** (Pride and Prejudice), and **WH** (Wuthering Heights)?

**Term frequencies (raw counts):**

| term | SaS | PaP | WH |
|---|---|---|---|
| affection | 115 | 58 | 20 |
| jealous | 10 | 7 | 11 |
| gossip | 2 | 0 | 6 |
| wuthering | 0 | 0 | 38 |

*(Note: to simplify the example, idf weighting is not applied — only log-frequency weighting + normalization is used.)*

**Step 1 — Log frequency weighting** `w = 1 + log₁₀(tf)`:

| term | SaS | PaP | WH |
|---|---|---|---|
| affection | 3.06 | 2.76 | 2.30 |
| jealous | 2.00 | 1.85 | 2.04 |
| gossip | 1.30 | 0 | 1.78 |
| wuthering | 0 | 0 | 2.58 |

**Step 2 — After length normalization (divide by L2 norm):**

| term | SaS | PaP | WH |
|---|---|---|---|
| affection | 0.789 | 0.832 | 0.524 |
| jealous | 0.515 | 0.555 | 0.465 |
| gossip | 0.335 | 0 | 0.405 |
| wuthering | 0 | 0 | 0.588 |

**Step 3 — Compute cosine similarities (dot products of normalized vectors):**
```
cos(SaS, PaP) ≈ (0.789×0.832) + (0.515×0.555) + (0.335×0) + (0×0) ≈ 0.94
cos(SaS, WH)  ≈ 0.79
cos(PaP, WH)  ≈ 0.69
```
**Question posed:** Why is `cos(SaS,PaP) > cos(SaS,WH)`? → Because *Sense and Sensibility* and *Pride and Prejudice* (both Jane Austen novels) share more similar term-usage patterns (e.g., high "affection", low "wuthering"/"gossip") than *Sense and Sensibility* and *Wuthering Heights* (a very different, darker Brontë novel that heavily uses "wuthering" and "gossip").

### Weighting May Differ in Queries vs Documents (SMART Notation)
Many search engines allow **different weighting schemes** for queries vs. documents.
- **SMART Notation:** denotes the combination in use, written as `ddd.qqq` — three letters for the document weighting scheme, three for the query weighting scheme.
- A very standard weighting scheme: **`lnc.ltc`**
  - **Document side (`lnc`):** **l**ogarithmic tf, **n**o idf, **c**osine normalization.
  - **Query side (`ltc`):** **l**ogarithmic tf, id**f** (t), **c**osine normalization.
- **Is leaving off idf on documents a bad idea?** No — leaving off idf weighting on **documents** is good for both **efficiency** and **system effectiveness** reasons.

### Worked Example: tf-idf with `lnc.ltc`
**Document:** "car insurance auto insurance"
**Query:** "best car insurance"

| Term | Query tf-raw | Query tf-wt | df | idf | Query wt | Query n'lize | Doc tf-raw | Doc tf-wt | Doc wt | Doc n'lize | Product |
|---|---|---|---|---|---|---|---|---|---|---|---|
| auto | 0 | 0 | 5000 | 2.3 | 0 | 0 | 1 | 1 | 1 | 0.52 | 0 |
| best | 1 | 1 | 50000 | 1.3 | 1.3 | 0.34 | 0 | 0 | 0 | 0 | 0 |
| car | 1 | 1 | 10000 | 2.0 | 2.0 | 0.52 | 1 | 1 | 1 | 0.52 | 0.27 |
| insurance | 1 | 1 | 1000 | 3.0 | 3.0 | 0.78 | 2 | 1.3 | 1.3 | 0.68 | 0.53 |

- **Final Score** = sum of products = 0 + 0 + 0.27 + 0.53 = **0.8**
- Notice: **"auto"** contributes 0 to the score because it doesn't appear in the query (query tf-raw = 0), even though it appears in the document — this illustrates that the score is computed only over the **intersection of query and document terms** for this scheme.
- **"best"** contributes 0 because it doesn't appear in the document.
- Document weighting here uses **no idf** (document tf-wt = raw/log tf directly, not multiplied by idf) — consistent with the `lnc` scheme, while the **query** weighting *does* multiply by idf (the `t` in `ltc`).

### Summary: Vector Space Ranking (Full Procedure)
1. Represent the **query** as a weighted tf-idf vector.
2. Represent **each document** as a weighted tf-idf vector.
3. Compute the **cosine similarity score** between the query vector and each document vector.
4. **Rank** documents with respect to the query by score.
5. Return the **top K** (e.g., K = 10) documents to the user.

---

# Quick-Reference Summary Table (All Lectures 1–22)

| Lecture(s) | Core Topic | Key Concepts / Algorithms |
|---|---|---|
| 1–2 | Introduction to IR | Documents, corpus, queries, IR process, structured/unstructured/semi-structured data, classical IR models |
| 3–4 | Boolean Retrieval | Term-document incidence matrix, AND/OR/NOT, exact match, sparsity problem |
| 5–6 | Inverted Index | Dictionary + postings, indexer steps, merge/intersect algorithm, query optimization, proximity search |
| 7–9 | Document Processing | Tokenization, normalization, case folding, stop words, thesauri, lemmatization, stemming (Porter's algorithm), skip pointers |
| 10 | Dictionary Structures | Hash tables vs B-trees for term dictionaries |
| 11–12 | Tolerant Retrieval | Wildcard queries (B-tree, Permuterm Index, K-gram Index), spelling correction (edit distance, n-gram overlap, Jaccard), Soundex |
| 13–16 | Index Construction | Hardware basics, BSBI, SPIMI, dynamic indexing, logarithmic merging, MapReduce, ACLs |
| 17 | Index Compression | Heaps'/Zipf's laws, dictionary compression, gap encoding, VB & γ codes |
| 19 | Vector Space Intro | Ranked retrieval, Jaccard for scoring, term-document count matrix, bag of words, term frequency |
| 20 | Parametric & Zone Indexing | Parametric indexing, zone indexing, weighted zone scoring |
| 21–22 | TF-IDF & Vector Space Scoring | Log-frequency weighting, idf, tf-idf, cosine similarity, length normalization, SMART notation, lnc.ltc |

---

*End of Notes — Good luck with your midterm!*
