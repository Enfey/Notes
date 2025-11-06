Template based natural language generation refers to the process of producing textual output based on predefined patterns/templates, plug in placeholders to generate coherent sentences to accomplish communication goal.

## Simple Gap Filling System
A gap filling system is the most basic form of template-based NLG. 
Define structures of sentences, gaps to be filled, plug in relevant data, done. 
```python
name = input("What is your name")
country_of_origin = input("Where are you from?")

output = "Hello {v1}, i see that you're from {v2}".format(v1 = name, v2 = country_of_origin)

print(output)
```

## A full template based system
Consider essential steps of template based NLG system:
content determination, document structuring, lexical choice, referring expression generation, aggregation, and realisation. 

### Content Determination
> Decide **what** to say(communicative goal, level of precision)

Selection required depending on communicative goal

#### Rule and Heuristics based approaches
Set explicit rules and heuristics to decide which pieces of data or facts should be included. Relies on if-then-else and perhaps probabilistic methods that are predefined based on application's requirements. 
```python
def rule_based_description(car):
	description = ""
	
	if car['type'] == 'sedan'
		description += "This is a family friendly car"
	elif car['type'] == 'sports'
		description += "This car is designed for speed"
		
	return description
	
	
car_data = {'type': 'sports', 'colour': ;'red'}
print(rule_based_description(car_data))
```

Can further apply rules derived from domain expertise, for example when generating whether reports, always including temperature and precipitation details could be a heuristic. Another heuristic could be when describing a car, use a horsepower threshold to change how the car is described. 

#### Data-Driven Methods
Using statistical or machine learning models trained in large datasets to learn the significance of pieces of content. These models can select content based on patterns in the training data. 

#### User Preferences
In this case, adapt content determination based on user profiles or feedback. Over time, the system can learn to present information that is more closely aligned with the interests or preferences of the individual user

```python
user_preferences = {
	'colour_interest': 'red',
	'type_preference': 'sports'
}

def user_pref_description(car, preferences):
	description = ""
	
	if car['colour'] == preferences['colour_interest']:
		description += "This car colour matches your
		preference"
	elif car['type'] == preferences['type_preference']:
		description += "Its the type of car you like"
		
	return description
	
	
car_data = {'type': 'sports', 'colour': ;'red'}
print(user_pref_description(car_data, user_preferences))
```


#### Ontologies
In the context of content determination, ontologies are structured frameworks that represent knowledge about a specific domain, categorising entities and defining relationships between them, facilitting the understanding and generation of content by providing a semantic hierarchy and context. For example, in the medical domain, if discussing a particular disease, an ontology might suggest including symptoms, causes , and treatments.

```python
car_ontology {
	'sports': {'related_terms': ['speed', 'race', 'performance']},
	'SUV': {'related_terms': ['family', 'travel', 'space']}
}

def ontology_description(car, ontology):
	car_type = car['type']
	if car_type in ontology:
		related_terms = ', '.join(ontology[car_type]['related_terms'])
		return f"This{car_type}" is associated with {related_terms}
	return "Unknown Car Type"
```


#### Query-Driven Determination
When the NLG system is tied to a query system e.g., a search engine, a personal assistant, content determination can be directly influenced by the user's query. Only information directly responding/related to query would be selected for conciseness and to attain communicative goal.

```python
user_query = "red powerful cars"

def query_driven_description(car, query):
	description = ""
	
	if 'red' in query and car['colour'] == 'red':
		description += "The car is red,"
	if 'powerful' in query and car['horsepower'] > 400:
		description += "it is powerful."
	return description
```

#### Interactive Systems
Allow users to interactively guide content determination process, can be done by allowing users to specify constraints, ask follow-up questions, or fine tune the scope of information. 

```python
def interactive_description(car):
	print("What do you want to know about the car?")
	attribute = input("Choose: type, colour, or horsepower")
	if attribute in car:
		return f"The car's {attribute} is {car{attribute}}"
	return "That's not a feature i know about"
```

### Document Structuring
> Decide the order of information to produce more natural results.

E.g., S1 = I went to the store, S2 = I bought some fresh fruits.
	Structure 1 places S1 first and S2 second
	Structure 2 vice versa, structure 1 is more natural. 

#### Rules, heuristics and templates
Can use predefined rules or schema as well as domain specific heuristics to dictate sequence/hierarchy of information presentation, based on the nature and relationship of the content. E.g., research paper, start with intro, then methods, results, discussion. 

