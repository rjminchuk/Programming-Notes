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

```js
// tests.js
'use strict';

// clear screen
for (var i = 0; i < 100; i++) { print(''); }

load('helpers/mockObjects.js'); // load mock objects
load('helpers/extend.js'); // load utils
load('helpers/utcDate.js'); // load utils

load('myUtility.js'); // load scripts & tests
load('myUtility.tests.js');

load('helpers/testSuite.js'); // load test suite code

// init the test suite and execute.
var TestSuite = TestSuite || {};
TestSuite.TestSuite = TestSuite.TestSuite || function() { return this; };
TestSuite.RunSuite = TestSuite.RunSuite || function () { debug('You forgot to include the TestSuite.'); };
TestSuite.TestSuite(MockObjects).RunSuite();
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
// testSuite.js
var TestSuite = TestSuite || {};
TestSuite = extend(
    TestSuite, 
    {
        _this: undefined,
        _dependency: undefined,
        TestSuite: function(p_dependency) {
            TestSuite._this = this;
            this._dependency = p_dependency;
            return this;
        },
        RunSuite: function() {
            for (var pkg in TestSuite) { // all packages in the test suite
                if (pkg.includes('TestPackage')) { 
                    TestSuite[pkg].forEach(function (tests, indx) { 
                        for (var test in tests) { // all tests in the package
                            if (typeof tests[test] === "function") { 
                                tests[test](); 
                            }
                        }
                    });
                }
            }
            print('');
            print('tests ran');
            print('');
        }
    }
);
```

```js
// MyUtility.tests.js
var TestSuite = TestSuite || {};
TestSuite = extend(TestSuite, {
    MyUtilityTestPackage: [
        { 
            The_name_of_the_test_function_which_should_be_descriptive: function() {
                if (!MyUtility.FunctionToTest(TestSuite['dependency'].window) {
                    debug(arguments.callee.name);
                }
            }
        }
    ]
});
```
