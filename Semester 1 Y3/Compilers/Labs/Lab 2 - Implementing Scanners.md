Implement simplified $Lisp$ scanner.

For our purposes, numbers in Lisp are integers with an optional minus sign before them. Symbols are sequences of letters, numbers, and hyphens that begin with a letter (we will only use lowercase letters to keep things simple). Here are some examples: 
- 0123: valid number 
- -3210: valid number
- 1.23: invalid number 
- abcd: valid symbol 
- ef1-2: valid symbol 
- 8hij: invalid symbol 
- -d34: invalid symbol 
- (a b 2): valid list

DESIGN LEXICAL GRAMMAR FOR SIMPLIFIED LISP, DETERMINE HOW MANY TOKEN TYPES THERE NEED TO BE.
