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
