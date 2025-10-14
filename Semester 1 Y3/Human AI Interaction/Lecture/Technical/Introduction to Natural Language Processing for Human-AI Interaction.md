## Natural language understanding
- NLU is a branch of artificial intelligence and a subset of NLP that focuses on machine reading comprehension i.e., enabling computers to understanding meaning, intent, and context in human language.
- Underpins applications in automated reasoning, large-scale content analysis (sentiment analysis, topical analysis), question answering, translation, information extraction.
- The breadth of an **NLU** system is measured by the size and coverage of its vocabulary and grammar, whereas the depth is determined by how closely the system's understanding approximates that of a fluent native speaker - incorporating semantics and world knowledge.
- Techniques in NLU involve
	- **Morphological analysis:** - understanding word structure (prefixes, suffixes, inflections)
	- **Syntactic parsing** (automatic analysis of syntactic structure of natural language)
	- **Discourse analysis**(extracting meaning out of input)
	- **Knowledge grounding**(linking language to external world models)
	- **Pragmatic reasoning** (e.g., intent recognition)
- Challenges include **ambiguity,** **context dependence**, **metaphor** and **idiom interpretation**, and **commonsense reasoning.** 
## Natural language Generation
- Subfield of artificial intelligence, NLP, and computational linguistics concerned constructing systems that generate coherent, contextually appropriate natural language from structured or unstructured data. 
- Disagreement about whether the inputs of NLG system must be non-linguistic e.g., **image descriptions**, **database records**, or whether linguistic to linguistic transformation e.g., **paraphrasing**, **summarisation** also qualifies.
- Conceptually complementary to **NLU:**
	- **NLU** maps **natural language** $\to$ **machine representation**
	- **NLG** maps **machine representation** $\to$ **natural language**
- NLG operates from precisely known inputs, so uncertainty and ambiguity handling are less central.
- Core stages in NLG pipeline:
	- Content Determination - selecting which information to express
	- Document Planning - structuring content into an organised form e.g., sections, sentences.
	- Microplanning - deciding how to express content linguistically (lexical choice etc)
	- Surface realisation - generating final grammatical text (word order, morphology)
	- Post-processing - fluency optimisation etc
## Historical trends in NLP
### Symbolic NLP (1950s = 1990s)
- Early work in NLP focused on using rule-based systems/knowledge driven systems grounded in linguistic theory and formal logic
- Systems used handcrafted grammars, lexicons and semantic networks
- Parsing and generation were achieved using explicit rules such as context-free grammars.
- High interpretability, and explicit linguistic knowledge, but scaling to open domains required infeasible amounts of manual work. Was not able to robustly handle ambiguity, variability and error in real-world language use
### Statistical NLP(1990s-present)
- Began with the availbility of large corpora and improved computational resources
- Language was modelled probabilistically rather than using hand-written rules
- Common techniques involve:
	- **n-gram language models** - predicts probability of next work in sequence based on fixed window of the preceding words.
	- **Hidden Markov Models** for POS tagging and speech recognition - apply probability model to estimate POS of next word (markov chain+markov property)
	- **Maximum Entropy Models** - can be used in POS tagging, named entity recognition, speech recognition. Model dependencies and does not assume conditional independence between consecutive states, consider past predictions when making current one. Chooses the model with the highest entropy from all models that fit the training data. 
