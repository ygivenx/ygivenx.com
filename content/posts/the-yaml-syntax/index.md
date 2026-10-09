---
title: "The YAML syntax - get over the config files"
date: 2022-09-13
categories: 
  - "developer"
tags: 
  - "data-science"
  - "developer"
  - "productivity"
---

A quick reference to basic YAML syntax

## Lists

All members of a list are lines beginning at the same indentation level starting with a "- " (a dash and a space)

```
# a list of models
- logreg
- randomforest
- xgboost
- mlp
```

## Dictionary

key: value (colon followed by a space)

```
rohan:
  name: Rohan Singh
  job: Data Scientist III
  skill: Elite
```

## Complicated Data Structures

```
# a list of dictionaries
- rohan:
      name: Rohan Singh
      job: Geneticist
      skill: Beginner

- emmanuelle:
      name: Emmanuelle Charpentier
      job: Professor and Researcher
      skill: Elite
```

## Flow Connections

It can be used to represent abbreviated data structures

```
rohan: {name: Rohan Singh, job: Data Scientist III, skill: Elite}
```

## Boolean

Use lower case `true/false` for compatible yamllint

for using literal "yes/no/true/false": use quotes

## Multiline Values

You can multiline values in **two** ways

### Literal Block Scalar `|`

It will include the newlines and any trailing spaces

```
as_is: |
    these lines will
    be shown as is in
    the value
```

### Folded Block Scalar `>`

It will fold newlines to spaces - use this for readability of longer lines to be used as values

```
one_line: >
    the value of
    this one_line will not
    have newlines
```

**Alternatively**, you can use newline `\n` character

```
convert_some_newlines_using_fold: >
    def hi():
        print('hello')

convert_some_newlines: "def hi():\n    print('hello')"
```

## Gotchas

`:` It is an indicator for mapping  
`#` Starts a comment

```
# this will fail
wrong: who is: sherlock

# this will work
the_c_drive: c:\c_drive
```

You can get around the gotchas by using `'` or `"` quotes

**But , hold on** - the quotes are not the same!

`In double quotes` you can `escapes`

```
escaped: "from an island called ..... \t Alcatraz \n near \t San Francisco"
```
