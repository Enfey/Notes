>  **Hyper-heuristics** are high-level search methods or learning mechanisms for selecting or generating low-level heuristics to solve computationally difficult problems. 

Hyper heuristics do not not directly solve the problem - instead operate upon the search space of low-level heuristics - whereas MHs operate upon the search space of solutions. 

Hyper heuristics introduce a layer of abstraction known as the **problem domain barrier**, sitting between the problem domain the decision-making process. Enforces separation between domain-specific information and logic used to guide search process.

So hyper heuristics don't directly access domain specific information such as solution representation, they are afforded a level of limited feedback with which to impose effective control strategies, with a view to creating the most general mechanism that achieves the best degree of optimality for a given problem domain. 

Hyper heuristics can be classified differently:
**Feedback**
	- **Online learning** - hyper heuristic learns and adapts dynamically during solving, using limited, domain-independent feedback to improve its decisions.
	- **Offline learning** - learns prior to deployment, and applies learned strategy without further adaptation.
	- **No-learning** - uses fixed strategy to choose heuristics, such as IE acceptance.
- **Nature of the heuristic search space**
	- **Heuristic selection** - operate by choosing from a pool of existing heuristics
		- **Constructive Heuristics** - build a solution step by step e.g., greedy strategies
		- **Perturbative Heuristics** - operate on existing solution to explore search space e.g., mutation
	- **Heuristic Generation** - generate new heuristics, often by combining components, e.g., low-level operators, specific rules etc.
		- **Constructive heuristics** - generates heuristics/procedures to construct solutions from components
		- **Perturbative heuristics** - constructs new ways to modify existing solutions.