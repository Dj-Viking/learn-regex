# learn-regex


Hey there! Welcome to this tutorial that I hope can help any web developer to better understand regex (short for Regular Expression)

---

## Summary

Regex is widely used among many computer languages to match strings, lines, groups of strings. Assists with any sort of algorithms that require specific matching methods to get a desired result.

(Full Disclosure I am making this in reference to JavaScript. Not every engine works the same across different computer languages! Or even different browser testers versus the built in node.js regex engine seem to process RegExp differently)

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
if (regex.test(myEmail)) {
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
In JavaScript there are 6 different flags to use. They are placed on the right side of the delimiter slashes ( ```/[0-9]/igmsuy``` ). These flags can tell the regex engine to account for the entire string to match against with certain parameters.

With no other flags the engine by default will try to match the very first match from reading the string left to right.
* ( ```g``` ) - the global flag which will search to match the expression against the entire string, instead of a single line, or only the first match from reading left to right. 

* ( ```i``` ) - the case-insensitive flag will disregard the difference between uppercase and lowercase letters.
* ( ```m``` ) - the multi-line mode flag will work with the anchors ( ```^``` beginning of a line ) and ( ```$``` end of a line ). This tells the regex engine to search for matches across multiple line breaks in the string.
* ( ```s``` ) - the 'dotall' mode enables the dot ( ```.``` ) to match a newline character ( ```\n``` ).
* ( ```u``` ) - This enables full unicode support which opens the engine up to characters from any language, hex-code characters, and currency symbols.
  - ```js
    /*
    The unicode flag must be used in conjunction with special regex properties

    The /\p{L}/gu refers to any letter from any language upper or lowercase. 
    
    which can change to {Ll} for lowercase only and {Lu} for uppercase only. There are many other options
    */

    const unicodeRegex = /\p{Lu}/gu;
    const cyrillicString = 'Приветствую вас! Меня зовут Андерс. Я люблю программирование!';
    const match = cyrillicString.match(unicodeRegex);
    console.log(match);
    //matches all the uppercase cyrillic characters in this sentence
    [ 'П', 'М', 'А', 'Я' ]

    //p.s. the sentence reads 'I say greetings to you all! My name is Anders. I love programming!' 
    ```
  
* ( ```y``` ) - Allows the search for a match to begin at a particular position in a string.
  - ```js
    const positionRegex =  /\w+/y;
    const words = 'Check out the position of this word';

    //the y flag makes the exec method look at the exact position of the last lastIndex property that we will set the value to the regex expression itself

    //adding the + quantifier to get one or more connected word characters right at this index. otherwise will get the letter 'w'

    positionRegex.lastIndex = 31;
    const execute = positionRegex.exec(words);
    console.log(execute);
    //result
    [
      'word',
      index: 31,
      input: 'Check out the position of this word',
      groups: undefined
    ]
    ```
---
### Grouping and Capturing
Capture groups can be denoted by whats inside the parentheses ```/()/```. This can get part of a match as a separate piece in a result array. If there is some quantifier after the parentheses this matcha applies to the whole capture group.
```js
const groupRegex = /(go)/g;
const words = 'Here we gooooooooooooooo'
const match = words.match(groupRegex);
console.log(match);
//this will only capture the group that consists of just 'go'
[ 'go' ]

//if we add the + quantifier we can capture one or more of the capture groups if they appear together
const groupRegex = /(go)+/g;
const words = 'Here we gogogogogogo'
const match = words.match(groupRegex);
console.log(match);
//outputs
[ 'gogogogogogogo' ]

//without this quantifier will match each one and separate them out
const groupRegex = /(go)/g;
const words = 'Here we gogogogogogo'
const match = words.match(groupRegex);
console.log(match);
//outputs
[ 'go', 'go', 'go', 'go', 'go', 'go' ]
```
---
### Bracket Expressions
This can also categorize into the same field as [Character Classes](#character-classes) when using  ```[]``` . 

However when using ```{}``` this is used to specify an exact amount of things to match against always used after an existing expression
```js
//when also capturing groups you can capture a specific amount of the capture group 
const braceRegex = /(na){3}/g;
const words = 'bananana';
const match = words.match(braceRegex);
console.log(match);
//output
[ 'nanana' ]
```
---
### Greedy and Lazy Match
Greedy and Lazy refer to the different quantifiers and how they are used to match strings according to the parameters set by the quantifier symbols
* Greedy - For every position in a string, the expression tries to match the pattern at that position, if there is no match move on to the next position
```js
/**
 * this expression tries to match a double-quote ( " ), any character after the double-quote with the ( . ) and ( + ) one or more things after any character after the double-quote and stop matching after the last double-quote in the expression
 * 
 * */

const greedyRegex = /".+"/;
const words = 'Harry Potter "and" the goblet of "fire" was awesome!';
const match = words.match(greedyRegex);
console.log(match);

//resulting output even without the global flag, it continues to match until the engine finds the next double-quote
[
  '"and" the goblet of "fire"',
  index: 13,
  input: 'Harry Potter "and" the goblet of "fire" was awesome!',
  groups: undefined
]
```
* Lazy - denoted by the ( ```?``` ) symbol placed after a quantifier such as ( ```*?``` ), ( ```+?``` ), or ( ```??``` ). This produces the opposite effect as greedy, in that if there is no match in the next position the engine stops and returns what it matched before there was no match.
```js
/**
 * This expression tries to match one or more of any character ( .+ ) connected to the first double-quote ( " ) in the string stops matching after it sees the next double-quote connected to any character matched with the first double-quote
 * */

const lazyRegex = /".+?"/g;
const words = 'Harry Potter "and     " the goblet of "fire"';
const match = words.match(lazyRegex);
console.log(match);
//resulting output with the global flag
[ '"and     "', '"fire"' ]

const lazyRegex = /".+?"/;
const words = 'Harry Potter "and     " the goblet of "fire"';
const match = words.match(lazyRegex);
console.log(match);
//resulting output without the global flag
[
  '"and     "',
  index: 13,
  input: 'Harry Potter "and     " the goblet of "fire"',
  groups: undefined
]
```
---
### Boundaries
The boundary metacharacter ( ```\b``` ) refers to matching a position in the string which is followed by a word character but not any word characters after that word character that is adjacent to a boundary. 

Or a position that has preceded a word character but is not followed by a word character.

The ( ```\B``` ) metacharacter does the opposite effect as the ( ```\b``` ) metacharacter.
```js
/**
 * I think the best way to explain the boundary is with this following example. The expression will match with the beginning letters of each word because they are word characters preceded by a boundary ( the literal beginning of the string with no space between the " and T )
 * 
 * Then it continues on to match the space ( ' ' ) character because it was preceded by a word character and then a boundary,
 * 
 * Then it continues on to match the ( w ) character since it was preceded by a non-word character and then a boundary, and so on...
 * 
 * */
const boundariesRegex = /\b.+?/g;
const words = "The weather is beautiful outside!";
const match = words.match(boundariesRegex);
console.log(match);
//outputs
[
  'T', ' ', 'w', ' ',
  'i', ' ', 'b', ' ',
  'o', '!'
]

```
---
### Back-references
A captured group can be 'back-referenced' denoted by the metacharacter ( ```\N``` ) where N is replaced with a number referring to the first ' ```\1``` ' or second ' ```\2``` ' established capture group from left to right inside the expression itself.
```js

 // This expression will back reference the first established 
 // capture group denoted by the \1 after the group list. 
 // And try to match anything that appears before or after the 
 // first capture groups inclusion brackets. 
 
 // I honestly don't quite understand this just yet, as it seems to be a defensive approach 
 // to finding things within a set of characters while 
 // not trying to confuse the engine to match things 
 // that we dont want or not match things that we do want.

const backRefRegex = /(['"])(.*?)\1/g;
const words = `They say: "If ya don't know, Now you know!".`;
const match = words.match(backRefRegex);
console.log(match);
//outputs
[ `"If ya don't know, Now you know!"` ]

//because if we type the literal expression again 
//instead of backreferencing, it will not match the result that we want.
const backRefRegex = /(['"])(.*?)(['"])/g;
//outputs
[ `"If ya don'` ]
```
---
### Look-ahead and Look-behind
Look-ahead and Look-behind can be used to match parts of a string only if the specified character's or expressions are contained in the look-ahead/behind capture group.

Look-ahead - written in the format as  ```X(?=Y)``` where ```X``` can be an expression or set of characters and the ``` (?=Y) ``` means to only match ```X``` if it is preceded by ```Y``` which can be another set of characters or expression.

Look-behind - written in the format as ```(?=Y)X``` which will match with ```X``` only if the ```Y``` expression is connected before ```X```.
```js
// here we are only looking ahead to match any digit in this string
// if it is followed by the Ruble symbol ( ₽ ) but not the digit
// at the beginning of the sentence since it is not followed by
// the ruble symbol.
const regex = /\d+(?=₽)/g;
const words = "1 taxi costs 30₽";
const match = words.match(regex);
console.log(match);
//outputs
[ '30' ]

//here we are looking behind to match the 30 in this string
// only if it is preceded by the Ruble symbol ( ₽ )
const regex = /(?<=₽)\d+/g;
const words = "1 turkey costs ₽30";
const match = words.match(regex);
console.log(match);
//outputs
[ '30' ]
```
---
## Author

### About Me
A little bit about me, my name is Anders and I love programming and the journey that goes along with it! My first love is music and will always be a part of my life in some way shape or form. I also do some sidework as an Audio Visual Technician and Stagehand for Corporate Events and Concerts. 

### Live Streaming
I occassionally live stream on my [twitch channel](https://twitch.tv/DJVikingsintheroad) doing live DJ sets and showcasing some visual art projects that I developed myself in JavaScript. My computer isn't quite as powerful yet to render such projects over a live OBS stream but one day soon this will be another source of fun for me!

### GitHub
If you want to check out my projects posted here on github you can find it all [here](https://github.com/Dj-Viking)

### Thank you!
Thanks for joining me on this quest for knowledge in the Regex world. If there are any corrections to be made on this that I didnt catch or if there are some better examples out there I would be more than happy to add them to this gist tutorial!

Thanks again and happy coding!