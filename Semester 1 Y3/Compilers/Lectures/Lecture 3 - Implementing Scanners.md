### Hopcroft's Algorithm
An algorithm which performs **$DFA$ Minimisation**. Transforms a given DFA into an equivalent DFA that has a minimum number of states. Two DFAs are equivalent if they recognise the same regular language.

Three states of classes that can be removed or merged from the original $DFA$ without affecting the language it accepts.
1. **Unreachable States** - states that are not reachable from the initial state of the DFA, for **any** input string. These states can be removed.
2. **Dead states** are the states from which no final state is reachable. These states can be removed unless the automaton is required to be complete (transition defined for every state.)
3. **Nondistinguishable** states - those that cannot be distinguished from one another for **any** input string. These states can be merged.

Hopcroft's Algorithm O(n log n)is concerned with merging the nondistinguishable states of a DFA and is based on partition refinement - partitioning the DFA states into groups by their behaviour. 

Initially, the set of final states and non-final states are placed into two separate partitions. The algorithm repeatedly refines these partitions based on input symbols, until no finer refinement is possible.

Each final partition represents one state in the minimised DFA, representing equivalence classes whereby states are equivalent if they have the same behaviour for every input sequence. That is for two states $q_1$ and $q_2$ that belong to the same block of partition $P$, and every input word $w$, the transitions determined by $\hat{\delta}$ with respect to $w$ should always take states $q_1$ and $q_2$ to either states that both accept or states that both reject. It should not be possible for $w$ to take $q_1$ to an accepting state and $q_2$ to a rejecting state and vice-versa.

Alternatively if $q_i \to_c q_x$ and $q_j \to_{c} q_y$ and $q_{i}, q_{j} \in p_{s}$, then $q_x, q_y$ must be in the same set $p_t$. This property holds for every set $p_s \in P$, for every pair of states. Thus the states in a given $p_s$ have the same behaviour with respect to input characters.

This is made concrete below:
![[Pasted image 20251009181709.png]]
The first one does not split, all initially in same set, all have transition defined for a, and lead to the same partition.
However in second case, $d_x$ is in a different partition, and will not have the same behaviour as $p_3$. Thus $p_1$ must be split, reflected on the RHS to account for this. Example below.

![[Pasted image 20251009182100.png]]

1. First examine the set $\{s3,s5\}$. Since neither state has an exiting transition, the state does not split on any character.
2. In the second step, it examines $\{s0,s1,s2,s4\}$; on the character $e$, it splits {s2,s4} out of the set. 
3. In the third step, it examines $\{s0,s1\}$ and splits it around the character f. 
4. At that point, the partition is $\{ \{s3,s5\}, \{s0\}, \{s1\}, \{s2,s4\} \}.$ The algorithm makes one final pass over the sets in the partition, splits none of them, and terminates.

![[Pasted image 20251009183612.png]]
![[Pasted image 20251009183729.png]]

### Limitations of Hopcroft
- In a scanner, different final states correspond to different lexical categories. If we merge final states, we will be unable to effectively classify lexemes.
- Solution is to simply not merge sets of final states when finishing the algorithm.


## Implementing Scanners
- Scanner construction is a tool where the theory of formal languages has produced tools that can automate implementation. 
- For most languages, a compiler writer can produce an acceptably fast scanner directly from a set of regular expressions. 
- The compiler creates a regular expression for each syntactic category and gives the $REs$ as input to a scanner generator. 
- The generator constructs an $NFA$, then creates a corresponding $DFA$ and minimises the $DFA$. The scanner generator must convert the $DFA$ into executable code.

There are three common implementation strategies for scanners.  All of them operate in the same manner - by simulating the $DFA$. This process stops when the $DFA$ **recognises** a word, which occurs when the current state $s$ has no outbound transition on the current input character. If $s$ is accepting, the scanner recognises the word and returns a **lexeme** and its **syntactic category** to the caller. If $s$ is a nonaccepting state, the scanner must determine whether or not it passed through an accepting state on its way to $s$. If it did, it should roll back its internal state and its input stream to that point and report success. If it did not, it should report the failure.

All of the below implementation strategies differ in the details of their runtime costs. They all have the same asymptotic complexity - constant cost per character, plus the cost of roll back. The differences in efficiency of well implemented scanners change the constant costs per character but not the overall complexity $O(n)$.
### Table-Driven Scanners
- A table driven lexer is a (usually automatically generated) program that uses some kind of lookup table to determine which action to take, simulating the transition table/function of a DFA.
- Each character lookup is done by indexing into a 2d array `next_state = transition_table[current_state][input_char]` (or char_category as j index.)
- When a final state is reached, emits lexeme + lexical category.
- Often uses additional.
	- Classic approach would have one column in transition table for each character for each state.
	- Instead group characters into classes e.g., digit, lowercase letter and use one column per class.
	- Vastly reduces table size.
- After a table has been generated, the main program is just a simple loop until reach `EOF`. 
	`*state = START;*`
	`*while ((ch = next_char()) != EOF) {*`
    `*cclass = char_class[ch];`*               
    `*state = transition_table[state][cclass];*  `
    `*if (state == ACCEPT_NUM) return TOKEN_NUMBER;*`
    `*if (state == ERROR) report_error();* }`
	![[Pasted image 20251010191654.png]]
