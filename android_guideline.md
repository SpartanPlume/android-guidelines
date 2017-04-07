# 1. Project guidelines

## 1.1 Project structure

New projects should follow the Android Gradle project structure that is defined on the [Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure).

Java files need to be create in an adequate package. For example, `LogInFragment` in `fragments` package.

## 1.2 File naming

### 1.2.1 Class files
Class names are written in [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase).

For classes that extend an Android component, the name of the class should end with the name of the component. For example: `SignInActivity`, `SignInFragment`, `ParticipantsListAdapter`, `ChangePasswordDialog`.

### 1.2.2 Resources files

Resources file names are written in lower [snake_case](https://en.wikipedia.org/wiki/Snake_case).

#### 1.2.2.1 Drawable files

Naming conventions for drawables:

| Asset Type   | Prefix            |		Example                   |
|--------------| ------------------|------------------------------|
| Selector     | `selector_`       | `selector_primary_button.xml`|
| Icon         | `ic_`	           | `ic_menu_white_24dp.png`     |

Naming conventions for selector states:

| State	       | Suffix          | Example                        |
|--------------|-----------------|--------------------------------|
| Normal       | `_normal`       | `primary_button_normal.xml`    |
| Pressed      | `_pressed`      | `primary_button_pressed.xml`   |
| Focused      | `_focused`      | `primary_button_focused.xml`   |
| Disabled     | `_disabled`     | `primary_button_disabled.xml`  |
| Selected     | `_selected`     | `primary_button_selected.xml`  |

For selectors, the suffix should be a global name like the above example. This suffix is used to create the differents state of the selector. For example, `primary_button_normal.xml`.

#### 1.2.2.2 Layout files

Layout files should match the name of the Android components that they are intended for but moving the top level component name to the beginning. For example, if we are creating a layout for the `SignInActivity`, the name of the layout file should be `activity_sign_in.xml`.

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `MainActivity`         | `activity_main.xml`           |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |

A slightly different case is when we are creating a layout that is going to be inflated by an `Adapter`, e.g to populate a `ListView`. In this case, the name of the layout should start with `item_`.

Note that there are cases where these rules will not be possible to apply. For example, when creating layout files that are intended to be part of other layouts. In this case you should use the prefix `partial_`.

#### 1.2.2.3 Menu files

Similar to layout files, menu files should match the name of the component. For example, if we are defining a menu file that is going to be used in the `UserActivity`, then the name of the file should be `activity_user.xml`

A good practice is to not include the word `menu` as part of the name because these files are already located in the `menu` directory.

#### 1.2.2.4 Values files

Resource files in the values folder should be __plural__, e.g. `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`

# 2. Code guidelines

## 2.1 Java guidelines

### 2.1.1 Google rules

You can found rules for Java code from Google [here](https://source.android.com/source/code-style). Here's a resume.

#### 2.1.1.1 Don't Ignore Exceptions

It can be tempting to write code that completely ignores an exception, such as:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

Do not do this. While you may think your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions as above creates mines in your code for someone else to trigger some day. You must handle every Exception in your code in a principled way; the specific handling varies depending on the case.

#### 2.1.1.2 Don't Catch Generic Exception

It can also be tempting to be lazy when catching exceptions and do something like this:

```java
try {
    someComplicatedIOFunction();        // may throw IOException
    someComplicatedParsingFunction();   // may throw ParsingException
    someComplicatedSecurityFunction();  // may throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // I'll just catch all exceptions
    handleError();                      // with one generic handler!
}
```

Do not do this. In almost all cases it is inappropriate to catch generic Exception or Throwable (preferably not Throwable because it includes Error exceptions). It is very dangerous because it means that Exceptions you never expected (including RuntimeExceptions like ClassCastException) get caught in application-level error handling.

#### 2.1.1.3 Don't Use Finalizers

Finalizers are a way to have a chunk of code executed when an object is garbage collected. While they can be handy for doing cleanup (particularly of external resources), there are no guarantees as to when a finalizer will be called (or even that it will be called at all).

#### 2.1.1.4 Fully Qualify Imports

Do not use `import foo.*;`. Use `import foo.Bar;`.

#### 2.1.1.5 Use Javadoc Standard Comments

Every file should have a copyright statement at the top, followed by package and import statements (each block separated by a blank line) and finally the class or interface declaration. In the Javadoc comments, describe what the class or interface does.

```java
/*
 * Copyright (C) 2015 The Android Open Source Project
 *
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 *
 *      http://www.apache.org/licenses/LICENSE-2.0
 *
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */

package com.android.internal.foo;

import android.os.Blah;
import android.view.Yada;

import java.sql.ResultSet;
import java.sql.SQLException;

/**
 * Does X and Y and provides an abstraction for Z.
 */

public class Foo {
    ...
}
```

Every class and nontrivial public method you write must contain a Javadoc comment with at least one sentence describing what the class or method does. This sentence should start with a third person descriptive verb.

#### 2.1.1.6 Write Short Methods

When feasible, keep methods small and focused. We recognize that long methods are sometimes appropriate, so no hard limit is placed on method length.

#### 2.1.1.7 Define Fields in Standard Places

Define fields either at the top of the file or immediately before the methods that use them.

#### 2.1.1.8 Limit Variable Scope

Keep the scope of local variables to a minimum. By doing so, you increase the readability and maintainability of your code and reduce the likelihood of error. Each variable should be declared in the innermost block that encloses all uses of the variable.

Local variables should be declared at the point they are first used. Nearly every local variable declaration should contain an initializer. If you don't yet have enough information to initialize a variable sensibly, postpone the declaration until you do.

For variables initialized in try-catch statements, prefer doing so :

```java
Set createSet(Class cl) {
    // Instantiate class cl, which represents some sort of Set
    try {
        return (Set) cl.newInstance();
    } catch(IllegalAccessException e) {
        throw new IllegalArgumentException(cl + " not accessible");
    } catch(InstantiationException e) {
        throw new IllegalArgumentException(cl + " not instantiable");
    }
}

...

// Exercise the set
Set s = createSet(cl);
s.addAll(Arrays.asList(args));
```

#### 2.1.1.9 Order Import Statements

The ordering of import statements is:

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax

To exactly match the IDE settings, the imports should be:

* Alphabetical within each grouping, with capital letters before lower case letters (e.g. Z before a).
* Separated by a blank line between each major grouping (android, com, junit, net, org, java, javax).

#### 2.1.1.10 Use Spaces for Indentation

We use four (4) space indents for blocks and never tabs. When in doubt, be consistent with the surrounding code.

We use eight (8) space indents for line wraps, including function calls and assignments.

#### 2.1.1.11 Follow Field Naming Conventions

* Non-public, non-static field names start with m.
* Static field names start with s.
* Other fields start with a lower case letter.
* Public static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.

#### 2.1.1.12 Use Standard Brace Style

Braces do not go on their own line; they go on the same line as the code before them.

We require braces around the statements for a conditional. Exception: If the entire conditional (the condition and the body) fit on one line, you may (but are not obligated to) put it all on one line. For example, this is acceptable:

```java
if (condition) {
    body();
}
```

and this is acceptable:

```java
if (condition) body();
```

but this is not acceptable:

```java 
if (condition)
    body();  // bad!
```

#### 2.1.1.13 Limit Line Length

Each line of text in your code should be at most 100 characters long. While much discussion has surrounded this rule, the decision remains that 100 characters is the maximum with the following exceptions:

* URL and example command with more than 100 caracters
* Import statements

#### 2.1.1.14 Use Standard Java Annotations

Annotations should precede other modifiers for the same language element. Simple marker annotations (e.g. @Override) can be listed on the same line with the language element. If there are multiple annotations, or parameterized annotations, they should each be listed one-per-line in alphabetical order.

Android standard practices for the three predefined annotations in Java are:

* `@Deprecated`: The @Deprecated annotation must be used whenever the use of the annotated element is discouraged. If you use the @Deprecated annotation, you must also have a @deprecated Javadoc tag and it should name an alternate implementation. In addition, remember that a @Deprecated method is still supposed to work. If you see old code that has a @deprecated Javadoc tag, please add the @Deprecated annotation.
* `@Override`: The @Override annotation must be used whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.
* `@SuppressWarnings`: The @SuppressWarnings annotation should be used only under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the @SuppressWarnings annotation must be used, so as to ensure that all warnings reflect actual problems in the code.
   
   When a @SuppressWarnings annotation is necessary, it must be prefixed with a TODO comment that explains the "impossible to eliminate" condition. This will normally identify an offending class that has an awkward interface. For example:
   
   ```java
   // TODO: The third-party class com.third.useful.Utility.rotate() needs generics
   @SuppressWarnings("generic-cast")
   List<String> blix = Utility.rotate(blax);
   ```
   
   When a @SuppressWarnings annotation is required, the code should be refactored to isolate the software elements where the annotation applies.

#### 2.1.1.14 Treat Acronyms as Words

Treat acronyms and abbreviations as words in naming variables, methods, and classes to make names more readable:

| Good            | Bad             |
|-----------------|-----------------|
|XmlHttpRequest   |XMLHTTPRequest   |
|getCustomerId    |getCustomerID    |
|class Html       |class HTML       |

#### 2.1.1.15 Use TODO Comments

Use TODO comments for code that is temporary, a short-term solution, or good-enough but not perfect. TODOs should include the string TODO in all caps, followed by a colon:

```java
// TODO: Remove this code after the UrlTable2 has been checked in.
```

#### 2.1.1.16 Log Sparingly

While logging is necessary, it has a significantly negative impact on performance and quickly loses its usefulness if not kept reasonably terse.

#### 2.1.1.17 Be Consistent

Our parting thought: BE CONSISTENT. If you're editing code, take a few minutes to look at the surrounding code and determine its style. If that code uses spaces around the if clauses, you should too. If the code comments have little boxes of stars around them, make your comments have little boxes of stars around them too.

