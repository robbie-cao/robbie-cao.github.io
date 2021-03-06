---
layout: post
title:  "Textile Syntax"
date:   2016-05-10 13:08:53 +0800
categories: markup
---

## Samples

h2. Heading

```
h1. level 1 heading
h2=. level 2 heading
h3>. level 3 heading
h4<{color:red}. level 4 heading
```

h2. Quote

```
bq. this is blockquoted text
fn1. footnote 1
fn2. footnote 2
This text refernces a footnote[1]
```

h2. List

```
# numbered list item 1
# numbered list item 2

* bulleted list first item
* bulleted list second item
```

h2. Phrase Modifiers

```
_emphasis_
*strong*
??citation??
-deleted text-
+inserted text+
^superscript^
~subscript~
%span%
```

h2. Paragraph

```
p(class). paragraph with a classname
p(#id). paragraph with an ID
p{color:red}. paragrah with a CSS style
p[fr]. paragraphe en français
p<. right aligned paragraph
p>. left aligned paragraph
p=. centered aligned paragraph
p<>. justified text paragraph
```

h2. Table

```
| _. head | _. table | _. row |
| a       | table    | row 1  |
| a       | table    | row 2  |
```

h2. Blocked Code

```
bc. 10 PRINT "I ROCK AT BASIC!"
20 GOTO 10
```


```
p. A line with @printf@
```


h2. Link

```
"Link to Textile on GitHub":https://github.com/textile
!https://avatars2.githubusercontent.com/u/362604?v=3&s=200!
```

h2. Abbreviation

```
ABBR(Abbreviation)
```


## Result

<script src="https://gist.github.com/robbie-cao/6d6cfa40db9075489f7a3e98d3ecdd63.js"></script>

