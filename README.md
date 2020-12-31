# Programming Spec for Lære Programming Language

## Grammar at bottom

Implementation:
* Interpreter (JS)

Design goals:
* Everything is a function
* Function parameters can be passed in various ways

Features:
* Lua-style methods (`obj.func(obj, 1)` == `obj:func(1)`)
* Optional passing of lambdas by block after function invocation
* Parameter modifiers (mut/mutable, lzy/lazy (Evaluates on first use and gets cached), dyn/dynamic value (Like lazy but w/o cache))

Possible additions: 
[ ] object literals `{key: value}`
[ ] array literals `[a, b, c]`
[ ] tuple literals `(a, b, c)`

Examples:
```js
fn('sum', 'dyn param1', 'param2') {
  return (std.add(param1, param2))
}

print(sum(1, 3))
```

Grammar:
```nearley

program -> null
  | program _ statement

statement -> functionCall
  | primitive
  | value {}

functionCall -> baseFunctionCall "{" _ program _ "}"
  | baseFunctionCall

baseFunctionCall -> identifier "(" _ list _ ")"
  | indentifier ":" identifier "(" _ list _ ")"

list -> null
  | listPart

listPart -> statement
  | listPart _ "," _ statement

primitive -> string
  | number

string -> "'" stringpart:* "'"

stringpart -> [a-zA-Z0-9!@#%^&*()\[\]+=_<>?,./`;:"~{} \t\n\r-]
  | "\\\u" hex hex hex hex
  | "\\\x" hex hex
  | "\\t"
  | "\\n"
  | "\\r"
  | "\\\""
  | "\\$"
  | "\\'"
  | "$" identifier
  | "${" statement "}"

hex -> [a-fA-F0-9]

number -> [0-9]:+
  | [0-9]:+ "." [0-9]:*
  | [0-9]:+ "E" [+-]:? [0-9]:+
  | [0-9]:+ "." [0-9]:* "E" [+-]:? [0-9]:+

identifier -> [a-zA-Z0-9_]:+

_ -> [\t\n\r\s]:*
__ -> [\t\n\r\s]:+

```
