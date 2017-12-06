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
                if (!MyUtility.FunctionToTest(TestSuite['dependency']) {
                    debug(arguments.callee.name);
                }
            }
        }
    ]
});
```