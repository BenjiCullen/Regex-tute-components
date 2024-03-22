# Title (replace with your title)

Introductory paragraph (replace this with your text)

## Summary

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

### Anchors
Anchors in a regex are characters that don’t match any character at all; instead, they match a position before, after or between characters. Anchors are useful for directing the regex engine’s position in the string so that the engine’s current position complies with the anchor points: most commonly, this allows us to match whole strings against patterns, or patterns at specific places in strings. The two main anchors are:



^ - A caret at the beginning of a pattern matches at the beginning of the input string. (If used in multiline mode – eg, with the m flag in some regex flavours – it matches the start of any line.)

$ – The currency symbol matches the position at the end of the input string. In multi‑line mode, it matches the end of any line.

let text = `Start of the line
middle of the line
end of a line`;

// Match lines that start with "Start"
let startRegex = /^Start/m;
console.log(text.match(startRegex)); // Prints lines starting with "Start"

// Match lines that end with "end"
let endRegex = /end$/m;
console.log(text.match(endRegex)); // Prints lines ending with "end"


### Quantifiers
Quantifiers in regex restrict matches to a specific number of instances of a single literal character, a subexpression, or a class of characters in the target, we want it to be flexible, yet there’s the issue of how many iterations are ‘enough’ to recognise the pattern. If there are too many repeating elements, the pattern of the whole loses its significance. One of the most common quantifiers is: But there are also ‘non-greedy’ quantifiers to equally/exactly complete a target sequence (‘reluctant quantifiers’ with the star/asterisk symbol *): Or, to take the example from before, you could ‘exhaust’ your search space with the non-greedy pattern:
* - Matches 0 or more occurrences of the preceding element. It makes the element optional and allows it to appear multiple times.
+ - Matches 1 or more occurrences of the preceding element. The element must appear at least once for a match to occur.
? - Matches 0 or 1 occurrence of the preceding element, making it optional.
{n} - Matches exactly n occurrences of the preceding element.
{n,} - Matches n or more occurrences of the preceding element.
{n,m} - Matches from n to m occurrences of the preceding element, inclusive.

For example let text = "There are 1234 apples, 567 oranges, and 89 bananas.";

// Match sequences of one or more digits
let digitRegex = /\d+/g;
console.log(text.match(digitRegex));
// Output: ["1234", "567", "89"]

// Match words that appear 0 or more times
let optionalWordsRegex = /apples?/g;
console.log(text.match(optionalWordsRegex));
// Output: ["apples"]

// Match a word followed by exactly 3 digits
let wordWithDigits = /\b\w{5}\d{3}\b/g;
console.log(text.match(wordWithDigits));
// No output since there's no word of 5 letters followed by exactly 3 digits

// Match a sequence of 2 to 4 digits
let rangeDigits = /\d{2,4}/g;
console.log(text.match(rangeDigits));
// Output: ["1234", "567", "89"]


### Grouping Constructs

Grouping constructs in regular expressions, are used to combine a sequence of tokens into a single unit for various purposes, such as applying quantifiers to multiple characters as a group, capturing substrings for later use, or creating alternatives within a pattern. The most common types of grouping constructs are:

Capturing Groups - Enclosed in parentheses (...), capturing groups match the part of the string that fits the pattern inside the parentheses and remember the matched substring, which can then be recalled later in the pattern or in the resulting array from methods like .match() or used with .replace().

Non-Capturing Groups - Written as (?:...), non-capturing groups group tokens together without remembering the matched substring. They're useful for applying quantifiers to a part of a pattern without cluttering the array of captured strings.

Named Capturing Groups - Using (?<name>...), named capturing groups function like regular capturing groups but allow you to assign a name to the group. This makes it easier to retrieve the matched value by name, improving pattern readability and maintenance.

Positive Lookahead - Specified with (?=...), this asserts that the given subpattern can be matched at the current position, without consuming characters. It looks ahead to see if a pattern matches further in the string.

Negative Lookahead - Specified with (?!...), this asserts that the given subpattern cannot be matched at the current position. It looks ahead to ensure a pattern does not match further in the string.

For example: 
let text = "John Smith and Jane Doe are contacts.";

// Capturing groups
let nameRegex = /(\w+) (\w+)/g;
let matches = [...text.matchAll(nameRegex)];
console.log(matches.map(match => `${match[1]} ${match[2]}`));
// Output: ["John Smith", "Jane Doe"]

// Non-capturing groups
let nonCaptureRegex = /(?:\w+) (?:\w+)/g;
console.log(text.match(nonCaptureRegex));
// Output: ["John Smith", "Jane Doe"]

