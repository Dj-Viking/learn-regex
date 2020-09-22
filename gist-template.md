# learn-regex


Hey there! Welcome to this tutorial that I hope can help any web developer to better understand regex (short for Regular Expression)

---

## Summary

Regex is widely used among many computer languages to match strings, lines, groups of strings. Assists with any sort of algorithms that require specific matching methods to get a desired result.

(Full Disclosure I am making this in reference to JavaScript and not all regex engines work the same across different computer languages! 😁 )

 If you are trying to match an email, an exact string, or group of strings you can store a regular expression inside a variable for later use in languages like JavaScript as shown below:

---
* Matching an email
```js
//literal notation
const regex = /\w+@\w+\.(net|com|org)/g;

//use the RegExp constructor with a string pattern as the first argument (with an optional second argument for flags)
const regex = new RegExp('\\w+@\\w+\\.(net|com|org)', 'g');

//use the RegExp constructor with EMCAScript 6 notation
const regex = new RegExp(/\w+@\w+\.(net|com|org)/, 'g');

//test if the email pattern matches
const myEmail = 'anders.swedishviking@gmail.com'
if (regex.test(myEmail)){
  return 'this email matches the regex pattern!'
} else {
  return 'this email does not match the regex pattern!'
}
```

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
Using anchors refers to a set of regex tokens that ***don't*** match with any character, But will let the regex engine know something about the string we are trying to match. 
* For example 
  - ```/^/``` will match the beginning of a line. 
  - ```/$/``` will match the end of a line.
    * In JavaScript, to notify the regex engine to match at the beginning of every line, we must include the flag (```m```) after the closing delimiter like so: 
    ```js 
    const dogRegexBeginsOnEveryLine = /^dog/m
    const dogRegexEndsOnEveryLine = /dog$/m
    ```
---

### Quantifiers
The quantifier ( ```+``` ) lets the regex engine know to match a certain amount or quantity of the character, token, or subexpression to the left of this ```+``` or any quantifier symbol
* this will match anywhere there is a letter c in a string
```js
const result = 'cat is a cat'.match(/c+/g);
console.log(result);
//will output
[ 'c', 'c' ]
//without global flag
(/c+/)
//will output
[ 'c', index: 0, input: 'cat is a cat', groups: undefined ]
```
The quantifier ( ```*``` ) applies to the metacharacter token ```\w```. Essentially will match against any word-character in the string ( word characters include anything lowercase a-z, uppercase A-Z, numbers 0-9, the _ underscore character, spaces occur in the match method if the character is not a word character and is replaced with a space )
```js
const result = 'cat is a<script>someFunction();</script> cat'.match(/ca\w*/g);
console.log(result);
//including global flag will match any amount of the same or similar connected word-characters and this expression outputs:
[ 'cat', 'cat' ]
//without global flag will match the first set of connected word-characters that match the expression
(/scr\w*/)
//will output
[
  'script',
  index: 0,
  input: 'cat is a<script>someFunction();</script> cat',
  groups: undefined
]
```
The ( ```?``` ) quantifier can be referred to as the optional, or the lazy quantifier

---
### OR Operator

### Character Classes

### Flags

### Grouping and Capturing

### Bracket Expressions

### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
