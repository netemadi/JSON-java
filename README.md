# 262P-milestone2

In this project we are forking the JSON-Java repo which has the package org.json.
The aim of this prorject is to add 2 new functionality to this library.  
In this project we are working on 3 different XML files in order to test our functionalities (small.xml, medium.xml, ...)
The first one is getting a reader and JSONPointer path:
  - The function will start parsing the XML file reading line by line (token by token)
  - Splitting the path: 
      * Path can be just the tag names like "wikimedia/siteinfo/namespaces" in "medium.xml" which should return an array of namespaces from XML file but in JSON file.
      * Or path can be "wikimedia/siteinfo/namespaces/namespace/2" in "medium.xml" which should return the third element of namespaces array as in JSON file.

Some of our edge cases which we are not covering in this project:
  - Not considering nested array ---> Path "wikimedia/siteinfo/namespaces/namespace/1/key" is not acceptable in our program  
  - Sometimes in some XML files we see something like the blew open tag which has some keys and values ---> 
    ```xml
    <mediawiki xmlns="Example1" xmlns:xsi="Example2" xsi:schemaLocation="Example3" version="0.10" xml:lang="en">
    ```
    this will be acceptable if there is no whitespaces between values, for example "xsi:schemaLocation="Example3 Example4" is not acceptable in our case and it         will only return the first value not both. 
  - Following tag formats are not acceptable in our functionality. Examples:
    ```xml 
    <mediawiki Example="value"/> 
    <mediawiki>Example<mediawiki/> 
    ```
  - If the last key in your path is an array the program will return the first element (index 0) of that array. Example: if you pass
    "wikimedia/siteinfo/namespaces/namespace" as a key and using "medium.xml" file will return:       
      {
        "key": -2,
        "case": "first-letter",
        "content": "Media"
      },

The second one is getting a reader and JSONPointer path and a JSONObject to replace:
  - This function will find the sub obj from path and will replace it with the JSONObject given as an argument
  

