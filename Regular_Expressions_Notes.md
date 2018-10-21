#Regular Expressions Notes
Hi! My name is Ryan Hussey and these are my personal notes for freeCodeCamp's Regex lessons.  Full disclosure - some of the example code comes directly from freeCodeCamp - I'm not trying to plagiarize, these are just my notes for keeping track of what I've learned. <br>Feel free to use them.

###Regular expressions, 
also referred to as regex or regexp, are used to match
parts of strings.  It is useful to test your regex.
One method to test is .test()
In this example, we test a regex that should match a word in a string
(we will try to find the word "Sand" using /Sand/).  If the regex is working,<span style=color:red> .test()</span> will return "true."
```JavaScript
let leopold = "A Sand County Almanac";
let testRegex = /Sand/;
console.log(testRegex.test(leopold)); //logs true
```
Notice that regex /word/ will only find the literal string that is being searched, e.g. you can't find "Word" or "worD" with the regexp /word/
To pull the string you have matched out (as opposed to stating true|false) use <span style=color:red>.match()</span>
```JavaScript
let aldo = "A Sand County Almanac";
let matchRegex = /Sand/;
console.log(aldo.match(matchRegex)); //logs Sand
```
To return all the matches instead of just one, use the g flag
```JavaScript
/word/g
```
To use multiple flags, just put them together:
```JavaScript
let multFlags = /IgnoreCaSe&MultipleMatches/gi
```
If you want to match a piece of a word, but not the whole word, use the wildcard operator:
```JavaScript
.
```
```JavaScript
let matchAllOfThese = "can man fan pan flan tan dan ban";
let oneRegexToRuleThemAll = /.an/g;
console.log(matchAllOfThese.match(oneRegexToRuleThemAll));
```
Character classes allows for flexibility of characters matched while honing in on the exact character possibilities you wish to match.
```JavaScript
let fin = "fin";
let fan = "fan";
let fun = "fun";
let fon = "fon";
let finFanFun = /f[iau]n/;
console.log(fin.match(finFanFun)); // logs fin
console.log(fun.match(finFanFun)); // logs fun
console.log(fan.match(finFanFun)); // logs fan
console.log(fon.match(finFanFun)); // logs null
```
To match a set of characters between certain letters of the alphabet
or a range of numbers use a hyphen:
```JavaScript
let str = "bat cat mat 3 2 0" ;
let batCat = /[a-d]at|[1-4]/g;
console.log(str.match(batCat)); // logs bat cat 3 2 , does not return mat or 0
```
A negated character set will match everything that is not what you have specified with a caret (^).
```JavaScript
let string = "abcdefg";
let noVowels = /[^aeiou]/g;
console.log(string.match(noVowels));//logs bcdfg
```
If you use the caret outside of a [], it will match if the specified
string is at the beginning of the variable.
```JavaScript
let str1 = "I can be found";
let success = /^I/;
console.log(success.test(str1));//return true
let str2 = "You can't caret me";
let fail = /^me/;
console.log(fail.test(str2)); //return false bc "me" is not at the beginning
```
Conversely, you can find the last word of the string with $.
```JavaScript
let str3 = "I cannot be found";
let fail1 = /I$/;
console.log(fail1.test(str3));//return false
let str4 = "You can $ me";
let success1 = /me$/;
console.log(success1.test(str4)); //return true
```
To find instances of a character being repeated consecutively, use +
```JavaScript
let a = "abc";
let aa = "aabc";
let a_a = "ababc"
let repeatFinder = /a+/gi
console.log(a.match(repeatFinder)); //logs ["a"]
console.log(aa.match(repeatFinder)); // logs["aa"]
console.log(a_a.match(repeatFinder)); // logs["a","a"]
```
To match instances used zero or more times, use *
```JavaScript
let la = "la";
let l = "l";
let laaaaa = "laaaaa";
let nada = "nada";
let regex = /la*/;
console.log(la.match(regex)); //logs la
console.log(l.match(regex));//logs l
console.log(laaaaa.match(regex));//logs laaaaa
console.log(nada.match(regex));//logs null
```
Regexes are by greedy by default, meaning they'll find the longest possibile string that satisfies the regex.  If you want the first piece of a string that will satisfy the regex, you need to make the regex lazy by using a ?
```JavaScript
let tattoo = "tattoo"
let greedy = /t[a-z]*o/
let lazy = /t[a-z]*?o/
console.log(tattoo.match(greedy));//logs tattoo
console.log(tattoo.match(lazy));// logs tatto
```
To match something (in this case we'll say any letter) that is used
x to y number of times, use
```JavaScript
/a{x,y}/
```
```JavaScript
let one = "a";
let two = "ab ";
let five = "abcde";
let ten = "abcdefghij";
let twoOrMore = /[a-z]{2,5}/;

console.log(one.match(twoOrMore)); //logs null
console.log(two.match(twoOrMore)); //logs ab
console.log(five.match(twoOrMore));//logs abcde
console.log(ten.match(twoOrMore));//logs abcde b/c it logs the first five
```
If we want to make sure the case above, wherein variable ten returned true,
we could follow the specified number w/ whitespace
```JavaScript
let twoToFive = /[a-z]{2,5}\s/;
console.log(ten.match(twoToFive)); //logs null
```
To match an exact number of repetitions, exclude the comma
```JavaScript
let exactlyTwo = /[a-z]{2}\s/;
console.log(two.match(exactlyTwo)); //logs ab
console.log(five.match(exactlyTwo)); //logs null
```
A lookahead will tell you if an element exists (for positive lookahead) or doesn't exist (for negative lookahead) w/o actually matching it.
```JavaScript
let password = "password1";
let badPword = "password";
let positivePwordCheck = /(?=\w{3,})(?=\D*\d+)/g;
```
The regex above checks for three or more alphanumeric characters and at least one number. Don't actually use this for a password checker - it's not exactly a watertight password formula ;)