```python
def rule_based_structure(city_data):
    report = ""
	
	if 'historical_landmarks' in city_data:
		report += f"Historical
		Landmarks:\n{city_data['historical_landmarks']}\n\n"

    if 'tourist_attractions' in city_data:
        report += f"Tourist
       Attractions:\n{city_data['tourist_attractions']}\n\n"
      
    if 'restaurants' in city_data:
        report += f"Recommended
        Restaurants:\n{city_data['restaurants']}\n"

    return report
    
city_info = {
	'tourist_attractions': 'Central Park, City Museum',
	'restaraunts': 'Bistro Cafe',
	'historical_landmarks': 'Old Clock Tower, Heritage
	Building'
}

print(rule_based_structure(city_info))
```

Alternatively, focus on more domain heuristics
![[Pasted image 20251105221301.png]]

#### Machine Learning 
Can also use machine learning and train models on structured documents to learn typical patterns and sequences for this kind of content, one trained, can predict the best structure for new information. 

```python
from sklearn.tree import DecisionTreeClassifier
import numpy as np

X_train = np.array([
	[1, 0, 1],
	[1, 1, 0],
	[0, 1, 1]
])

y_train = [1, 2, 3] # 1: htr, 2: trh, 3: rht

model = DecisionTreeClassifier()
model.fit(X_train, y_train)

def ml_structure(city_data):
	sequence = model.predict([[
	1 if 'tourist_attractions' in city_data else 0,
	1 if 'restaraunts' in city_data else 0,
	1 if 'historical_landmarks' in city_data else 0
	]])[0]
	
	#you get the idea
```
#### Discourse models
Can make use of models that understand the conventions of textual coherence, ensuring that the document follows recognised patterns of discourse, such as introducing a topic before going into details. 

![[Pasted image 20251105222120.png]]
#### User Guided Structuring
We can also allow users to define or tweak the structure according to preference, making generated content customisable and more suited to user needs, makes sense if we have reason to think that users will have large differences in how they consume information provided.
![[Pasted image 20251105222224.png]]
#### Graph-Based Methods
We can represent content as nodes in a graph where edges indicate relationships. Algorithms can then traverse graph to determine optimal structure to present the information. 
![[Pasted image 20251105222635.png]]
Obviously can be significantly more complex than this. 

### Aggregation
Involves combining separate pieces of information into coherent, gramatically sensible, and often more concise sentences, ensuring generated content is not overly verbose.

#### Syntactic Aggregation
This aims to merge pieces of information based on the syntactic structures of sentences, without necessarily considering deeper semantics or conceptual meanings. 

E.g., John plays football, John plays basketball -> John plays basketball and football.

Aggregation performed on the common subject and verb, and uses the cojunction 'and' to bridge them. 

![[Pasted image 20251105223006.png]]
#### Conceptual Aggregation
This method merges information on the basis of their underlying concepts or meanings. It takes into account the semantics of the sentences and can potentially reformulate the sentences to convey combined concepts more effectively. 

For example:
	John is a software developer
	John creates mobile applications.
Would yield the following contextual aggregation "John is a software developer specialising in mobile applications."

Goes beyond just the simple syntactic structure to derive more concise and meaningful sentence. 

#### Heuristic Approach
```python
def aggregate_person_info(facts):
	if not facts:
		return None
		
		entity_name = facts[0].get('entity')
		if not  entity_name:
			return "information is missing an entity"
		
		occupation = None
		specialisation = None
		
		for fact in facts:
			if fact.get('entity') != entity_name
			
		attr = fact.get('attribute')
		val = fact.get('value')
		
		if attr == 'occupation'
			occuptation = val
		elif attr == 'specailisation'
			specialisation = val
		
		# Rule 1
		if occupation and specialisation:
			return f"{entity_name} is a {occupation}who
			specialises in {specailisation}"
		# Rule 2
		elif occupation:
			return f"{entity_name} is a {occupation}"
		
		return f"{entity_name} has unknown attributes"

fact_list_john = [
	{'entity': 'John', 'attribute': 'occupation', 'value':
	'data scientist'}
]
```
Overly simplistic example, in practice special data structures such as knowledge graphs can be used to keep track of conceptual relationships and aggregate them appropriately. 
#### Graph Approach
Structured representation of data, nodes for entities and edges for relationships. Often use knowledge graphs to provide semantic relationships e.g., is-a relationship