- Emphasises data-driven learning and evaluation metrics
- Enabled scalable applications of information extraction, machine translation etc.
- Lacked deep semantic undertanding and struggled with long-range dependencies.
## Common NLP tasks
### Text Classification
- Assigning predefined labels to text
- Examples: sentiment analysis, topical classification, spam detection
- Naive Bayes → SVMs → CNN/RNN → Transformers.
### Part of Speech Tagging
- Assigning syntactic categories to words
### Named Entity Recognition
- Identifying and classifying entities
- Applications: ifnromation extraction, QA, etc
### Syntactic Parsing
- Deriving the grammatical structure of sentences (dependency or constituency trees)
- Used in downstream tasks such as relation extraction
### Semantic Role Labeling
- Determining who did what to whom, when, where, assigning roles to sentence constituents based on predicate-argument structure.
### Coreference Resolution
- Determining which expressions refer to the same entity.
### Machine Translation 
- Translating text between languages.
- Evolution: Rule-based → Statistical → Neural MT
### Question answering
- Extractive QA(finding answers in text), generative QA(producing full responses) and open-domain QA(retrieving information from large corpora.)

## Preliminary notions
- Each unit of interest we analyse is a **document**. 
	- A book, a book chapter, a paragraph, a tweet etc
- A collection of documents of interest is a **corpus**
	- The Gutenberg project library, all of John McAfee's interview scripts etc
	- Any arbitrary subset of a corpus can also be a corpus
- All about framing what we want to analyse.
- A set of consectuive words is called an **n-gram**. n determine how many tokens we observe.
## NLP pipeline
![[Pasted image 20251013200422.png]]

### Preprocessing
![[Pasted image 20251013200455.png]]
#### Tokenisation
This is the process of breaking text into smaller units, called tokens. Similar process to lexing in compiler frontends without the inclusion of lexical categorisation (POS tagging is often complementary though). Allows for more straightforward processing of text further down NLP pipeline e.g., counting word frequencies, parsing, or vectorisation. 
#### Part of speech tagging
Part of speech refers to the category of words that share similar grammatical properties e.g., noun, verb, adjective, article, conjunction, pronoun etc.
POS tagging gives each token/word its grammatical function in a sentence. Different languages have different parts of speech, different POS parsers have different POS tags for the same language. Using POS tags helps disambiguate meaning using grammatical context as a reasoning vehicle.
#### Word standardisation
##### Stemming
The goal of stemming is to recover the stem of a word, which is the common part of all inflections of that word. This is then used to group words with the same root, such as *walking* and *walked* becoming walk, to improve efficiency and accuracy. The stem produced need not be identical to the morphological root of the word e.g., the resulting stem for *arguing* is *argu*; this is acceptable as the goal is just to group words, not create linguistically perfect roots. There are several types of stemming algorithms which differ in respect to performance and accuracy.

A simple stemmer looks up the inflected form in a lookup table, simple fast, handles exceptions. However all inflected forms must be explicitly listed in the table: new or unfamiliar words are not handled, and the table may be large, especially for highly inflected languages like Turkish.

Suffix stripping algorithms, which are rule-based, attempt to find the root form of a word by crudely removing the suffix. Prefix stripping may also be implemented. Doing both is called affix stemming. 

Stochastic algorithms involve using probability to identify the root form of a word. Learn of a table of the root form to inflected form relations to develop a probailistic model.

Either brute-force and exhaustively search, or use implicit reasoning to stem a word.
##### Lemmatising 
Lemmatisation is the process of grouping inflected forms of word so that they can be analysed as a single item, identified by the word's lemma(dictionary form, morphological root). This is the key difference from stemming, focuses on the morphological root to give more meaningful results. A trivial way to accomplish this is via dictionary lookup. Words well for straightforward inflected forms, but a rule-based system will be needed for cases such as in languages with long compound words. 

This process involves first determining the part of speech(category of words that share similar grammatical properties e.g., noun, verb, adjective, article, conjunction, pronoun) of a word, and applying different normalisation rules for each part of speech. This approach is conditional upon obtaining the correct lexical category of a given word. Identifying the wrong category or being unable to produce the right category limits the added benefit of this approach over crude suffix stripping algorithms. 


#### Filtering: stop words
Refers to filtering out common words that do not add much meaning to textual analysis. Reduces noise in data, and allows NLP pipeline to focus on meaningful characteristics in the text that carry the semantic weight. Any group of words may be used as a list of stopwords.