Notice that the lookahead is enclosed in ().  A negative lookahead uses (?!). e.g.,
```JavaScript
console.log(positivePwordCheck.test(password))//logs true
console.log(positivePwordCheck.test(badPword))//logs false
```
Capture groups allow you to search for repeating sequences or even change sequences within a pattern.
```JavaScript
let repeatedNumbers = "1 1 1";
let captureNumbers = /^(\d+)\s\1\s\1$/;
console.log(repeatedNumbers.match(captureNumbers));//logs 1 1 1
```
The () captures the number.  Every capture is a new group, so this capture is referred to later as group 1 with the syntax \1.
To replace the a group, instead of matching it (as above), use the syntax $groupNumber.
```JavaScript
let oneTwoThree = "1 2 3"
let switchNumbers = /^(\d+)\s(\d+)\s(\d+)$/;

console.log(oneTwoThree.replace(switchNumbers, '$3 $2 $1')); //logs 3 2 1
console.log(oneTwoThree.replace(switchNumbers, 'One Two Three'))//logs
//One Two Three
```
RegExp is a constructor fxn in your browser. e.g.
```JavaScript`
let vowels = new RegExp ("[aeiou]", "gi"); // creates regex ```JavaScript
[aeiou]/gi
```
This is especially useful if you want to add a variable into a regex:
```JavaScript
let variableInRegex = new RegExp ("^" + variable +, "i")
```
Regex Conditional Statement
Sometimes it is necessary to make match characters only under certain conditions. In this case, use a conditional statement, which follows the following formats:
If X is true, match Y:
```javascript
(?(X)Y)
```
If X is NOT true, match Y:
```javascript
(?(X)|Y)
```
If X is true, match Y. If X is NOT true, match Z:
```javascript
(?(X)Y|Z)
```


### Regex Glossary
 Literal Match:
```JavaScript
let literalMatch = /exampleWord/```
 OR operator: | e.g.
```JavaScript
let or = /yes|no/```
Ignore Case:
```JavaScript
let ignoreCase = /IgnoreCaSe/i```
Extract matches:
```JavaScript
let extractMatches = aldo.match(matchRegex)
```
Multiple Matches:
```JavaScript
let multMatches = /MultipleMatches/g
```
Wildcard Operator:
```JavaScript
let wildcard = /.ildcard/
```
Character classes:
``` JavaScript
let characterClass = /[aeiou]/
```
Character sets:
```JavaScript
let characterSets = /[a-z1-128]/
```
Negated character set:
```JavaScript
let negatedSets = /[^a-z0-9]/
```
Match beginning string patterns
```JavaScript
let firstWordOfString = /iffirstWordOfString/
```
Match end of string patterns
```JavaScript
let lastWordOfString = /$lastWordOfString/
```
Match one or more occurrences
```JavaScript
let matchOneOrMore = /a+/
```
Match zero or more occurrences
```JavaScript
let matchZeroOrMore = /la*/
```
Must be at least x number of characters:
```JavaScript
let atLeastX = /{x,}/
```
"a" must be repeated between x and y number of times:
```JavaScript
let betweenXandY = /a{x,y}/
```
Match whitespace
```JavaScript
let whitespace = /\s/
```
Match return carriage
```JavaScript
let returnCarriage = /\r/
```
Match tab
```JavaScript
let tab = /\t/
```
Match new line
```JavaScript
let newLine = /\n/
```
Match vertical tab character
```JavaScript
let vertTab = /\v/
```
For the escape characters listed above, to find their opposite, use uppercase
e.g. Match non-whitespace
```JavaScript
let nonWhitespace = /\S/
```
Match for 0 or 1 of x
```JavaScript
let optional = /x?/
```
Positive lookahead
```JavaScript
let posLookahead = /(?=)/
```
Negative lookahead
```JavaScript
let negLookahead = /(?!)/
```
Capture groups
```JavaScript
let captured = /(imCaptured)/
```
Match captured group
```JavaScript
let groupUno = /(imGroup1)\1/
```
Refer to the captured group you want to move or replace
```JavaScript
let replaceGroup = /$1/
```
Conditional Statement
If X is true, match Y:
```javascript
(?(X)Y)
```
If X is NOT true, match Y:
```javascript
(?(X)|Y)
```
If X is true, match Y. If X is NOT true, match Z:
```javascript
(?(X)Y|Z)
```

### Shorthand glossary:

#### All numbers and letters:
Shorthand
```JavaScript
let numbersAndLetters = /\w/
```
Longhand
```JavaScript
let longNumbAndLet = /[A-Za-z0-9_]/
```
#### To negate Shorthand, capitalize:
Shorthand
```JavaScript
let nonLetAndNum = /\W/
```
Longhand
```JavaScript
let longNonLetAndLet = /[^A-Za-z0-9_]/
```
#### Digits
Shorthand
```JavaScript
let numbers = /\d/
```
Longhand
```JavaScript
let longNumbers = /[0-9]/
```
#### Not digits
Shorthand
```javascript
let nonNumbers = /\D/
```
Longhand
```javascript
let longNonNumbers = /[^0-9]/
```