#### Ontology Approach
Formalised conceptualisations of domains, providing semantic grounding to data.

#### List of differing approaches
- Rule based - define explicit rules to determine how and when to aggregate information
- Statistical - use statistical measures or patterns derived from large datasets to decide on aggregation e.g., certain sentence structures may be more common and preferred during aggregation
- Template based - define templates where multiple pieces of information can be inserted into predefined slots in the template
- Machine learning - train models on examples of aggregated content, allowing the model to learn and predict how the new content should be aggregated



### Lexical Choice
During this step, appropriate words or phrases are chosen to convey a specific meaning, sentiment, style or tone, selecting best words and sequences to use in generated sentences given context and intended communication goal.

Factors influencing lexical choice include:
- **Specificity** - Some information requires a more specific term, while others can be more generic e.g., Canine vs Dog
- **Formality** - The level of formality required can influence the choice of words e.g., Purchase vs Buy
- **Tone and Sentiment** - Depending on whether the content is meant to be positive, negative or neutral, word choice can vary e.g., Challenging vs Problematic
- **Variability** - To avoid repetition or to engage the reader, different yet potentially equivalent lexical items may be chosen for variety.
- **Audience understanding** -  Consideration of audience's familiarity with terms is vital.

#### Rule-based Methods
Predefined rules or templates dictating which terms or phrases to use based on specific conditions or contexts. 

```python
def rule_based_choice(context)
	rules = {
		"greeting": "Hello",
		"farewell": "Goodbye",
		"thank"   : "Thank you"
	}
	return rules.get(context, "Unknown")
```
#### Thesaurus Based Methods
Use a thesaurus or lexicon to find synonyms and select them according to context.

```python
import nltk
from nltk.corpus import wordnet

nltk.download('wordnet')

def thesaurus_based_choice(word):
	synonyms = []
	
	for syn in wordnet.synsets(word):
		for lemma in syn.lemmas():
			synonyms.append(lemma.name())
	
	synonyms = list(set(synonyms))
	if word in synonyms:
		synonyms.remove(word)
		
	return synonyms[0] if synonyms else word
```

#### Statistical Methods
Statistical language models can be used to determine likelihood of the appropriateness of a word or phrase in a particular context. 

#### Machine Learning
Deep learning models like transformers can be trained on vast amounts of text to make contextually appropriate lexical choices. Using the transformers library from Huggingface, we can achieve this. 

```python
from transformers import GPT2LMHeadModel, GPT2Tokenizer

tokenizer = GPT2Tokenizer.from_pretrained("gpt2-medium")
model = GPT2LMHeadModel.from_pretrained("gpt2-medium")

def ml_choice(partial_text):
	inputs = tokenizer.encode(partial_text, 
	return_tensors="pt")
	outputs = model.generate(inputs, 
	max_length=inputs.shape[1]+1, do_sample = False)
	decoded = tokenizer.decode(outputs[0])
	return decoded.split()[-1] # Return last word
```
#### User feedback
We can also incorporate user feedback to refine lexical choices over time, especially in interactive NLG systems. This is particularly useful if you do not know enough about the user and want to leave them the choice to personalise their experience based on their preferences and level of expertise
![[Pasted image 20251106090531.png]]

### Referring Expression Generation
Creation of expressions referring to entities. Focuses on determining how to refer to entities such that they can be unambigously recognised by the listener or reader. When generating natural language, must refer to entities such that intended audience can identify and distinguish them. 

E.g., there are two apples on table, instead of just saying "The apple", a proper referring expression would specify "The green apple" or "The red apple".

