# Project 2: Datalog Parser

## General Project Requirements

- Projects are to be completed by each student individually (not by groups of students).
- Projects must be passed-off through Github Classroom.
- Even though we provide the test cases, the definition of your project working is that it passes all tests on Github Classroom. The TA's will only grade what is on Github Classroom.

## Resources

- Jupyter Notebook Tutorial: https://github.com/BYU-CS-236/jupyter-notebook-tutorials/tree/main/Recursive%20Descent%20Parsing
- Starter Code: https://classroom.github.com/a/o0xJt-27
- Please watch the [TA help video](https://www.youtube.com/watch?v=lTdpGL-qCS4) and review the [TA help slides](https://docs.google.com/presentation/d/1OhRZPN43zWOSJAMgddOZvsD_B4WwizdP5n5akUvSRec/edit#slide=id.g24652ea9475_0_0). They will be extremely helpful!
- Project Diagrams: <a href="diagrams" target="_blank">Link</a>

Write a parser that reads a datalog program from a text file, builds a data structure that represents the datalog program, and outputs the contents of the datalog program data structure.

## Example Input
```
Schemes:
  snap(S,N,A,P)
  HasSameAddress(X,Y)

Facts:
  snap('12345','C. Brown','12 Apple','555-1234').
  snap('33333','Snoopy','12 Apple','555-1234').

Rules:
  HasSameAddress(X,Y) :- snap(A,X,B,C),snap(D,Y,B,E).

Queries:
  HasSameAddress('Snoopy',Who)?
```
## Example Output
```
Success!
Schemes(2):
  snap(S,N,A,P)
  HasSameAddress(X,Y)
Facts(2):
  snap('12345','C. Brown','12 Apple','555-1234').
  snap('33333','Snoopy','12 Apple','555-1234').
Rules(1):
  HasSameAddress(X,Y) :- snap(A,X,B,C),snap(D,Y,B,E).
Queries(1):
  HasSameAddress('Snoopy',Who)?
Domain(6):
  '12 Apple'
  '12345'
  '33333'
  '555-1234'
  'C. Brown'
  'Snoopy'
```
## Testing
##### _As stated before, testing will be done through Github Classroom. You can also test your code locally with your own input, and by running the test cases in VSCode. If you forgot how to do this, review the Project 0 help video here: https://www.youtube.com/watch?v=jZOf5oN-lKA. Just like Project 0, your grade will be whatever grade is on Github Classroom when you submit your code._

Here are some of the edge cases that we will test you on, so keep them in mind:
1. A program with extra tokens at the end of the file.
2. A program that uses the wrong punctuation.
3. A program that has missing punctuation.
4. A program with empty fact or rule lists.
5. A program with empty scheme or query lists.
6. A program with schemes, facts, rules, and queries, but in the wrong order.

## Design 
You will build a datalog interpreter in later projects. The datalog interpreter will need to access the datalog program data structure created by the parser. The data structures should be designed such that the datalog interpreter is able to easily get the lists of schemes, facts, rules and queries from the parser.

## The Datalog Grammar

The Datalog Grammar defines valid datalog programs. Use the grammar to write a recursive-descent parser for datalog programs. Use the lexer from the previous project to provide input tokens for the parser.

The nonterminals in the grammar begin with a lower-case letter. Terminal symbols in the grammar are written in all upper-case letters. The word 'lambda' in the grammar represents the empty string.

Note that comments do _not_ appear in the grammar because comments should be ignored. See the TA help video and slides for recommendations on how to do this.

```
datalogProgram	->	SCHEMES COLON scheme schemeList FACTS COLON factList RULES COLON ruleList QUERIES COLON query queryList EOF

schemeList	->	scheme schemeList | lambda
factList	->	fact factList | lambda
ruleList	->	rule ruleList | lambda
queryList	->	query queryList | lambda

scheme   	-> 	ID LEFT_PAREN ID idList RIGHT_PAREN
fact    	->	ID LEFT_PAREN STRING stringList RIGHT_PAREN PERIOD
rule    	->	headPredicate COLON_DASH predicate predicateList PERIOD
query	        ->      predicate Q_MARK

headPredicate	->	ID LEFT_PAREN ID idList RIGHT_PAREN
predicate	->	ID LEFT_PAREN parameter parameterList RIGHT_PAREN
	
predicateList	->	COMMA predicate predicateList | lambda
parameterList	-> 	COMMA parameter parameterList | lambda
stringList	-> 	COMMA STRING stringList | lambda
idList  	-> 	COMMA ID idList | lambda
parameter	->	STRING | ID
```
## Datalog Program Data Structures

A datalog program consists of lists of schemes, facts, rules and queries. Use Python's built in list functionality. Please do not write your own list class.

You need to store the information for the schemes, facts, rules, and queries in the lists. Because schemes, facts, and queries all have the same form as predicates, one class named Predicate can be used to represent all of these types of objects. You don't need classes named Scheme, Fact, and Query. Schemes, facts, and queries can (and should) be stored in Predicate objects. You will find a Rule class helpful to store the head Predicate (the left-hand side of the colon-dash) and a list for the body Predicates (the right-hand side of the colon-dash).

This <a href="diagrams/project-2-data-structures.png" target="_blank">diagram</a> may be helpful in understanding the data structures.

You are required to have the following classes: DatalogProgram, Rule, Predicate, and Parameter. You may also have others, but you must have these.

Write a 'to_string' method for each of these classes so that you can easily print them out. Return a DatalogProgram object from the parser and then traverse this structure and use 'to_string' as needed to return the required output.

To integrate this project with later projects, be sure to have a way to get the lists of schemes, facts, rules and queries out of the DatalogProgram.

## Output Format
If the parse is successful, output 'Success!' followed by the lists of schemes, facts, rules, queries in the datalog program. Output the number of items in each list as shown in the example output. Indent each list item by 2 spaces (not using the tab character!) Output the schemes, facts, rules, and queries in the same order in which they appear in the input.

Also output the set of the domain values that appear in the datalog program. The domain values are the strings (surrounded by quotes) that appear in the facts. Don't include strings in the domain if they only appear in the rules or queries sections. The domain only includes strings found in the facts section. Note that the domain is a set of strings. (The same string is not printed more than once.) Output the domain strings in sorted order. (Sort the strings based on the characters inside the strings, not including the quotes.) You can and should use the built-in Python 'set' for this.

## Syntax Errors
If the parse is unsuccessful, output 'Failure!' followed by the offending token. Output the type, value, and line number of the token surrounded by parentheses and indented by 2 spaces on a new line of output. Note that the parser stops after encountering the first offending token.
### Example Input
```
Schemes:
    snap(S,N,A,P) 
    NameHasID(N,S)
 
Facts:
    snap('12345','C. Brown','12 Apple','555-1234').
    snap('67890','L. Van Pelt','34 Pear','555-5678').
 
Rules:
    NameHasID(N,S) :- snap(S,N,A,P)?
 
Queries:
    NameHasID('Snoopy',Id)?
```
### Example Output
```
Failure!
  (Q_MARK,"?",10)
```
## Implementation Requirements
1. You must implement a deterministic top-down parser that chooses the rule to expand based on the current token.
2. You must implement the parser using the recursive-descent approach.
3. You must use the lexer from the previous project to read tokens from the input.
4. You must create classes for DatalogProgram, Rule, Predicate, and Parameter to hold the internal representation of a datalog program. Each class must have a to_string method and the output of the program must be formed by these to_string methods.
5. The parser must run to completion with a normal exit status for any input. Do not terminate with a non-zero exit status for any input, including inputs that have errors.
6. What strings are included in the domain? The domain consists of all strings found in the facts. It does not include strings found only in the rules or queries.

## Implementation Suggestions
1. ### What's a good way to organize your work on this project?
    A good approach is to complete the project in three steps:

    - First: write a parser that only checks syntax -- this is the recursive-descent part. (Does not build any data structures.)
    - Second: write classes for data structures. (Rule, Predicate, Parameter, etc.)
    - Third: add code to the parser to create data structures. This can be done easily without modifying the lines of code that were created in the first step. For example when a parameter is being parsed a Parameter object is created and returned, then saved in the appropriate place.
2. ### What's a good way to handle syntax errors?
    A good approach is to use exceptions to handle syntax errors. Throw an exception when the parser detects a syntax error. Catch the exception at the top level of the parser to report either success or failure. (Don't return a boolean from each parsing routine to indicate a syntax error; that would mean that each return value must be checked, resulting in far more complicated code.)
3. ### Why do you need the Parameter, Predicate, Rule, and Datalog Program classes, and what should be in these classes?
    You need these classes because the next project builds on this project and these classes are needed for the next project. These new classes should _not_ store tokens.

    To determine what needs to be in these classes, use the grammar as a guide.

    For example, in the grammar a predicate is defined as: ID LEFT_PAREN parameter parameterList RIGHT_PAREN. This means a predicate object needs to store a string (to hold the value of the initial ID) and a list of Parameter objects.
4. ### Should you store token types in the data structures created by the parser?
    **_No_**, don't store tokens or token types in the data structures created by the parser. The data structures created by the parser should be decoupled from both the scanner and the parser.