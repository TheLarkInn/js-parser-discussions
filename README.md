# js-parser-discussions
Discussions &amp; Collaboration on a set of parser features for JavaScript for interoperability.

## Disclaimer
This is a blameless medium. Focusing on a specific toolings shortcomings or bias or personal agendas, will not help accomplish the following goals. Therefore I urge that anyone involved uses an unbiased inspection of these tools to help identify ways to create an unopinionated, and extensible foundation.

## Synopsis
Currently there are a variety of JavaScript parsers that are available. Each of these parsers have specific purposes. These parsers include (but is not limited to): 

- [Esprima](https://github.com/jquery/esprima)
- [Espree](https://github.com/eslint/espree)
- [Acorn](https://github.com/ternjs/acorn)
- [SweetJS](https://github.com/sweet-js/sweet.js)
- [Babylon](https://github.com/babel/babylon)
- [UglifyJS](https://github.com/mishoo/UglifyJS/) (no dependant parser)
- [UglifyJS2](https://github.com/mishoo/UglifyJS2) (?)
- [Typescript](https://github.com/microsoft/typescript)
- [Flow](https://github.com/facebook/flow) (ml based)
- [Shift](https://github.com/shapesecurity/shift-parser-js)


Each of these parsers have specific feature set. For this synopsis, the reasonings behind those featuresets are irrelevant, however it is noteworthy to mention that often specific parsers are companions to specific frameworks, libraries, toolings, etc. 

### Features? 
- Esprima
- Acorn
- Babylon
- Uglify

## Current Challenges
Currently, frameworks, libraries, tools, optimizers, bundlers, coverage mappers, source-mappers (?), instrumenters, testing utilities, etc. have to make the choice to use 1 of the following parsers. 

This causes many issues for interoperability:
- istanbul/coverage + transpiled libraries
- webpack + acorn + limited type support
- program flow based optimizations have to be implemented differently for every parser

## Naive Proposal/Discussion
What if we think of these different parsers as simply a set of features for _a single_ base parser? This could unify tools, parser _consumers_ and promote interoperability. A rich and configurable feature/plugin system, then allows for multi-framework support, and extensibility. (Experimental use of ES features, etc.) 

```
parse
  |-- event delegation
  |-- syntax flagging
  |-- custom estrees(?)/AST's
  |-- typed language support
  |-- (?)
```

## Lessons Learned from:
### Acorn
- Plugin system is hard, complex. Where can we learn from this? 
- Event delegation drives important bundlers and tools like webpack. If this is a feature, should it be written in a way to interop with other plugins features? IE: extended, transpiled languages (events for TS, Flow, CJS, etc.).

### Esprima
- Limiting features can yield higher parse performance. Where is this relevant, can this help establish a base

### Babylon/Babili
- Program Flow is important and can be leveraged for optimizations, compression, minimizing. However the level of optimization is based on the amount of knowledge known (types, etc.). Can this be applied to more than just ES6?