Some techniques and considerations for implementing REG are:
- **Attribute Selection** - Determine which attributes are essential to distinguish the entity from others in its context
- **Definiteness** - decide whether to use definite ('the cat') or indefinite ('a cat')a articles based on familarity or uniqueness of the entity
- **Pronominal reference** - Decide whether it is appropriate to use a pronoun based on prior mentions and clarity
- **Salience** - Entities that have been mentioned or play a significant role in the discourse are more salient and might be referred to differently.
```python
developers = [
    {"name": "Alice", "specialisation": "mobile apps", "years_of_experience": 5, "company": "TechCorp"},
    {"name": "Bob", "specialisation": "web development", "years_of_experience": 3, "company": "WebSoft"},
    {"name": "Dominic", "specialisation": "robotics", "years_of_experience": 10, "company": "CMS"},
    {"name": "Dana", "specialisation": "embedded systems", "years_of_experience": 10, "company": "HardwareTech"}
]

def generate_referring_expression(target_entity, context):
    distractors = [e for e in context if e != target_entity]
    description = {}
    attributes_order = ["specialisation", "company", "years_of_experience"]

    for attr in attributes_order:
        if not distractors:
            break
        value = target_entity[attr]
        distractors_with_property = [d for d in distractors if d[attr] == value]
        if len(distractors_with_property) < len(distractors):
            description[attr] = value
            distractors = distractors_with_property

    return description

def realise_expression(properties):
    if not properties:
        return "The developer."
    phrase_map = {
        "specialisation": " who specialises in {value}",
        "company": " who works at {value}",
        "years_of_experience": " who has {value} years of experience"
    }
    phrases = []
    for attr, value in properties.items():
        if attr in phrase_map:
            phrases.append(phrase_map[attr].format(value=value))
    if not phrases:
        return "The developer with unstated properties."
    base = "The developer"
    if len(phrases) == 1:
        return base + phrases[0] + "."
    else:
        for i in range(1, len(phrases)):
            phrases[i] = phrases[i].replace(" who ", "")
        return base + " and ".join(phrases) + "."

developer_to_refer = {
    "name": "Dominic",
    "specialisation": "robotics",
    "years_of_experience": 7,
    "company": "CMS"
}

properties = generate_referring_expression(developer_to_refer, developers)
print(f"Generated properties: {properties}")

sentence = realise_expression(properties)
print(f"{sentence}")

```

### Realisation
The realisation stage is the final step in the NLG process, where structured intermediate representations or abstract specifications are transformed into coherent, grammatically correct, and fluent natural language text. 

- **Morphological inflection** - Correct morphological forms of words are chosen based on context e.g., ensuring verbs agree with their subjects in number and tense
- **Sentence Aggregation** - combining smaller sentences or clauses into more complex and fluent ones.
- **Word order determination** - in languages with flexible word order, the best order is chosen based on fluency and emphasis.
- **Insertion of function words** - Words like 'of', 'is', 'the' which might not be present in the abstract representation, are inserted for grammatical correctness.
- **Punctuation and formatting** - proper punctuation marks are inserted and the text is correctly formatted
Subtle overlap with prior stages, particularly regarding aggregation, but have different primary objectives: aggregation is about content synthesis and elimination of redundancy, while realisation is about linguistic correctness and fluency. In a well structured NLG system, aggegation occurs well before realisation, but these are only a guideline. 

```python
# List of developer dictionaries
developers = [
    {"subject": "Alice", "verb": "work", "duration": 5, "field": "mobile apps",
     "languages": ["Java", "Kotlin"], "company": "TechCorp", "location": "at"},
    
    {"subject": "Bob", "verb": "specialise", "duration": 3, "field": "web development",
     "languages": ["JavaScript", "React"], "company": "WebSoft", "location": "with"},
    
    {"subject": "Charlie", "verb": "work", "duration": 7, "field": "mobile apps",
     "languages": ["Swift"], "company": "TechCorp", "location": "at"}
]


def realise(developer):
    # Morphological Inflection
    if developer["verb"] == "work" and developer["duration"] > 1:
        verb_form = "has worked"
    elif developer["verb"] == "specialise" and developer["duration"] > 1:
        verb_form = "has specialised"
    else:
        verb_form = developer["verb"]

    # Handle list of languages
    langs = developer["languages"]
    if len(langs) > 1:
        languages = ", ".join(langs[:-1]) + " and " + langs[-1]
    else:
        languages = langs[0]

    # Construct sentence
    if developer["location"] == "at":
        sentence = (f"{developer['subject']} {verb_form} for {developer['duration']} years "
                    f"in {developer['field']} at {developer['company']} and is proficient in {languages}.")
    else:
        sentence = (f"{developer['subject']} {verb_form} for {developer['duration']} years "
                    f"in {developer['field']} with {developer['company']} and knows {languages}.")
    
    return sentence


# Generate bios
for developer in developers:
    bio = realise(developer)
    print(bio)

```