# localStorage Bug on IE

Run these code on your IE and open the console to see the error!

```javascript
function _setItem(key, value) {
  localStorage.constructor.prototype.setItem.call(localStorage, key, value);
}

function _getItem(key) {
  return localStorage[key];
}

function _clearAll() {
  localStorage.constructor.prototype.clear.call(localStorage);
}

function assert(condition, warning) {
  if (condition) {
    console.log('Test OK', warning);
  } else {
    console.error(warning);
  }
}

function test(type) {
  return function(funName) {
    var fn = _getItem(funName);
    assert(typeof fn === type, 'localStorage.' + funName + ' should be a ' + type);
    _setItem(funName, '__changed__');
    fn = _getItem(funName);
    assert(typeof fn === type, 'after set, localStorage.' + funName + ' should still be a ' + type);
  }
}

var fns = 'getItem|key|setItem|removeItem|clear'.split('|');

for(var i = 0; i < fns.length; i++) {
  test('function')(fns[i]);
}

test('number')('length');

_clearAll();
```
