Ferite Testlib
==============

Ferite testlib consists of several ferite modules and a shell script to write and
run tests. Tests are organized into test suites and test cases. The API has been
designed to be easy to read and requires minimal boilerplate to write:

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

[testlib](#testlib) on its own does not provide any assertion functionality
and detects test failures purely from raised errors. Of course, you may manually
`raise` an error yourself but manually writing `if` statements and raising errors
can quickly become tedious. Ferite Testlib includes the [assert](#assert)
module allowing you to write quick, easy to read assertions.

Finally there is the [fe-runtest](#fe-runtest) script that you can use to recursively
scan your project directory for test directories and it will execute all test
scripts it finds.

# Testlib

...

# Assert

...

# Fe-Runtest

...
