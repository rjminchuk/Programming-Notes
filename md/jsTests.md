#### 2017-12-15 Unit Testing on MacOS/Windows

I wanted to create a cross OS unit testing suite that I could run straight from the command line/terminal. Here is what I found.

# JSC on MacOS

###### initial setup... 

Setup WebKits' JavaScript Core on MacOS. To get command line access to JSC, create a symbolic link in your usr directory to the command (working in MacOS 10.13). [more on that](http://www.phpied.com/javascript-shell-scripting/) 

```sh
sudo ln -s /System/Library/Frameworks/JavaScriptCore.framework/Versions/A/Resources/jsc /usr/local/bin
```

###### Run Tests
Run the tests.js script file to see if your tests work. `cd` to the directory, then `jsc tests.js`. Doesn't get simpler than that. 

# CScript on windows.

No setup required, just `cd` to the directory, then `cscript tests.js`

# The TestSuite

```js
// tests.js
'use strict';

var jsCode = '';
var print = print || function print (msg) { WScript.StdOut.WriteLine(msg); };
var load = load || function load (file) { jsCode += new ActiveXObject('Scripting.FileSystemObject').OpenTextFile(file, 1).ReadAll(); };

// clear screen
for (var i = 0; i < 100; i++) { print(''); }

load('helpers/array.forEach.js'); // polyfills
load('helpers/mockObjects.js'); // objects
load('helpers/extend.js'); // utils
load('helpers/utcDate.js'); // utils

// scripts & tests
load('MyUtility.js');
load('MyUtility.tests.js');

// test suite code, init, and execute.
load('helpers/testSuite.tests.js');
load('helpers/testSuite.js');

// if cscript, eval the loaded code.
eval(jsCode);
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
TestSuite = extend( TestSuite, 
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
                            if (typeof tests[test] === 'function') { 
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
        AssertEqual: function(a, b, testIsSilent) {
            return TestSuite._logging((a === b), !!testIsSilent, arguments);
        },
        AssertNotEqual: function(a, b, testIsSilent) {
            return TestSuite._logging((a !== b), !!testIsSilent, arguments);
        },
        _logging: function(result, testIsSilent, args) {
            if (!testIsSilent) { TestSuite._summary_cnt++; }
            if (result && !testIsSilent) { TestSuite._summary_passed++; }
            if (!result && !testIsSilent) { print(args.callee.caller.name); }
            return result;
        }
    }
);

TestSuite.TestSuite(MockObjects).RunSuite();
```

```js
// testSuite.tests.js
var TestSuite = TestSuite || {};
TestSuite = extend(TestSuite, {
    TestSuiteTestPackage: [
        { 
            AssertEqualEqualEqual_true: function(d) {
                TestSuite.AssertEqual(true, true);
            },
            AssertEqualEqualEqual_false: function(d) {
                TestSuite.AssertEqual(false, false);
            },
            AssertEqualEqualEqual_0: function(d) {
                TestSuite.AssertEqual(0, 0);
            },
            AssertEqualEqualEqual_1: function(d) {
                TestSuite.AssertEqual('1', '1');
            },
            AssertEqualEqualEqual_undefined: function(d) {
                TestSuite.AssertEqual(undefined, undefined);
            },
            AssertNotEqualEqual_false: function(d) {
                TestSuite.AssertNotEqual('false', false);
            },
            AssertNotEqualEqual_true: function(d) {
                TestSuite.AssertNotEqual('true', true);
            },
            AssertNotEqualEqual_0: function(d) {
                TestSuite.AssertNotEqual('0', 0);
            },
            AssertNotEqualEqual_1: function(d) {
                TestSuite.AssertNotEqual('1', 1);
            },
            AssertNotEqualEqual_undefined: function(d) {
                TestSuite.AssertNotEqual('undefined', undefined);
            },
            AssertEqualEqualEqual_shouldfail_when_1_equals_true: function(d) {
                TestSuite.AssertEqual(
                    TestSuite.AssertEqual(1, true, true), 
                    false
                );
            },
            AssertEqualEqualEqual_shouldfail_when_0_equals_false: function(d) {
                TestSuite.AssertEqual(
                    TestSuite.AssertEqual(0, false, true), 
                    false
                );
            }
        }
    ]
});
```

```js
// MyUtility.tests.js
var TestSuite = TestSuite || {};
TestSuite = extend(TestSuite, 
{
    MyUtilityTestPackage:
    [
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
// MyUtility.js
'use strict';

var MyUtility = MyUtility || {};
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
...
```

```js
// helper/array.forEach.js
// http://es5.github.io/#x15.4.4.18
...
```

```js
// utcDate.js
function utcDateMaker(NotZeroIndex_Hour) {
    var d = new Date(Date.UTC(2017,0,1,NotZeroIndex_Hour,0,0) 
            + new Date(2017,0,1,NotZeroIndex_Hour,0,0).getTime() 
            - Date.UTC(2017,0,1,NotZeroIndex_Hour,0,0));
    return d;
}
```