// Named capturing groups
let namedCaptureRegex = /(?<firstName>\w+) (?<lastName>\w+)/g;
let namedMatches = [...text.matchAll(namedCaptureRegex)];
console.log(namedMatches.map(match => `${match.groups.firstName} ${match.groups.lastName}`));
// Output: ["John Smith", "Jane Doe"]

// Positive lookahead
let lookaheadRegex = /\w+(?= Smith)/g;
console.log(text.match(lookaheadRegex));
// Output: ["John"]

// Negative lookahead
let negativeLookaheadRegex = /\w+(?! Smith)/g;
console.log(text.match(negativeLookaheadRegex));
// Output: ["John", "and", "Jane", "Doe", "are", "contacts"]


### Bracket Expressions

Bracket expressions in regular expressions, are used to define a set of characters from which a match can be made. They allow for the specification of a character class, where any one character in the class will satisfy the match. Bracket expressions are highly versatile and can include individual characters, ranges of characters, and predefined character classes. Here are some key points about bracket expressions:

Individual Characters: Placing characters inside brackets [] matches any one of those characters. For example, [abc] matches either a, b, or c.
Character Ranges: You can specify a range of characters using a hyphen -. For instance, [a-z] matches any lowercase letter from a to z, and [0-9] matches any digit.

Negated Character Classes: Placing a caret ^ at the start of the expression (immediately after the opening bracket) negates the class, matching any character that is not specified. For example, [^abc] matches any character except a, b, or c.

Predefined Character Classes: Regex supports predefined character classes within bracket expressions like \d (any digit), \w (any word character), and \s (any whitespace character), among others. However, their behavior might slightly differ when used inside vs. outside of bracket expressions.

For example: 
let text = "The quick brown fox jumps over 13 lazy dogs.";

// Matches any vowel
let vowelsRegex = /[aeiou]/gi;
console.log(text.match(vowelsRegex));
// Output: Array of all vowels in the text, case-insensitive

// Matches any character that is not a vowel
let nonVowelsRegex = /[^aeiou\s\d\W]/gi;
console.log(text.match(nonVowelsRegex));
// Output: Array of all non-vowels excluding spaces, digits, and non-word characters

// Matches any digit
let digitRegex = /[0-9]/g;
console.log(text.match(digitRegex));
// Output: ["1", "3"]

// Matches letters in the range of 'a' to 'f' and digits
let lettersDigitsRegex = /[a-f0-9]/gi;
console.log(text.match(lettersDigitsRegex));
// Output: Array of all occurrences of letters a-f (case-insensitive) and digits

### Character Classes

Character classes in regular expressions, provide a way to match any one of a defined set of characters. They are a fundamental part of regex syntax, allowing for more flexible and concise patterns. Here’s a brief overview of common character classes:

Dot .: Matches any single character except newline characters. For example, a.c matches any three-character string that begins with "a" and ends with "c", with any character in between.

\d: Matches any digit character (0-9). Equivalent to [0-9].

\D: Matches any non-digit character. Equivalent to [^0-9].

\w: Matches any word character (alphanumeric characters plus underscore). Equivalent to [A-Za-z0-9_].

\W: Matches any non-word character. Equivalent to [^A-Za-z0-9_].

\s: Matches any whitespace character (spaces, tabs, line breaks).

\S: Matches any non-whitespace character.

These character classes can greatly simplify your regex patterns, making them easier to read and write.

For example: 
let text = "My phone number is 123-456-7890.";

// Matches any digit sequences
let digitsRegex = /\d+/g;
console.log(text.match(digitsRegex));
// Output: ["123", "456", "7890"]

// Matches any word
let wordRegex = /\w+/g;
console.log(text.match(wordRegex));
// Output: ["My", "phone", "number", "is", "123", "456", "7890"]

// Matches whitespace characters
let whitespaceRegex = /\s+/g;
console.log(text.split(whitespaceRegex));
// Output: ["My", "phone", "number", "is", "123-456-7890."]

// Matches non-word characters
let nonWordRegex = /\W+/g;
console.log(text.match(nonWordRegex));
// Output: [" ", " ", " ", " ", "-", "-", "."]


### The OR Operator

The OR operator in regular expressions, is represented by the vertical bar | and is used to specify alternatives within a pattern. This means that the regex engine will match either the pattern on the left side of the | or the pattern on the right side. It effectively functions as a logical OR, allowing for the matching of one of multiple possible sequences within the same regex. The OR operator can be used within a single regex pattern to match different possible strings, making it highly versatile for patterns where multiple variations are common.

An example of this would be:

Single Characters or Sequences: The OR operator can be used to match single characters or entire sequences of characters. For example, cat|dog matches "cat" or "dog".
Within Grouping Constructs: When used within parentheses (…), the OR operator allows for part of a regex to match different possible sub-patterns. For example, (cat|dog) house matches "cat house" or "dog house".

