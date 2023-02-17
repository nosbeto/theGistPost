# Explaining how Regex is used to match a valid email address

The purpose of this readME is to explain how regex helps to validate an email address in your app

## Summary

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary. Normally, when you want to verify for an email we use the following REGular EXpression components

Matching an Email – `/^([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})$/`

Lets go ahead and decompose this

theGistPost\img\RegexExplanation.png

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors
Anchors have special meaning in regular expressions. They do not match any character. Instead, they match a position before or after characters:
* ^ – The caret anchor matches the beginning of the text.
* $ – The dollar anchor matches the end of the text.

In our example, we use `^` character to state that the string must start and end with a pattern of `[a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]`. You'll notice that we didn't include the pattern {2,6}, which precedes the $ character. Why? Because this is a special component called a quantifier. We'll come back to quantifiers in a moment.

Let's take a look at the pattern `[a-z0-9_\.-]` and see what it means.

### Bracket Expressions
Anything inside a set of square brackets `([])` represents a range of characters that we want to match. These patterns are known as bracket expressions, but they are also known as a positive character group, because they outline the characters we want to include. We can write these expressions to include all of the characters we want to match. For example, `[abc]` will look for a string that includes a or b or c, regardless of the length of the string. So all of the following examples would match: `"aaa"`, `"bin"` `"court"`, `"abracadabra"`, and `"bca"`.

You'll more commonly see a hyphen (-) used between alphanumeric characters (letters and numbers only) to represent a range of those possible characters. This means that `[a-c]` and `[abc]` will look for the exact same thing.

In our “Matching a email” regex example, we can break down the bracket expressions as follows:
`[a-z]` - The string can contain any lowercase letter between a–z. Keep in mind that this looks for lowercase characters only. If we wanted to include uppercase characters, we would need to change the expression to `[a-zA-Z].`
`[0-9]` - The string can contain any number between `0–9`
`_` -  The string can contain underscore. Both the underscore and the hyphen are called special characters. Special characters include any non-alphanumeric characters, such as punctuation or symbols. In this case, we only want a string that includes `_`
`\.` - The string (email) can contain a dot. The The backslash `\` is used to treat the dot as a dot and not a regular expresssion

We finalized studying our square bracket `[a-z0-9_\.-]` 

Continuing with our email example, the next thing that follows is our `+` which is a quantifier. So, let's explain what quantifiers are.

### Quantifiers
Quantifiers indicate numbers of characters or expressions to match. They set the limits of the string that your regex matches (or an individual section of the string). They frequently include the minimum and maximum number of characters that your regex is looking for.

They include the following
* `*` --> Matches the preceding item as many times as possible
* `+` –-> Matches the preceding item "x" 1 or more times.For example, /a+/ matches the "a" in "candy" and all the "a"'s in "caaaaaaandy"
* `?` --> Matches the preceding item "x" 0 or 1 times. For example, /e?le?/ matches the "el" in "angel" and the "le" in "angle."
* `{}` --> Curly brackets can provide three different ways to set limits for a match:
    * `{n}` --> Matches exactly the number (n) of ocurrences of the preceding item.
       * Example: `/d{4}/` matches a four-digit number like `"2023"`,`"1989"`,`"1234"`
    * `{n,}` --> Matches the pattern at least n number of times
        * Example: `/a{2,}` matches at least 2 occurences of `a`. That means that it would match `mountaain`, `mountaaaaaain`, but it won't match `mountain` because it only has one ocurrence of `a`
    * `{n,m}` --> Matches a character or character class from `n` to `m` times
        * Example: Similar to the above, but now you have a constraint on the right hand side. So, in our example above `/a{1,3}` would match `mountaain`, `mountaaaaaain`, `mountain`. However, a diference is that it's matching `mountaaaaaain` because it has three `aaa` consecutively. 

In our “Matching a email” regex example, we use the `+` quantifier to match as many times as possible the `[a-z0-9_\.-]` group.

So far we have explained the following from our matching email `/^([a-z0-9_\.-]+)`. However, we can also explain some of the items ahead since they are covered by the previous sections.  

Continuing with the linear process of our matching email, we have an `@` which actually makes sense. This is there to state we need an `@` to be a valid email. 
* i.e. alberto`@`gmail.com. After the `@` we find the beginning of a new bracket expression followed by a character class of `\d`. Let's explain Character Classes

### Character Classes
A character class in a regex defines a set of characters that can happen in a string to fulfill a match.

Here are some of the other common character classes:
* `.` Matches any character except the newline character (\n)
* `\d` Matches any Arabic numeral digit. This class is equivalent to the bracket expression `[0-9]`.
* `\w` Matches any alphanumeric character from the basic Latin alphabet, including the underscore ( _ ). This class is equivalent to the bracket expression `[A-Za-z0-9_]`.
* `\s` Matches a single whitespace character, including tabs and line breaks

Each of the last three character classes can be changed to perform an inverse match by capitalizing the letter character. For example, `\D` matches a non-digit character.

In our matching email example, we have two of these across the example:
`\d` - This is there to match any arabic numeral digit
`\.` - Matches a `.` character. This is just there to allow emails with dots
* i.e. `alberto.de.armas@gmail.com`


The following two sections are not used in our email matching example, but I will covering them as an extra reading for those who are insterested

### OR Operator
We don't have any OR operator in our example, but it is identified as the `|` symbol 

So, the code `abc|xyz` would find anything that matches abc OR xyz

### Flags
Regular expressions may have flags that affect the search and they are placed and the of a regex.

There are only 6 of them in JavaScript:
* `i` - the search is case-insensitive: no difference between A and a (see the example below).
* `g` - the search looks for all matches, without it – only the first match is returned.
* `m` - Multiline mode (covered in the chapter Multiline mode of anchors ^ $, flag "m").
* `s` - Enables “dotall” mode, that allows a dot . to match newline character \n (covered in the chapter Character classes).
* `u` - Enables full Unicode support. The flag enables correct processing of surrogate pairs. More about that in the chapter Unicode: flag "u" and class \p{...}.
* `y` - “Sticky” mode: searching at the exact position in the text (covered in the chapter Sticky flag "y", searching at position)

In our matching example, we don't use any flags

---
---
For summary, I've covered everything that we need to explain in our our email matching. I've created the following graph to help to breakout what each of the section do in our example 

## Author
Alberto De Armas is an Implementation Consultant working in the tech-fraud detection industry who is currently taking a full-stack web-developer course because it's an area of interest to hopefully become a Product Owner in the short term
