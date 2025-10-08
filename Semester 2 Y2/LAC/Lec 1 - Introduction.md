### Intro
Module regards two fundamental notions in computer science: languages and computation, and how they are related. Topics include:
- Automata theory
- Formal languages
- Models of Computation
To answer questions such as how to describe the set of strings that are valid Java programs and given a string, how to determine if it is valid Java program and how to recover the structure of a Java program from a 'flat' string. 
Study regular expressions and grammars to precisely describe languages, and various kinds of automata to decide if string belongs to language, and how to derive programs that answer this type of question. 

25% cw
75% exam

### Chomsky Hierarchy
Classifies grammars and languages + their descriptive power in an attempt to describe languages formally
![[Pasted image 20250211055326.png]]
#### Level 3
Regular languages
A language is regular if it can be recognised by DFA or NFA/can write program which uses finite amount of memory and can say whether something belongs to the language. 
Automata: Finite automata
	DFA, NFA
Grammars: Regular expression

#### Level 2
Context free languages
Automaton which has access to a stack, and can be arbirarily large unlike fixed.
Automata: Pushdown automata
	PDA, NPDAs result in different languages either NCF or DCF
Grammars: CFG

#### Level 1
Context sensitive languages
Define the context, think info extraction

#### Subclass - Decidable languages
A language is decidable if there is a method to determine if a word is in the language
#### Level 0
Recursively enumerable languages