A coding example: 
let text = "I have a cat, a dog, and a parrot.";

// Matches "cat", "dog", or "parrot"
let animalRegex = /cat|dog|parrot/g;
console.log(text.match(animalRegex));
// Output: ["cat", "dog", "parrot"]

// Matches "cat house" or "dog house"
let houseRegex = /(cat|dog) house/g;
let newText = "I'm looking for a cat house or a dog house.";
console.log(newText.match(houseRegex));
// Output: null, but would match "cat house" or "dog house" in a suitable text

// Combine with other elements
let petCareRegex = /(cat|dog|parrot) (food|toys)/g;
let petText = "In the store, you can find cat food, dog toys, and parrot food.";
console.log(petText.match(petCareRegex));
// Output: ["cat food", "dog toys", "parrot food"]


### Flags

Flags in regular expressions (regex) modify how the search is performed, extending the capabilities of regex patterns. Flags are added at the end of the regex pattern and can be combined to achieve various effects. Here are some of the most commonly used flags in JavaScript:

g (Global): Without this flag, the regex engine stops after finding the first match. The g flag allows the pattern to be applied globally to the string, finding all matches instead of stopping at the first one.

i (Case-Insensitive): By default, regex searches are case-sensitive. The i flag makes the search case-insensitive, allowing the pattern to match letters regardless of case.

m (Multiline): This flag changes the behavior of ^ (start of string) and $ (end of string) anchors to match the start and end of a line, respectively, instead of the whole string.

s (DotAll): Normally, the dot . matches any character except newline characters. The s flag makes the dot match all characters, including newlines.
u (Unicode): This flag enables full Unicode matching, allowing for the correct handling of surrogate pairs in Unicode characters.

y (Sticky): The sticky flag makes the search start exactly at the current position in the target string, not moving forward to search the rest of the string after a failed match.

For example: 
let text = `First line
second LINE
Third line`;

// Global and case-insensitive search for "line"
let globalInsensitiveRegex = /line/gi;
console.log(text.match(globalInsensitiveRegex));
// Output: ["line", "LINE", "line"]

// Multiline search for start of line
let multilineRegex = /^second/gim;
console.log(text.match(multilineRegex));
// Output: ["second"]

// DotAll mode: '.' matches newlines
let dotAllRegex = /^.*$/s;
console.log(text.match(dotAllRegex)[0]);
// Output: entire text, including newlines

// Unicode flag example
let unicodeText = "𝟙𝟚𝟛"; // These are not simple digits, they're mathematical bold digits
let unicodeRegex = /\d+/u; // Without the 'u' flag, this wouldn't match
console.log(unicodeText.match(unicodeRegex));
// Output: ["𝟙𝟚𝟛"]

// Sticky search
let stickyText = "There is a cat";
let stickyRegex = /is/y; // Starts searching from index 0
stickyRegex.lastIndex = 5; // Set search start position to index 5
console.log(stickyText.match(stickyRegex));
// Output: ["is"]


### Character Escapes

Character escapes in regular expressions (regex) are used to specify characters that would otherwise be interpreted in a special way by the regex engine. These are typically characters that have a specific function in regex syntax, such as ., *, ?, +, (, ), {, }, [, ], |, ^, and $. To use these characters as literal characters in a regex pattern (meaning you want to search for these symbols themselves, not use their special function), you precede them with a backslash \, turning them into character escapes.

Here's a brief overview of character escapes in regex:

Backslash \: Used to escape special characters, enabling them to be treated as literal characters.

Using \ with Characters: For example, to match a period character directly, you'd use \. in your pattern, because simply using . would match any character.

Predefined Character Classes: The backslash is also used to denote predefined character classes such as \d (digits), \w (word characters), and \s (whitespace characters).

For example: 
let text = "This is a test. Does it match? Yes, it matches 100%!";

// Match the literal dot character
let dotRegex = /\./g;
console.log(text.match(dotRegex));
// Output: [".", "."]

// Match the literal question mark
let questionMarkRegex = /\?/g;
console.log(text.match(questionMarkRegex));
// Output: ["?"]

// Match the literal plus sign
let plusRegex = /\+/g;
console.log(text.match(plusRegex));
// Output: ["+"]

// Escaping special characters to match a specific pattern
let specialPattern = /\b100\%!/; // Match "100%!" as a whole word
console.log(text.match(specialPattern));
// Output: ["100%!"]


## Author

My name is benjamin Cullen, i am a junior developer at the end of my course and here are examples of Regex Components, with descriptions and examples.
Here is a link to my github: benjicullen@github.com
I appologise for the lack of formatting with the examples of code that i used.
