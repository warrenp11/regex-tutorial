# Regex Tutorial: *Validating a Hexadecimal Color Code* 

The following documentation will attempt to explain the components of regular expressions (regex) and how they are used for many purposes, one of which is to validate a hexadecimal color code.

## Summary

The following regex is used when validating a hexadecimal color code:

        /^#?([0-9a-f]{6}|[0-9a-f]{3})$/i

The values are validated based on the following rule sets:

* Optionally begins with a hash (`#`)

* Must be 3 *or* 6 characters in length

* Uses the letters `a-f` and/or digits from `0-1`

* Case insensitive

* All expressions must be valid to pass


The following are examples of hexadecimal color codes that will pass:

    // Examples
    #ffffff
    #FFF
    #fFfFfF
    #000
    #1f2f3F
    #345678

The following are examples of hexadecimal color codes that will fail:

    // Examples
    #fffff => // 5 characters
    #fF => // 2 characters
    #1f2f3T => // uses invalid character "T"

Here's a brief breakdown of the validation occuring with the regex expression `/^#?([0-9a-f]{6}|[0-9a-f]{3})$/i`:

* ( `/` )
    * marks the opening of the regex

* ( `^` )
    * asserts position at start of the string

* ( `#` )
    * matches the hastag character `#`

* ( `?` )
    * matches the previous token but makes that token optional

* Capuring Group `([0-9a-f]{6}|[0-9a-f]{3})`

    * Option 1 (uses 6-character hexadecimal value):
        * Match a single character from the following regex `[0-9a-f]{6}`:
            * `0-9` matches a single character in the range between 0 and 9
            * `a-f` matches a single character in the range between a and f (*case insensitive*)
            * `{6}` matches the previous token exactly 6 times
    * Option 2 (uses 3-character hexadecimal value):
        *  Match a single character from the following regex `[0-9a-f]{3}`:
            * `0-9` matches a single character in the range between 0 and 9
            * `a-f` matches a single character in the range between a and f (*case insensitive*)
            * `{3}` matches the previous token exactly 3 times

* ( `$` )
    * asserts position at the end of the string

* ( `/` )
    * marks the ending of the regex

