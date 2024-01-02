# Project 1: Datalog Lexer

## General Project Requirements

- Projects are to be completed by each student individually (not by groups of students).
- Projects must be passed-off through Github Classroom.
- Even though we provide the test cases, the definition of your project working is that it passes all tests on Github Classroom. The TA's will only grade what is on Github Classroom.

## Special Requirement

- You may not use a regular expression library for this project.
- You must use Finite State Automata along with a Finite State Machine.

## Resources

- Jupyter Notebook Tutorial: https://github.com/BYU-CS-236/jupyter-notebook-tutorials/tree/main/Finite%20State%20Automata
- Starter Code: https://classroom.github.com/a/Hz58MXCw
- Please watch the [TA help video](https://www.youtube.com/watch?v=q7Vhi4iF9yk) and review the [TA help slides](https://docs.google.com/presentation/d/1UWBySn_zOmFnB6mab0Subqf72I7vSU8Sfaj4ZLlZfmM/edit#slide=id.g247d0b6917c_0_0). They will be extremely helpful!
- Project Diagrams: <a href="diagrams" target="_blank">Link</a>

## Project Description

The purpose of this project is to write a lexer that will read in a sequence of characters, identify the datalog language tokens found in the file, and output each token using Finite State Automata and a Finite State Machine.

## Examples

### Example Input
```
Queries:
   marriedTo ('Bea' , 'Zed')?

Rules:
   marriedTo( X,Y ) :- marriedTo(Y,X) .
```

### Example Output
```
(QUERIES,"Queries",1)
(COLON,":",1)
(ID,"marriedTo",2)
(LEFT_PAREN,"(",2)
(STRING,"'Bea'",2)
(COMMA,",",2)
(STRING,"'Zed'",2)
(RIGHT_PAREN,")",2)
(Q_MARK,"?",2)
(RULES,"Rules",4)
(COLON,":",4)
(ID,"marriedTo",5)
...
(EOF,"",5)
Total Tokens = 26
```

### Example Input
```
,
'a string'
# a comment
Schemes
FactsRules
::-
```

### Example Output
```
(COMMA,",",1)
(STRING,"'a string'",2)
(COMMENT,"# a comment",3)
(SCHEMES,"Schemes",4)
(ID,"FactsRules",5)
(COLON,":",6)
(COLON_DASH,":-",6)
(EOF,"",6)
Total Tokens = 8
```

## Testing
<p>
As stated before, testing will be done through Github Classroom. You can also test your code locally with your own input, and by running the test cases in VSCode. If you forgot how to do this, review the Project 0 help video here: https://www.youtube.com/watch?v=jZOf5oN-lKA. Just like Project 0, your grade will be whatever grade is on Github Classroom when you submit your code.
</p>

Here are some of the edge cases that we will test you on, so keep them in mind:

- An empty input file.
- A colon immediately followed by another token (no space between the colon and the next token).
- An identifier that contains a number.
- An identifier that contains a keyword.
- An empty string (nothing between the quotes '').
- An unterminated string.

## Design

You will build a datalog parser in the next project. The datalog parser will read tokens from the datalog lexer. The lexer should be designed such that the parser is able to easily get the tokens from it.

## White Space

White space is a sequence of space, tab, or newline characters. Your lexer should always skip over white space between tokens. White space is not completely ignored because it is sometimes needed to separate tokens. In Python, an easy way to recognize white space characters is to use the 'isspace' function.

## Output Format

The expected output is a list of the tokens found in the input file followed by a count of the number of tokens found. The tokens are output one token per line.

Each line has the form:

`(type,"value",line)`

The 'type' must be one of the types listed in the table. The 'value' is the actual input text that forms the token. The 'line' is the line number where the token is found. Notice there are no spaces on either side of the commas separating the token's elements.

The last line of output has the form:

`Total Tokens = N`

where 'N' is the number of tokens found.

### **All output should be returned from your 'project_1' function.**

## Input Errors

When the input contains an error, create a 'UNDEFINED' token, and terminate execution by returning your current list of tokens. An UNDEFINED token's value will always be the first character of the invalid portion of the input.

Undefined tokens are:

- A single character that cannot be the first character of a valid token.
- A string that is not terminated.

## Token Types

The following table describes the types of tokens your lexer must recognize.

| Token Type    | Description                                         | Examples                 |
|---------------|-----------------------------------------------------|--------------------------|
| COMMA         | The ',' character                                   | ,                        |
| PERIOD        | The '.' character                                   | .                        |
| Q_MARK        | The '?' character                                   | ?                        |
| LEFT_PAREN    | The '(' character                                   | (                        |
| RIGHT_PAREN   | The ')' character                                   | )                        |
| COLON         | The ':' character                                   | :                        |
| COLON_DASH    | The string ":-"                                     | :-                       |
| MULTIPLY      | The '*' character                                   | *                        |
| ADD           | The '+' character                                   | +                        |
| SCHEMES       | The string "Schemes"                                | Schemes                  |
| FACTS         | The string "Facts"                                  | Facts                    |
| RULES         | The string "Rules"                                  | Rules                    |
| QUERIES       | The string "Queries"                                | Queries                  |
| ID            | An identifier is a letter followed by zero or more letters or digits, and is not a keyword (Schemes, Facts, Rules, Queries). Note that for the input "1stPerson" the lexer would find two tokens: an 'undefined' token made from the character "1" and an 'identifier' token made from the characters "stPerson". | <table>  <thead>  <tr>  <th></th>  <th>Valid Identifiers</th>  <th>Invalid Identifiers</th>  </tr>  </thead>  <tbody>  <tr>  <td></td>  <td>Identifier1</td>  <td>1stPerson</td>  </tr>  <tr>  <td></td>  <td>Person</td>  <td>Schemes</td>  </tr>  </tbody>  </table> |
| STRING        | A string is a sequence of characters enclosed in single quotes. White space (space, tab, etc.) is not skipped when inside a string. Two adjacent single quotes within a string denote an apostrophe. The line number for a string token is the line where the string begins. If a string is not terminated (end of file is encountered before the end of the string), the token becomes an undefined token.<br><br> The 'value' of a token printed to the output is the sequence of input characters that form the token. For a string token this means that two adjacent single quotes in the input are printed as two adjacent single quotes in the output. (In other words, don't convert two adjacent single quotes in a string to just one apostrophe in the output.) | 'This is a string'<br><br>'' -- (The empty string)<br><br>'This isn''t two strings' |
| COMMENT       | A comment starts with a hash character (#) and ends at the end of the line or end of the file. | # This is a comment |
| UNDEFINED     | Any character not tokenized as a string, keyword, identifier, symbol, or white space is undefined. Additionally, any non-terminating string is undefined. In that case, you reach EOF before finding the end of the string. Any undefined token should result in the creation of one undefined token (this will always be the last token you create). The program should then end execution by returning the current list of tokens. | $&^ (Three undefined tokens)<br><br>'a string that does not end |
| EOF           | The end of the input file. |

