## Essential module info
- 40% CW, 60% in-person examsys closed book
- Weeks 1-5 labs, one worksheet per week (**CRUCIAL**), weeks 6-8 project, weeks 9-11 project interviews
- Project issued **28th October**, due **20th Nov**, encode knowledge of a given real-world like situation, and using automated reasoning software, perform the reasoning to find a solution, and analyse the performance of the solution method, including a brief report. Compulsory interview during the computing classes.

## Long-Standing split in AI
### Symbolic AI
- The term for the collection of all methods in AI research that are based on high-level, symbolic(human-readable) representations of problems, logic, and search.
- Dominant until 90s, model is explicit e.g., in the form of rules
- The model is provided by a human
- Formal reasoning
### Non-Symbolic AI
- Dominant since 2000s, the model is implicit
- The model is automatically extracted from patterns observed in the data
- Statistical or implicit reasoning
Why symbolic? Because knowledge and rules and specified using symbols, symbols which can represent concepts, relationships, and logical formulae.


### Limitations
#### Symbolic AI
- **Rigid model** - hard to update; the input has to be unambiguous. 
- Some tasks are easy for humans but unlikely to ever be formalisable.
#### Non-Symbolic AI
- Interpretation of non-symbolic models is hard or impossible, hard to say what an ML system will do, the learning process and reaching a conclusion is obscured from humans.![[Pasted image 20251001111653.png]] ![[Pasted image 20251001111705.png]]![[Pasted image 20251001112825.png]]


## Knowledge, Representation, and Reasoning
- Want to know how knowledge can be represented symbolically and manipulated in an automated way by reasoning programs
- **Knowledge:** some information about the world e.g.,
	- Medical information about some particular set of diseases: what causes them, how to diagnose them
	- Geographical Data: which city is the capital of which country, population statistics.
- **Representation**: how/in which language do we represent this information (symbolically)
- **Reasoning** = how to extract more information from what is explicitly represented/how to reach a conclusion (as cannot possibly hope to represent every single fact explicitly in a database).
## Knowledge Based systems
- Program that reasons and uses a **knowledge base** to solve complex problems.
- Want to be able to talk about some AI programs in terms of what they know - and therefore must contain **explicitly represented symbolic domain-specific knowledge**. 
- This knowledge may be structured by means of ontology, frames, logical assertions etc.
- All knowledge based systems contain a knowledge base, and a reasoning system that allows them to derive new knowledge, also known as an inference engine. 
### Example
```prolog
printColour(snow) :- !, write("It’s white."). printColour(grass) :- !, write("It’s green."). printColour(sky) :- !, write("It’s yellow."). printColour(X) :- !, write("Beats me.").
```
Essentially a lookup table hardcoded in Prolog. When ask `printColour(snow)`, immediately matches the first clause and prints that it is white. There is **no representation of the fact the snow is white**. Instead, the rule itself directly produces the output. If the clause for snow is removed, there;s no knowledge left that could be used to infer or reconstruct its colour. This is just a program encoding specific behaviour, not storing or reasoning with knowledge.

### Example
```
printColour(X) :- colour(X,Y), !, write("It’s "), write(Y), write("."). 
printColour(X) :- write("Beats me."). 

colour(snow, white). 
colour(sky, yellow). 
colour(X,Y) : - madeof(X,Z), colour(Z,Y). 

madeof(grass, vegetation). 
colour(vegetation, green).
```
Here, the behaviour of printColour depends on querying the knowledge base either directly, or using inference rules.
For example:
	Query printColour(grass)
	Checks colour(grass, Y)
	Doesn't find a direct fact, but is knows madeof(grass, vegtation), Then uses colour(vegetation, green.)
	Concludes colour(grass, green).
This is knowledge based, as the system contains explicit representations of knowledge, that are queryable, and able toreasoned with. If a piece of knowledge is removed, the system will fail to give the correct answer, not due to missing code, but due to missing knowledge. 
![[Pasted image 20251001114917.png]]


### Examples of knowledge based systems
#### Mycin
- Production Rule system
- Purpose: Automatic Diagnosis of bacterial infections
- Lots of interviews with experts on infectious diseases, translated into rules
- Approx 500 rules.
- ![[Pasted image 20251001151738.png]]
Some facts and conclusions of the rules are not absolutely certain
MYCIN uses numerical certainity factors, range between - 1 and 1. When tested on real cases, did as well or better than members of Stanford medical school.
#### XCON
- Expert Configurer, system for configuring VAX computers, production rule system written using OPS5, a language for production systems written in LISP
- 10k rules
- Used commercially

#### CYC
- CYC knowledge server is a large knowledge base and inference engine
- Developed by Cycorp
- Aims to provide a deep layer of common sense knowledge to be used by other knowledge-intensive programs.
##### CYC Knowledge BAse
- Contains terms and assertions in formal language CycL, based on FOL, syntax similar to LISP
- Knowledge base contains classification of thing, and also facts, rules of thumb, heuristics for reasoning etc.
- Currently, over 200k terms, and many human-entered assertions involving each term; Cyc can derive/infer new assertions from those.
- General knowledge: things, intangible things, physical objects, individuals, collections, sets, relations
- Domain specific knowledge e.g., physiology, chemistry

#### Watson