How to run the program: 
- Fork the project
- Go to src > main > java and run:
  * javac org/json/*.java
  * jar cf json-java.jar org/json/*.class
- Navigate to src > test > java > org.json.junit > XMLTest.java and run the JUNIT test
- Actual functionality is under src > main > java > org.json > XML.java


# 262P-milestone3
In this part of the project we are adding a new function to change all the keys in the XML file and return Json Object.

Function we added to this part: 

```static JSONObject toJSONObject(Reader reader, YOURTYPEHERE keyTransformer)``` 
this function will take a reader and a function which we pass to (YOURTYPEHERE keyTransformer) in order 
to change all the keys in that returing Json Object. 

Time Efficacy:
Since we have done the same function outside the library on the client side in milestone 1, now we can compare the performance. 
We are testing both functionalities on 133 MB XML file.   
  - The performance implications of doing this inside the library is 8.067072971 seconds. 
  - The performance implications of doing it in client code is 10.635930817 seconds.

As you can see the implementation on library side is faster than client side. 
  

How to run the program: 
- Fork the project
- Go to src > main > java and run:
  * javac org/json/*.java
  * jar cf json-java.jar org/json/*.class
- Navigate to src > test > java > org.json.junit > XMLTest.java and run the JUNIT test
- Actual functionality is under src > main > java > org.json > XML.java

# 262P-milestone4
In this part we are adding streaming method to the library that allow the client code to chain operations on JSON nodes. This functionality 
will only works on top level keys and their values. 

Function we added to this part:

```public Stream<Entry<String, Object>> toStream()```:
This function returns a stream from top level of Json Object which we can apply other stream methods such as filter, forEach and collect.
This method will work for all types of Json Objects.

Some of the limitation of this method:
In this case if the user wants to use the inner Json Object, they have to use other methods from the library, pass the path and get the Json object. Then pass the returned Json Object to the stream method. This also applies to case where the user wants to iterate through the nodes of Json Object. 

JUNIT Test Cases:
- ```public void JSONObjectToStream()```
  - Testing a JSONObject.toStream() function to see if it returns the correct and expected type

- ```public void JSONObjectToStreamCompareTopLevelValue()```
  - Testing if using toStream() returns the correct top level values

- ```public void JSONObjectToStreamUsingFilter()```
  - Testing if using toStream(), can filter for a specific key in the top level and add a prefix 
  
- ``` public void JSONObjectToStreamForEach()```
  - Testing ForEach() method on the stream and add a prefix after using toStream()

- ```public void JSONObjectToStreamFromFileGetValue()```
  - Testing toStream() method on a json object that was created using a xml file

- ```public void JSONObjectToStreamGetValueForKey()```
  -  Testing if using toStream(), we can get a value for a specific key


How to run the program: 
- Fork the project
- Go to src > main > java and run:
  * javac org/json/*.java
  * jar cf json-java.jar org/json/*.class
- Navigate to src > test > java > org > json > junit > JSONObjectTest.java and run the JUNIT test
- Actual functionality is under src > main > java > org > json > JSONObject.java

# 262P-milestone5
In this part of project, we are focusing on understanding concurrent programming by providing asynchronous methods to application code. This method will allow the client code to proceed, while specifying what to do when the JSONObject becomes available. This is useful for when reading very large files.


Function we added to this part:

```public static Future<JSONObject> toFutureJSONObject(Reader reader)```
This function takes a reader, then inside the method, the lambda function is defined to convert the XML reader to JSONObject which will be used with a Future object. Finally, return a Future object.

JUNIT Test Cases:
- ```public void testingFuturePromiseForJSONObject()```
  - Testing if the promissed JSONObject is the same as expected object with small XML file
  
- ```public void testingFuturePromiseForJSONObjectUsingLargeFile()```
  - Testing if the promissed JSONObject is the same as expected object with large XML file

- ```public void testingFuturePromiseForJSONObjectWithTimeout()```
  - Testing a TimeoutException with null message. If JSONObject is not returned within the specified time, a TimeoutException is expected 

- ```public void testingFuturePromiseForJSONObjectCancelTask()```
  - Testing isCancelled() should return true, when canceling the task before it completes execution

- ```public void testingFuturePromiseForJSONObjecXMLString()```
  - Testing invalid XML which throws JSONException while promise throws ExecutionException


How to run the program: 
- Fork the project
- Go to src > main > java and run:
  * javac org/json/*.java
  * jar cf json-java.jar org/json/*.class
- Navigate to src > test > java > org > json > junit > XMLTest.java and run the JUNIT test
- Actual functionality is under src > main > java > org > json > XML.java


JSON in Java [package org.json]
===============================

[![Maven Central](https://img.shields.io/maven-central/v/org.json/json.svg)](https://mvnrepository.com/artifact/org.json/json)

**[Click here if you just want the latest release jar file.](https://repo1.maven.org/maven2/org/json/json/20201115/json-20201115.jar)**

# Overview

[JSON](http://www.JSON.org/) is a light-weight language-independent data interchange format.

The JSON-Java package is a reference implementation that demonstrates how to parse JSON documents into Java objects and how to generate new JSON documents from the Java classes.

Project goals include:
* Reliable and consistent results
* Adherence to the JSON specification 
* Easy to build, use, and include in other projects
* No external dependencies
* Fast execution and low memory footprint
* Maintain backward compatibility
* Designed and tested to use on Java versions 1.6 - 1.11

The files in this package implement JSON encoders and decoders. The package can also convert between JSON and XML, HTTP headers, Cookies, and CDL.

The license includes this restriction: ["The software shall be used for good, not evil."](https://en.wikipedia.org/wiki/Douglas_Crockford#%22Good,_not_Evil%22) If your conscience cannot live with that, then choose a different package.

**If you would like to contribute to this project**

Bug fixes, code improvements, and unit test coverage changes are welcome! Because this project is currently in the maintenance phase, the kinds of changes that can be accepted are limited. For more information, please read the [FAQ](https://github.com/stleary/JSON-java/wiki/FAQ).

# Build Instructions

The org.json package can be built from the command line, Maven, and Gradle. The unit tests can be executed from Maven, Gradle, or individually in an IDE e.g. Eclipse.
 
**Building from the command line**

*Build the class files from the package root directory src/main/java*
````
javac org\json\*.java
````

*Create the jar file in the current directory*
````
jar cf json-java.jar org/json/*.class
````

*Compile a program that uses the jar (see example code below)*
````
javac -cp .;json-java.jar Test.java 
````

*Test file contents*

````
import org.json.JSONObject;
public class Test {
    public static void main(String args[]){
       JSONObject jo = new JSONObject("{ \"abc\" : \"def\" }");
       System.out.println(jo.toString());
    }
}
````

*Execute the Test file*
```` 
java -cp .;json-java.jar Test
````

*Expected output*

````
{"abc":"def"}
````

 
**Tools to build the package and execute the unit tests**

Execute the test suite with Maven:
```
mvn clean test
```

Execute the test suite with Gradlew:

```
gradlew clean build test
```

# Notes

**Recent directory structure change**

_Due to a recent commit - [#515 Merge tests and pom and code](https://github.com/stleary/JSON-java/pull/515) - the structure of the project has changed from a flat directory containing all of the Java files to a directory structure that includes unit tests and several tools used to build the project jar and run the unit tests. If you have difficulty using the new structure, please open an issue so we can work through it._

**Implementation notes**

Numeric types in this package comply with
[ECMA-404: The JSON Data Interchange Format](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf) and
[RFC 8259: The JavaScript Object Notation (JSON) Data Interchange Format](https://tools.ietf.org/html/rfc8259#section-6).
This package fully supports `Integer`, `Long`, and `Double` Java types. Partial support
for `BigInteger` and `BigDecimal` values in `JSONObject` and `JSONArray` objects is provided
in the form of `get()`, `opt()`, and `put()` API methods.

Although 1.6 compatibility is currently supported, it is not a project goal and might be
removed in some future release.

In compliance with RFC8259 page 10 section 9, the parser is more lax with what is valid
JSON then the Generator. For Example, the tab character (U+0009) is allowed when reading
JSON Text strings, but when output by the Generator, the tab is properly converted to \t in
the string. Other instances may occur where reading invalid JSON text does not cause an
error to be generated. Malformed JSON Texts such as missing end " (quote) on strings or
invalid number formats (1.2e6.3) will cause errors as such documents can not be read
reliably.

Some notable exceptions that the JSON Parser in this library accepts are:
* Unquoted keys `{ key: "value" }`
* Unquoted values `{ "key": value }`
* Unescaped literals like "tab" in string values `{ "key": "value   with an unescaped tab" }`
* Numbers out of range for `Double` or `Long` are parsed as strings

Recent pull requests added a new method `putAll` on the JSONArray. The `putAll` method
works similarly to other `put` methods in that it does not call `JSONObject.wrap` for items
added. This can lead to inconsistent object representation in JSONArray structures.

For example, code like this will create a mixed JSONArray, some items wrapped, others
not:

```java
SomeBean[] myArr = new SomeBean[]{ new SomeBean(1), new SomeBean(2) };
// these will be wrapped
JSONArray jArr = new JSONArray(myArr);
// these will not be wrapped
jArr.putAll(new SomeBean[]{ new SomeBean(3), new SomeBean(4) });
```

For structure consistency, it would be recommended that the above code is changed
to look like 1 of 2 ways.

Option 1:
```Java
SomeBean[] myArr = new SomeBean[]{ new SomeBean(1), new SomeBean(2) };
JSONArray jArr = new JSONArray();
// these will not be wrapped
jArr.putAll(myArr);
// these will not be wrapped
jArr.putAll(new SomeBean[]{ new SomeBean(3), new SomeBean(4) });
// our jArr is now consistent.
```

Option 2:
```Java
SomeBean[] myArr = new SomeBean[]{ new SomeBean(1), new SomeBean(2) };
// these will be wrapped
JSONArray jArr = new JSONArray(myArr);
// these will be wrapped
jArr.putAll(new JSONArray(new SomeBean[]{ new SomeBean(3), new SomeBean(4) }));
// our jArr is now consistent.
```

**Unit Test Conventions**

Test filenames should consist of the name of the module being tested, with the suffix "Test". 
For example, <b>Cookie.java</b> is tested by <b>CookieTest.java</b>.

<b>The fundamental issues with JSON-Java testing are:</b><br>
* <b>JSONObjects</b> are unordered, making simple string comparison ineffective. 
* Comparisons via **equals()** is not currently supported. Neither <b>JSONArray</b> nor <b>JSONObject</b> override <b>hashCode()</b> or <b>equals()</b>, so comparison defaults to the <b>Object</b> equals(), which is not useful.
* Access to the <b>JSONArray</b> and <b>JSONObject</b> internal containers for comparison is not currently available.

<b>General issues with unit testing are:</b><br>
* Just writing tests to make coverage goals tends to result in poor tests. 
* Unit tests are a form of documentation - how a given method works is demonstrated by the test. So for a code reviewer or future developer looking at code a good test helps explain how a function is supposed to work according to the original author. This can be difficult if you are not the original developer.
*   It is difficult to evaluate unit tests in a vacuum. You also need to see the code being tested to understand if a test is good. 
* Without unit tests, it is hard to feel confident about the quality of the code, especially when fixing bugs or refactoring. Good tests prevent regressions and keep the intent of the code correct.
* If you have unit test results along with pull requests, the reviewer has an easier time understanding your code and determining if it works as intended.


# Files

**JSONObject.java**: The `JSONObject` can parse text from a `String` or a `JSONTokener`
to produce a map-like object. The object provides methods for manipulating its
contents, and for producing a JSON compliant object serialization.

**JSONArray.java**: The `JSONArray` can parse text from a String or a `JSONTokener`
to produce a vector-like object. The object provides methods for manipulating
its contents, and for producing a JSON compliant array serialization.

**JSONTokener.java**: The `JSONTokener` breaks a text into a sequence of individual
tokens. It can be constructed from a `String`, `Reader`, or `InputStream`. It also can 
parse text from a `String`, `Number`, `Boolean` or `null` like `"hello"`, `42`, `true`, 
`null` to produce a simple json object.

**JSONException.java**: The `JSONException` is the standard exception type thrown
by this package.

**JSONPointer.java**: Implementation of
[JSON Pointer (RFC 6901)](https://tools.ietf.org/html/rfc6901). Supports
JSON Pointers both in the form of string representation and URI fragment
representation.

**JSONPropertyIgnore.java**: Annotation class that can be used on Java Bean getter methods.
When used on a bean method that would normally be serialized into a `JSONObject`, it
overrides the getter-to-key-name logic and forces the property to be excluded from the
resulting `JSONObject`.

**JSONPropertyName.java**: Annotation class that can be used on Java Bean getter methods.
When used on a bean method that would normally be serialized into a `JSONObject`, it
overrides the getter-to-key-name logic and uses the value of the annotation. The Bean
processor will look through the class hierarchy. This means you can use the annotation on
a base class or interface and the value of the annotation will be used even if the getter
is overridden in a child class.   

**JSONString.java**: The `JSONString` interface requires a `toJSONString` method,
allowing an object to provide its own serialization.

**JSONStringer.java**: The `JSONStringer` provides a convenient facility for
building JSON strings.

**JSONWriter.java**: The `JSONWriter` provides a convenient facility for building
JSON text through a writer.


**CDL.java**: `CDL` provides support for converting between JSON and comma
delimited lists.

**Cookie.java**: `Cookie` provides support for converting between JSON and cookies.

**CookieList.java**: `CookieList` provides support for converting between JSON and
cookie lists.

**HTTP.java**: `HTTP` provides support for converting between JSON and HTTP headers.

**HTTPTokener.java**: `HTTPTokener` extends `JSONTokener` for parsing HTTP headers.

**XML.java**: `XML` provides support for converting between JSON and XML.

**JSONML.java**: `JSONML` provides support for converting between JSONML and XML.

**XMLTokener.java**: `XMLTokener` extends `JSONTokener` for parsing XML text.


# Release history:

JSON-java releases can be found by searching the Maven repository for groupId "org.json"
and artifactId "json". For example:
[https://search.maven.org/search?q=g:org.json%20AND%20a:json&core=gav](https://search.maven.org/search?q=g:org.json%20AND%20a:json&core=gav)

~~~
20201115    Recent commits and first release after project structure change

20200518    Recent commits and snapshot before project structure change

20190722    Recent commits

20180813    POM change to include Automatic-Module-Name (#431)

20180130    Recent commits

20171018    Checkpoint for recent commits.

20170516    Roll up recent commits.

20160810    Revert code that was breaking opt*() methods.

20160807    This release contains a bug in the JSONObject.opt*() and JSONArray.opt*() methods,
it is not recommended for use.
Java 1.6 compatability fixed, JSONArray.toList() and JSONObject.toMap(),
RFC4180 compatibility, JSONPointer, some exception fixes, optional XML type conversion.
Contains the latest code as of 7 Aug 2016

20160212    Java 1.6 compatibility, OSGi bundle. Contains the latest code as of 12 Feb 2016.

20151123    JSONObject and JSONArray initialization with generics. Contains the latest code as of 23 Nov 2015.

20150729    Checkpoint for Maven central repository release. Contains the latest code
as of 29 July 2015.
~~~
