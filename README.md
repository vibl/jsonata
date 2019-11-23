# JSONata with COW cloning for the Transform operator

This package is forked from https://github.com/jsonata-js/jsonata. 

In the original JSONata package, the [Transform operator (... ~> | ... | ... |)](https://docs.jsonata.org/other-operators#transform-) deep clones the input object, which can be very costly in memory, in GC resources and in deep equality checking afterwards (e.g. for React components with a Redux store `transform`ed with JSONata in reducers). 

In this package, the Transform operator does copy-on-write cloning, which means it only clones the "lineage" of the transformed values  (i.e. the parents of the transformed values, up to the root input object). It does not mutate any object in the process.

Caveats: 
- It does NOT work when the location pattern includes a descendant (**) operator.
- The `head` part (before the `~>`) should only be `$`, i.e. the root element. So, the whole expression should always start with `jsonata("$ ~> |...`.

## Installation

- `npm install @vibl/jsonata`

Below is the original README of the JSONata package:

# JSONata

JSON query and transformation language

[![NPM statistics](https://nodei.co/npm/jsonata.png?downloads=true&downloadRank=true)](https://nodei.co/npm/jsonata/)

[![Build Status](https://travis-ci.org/jsonata-js/jsonata.svg)](https://travis-ci.org/jsonata-js/jsonata)
[![Coverage Status](https://coveralls.io/repos/github/jsonata-js/jsonata/badge.svg?branch=master)](https://coveralls.io/github/jsonata-js/jsonata?branch=master)

Reference implementation of the [JSONata query and transformation language](http://jsonata.org/).

* [JSONata in 5 minutes](https://www.youtube.com/embed/ZBaK40rtIBM)
* [JSONata language documentation](http://docs.jsonata.org/)
* [Try it out!](http://try.jsonata.org/)


## Quick start

In Node.js:

```javascript
var jsonata = require("jsonata");

var data = {
  example: [
    {value: 4},
    {value: 7},
    {value: 13}
  ]
};
var expression = jsonata("$sum(example.value)");
var result = expression.evaluate(data);  // returns 24
```

In a browser:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>JSONata test</title>
    <script src="https://cdn.jsdelivr.net/npm/jsonata/jsonata.min.js"></script>
    <script>
      function greeting() {
        var json = JSON.parse(document.getElementById('json').value);
        var result = jsonata('"Hello, " & name').evaluate(json);
        document.getElementById('greeting').innerHTML = result;
      }
    </script>
  </head>
  <body>
    <textarea id="json">{ "name": "Wilbur" }</textarea>
    <button onclick="greeting()">Click me</button>
    <p id="greeting"></p>
  </body>
</html>
```

## More information
- JSONata [documentation](http://docs.jsonata.org/)
- [JavaScript API](http://docs.jsonata.org/embedding-extending)
- [Intro talk](https://www.youtube.com/watch?v=TDWf6R8aqDo) at London Node User Group
- JSONata [tech talk](https://www.youtube.com/watch?v=ZRtlkIj0uDY) 

## Contributing

See the [CONTRIBUTING.md](CONTRIBUTING.md) for details of how to contribute to this repo.
