# api documentation for  [json-stringify-safe (v5.0.1)](https://github.com/isaacs/json-stringify-safe)  [![npm package](https://img.shields.io/npm/v/npmdoc-json-stringify-safe.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-json-stringify-safe) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-json-stringify-safe.svg)](https://travis-ci.org/npmdoc/node-npmdoc-json-stringify-safe)
#### Like JSON.stringify, but doesn't blow up on circular refs.

[![NPM](https://nodei.co/npm/json-stringify-safe.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/json-stringify-safe)

[![apidoc](https://npmdoc.github.io/node-npmdoc-json-stringify-safe/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-json-stringify-safe/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-json-stringify-safe/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-json-stringify-safe/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Isaac Z. Schlueter",
        "url": "http://blog.izs.me"
    },
    "bugs": {
        "url": "https://github.com/isaacs/json-stringify-safe/issues"
    },
    "contributors": [
        {
            "name": "Andri Möll",
            "url": "http://themoll.com"
        }
    ],
    "dependencies": {},
    "description": "Like JSON.stringify, but doesn't blow up on circular refs.",
    "devDependencies": {
        "mocha": ">= 2.1.0 < 3",
        "must": ">= 0.12 < 0.13",
        "sinon": ">= 1.12.2 < 2"
    },
    "directories": {},
    "dist": {
        "shasum": "1296a2d58fd45f19a0f6ce01d65701e2c735b6eb",
        "tarball": "https://registry.npmjs.org/json-stringify-safe/-/json-stringify-safe-5.0.1.tgz"
    },
    "gitHead": "3890dceab3ad14f8701e38ca74f38276abc76de5",
    "homepage": "https://github.com/isaacs/json-stringify-safe",
    "keywords": [
        "json",
        "stringify",
        "circular",
        "safe"
    ],
    "license": "ISC",
    "main": "stringify.js",
    "maintainers": [
        {
            "name": "isaacs"
        },
        {
            "name": "moll"
        }
    ],
    "name": "json-stringify-safe",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git://github.com/isaacs/json-stringify-safe.git"
    },
    "scripts": {
        "test": "node test.js"
    },
    "version": "5.0.1"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module json-stringify-safe](#apidoc.module.json-stringify-safe)
1.  [function <span class="apidocSignatureSpan"></span>json-stringify-safe (obj, replacer, spaces, cycleReplacer)](#apidoc.element.json-stringify-safe.json-stringify-safe)
1.  [function <span class="apidocSignatureSpan">json-stringify-safe.</span>getSerialize (replacer, cycleReplacer)](#apidoc.element.json-stringify-safe.getSerialize)
1.  [function <span class="apidocSignatureSpan">json-stringify-safe.</span>toString ()](#apidoc.element.json-stringify-safe.toString)



# <a name="apidoc.module.json-stringify-safe"></a>[module json-stringify-safe](#apidoc.module.json-stringify-safe)

#### <a name="apidoc.element.json-stringify-safe.json-stringify-safe"></a>[function <span class="apidocSignatureSpan"></span>json-stringify-safe (obj, replacer, spaces, cycleReplacer)](#apidoc.element.json-stringify-safe.json-stringify-safe)
- description and source-code
```javascript
function stringify(obj, replacer, spaces, cycleReplacer) {
  return JSON.stringify(obj, serializer(replacer, cycleReplacer), spaces)
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.json-stringify-safe.getSerialize"></a>[function <span class="apidocSignatureSpan">json-stringify-safe.</span>getSerialize (replacer, cycleReplacer)](#apidoc.element.json-stringify-safe.getSerialize)
- description and source-code
```javascript
function serializer(replacer, cycleReplacer) {
  var stack = [], keys = []

  if (cycleReplacer == null) cycleReplacer = function(key, value) {
    if (stack[0] === value) return "[Circular ~]"
    return "[Circular ~." + keys.slice(0, stack.indexOf(value)).join(".") + "]"
  }

  return function(key, value) {
    if (stack.length > 0) {
      var thisPos = stack.indexOf(this)
      ~thisPos ? stack.splice(thisPos + 1) : stack.push(this)
      ~thisPos ? keys.splice(thisPos, Infinity, key) : keys.push(key)
      if (~stack.indexOf(value)) value = cycleReplacer.call(this, key, value)
    }
    else stack.push(value)

    return replacer == null ? value : replacer.call(this, key, value)
  }
}
```
- example usage
```shell
...
The default 'decycler' function returns the string ''[Circular]''.
If, for example, you pass in 'function(k,v){}' (return nothing) then it
will prune cycles.  If you pass in 'function(k,v){ return {foo: 'bar'}}',
then cyclical objects will always be represented as '{"foo":"bar"}' in
the result.

'''
stringify.getSerialize(serializer, decycler)
'''

Returns a serializer that can be used elsewhere.  This is the actual
function that's passed to JSON.stringify.

**Note** that the function returned from 'getSerialize' is stateful for now, so
do **not** use it more than once.
...
```

#### <a name="apidoc.element.json-stringify-safe.toString"></a>[function <span class="apidocSignatureSpan">json-stringify-safe.</span>toString ()](#apidoc.element.json-stringify-safe.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
