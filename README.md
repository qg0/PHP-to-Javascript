PHP-to-Javascript
=================

A tool for converting simple PHP objects to Javascript code, so that code for manipulating objects can be used both server-side and client-side.

There is a page with a set of example code that can be [converted online here] (http://www.basereality.com/PHPToJavascript/) "PHP to Javascript conversion").


How to use
==========

Examples
--------

cd tests
php examples.php

This will convert the PHP files in the tests directory and for each write a javascript file equivalent.


Programmatically
----------------

* Install Composer from http://getcomposer.org/

* Add the "base-reality/php-to-javascript": ">=0.0.3" to your project's composer.json file:

    "require":{
		"base-reality/php-to-javascript": ">=0.1.0"
	}

  Or the latest version.

* Include the Composer SPL autoload file in your project:

    require_once('../vendor/autoload.php');


* Call the converter:

    $phpToJavascript = new PHPToJavascript\PHPToJavascript();
    $phpToJavascript->addFromFile($inputFilename);
    $jsOutput = $phpToJavascript->toJavascript();

$jsOutput will now contain an auto-generated Javascript version of the PHP source file.


TODO
====

* Add more tests, setup some automated testing.

* Add hasOwnProperty check to foreach loops.

* Add support for const to be same as public static.

* Figure out what to do about Javascript reserved keywords. Probably out to detect them and either warn or give an error on detection.  testObject.delete();

* Support variables inside strings. e.g. echo "Hello my name is $name" would be the tokens.
    CodeConverterState_Default token [T_ENCAPSED_AND_WHITESPACE] => [Hello my name is ]
    CodeConverterState_Default token [T_VARIABLE] => [$name]

Limitations
===========

There are several features of the PHP language that either are too difficult to map into Javascript, or are just not possible in Javascript. These features will almost certainly never be implemented (at least by myself) so if you're waiting for these to be done, give up early.

Pass by reference
-----------------

This feature does not exist in Javascript and so cannot be supported without a huge amount of effort.

Static class variables are always public
----------------------------------------

Due to the way objects in Javascript are implemented static class variables will always have public scope.


Defines are converted to value, but not defined in Javascript.
-------------------------------------------------------------

The code:

    define('DATABASE_TYPE', 'MySQL');
	echo "Database type is ".DATABASE_TYPE;

is converted to:

    // define('DATABASE_TYPE', 'MySQL');
    document.write( "Database type is " + 'MySQL');

If this is a problem for you - current solution is don't use defines. Instead use classes to define your const variables. It would be possible to add the define as a variable in the Global scope for Javascript. But that would be kind of sucky.