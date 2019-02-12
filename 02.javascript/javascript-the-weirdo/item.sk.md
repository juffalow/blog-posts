---
title: 'Javascript, the weirdo'
media_order: javascript.png
published: false
taxonomy:
    tag:
        - JavaScript
content:
    items:
        - '@self.children'
    limit: 5
    order:
        by: date
        dir: desc
    pagination: true
    url_taxonomy_filters: true
---

Collection of JavaScript examples when it behaves...weird? Funny?

===

```javascript
2 + true // => 3

0.1 + 0.2 // => 0.300000000000000004

typeof NaN // => number

'5' - 3 // => 2

'5' + 3 // => 53

'5' - '4' // => 1

'5' + + '5' // => 55

'foo' + + 'foo' // => fooNan

'5' + - '2' // => 5-2

'5' + - + - - + - - + + - + - + - + - - - '-2' // => 52

[] + [] // => ""

[] + {} // => [object Object]

{} + [] // => 0

[+!+[]]+[!+[]+!+[]]+[!+[]+!+[]+!+[]] // => "123"

null > 0 // => false
null == 0 // => false
null >= 0 // => true

parseInt(0.000001) // => 0
parseInt(0.0000001) // => 1

parseInt(0.000006) // => 0
parseInt(0.0000006) // => 6
```
