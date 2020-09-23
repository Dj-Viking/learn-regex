# learn-regex


Hey there! Welcome to this tutorial that I hope can help any web developer to better understand regex (short for Regular Expression)

---

## Summary

Regex is widely used among many computer languages to match strings, lines, groups of strings. Assists with any sort of algorithms that require specific matching methods to get a desired result.

(Full Disclosure I am making this in reference to JavaScript. So not every engine works the same across different computer languages! Or even different browser testers versus the built in node.js regex engine seem to process RegExp differently)

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
* Tries to match 1 or more times
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

* These can all match 0 or more times and will greedily grab all connecting word characters to the matched pattern, but if nothing specified in front of the first delimiter will match against spaces and non-word characters and replace them with a (```' '```) space character
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
The ( ```?``` ) quantifier can be referred to as the optional, or the lazy quantifier.
* Optionally it can be used to match against some words that can be spelled differently like in the english language favour and favor, or neighbour and neighbor.
```js
const british = 'I wanted to ask my neighbour a favour';
const USA = 'I wanted to ask my neighbor a favor';
const optionalRegex = /\w+ou?r/g;

const matchBrit = british.match(optionalRegex);
console.log("british", matchBrit);
//will output
british [ 'neighbour', 'favour' ]

const matchUSA = USA.match(optionalRegex);
console.log("USA", matchUSA);
//will output
USA [ 'neighbor', 'favor' ]
```
In the lazy variation of this quantifier ( ```?``` ) it stands up to its name: it only has to match as many characters as specified in the regular expression and nothing more.
```js
const lazyRegex = /Ava?/;
const words = 'How many Avalanches happen per year?';
const lazy = words.match(lazyRegex);
console.log(lazy);
//will output to the console
[
  'Ava',
  index: 9,
  input: 'How many Avalanches happen per year?',
  groups: undefined
]
//using global flag will just simply output the characters it only needed to match
(/Ava?/g)
[ 'Ava' ]
```

---
### OR Operator
The OR operator ( ```|``` ) can be used in conjunction with capture groups to grab a certain variation of similary spelled words but given other parameters we can narrow down a specific match.
```js
const orRegex = /th(e|i|n)ng.?/g;
const words = 'then I was there to do the thing';
const match = words.match(orRegex);
console.log(match);
//matches
[ 'thing' ]

const orRegex = /th(e|i|n)r.?/g;
const words = 'then I was there to do the thing';
const match = words.match(orRegex);
console.log(match);
//matches
[ 'there' ]

const orRegex = /th(e|a|i|n)n.?/g;
const words = 'then I was there to do the thing';
const match = words.match(orRegex);
console.log(match);
//matches
[ 'then ', 'thing' ]
```
---
### Character Classes

Denoted by characters placed inside ( ```[ ]``` ) Also can be called a character set. These can tell the regex engine to match (or not match) only one out of many different characters. Use flags to further optimize the match to something more specific or broad.
* Options include but not limited to:
  - ```/[0-9]/``` if searching for a single character that is either 0 through 9 
  - ```/[a-z]/``` if searching for a single lowercase character in the category of the english alphabet a through z
  - ```/[A-Z]/``` if searching for a single uppercase character in the category of the english alphabet A through Z
  - ```/[i-m0-8]/``` if searching for an isolated set of characters or any sort of range of characters
  - ```/[^a-z0-9]/``` using the carat anchor here can negate this particular set of characters from the engine to match against
```js
const charClass = /[0-9].?/;
const words = 'I once saw a giraffe who was 30 feet tall!';
const match = words.match(charClass);
console.log(match);
//using the .? we can match the entire number 30 in the sentence
[
  '30',
  index: 29,
  input: 'I once saw a giraffe who was 30 feet tall!',
  groups: undefined
]
```
---
### Flags

### Grouping and Capturing

### Bracket Expressions

### Greedy and Lazy Match

### Boundaries

### Back-references

### Look-ahead and Look-behind

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
