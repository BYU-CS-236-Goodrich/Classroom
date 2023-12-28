# HW04 - Complex FSAs and Regular Expressions

---

### Question 1

Draw a finite state automaton (FSA) for a string representing a single time of day. Your FSA should accept times such as:

`12:36 pm 1:59 am 4:00 pm 2:45 am`

It also should reject times like:

`01:00 pm 99999:00 am 13:99 pm 10:60 pm`

Note that there are no leading zeros. Also, am and pm are lowercase and have no periods. A terminal error state is not necessary.

To make the task easier, you can make up any grouping you want. For example, you may wish to let:

\<digit\> represent any digit

\<1\> represent the digit 1

\<0-2\> represent any digit 0 through 2

\<space\> represent a space.

Time statements are formatted in many ways, but we want your FSA to only accept the above time format and not military time or any other time format.

A useful site for drawing them digitally: [https://www.madebyevan.com/fsm/](https://www.madebyevan.com/fsm/)

---

### Question 2

Give a regular expression for the FSA described in Question 1.

Here are the symbols we would like you to use:

> () - parenthesis
>
> \* - Kleene Star
>
> $\cup$ or | - Union
>
> [] - brackets ([2-5] is a shorthand for (2 | 3 | 4 | 5))

Test your regex [here](https://regexr.com/)

### Question 3

Draw the following Finite State Automata (FSA) needed for project 1:

---

#### Part A

An FSA that recognizes the language consisting of "strings" in Project 1 (see Definition 4 from section 13.3.4 and the description of Project 1 under Course Content).

```
A string is a sequence of characters enclosed in single quotes.

White space (space, tab) is not skipped when inside a string. Two adjacent single quotes within a string denote an apostrophe.

The line number for a string token is the line where the string begins.

If a string is not terminated (end of file is encountered before the end of the string), the token becomes an undefined token.
```

---

#### Part B

An FSA that recognizes the language consisting of "line comments" in Project 1.

```
A line comment starts with a hash character (#) and ends at the end of the line or end of the file.

A line comment does not have '|' as it's second character
```

---

#### Part C

An FSA that recognizes the language consisting of "block comments" in Project 1.

```
A block comment starts with #| and ends with |#.

Block comments may cover multiple lines.

Block comments can be empty and multiple comments can appear on the same line.

The line number for a comment token is the line where the comment begins.

If a block comment is not terminated (end of file is encountered before the end of the comment), the token becomes an undefined token.
```