* ( `i` )
    * match case insensitive on entire regex

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Greedy and Lazy Match](#greedy-and-lazy-match)

## Regex Components

### Anchors

The regex example `/^#?([0-9a-f]{6}|[0-9a-f]{3})$/i` uses 2 anchors, ( `^` ) and ( `$` ). The ( `^` ) symbol indicates the beginning of a string while the ( `$` ) symbol indicates the end of a string. For reference the string is what we're looking to validate.

Anchors are unique in that they match a position within a string, not a character. Thus, in the example above both the achors indicate the beginning and the end of the string we're looking to match, not a specific character.

### Quantifiers

Quantifiers indicate that the preceding token must be matched a certain number of times. By default, quantifiers are greedy, and will match as many characters as possible.

While there are seeveral different qualifiers, the regex `/^#?([0-9a-f]{6}|[0-9a-f]{3})$/i` only uses 3 basic quantifiers, technically only 2 different quantifier types. 

The ( `?` ) is called an optional because the regex considers the token/character preceding the ( `?` ) to be optional, and will return matches with and without the specified token, basically making that token nonexistant. For the expression, the ( `?` ) character is preceded by the ( `#` ) token, meaning the regex will return matches with and without the ( `#` ) character attatched. 

Below is another good example of an optional quantifier:

        // Example
        colou?r => // accepts "color" or "colour"

The second type of quantifier used in the regex is called a quantifier and is expressed as `{6}` and `{3}`. This quantifier type matches the specified quantity of the previous token. For example, `{1,3}` will match 1 to 3, `{3}` will match exactly 3, and `{3,}` will match 3 or more.

Here's the first part of the regular expression:

        [0-9a-f]{6}

Here we're saying, "validate if the string matches any of the following characters `0-9` or `a-f` and is 6 characters in length exactly (`{6}`)".

The second part of the expression is very similiar to the first:

        [0-9a-f]{3}

This is almost the same as the first quantifier, but here we're saying "validate if the string matches any of the following characters `0-9` or `a-f` and is 3 characters in length exactly (`{3}`).

### OR Operator

The regular expression `/^#?([0-9a-f]{6}|[0-9a-f]{3})$/i` is interesting because it has 2 different outcomes which would both validate the input given. 

Luckily it can test for both cases simultaneously. The ( `|` ) character is called an alternation. It acts like a boolean `OR`, matching one sequence or another. It can operate within a group, or on a whole expression. The patterns will be tested in order.

Hexadecimal color codes can be 6 characters in length (`#ffffff`) or 3 characters in length (`#fff`), without counting the hashtag. Both are valid hexadecimal color codes so it needs to account for both cases. Also worth noting, the following example only accepts inputs of length 6 or 3, so a hexadecimal color code of `#ffff` would fail to validate using this regex.

In reference to the regex the alternation character tells us that the code will look to match hexadecimal codes with a length of 6 first, then try to find hexadecimal codes with a length of 3.

### Character Classes

Character classes match a character from a specific set. There are a number of predefined character classes and you can also define your own sets.

In the regular expression `/^#?([0-9a-f]{6}|[0-9a-f]{3})$/i` which checks for the validity of a hexadecimal color code we define one character class:

        [0-9a-f]

It's actually defined twice to check it for the two possible hexadecimal outcomes. As the character class is defined above it states that we're looking to match any digit `0-9` and any *lowercase* instance of a letter `a-f`.

There's also an `i` token at the end of the regex which ultimately infulences the defined character class to match *any* case of a letter `a-f`.

### Flags

Expression flags change how the expression is interpreted. Flags follow the closing forward slash of the expression (ex. `/.+/igm` ).

As stated in the previous section, the decision to add an `i` token at the end of the regex changes how the expression is interpreted. The ignore case (`i`) makes the whole expression case-insensitive, hence it can decipher both `#fffFFF` and `#fffffF` as the same hexadecimal color value.

Here's another example of `i` in use:
        
        // Example
        /aBc/i => matches: "AbC" == "aBc" === "abc" === "ABC"

### Grouping and Capturing

We can group multiple tokens together and create a capture group for extracting a substring or using a backreference. Here's a quick example:

        // Example
        // The plus (`+`) matches 1 or more of the preceding token, in this case a reference of `ha`
            (ha)+ => // matches "hahaha" "haa" "hah!"

The regex for hexadecimal color codes has 1 capturing group: 

        ([0-9a-f]{6}|[0-9a-f]{3})

And as we saw earlier the alternation token ( `|` ) allows us to have a first and second option for matching hexadecimal codes.

### Greedy and Lazy Match

We have 2 good examples of using greedy and lazy matching.

First, let's take a look at an example of greedy matching. In the regex `{6}` and `{3}` are the quantifiers. Although in this instance we are requiring the match to be lengths of `{6}` or `{3}` *only*, there are other ways to define a quantifier to show an example of greediness.

What if we changed the regex to look like this:

        /^#?([0-9a-f]{3,6})$/i

Well, in this instance it would validate hexadecimal color codes from lengths 3 up to 6, with lengths of 4 and 5 validated. We don't want that but that's greediness. Remember that the regex engine is eager to return a match. It will not continue backtracking further to see if there is another possible match. 

Like the plus ( `+` ), the star ( `*` ) and the repetition using curly braces are greedy.

We also have an example of lazy matching. 

Remember the ( `?` ) token, which basically makes the preceding token nonexistant and acts as an example of lazy matching. In the regex, the part of the expression referencing the hashtag ( `#?` ) is saying, "Match anything with a hashtag, but you know what it's totally fine if there's no hashtag you can match that too".

## Author

If you have any questions about this project contact me directly at warrenp11@gmail.com 
  
Visit this project's gist at https://gist.github.com/warrenp11/cc25ad43eecf1fec5df9802203a5dd8f

View more of my projects at https://www.github.com/warrenp11