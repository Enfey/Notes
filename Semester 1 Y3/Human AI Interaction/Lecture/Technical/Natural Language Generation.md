Post intent matching, natural language generation occurs in a generic NLP pipeline. 

# Natural Language Generation
The subfield of AI and computation linguistics concerned with automatically producing human-readable text from structured data or internal representations.


## NLG use cases
Informational: weather reports, medical summaries, financial reports etc
Entertainment: story generation, AI roleplaying etc
Interaction: text interface to websites
Assistive: aids for people with special communication needs
Bot journalism/propaganda

## Template-Based vs Dynamic NLG
Two broad schools of though in NLG 

1. Template-Based
	Uses fixed sentence patterns with slots, i.e., templates, and then plug the slots. Gap filling systems are the simplest approach, then rule based which are expanded gap-filling systems, and then grammatical functions which are more complex but are not fully dynamic.
2. Dynamic
	Uses extensive rules or machine-learned models to build sentences dynamically. Flexible and creative, but there is a high error rate potential. 

## How NLG fits into NLP pipeline
Typical command processing e.g., What is the time?
One converts speech to tokens and further to some representation, and understands the intent of the question via some method of classification. Once intent has been understood, generation can be produced. 

Thus, it is the prepare result stage - turning structured intent or data into a fluent human-readable response.

### General Architecture of a template-based NLG system
1. **Document Planner**
	*Content Determination* - Decide **what** to say(communicative goal, level of precision)
	*Document Structuring* - Decide in what order to say it (order of information) to produce more natural results.
		E.g., S1 = I went to the store, S2 = I bought some fresh fruits.
		Structure 1 places S1 first and S2 second
		Structure 2 vice versa, structure 1 is more natural. 
2. **Microplanner**
	*Lexicalisation* - Choose which words to use, depending on genre, context and perception
		Is word appropriate to the context and would this sentence have multiple interpretations?
	*Referring Expression Generation* - Creation of expressions referring to entities ("Paul", "he", "the man")
		Multiple types: proper-noun noun, definite noun phrase, spatial, temporal. Referring expressions must be unambiguous and easy to understand and compute. 
	*Aggregation* - Merge syntactic or conceptual constituent sentences 
		e.g., "Paris is warm. London is warm" -> "Paris and London are warm" is syntactic
3. **Realiser**
	*Syntactic realisation* - Build sentence structure
	*Morphological realisation* - Handle plurals, tenses etc
	*Orphographic realisation* - Apply punctuation and capitalisation

## Statistical Language Models
Before neural models, NLG used $n-gram$ probability models. Given a sequence of tokens, predict the next token. 
