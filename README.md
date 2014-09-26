Ferite Testlib
==============

Ferite testlib consists of several ferite modules ([testlib](#testlib),[assert](#assert))
and a shell script ([fe-runtests](#fe-runtests)) that allows you to:

- Easily write clean, readable tests.
- Easily run tests.
- Organize test suites and test cases.

The API has been designed to be easy to read and requires minimal boilerplate to write:

	uses 'testlib', 'assert'; // really, this is all the boilerplate
	
	testsuite('Numerical values') using {
		testcase('equality of integer representations') using {
			assert(100).equals(0x64);
			assert(0144).equals(100);
			assert(0x64).equals(0b1100100);
		};
		
		testcase('canonical string conversions') using {
			assert('' + 0x64).equals('100');
			assert('' + 0144).does_not.equal('0144');
		};
	};

The core module is [testlib](#testlib) which is basically a test harness framework.

[testlib](#testlib) on its own does not provide any assertion functionality and detects
test failures purely from raised errors. Of course, you may manually `raise` an error
yourself but manually writing `if` statements and raising errors can quickly become
tedious. Ferite Testlib includes the [assert](#assert) module allowing you to write
quick, easy to read assertions.

Finally there is the [fe-runtest](#fe-runtest) script that you can use to recursively
scan your project directory for test directories and it will execute all test scripts
it finds.

# Testlib

`testlib` exposes two functions: [testsuite](#function-testsuite) and
[testcase](#function-testcase). Testcases are executed in a closure using `testsuite`:

    testsuite("description") using {
		// testcases go here
	};

`testlib` defines failure as uncaught errors. To signal to testlib that a test has
failed simply raise an error:

    testsuite('Failure') using {
		testcase('this demonstrates a failed test') {
			raise new Error('example failure');
		};
	};

To run a test you simply execute the test script:

	ferite my_test_script.fe

The `testsuite` function checks command line arguments to determine how to format the
test output. You can change the output format by supplying the formatter name:

	ferite my_test_script.fe -- verbose
	
The following formatters are currently supported:

- default - The default formatter, prints minimal info
- verbose - Prints verbose test reports
- tap - Prints test reports conforming to the [test anything protocol (TAP)](http://testanything.org/)

## Function: testsuite

    testsuite (string description) using {};

This function is responsible for invoking the testcases. The testcases themselves
merely register their closures with the testsuite. This allows the testsuite to
automatically keep track of how many testcases are defined before even running any
of the tests. After invoking the closure passed to it the `testsuite` function runs
all the registered testcases and generates the test output.

## Function: testcase

    testcase (string description) using {};
	
This function registers a closure to be executed by the `testsuite` function.

# Assert

The `assert` module exposes two functions: [assert](#function-assert) and
[assert_error](#function-assert_error). They provide an clean interface for raising
assertion errors to signal to `testlib` that a test has failed.

## Function: assert

    assert(void object_to_test);

This is actually a factory function that returns an [Assert](#class-assert) object. The returned object
exposes methods that allows tests to be done on the input of `assert` which will
raise assertion errors when the tests fail.

## Class: Assert

...

# Fe-Runtest

...