- Furthermore, must include a token type/lexical category table which maps each DFA state to the lexical category it represents:$$\text{Type[s] = token type produced if DFA halts in state s}$$
- If a state is not accepting, its entry in `type` is `invalid`.
#### Rollback
- We must also consider a rollback loop which uses a stack of states to revert the scanner to its most recent final/accepting in case the $DFA$ overshoots the end of the token.
- As ingesting input, place states onto a stack, if the end of input sequence/word is reached and the state is not accepting, pop() until reach accepting state whilst truncating lexeme, and then according to type table assign lexical category and return valid lexeme. 
- Below details a rollback example, and also an example of where rollback is not favourable; certain DFAs can result in quadratic rime spent reading the input stream, according to rollback strategy.
![[Pasted image 20251010192832.png]]

### Maximal munch scanner
- The principle that when creating some construct, as much of the available input as possible should be consumed.
- This principle can be used to combat excessive rollback in table-driven scanners, and to avoid the problem of inherent ambiguity in commonly used regular expressions e.g., via use of the kleene star.
- This differs in three important ways:
	1. It has a global counter, `InputPos` to record position in input stream.
	2. It has a bit-array `Failed[state, position]` to record dead-end transitions as the scanner finds them. 2d array with a row for each state and a column for each position in the input stream. 
	3. Has initialisation routine that must be called before `NextWord` is initially invoked. This routine sets `InputPos` to zero and sets `Failed` uniformly to false.

#### Algorithm
- Start from initial DFA state $s_0$ 
- Initialisation routine executed - reset `InputPos` and set `Failed` to false.
- Read characters, advancing `InputPos` and following transitions using transition table.
- If a transition `(curr_state, char_category)` fails:
	- Mark `Failed[curr_state, InputPos] = true`
	- Roll back to most recent accepting state if one exists, performed easily by quick index into failed.
- When an accepting state is reached:
	- Apply maximal munch rule: continue reading as long as valid transitions exist
	- Only stop when next transition would lead to failure.
	- Emit type from type table.
Enhances table-driven as it remembers which state, position transitions failed, avoiding redundant work. 

### Direct-coding
- To improve the performance of a table-driven scanner, we must reduce the cost of one or both of its basic actions: *read a character* and *compute the next $DFA$ transition*.
- Direct coded scanners replace the explicit representation of the DFAs transition table with an implicit representation which simplifies the two-step table lookup computation, lower overhead per character.
	- This is warranted because for each character, the table-driven scanner performs address computations and 2 load operations, one in $CharCat$ and another in $\delta$.
	- Both lookups take $O(1)$ time e.g., hashmap. To access the $i^{th}$ element of $CharCat$ must compute its address: $CharCat_0 + i \times w$, start address, index, number of bytes in each element of $CharCat$. More complex cost for $\delta$, as has 2 dimensions. 
- To achieve, we embeds $DFA$ logic into program control structure
- Each state of the DFA becomes a block of code, and transitions are implemented as:
	- `switch` statements
	- `goto`
	- nested `if/else`
Example:
```C
int nextToken() {
    int c = nextChar();

    if (isalpha(c)) {  // start of identifier
        while (isalnum(c))
            c = nextChar();
        rollback(c);
        return IDENTIFIER;
    }
    else if (isdigit(c)) {  // start of number
        while (isdigit(c))
            c = nextChar();
        rollback(c);
        return NUMBER;
    }
    else if (isspace(c)) {
        return nextToken(); 
    }
    else {
        return UNKNOWN;
    }
}
```
Can jump to other states, depending on what is necessary. see below for another example.
![[Pasted image 20251010202251.png]]
No complicated address caluclations, just branching and jumps. Uses same rollback mechanisms.

### Hand-Coded scanner
- This is a lexer manually written by the programmer without an automatic generator or transition tables.
- Directly use procedural logic to recognise tokens from the input. 
- Has more manual control over character reading and rollback mechanism as opposed to generated lexers.
```C
int nextToken() {
    skipWhitespace();

    char c = nextChar();
    if (c == EOF) return END_OF_FILE;

    // Identifiers
    if (isalpha(c)) {
        string lexeme = "";
        do {
            lexeme += c;
            c = nextChar();
        } while (isalnum(c));
        rollback(c);
        return IDENTIFIER;
    }

    // Numbers
    if (isdigit(c)) {
        string lexeme = "";
        do {
            lexeme += c;
            c = nextChar();
        } while (isdigit(c));
        rollback(c);
        return NUMBER;
    }

    // Single-character tokens
    if (c == '+' || c == '-' || c == '*') {
        return OPERATOR;
    }

    return INVALID;
}
```
Written to behave like a $DFA$ but its not generated from one. Makes code much more readable, but is much harder to scale for complex lexical grammars. Suffices for most purposer.

Can also perform optimisations here. E.g., buffering the input stream, where each read operation returns a longer string of characters, and the scanner then indexes through the buffer. (each `getc()` or equivalent can have large fixed cost for syscall and per-character cost for reading and moving data). Using buffer amortises the fixed I/O cost over $n$ buffer size.

The scanner maintains a pointer into the buffer. Responsibility for keeping the buffer filled and tracking current location in buffer falls to NextChar. This leads to a simple and efficient implementation of the Rollback operation that occurs in lexers. To roll input back, scanner can simply decrement the input pointer. 

But if the pointer is already at the beginning of the buffer, cannot rollback any further as the previous characters were overwritten. To safely allow rollbacks across buffer boundaries, use two adjacent $n$ size buffers and treat as circular array.
- When move the input pointer - `InputPtr = (InputPtr + 1) mod (2n)`
- When rolling back - `InputPtr = (InputPtr - 1 + 2n) mod (2n)`



