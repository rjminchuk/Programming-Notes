# Unit Testing on MacOS

## JavaScriptCore by WebKit

Setup WebKits JavaScript Core on MacOS. Run the script below to setup command line access to js. [more](http://www.phpied.com/javascript-shell-scripting/)

```sh
sudo ln -s /System/Library/Frameworks/JavaScriptCore.framework/Versions/A/Resources/jsc /usr/local/bin
```

Run the tests.js script file to see if your tests work. Doesn't get simpler than that.

```sh
cd wwwroot/js
jsc tests.js
```

or on windows.

```cmd
cscript tests.js
```

```js
// tests.js
'use strict';

var tests = '';
var print = print || function print (msg) { WScript.StdOut.WriteLine(msg); };
var load = load || function load (file) { tests += new ActiveXObject("Scripting.FileSystemObject").OpenTextFile(file, 1).ReadAll(); };

// clear screen
for (var i = 0; i < 100; i++) { print(''); }

load('helpers/array.forEach.js'); // load polyfills
load('helpers/mockObjects.js'); // load mock objects
load('helpers/extend.js'); // load utils
load('helpers/utcDate.js'); // load utils

load('horizn.js'); // load scripts & tests
load('horizn.tests.js');

load('helpers/testSuite.js'); // load test suite code, init, and execute.

// if cscript, eval the loaded code.
eval(tests);
```

```js
// helpers/mockObjects.js
'use strict';

var MockObjects = {
    window: {
        screen: {
            width: 375,
            height: 812
        },
        devicePixelRatio: 3
    },
    document: 
    {
        body: {
            _attr: undefined,
            getAttribute: function (attr) {
                MockObjects.document.body._attr = ' body' + attr;
                return MockObjects.document.body._attr;
            },
            setAttribute: function (attr) {
                MockObjects.document.body._attr = (MockObjects.document.body._attr || '') + attr;
            }
        }
    },
    navigator: {
        userAgent: 'iPhone'
    }
};
```

```js
// helpers/testSuite.js
var TestSuite = TestSuite || {};
TestSuite = extend(
    TestSuite, 
    {
        _this: undefined,
        _dependencies: undefined,
        _summary_cnt: 0,
        _summary_passed: 0,
        TestSuite: function(p_dependencies) {
            TestSuite._this = this;
            this._dependencies = p_dependencies;
            return this;
        },
        RunSuite: function() {
            for (var pkg in TestSuite) { // all packages in the test suite
                if (pkg.indexOf('TestPackage') !== -1) { 
                    TestSuite[pkg].forEach(function (tests, indx) { 
                        for (var test in tests) { // all tests in the package
                            if (typeof tests[test] === "function") { 
                                tests[test](TestSuite._dependencies); 
                            }
                        }
                    });
                }
            }
            print('');
            print('tests ran    : ' + TestSuite._summary_cnt);
            print('tests passed : ' + TestSuite._summary_passed);
        },
        AssertEqual: function(a, b) {
            TestSuite._summary_cnt++;
            if (a === b) { TestSuite._summary_passed++; } 
            else { debug(arguments.callee.caller.name); }
        }
    }
);

TestSuite.TestSuite(MockObjects).RunSuite();
```

```js
// MyUtility.tests.js
var TestSuite = TestSuite || {};
TestSuite = extend(TestSuite, {
    MyUtilityTestPackage: [
        { 
            Brightness_Is_Easy_On_The_Eyes_At_6PM: function(d) {
                TestSuite.AssertEqual(
                    MyUtility.EasyEyes.EasyEyes(utcDateMaker(18), Math, d.document), true
                );
            }
        }
    ]
});
```

```js
// MyUtility.tests.js
'use strict';

var MyUtility = Horizn || {};
MyUtility.EasyEyes = MyUtility.EasyEyes || {};

MyUtility.EasyEyes = {
    EasyEyes: function (p_date, p_math, p_document) {
        var bodyClasses = p_document.body.getAttribute('class') || '';
        bodyClasses = bodyClasses.replace(' easyEyes', '');
        if (p_date.getHours() - 12 >= 6 || p_date.getHours() - 12 < -6) { bodyClasses += ' easyEyes'; }
        p_document.body.setAttribute('class', bodyClasses);
        return bodyClasses.indexOf('easyEyes') !== -1;
    }
};
```

```js
// helpers/extend.js
// https://gist.github.com/cferdinandi/4f8a0e17921c5b46e6c4
var extend = function ( defaults, options ) {
    var extended = {};
    var prop;
    for (prop in defaults) {
        if (Object.prototype.hasOwnProperty.call(defaults, prop)) {
            extended[prop] = defaults[prop];
        }
    }
    for (prop in options) {
        if (Object.prototype.hasOwnProperty.call(options, prop)) {
            extended[prop] = options[prop];
        }
    }
    return extended;
};
```

```js
// helper/array.forEach.js
// Production steps of ECMA-262, Edition 5, 15.4.4.18
// Reference: http://es5.github.io/#x15.4.4.18
if (!Array.prototype.forEach) {
    
      Array.prototype.forEach = function(callback/*, thisArg*/) {
    
        var T, k;
    
        if (this == null) {
          throw new TypeError('this is null or not defined');
        }
    
        // 1. Let O be the result of calling toObject() passing the
        // |this| value as the argument.
        var O = Object(this);
    
        // 2. Let lenValue be the result of calling the Get() internal
        // method of O with the argument "length".
        // 3. Let len be toUint32(lenValue).
        var len = O.length >>> 0;
    
        // 4. If isCallable(callback) is false, throw a TypeError exception. 
        // See: http://es5.github.com/#x9.11
        if (typeof callback !== 'function') {
          throw new TypeError(callback + ' is not a function');
        }
    
        // 5. If thisArg was supplied, let T be thisArg; else let
        // T be undefined.
        if (arguments.length > 1) {
          T = arguments[1];
        }
    
        // 6. Let k be 0.
        k = 0;
    
        // 7. Repeat while k < len.
        while (k < len) {
    
          var kValue;
    
          // a. Let Pk be ToString(k).
          //    This is implicit for LHS operands of the in operator.
          // b. Let kPresent be the result of calling the HasProperty
          //    internal method of O with argument Pk.
          //    This step can be combined with c.
          // c. If kPresent is true, then
          if (k in O) {
    
            // i. Let kValue be the result of calling the Get internal
            // method of O with argument Pk.
            kValue = O[k];
    
            // ii. Call the Call internal method of callback with T as
            // the this value and argument list containing kValue, k, and O.
            callback.call(T, kValue, k, O);
          }
          // d. Increase k by 1.
          k++;
        }
        // 8. return undefined.
      };
    }
```
