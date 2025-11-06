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

#### Graph Approach

#### Ontology Approach

#### List of differing approaches



### Lexical Choice

### Realisation

### Full System

