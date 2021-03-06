# String Sequence Literals

| Field           | Value                                                           |
|-----------------|-----------------------------------------------------------------|
| DIP:            | (number/id -- assigned by DIP Manager)                          |
| Review Count:   | 0 (edited by DIP Manager)                                       |
| Author:         | Jason Hansen, Jonathan Marler                                   |
| Implementation: | https://git.io/fpSUA                                            |
| Status:         | Will be set by the DIP manager (e.g. "Approved" or "Rejected")  |

## Abstract

This DIP proposes adding a "string sequence literal"(explain more what this means) to D, which opens up tons up possibilities, including string interpolation.

### Reference

- Exploration: https://github.com/marler8997/interpolated_strings
- Example Library Solution: https://github.com/dlang/phobos/pull/6339/files
- Implementation: https://github.com/dlang/dmd/pull/7988
- https://forum.dlang.org/thread/khcmbtzhoouszkheqaob@forum.dlang.org
- https://forum.dlang.org/thread/c2q7dt$67t$1@digitaldaemon.com
- https://forum.dlang.org/thread/qpuxtedsiowayrhgyell@forum.dlang.org
- https://forum.dlang.org/thread/ncwpezwlgeajdrigegee@forum.dlang.org
- https://dlang.typeform.com/report/H1GTak/PY9NhHkcBFG0t6ig (#3 in "What language features do you miss?")

## Contents
* [Rationale](#rationale)
* [Description](#description)
* [Breaking Changes and Deprecations](#breaking-changes-and-deprecations)
* [Copyright & License](#copyright--license)
* [Reviews](#reviews)

## Rationale

Sequence literals apply to a wide range of use cases. A few of these use cases are outlined below.

#### String Interpolation
One notable use for sequence literals is in string interpolation, which allows for more concise, readable, and maintanable code. For example:


src/build.d:556:<br>
`auto hostDMDURL = "http://downloads.dlang.org/releases/2.x/"~hostDMDVer~"/dmd."~hostDMDBase;`<br>
Becomes:<br>
`auto hostDMDURL = i"http://downloads.dlang.org/releases/2.x/$hostDMDVer/dmd.$hostDMDBase".text;`<br>
And, with syntax highlighing:<br>
![https://i.imgur.com/tXm6rBU.png](https://i.imgur.com/tXm6rBU.png)


src/dmd/json.d:1058:<br>
``s ~= prefix ~ "`" ~ enumName ~ "`";``<br>
Becomes:<br>
``s ~= i"prefix`$enumName`".text;``<br>
With syntax highlighting:<br>
![https://i.imgur.com/KTcOS0F.png](https://i.imgur.com/KTcOS0F.png)


#### Database Queries
TODO: ...


(also, add other use cases)

## Description

Lexer Change:

Current:

```
Token:
   ...
   StringLiteral
   ...
```
New:

```
Token:
   ...
   StringLiteral
   i StringLiteral
   ...
```

No change to grammar. Implementation consists of a small change to `lex.d` to detect when string literals are prefixed with the `i` character.  It adds a boolean flag to string literals to keep track of which ones are "interpolated".  Then in the parse stage, if a string literal is marked as "interpolated" then it lowers it to a tuple of strings and expressions.

Implementation and tests can be found here: https://github.com/dlang/dmd/pull/7988/files


## Breaking Changes and Deprecations
None. :smile:

## Copyright & License

Copyright (c) 2018 by the D Language Foundation

Licensed under [Creative Commons Zero 1.0](https://creativecommons.org/publicdomain/zero/1.0/legalcode.txt)

## Reviews

The DIP Manager will supplement this section with a summary of each review stage
of the DIP process beyond the Draft Review.
