# Parser Features

The following is a list of all possible parser features, along with commonly agreed terminology for them.

- [Node location](#node-location)
- [Token collection](#token-collection)
- [Comment collection](#comment-collection)
- [Comment attachment](#comment-attachment)
- [Event delegation](#event-delegation)
- [Exported parser class](#exported-parser-class)
- [Pure tokenization mode](#pure-tokenization-mode)

## Node Location

Node location refers to the addition of properties to each syntax node to denote the location of the node in the source text.

### Location object: Variant A

Implemented by: SpiderMonkey parser, Esprima, Acorn, Flow parser.

The `loc` property is an object with two properties, `start` and `end`.
Each property represents a position, a object of a `line` number (one-indexed) and a `column` number (zero-indexed):

If the node contains no information about the source location, `loc` is null.

```
interface Node {
    type: string;
    loc: SourceLocation | null;
}
interface SourceLocation {
    start: Position;
    end: Position;
}
interface Position {
    line: number; // >= 1
    column: number; // >= 0
}
```

Example:
```js
> parser.parse('x = 2', options);
Program {
  type: 'Program',
  body:
   [ ExpressionStatement {
       type: 'ExpressionStatement',
       expression: [Object],
       loc: [Object] } ],
  loc: { start: { line: 1, column: 0 }, end: { line: 1, column: 5 } } }
```

### Location object: Variant B

Implemented by: Traceur parser.

```
interface Node {
    type: string;
    location: Location;
}
interface Location {
    start: Position;
    end: Position;
}
interface Position {
    offset: number;
    line: number;
    column: number;
}
```

### Zero-based character range

Implemented by: Esprima, Acorn, Flow parser.

The `range` property is a two-element array that holds the start and end position (exclusive).
Each array is an integer number where 0 points to the beginning of the source text.

```
interface Node {
    type: string;
    range: Range;
}
```

Example:
```js
> parser.parse('x = 2', options);
Program {
  type: 'Program',
  body: 
   [ ExpressionStatement {
       type: 'ExpressionStatement',
       expression: [Object],
       range: [Object] } ],
  range: [ 0, 5 ] }
```

### Start and end location

Implemented by: Acorn, Babylon, TypeScript parser.

The `start` and `end` properties are two integer numbers representing the location of the node in the source text.
Each number is a zero-based index, 0 points to the beginning of the source text

```
interface Node {
    type: string;
    start: number;
    end: number;
}
```

Example:
```js
> parser.parse('x = 2')
Node {
  type: 'Program',
  start: 0,
  end: 5,
  body: 
   [ Node {
       type: 'ExpressionStatement',
       start: 0,
       end: 5,
       expression: [Object] } ],
  sourceType: 'script' }
```

**Note**: TypeScript parser produces the pair `pos` and `end`.

### Start and end token

Implemented by: UglifyJS2 parser.

The `start` and `end` properties are two objects representing the first token and the last token of the node.
Each token contains the information of its location in the form of zero-based indexes (`pos` and `endpos`), one-based line numbers (`line` and `endline`), and zero-based column numbers (`col` and `endcol`).

```
interface Node {
    type: string;
    start: Token;
    end: Token;
}
interface Token {
    type: string;
    pos: number;
    col: number;
    line: number;
    endpos: number;
    endcol: number;
    endline: number;
}
```

Example:
```js
> require('uglify-js').parse('x = 2')
AST_Node {
  end: 
   AST_Token {
     raw: '2',
     file: null,
     comments_before: [],
     nlb: false,
     endpos: 5,
     endcol: 5,
     endline: 1,
     pos: 4,
     col: 4,
     line: 1,
     value: 2,
     type: 'num' },
  start: 
   AST_Token {
     raw: undefined,
     file: null,
     comments_before: [],
     nlb: false,
     endpos: 1,
     endcol: 1,
     endline: 1,
     pos: 0,
     col: 0,
     line: 1,
     value: 'x',
     type: 'name' },
body: [ AST_Node { end: [Object], start: [Object], body: [Object] } ] }
```

### Location mapping

Implemented by: Shift parser.

The `locations` property is a WeakMap that maps every node with its location information.

```
interface SyntaxTree {
    tree: Script | Module;
    locations: WeakMap<Node, Location>
}
interface Location {
    line: number;
    column: number;
    offset: number;
}
```

Example:
```js
> var ast = shift.parseScriptWithLocation('x = 2')
{ tree: Script { type: 'Script', directives: [], statements: [ [Object] ] },
  locations: WeakMap {} }
> ast.locations.get(ast.tree)
{ start: { line: 1, column: 0, offset: 0 },
  end: { line: 1, column: 5, offset: 5 } }
```

## Token Collection

To be written.

## Comment Collection

To be written.

## Comment Attachment

To be written.

## Event Delegation

To be written.

## Exported Parser Class

To be written.

## Pure Tokenization Mode

To be written.

