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

## 2.1 Java rules

You can found rules for Java code from Google [here](https://source.android.com/source/code-style). Here's a resume.

### 2.1.1 Don't Ignore Exceptions

It can be tempting to write code that completely ignores an exception, such as:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

Do not do this. While you may think your code will never encounter this error condition or that it is not important to handle it, ignoring exceptions as above creates mines in your code for someone else to trigger some day. You must handle every Exception in your code in a principled way; the specific handling varies depending on the case.

### 2.1.2 Don't Catch Generic Exception

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

### 2.1.3 Don't Use Finalizers

Finalizers are a way to have a chunk of code executed when an object is garbage collected. While they can be handy for doing cleanup (particularly of external resources), there are no guarantees as to when a finalizer will be called (or even that it will be called at all).

### 2.1.4 Fully Qualify Imports

Do not use `import foo.*;`. Use `import foo.Bar;`.

### 2.1.5 Use Javadoc Standard Comments

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

Examples:

```java
/** Returns the correctly rounded positive square root of a double value. */
static double sqrt(double a) {
    ...
}
```

or

```java
/**
 * Constructs a new String by converting the specified array of
 * bytes using the platform's default character encoding.
 */
public String(byte[] bytes) {
    ...
}
```

You do not need to write Javadoc for trivial get and set methods such as `setFoo()` if all your Javadoc would say is "sets Foo". If the method does something more complex (such as enforcing a constraint or has an important side effect), then you must document it. If it's not obvious what the property "Foo" means, you should document it.

Every method you write, public or otherwise, would benefit from Javadoc. Public methods are part of an API and therefore require Javadoc. Android does not currently enforce a specific style for writing Javadoc comments, but you should follow the instructions [How to Write Doc Comments for the Javadoc Tool](http://www.oracle.com/technetwork/java/javase/documentation/index-137868.html).

### 2.1.6 Write Short Methods

When feasible, keep methods small and focused. We recognize that long methods are sometimes appropriate, so no hard limit is placed on method length.

### 2.1.7 Define Fields in Standard Places

Define fields either at the top of the file or immediately before the methods that use them.

### 2.1.8 Limit Variable Scope

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

### 2.1.9 Order Import Statements

The ordering of import statements is:

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax

To exactly match the IDE settings, the imports should be:

* Alphabetical within each grouping, with capital letters before lower case letters (e.g. Z before a).
* Separated by a blank line between each major grouping (android, com, junit, net, org, java, javax).

### 2.1.10 Use Spaces for Indentation

We use four (4) space indents for blocks and never tabs. When in doubt, be consistent with the surrounding code.

We use eight (8) space indents for line wraps, including function calls and assignments.

### 2.1.11 Follow Field Naming Conventions

* Non-public, non-static field names start with m.
* Static field names start with s.
* Other fields start with a lower case letter.
* Public static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.

### 2.1.12 Use Standard Brace Style

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

### 2.1.13 Limit Line Length

Each line of text in your code should be at most 100 characters long. While much discussion has surrounded this rule, the decision remains that 100 characters is the maximum with the following exceptions:

* URL and example command with more than 100 caracters
* Import statements

### 2.1.14 Use Standard Java Annotations

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

### 2.1.14 Treat Acronyms as Words

Treat acronyms and abbreviations as words in naming variables, methods, and classes to make names more readable:

| Good            | Bad             |
|-----------------|-----------------|
|XmlHttpRequest   |XMLHTTPRequest   |
|getCustomerId    |getCustomerID    |
|class Html       |class HTML       |

### 2.1.15 Use TODO Comments

Use TODO comments for code that is temporary, a short-term solution, or good-enough but not perfect. TODOs should include the string TODO in all caps, followed by a colon:

```java
// TODO: Remove this code after the UrlTable2 has been checked in.
```

### 2.1.16 Log Sparingly

While logging is necessary, it has a significantly negative impact on performance and quickly loses its usefulness if not kept reasonably terse.

### 2.1.17 Be Consistent

Our parting thought: BE CONSISTENT. If you're editing code, take a few minutes to look at the surrounding code and determine its style. If that code uses spaces around the if clauses, you should too. If the code comments have little boxes of stars around them, make your comments have little boxes of stars around them too.

## 2.2 Java guidelines

### 2.2.1 Class member ordering

There is no single correct solution for this but using a logical and consistent order will improve code learnability and readability. It is recommendable to use the following order:

1. Constants
2. Fields
3. Constructors
4. Override methods and callbacks (public or private)
5. Static methods
6. Public methods
7. Private methods
8. Inner classes or interfaces

__IMPORTANT:__ Variables like `OnClickListener` are considered like methods.

If your class is extending an __Android component__ such as an Activity or a Fragment, it is a good practice to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.2 Parameter ordering in methods

When programming for Android, it is quite common to define methods that take a `Context`. If you are writing a method like this, then the __Context__ must be the __first__ parameter.

The opposite case are __callback__ interfaces that should always be the __last__ parameter.

### 2.2.3 String constants, naming, and values

Many elements of the Android SDK such as `SharedPreferences`, `Bundle`, or `Intent` use a key-value pair approach so it's very likely that even for a small app you end up having to write a lot of String constants.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicated below.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

Note that the arguments of a Fragment - `Fragment.getArguments()` - are also a Bundle. However, because this is a quite common use of Bundles, we define a different prefix for them.

Example:

```java
public static final String BUNDLE_NAME = "BUNDLE_NAME";
private static final String ARGUMENT_USER = "ARGUMENT_USER";
```

### 2.2.4 Activities and Fragments

For activities and fragments, a `public static` method that facilitates the creation of the relevant `Intent` or `Fragment` should be created.

For activites, create a `getStartIntent()` method:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

For fragments, a variable `public static final String TAG` should be created and also the creation of a `newInstance()` method:

```java
public static final String TAG = "UserFragment";

public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment();
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args);
	return fragment;
}
```

__IMPORTANT:__ These methods should go at the top of the class before `onCreate()`. 

### 2.2.5 Line-wrapping strategies

In harmony with the 2.1.13 (Limit Line Length), here's strategies for line-wrapping.

There isn't an exact formula that explains how to line-wrap and quite often different solutions are valid. However there are a few rules that can be applied to common cases.

__Break at operators__

When the line is broken at an operator, the break comes __before__ the operator. For example:

```java
int longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne;
boolean boolVar = firstCondition || secondCondition
	&& third condition;
```

__Assignment Operator Exception__

An exception to the `break at operators` rule is the assignment operator `=`, where the line break should happen __after__ the operator.

```java
int longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne;
```

__Method chain case__

When multiple methods are chained in the same line - for example when using Builders - every call to a method should go in its own line, breaking the line before the `.`

```java
Picasso.with(context).load("http://google.co.uk/images/image.jpg").into(imageView);
```

```java
Picasso.with(context)
        .load("http://google.co.uk/images/image.jpg")
        .into(imageView);
```

__Long parameters case__

When a method has many parameters or its parameters are very long, we should break the line after every comma `,`

```java
loadPicture(context, "http://google.co.uk/images/image.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
loadPicture(context,
        "http://google.co.uk/images/image.jpg",
        mImageViewProfilePicture,
        clickListener,
        "Title of the picture");
```

### 2.2.6 Widgets variables naming

For widgets variable, if the widget is only used one time and is uncommon (like `Toolbar`), name the variable like the widget (do not forget to respect 2.1 Java Rules). Else, a good practice is to name the variable with the abbreviation of the widget. Examples:

| Widget            | Prefix            | Example           |
|-------------------|-------------------|-------------------|
| `TextView`        | `tv`              | `tvName`          |
| `ImageView`       | `iv`              | `ivProfile`       |
| `Button`          | `bt`              | `btNext`          |
| `ListView`        | `lv`              | `lvParticipants`  |
| `LinearLayout`    | `ll`              | `llBottomButtons` |

__Exception:__ There is an exception for widgets that have the same effect that another more common widget. For example, `ImageButton` has the same effect that a `Button` so it's prefix is `bt`.

## 2.3 XML guidelines

### 2.3.1 Use self closing tags

When an XML element doesn't have any contents, you __must__ use self closing tags.

This is good:

```xml
<TextView
	android:id="@+id/tv_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/tv_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```

### 2.3.2 Resources naming

Resource IDs and names are written in lower __snake_case__.

#### 2.3.2.1 ID naming

For IDs, if the widget is only used one time and is uncommon (like `Toolbar`), name the ID like the widget. Else, IDs should be prefixed with an abbreviation of the element. For example:

| Element            | Prefix            |
| -----------------  | ----------------- |
| `TextView`         | `tv_`             |
| `ImageView`        | `iv_`             |
| `Button`           | `bt_`             |
| `ListView`         | `lv_`             |
| `LinearLayout`     | `ll_`             |

__Exceptions:__ There is an exception for widgets that have the same effect that another more common widget. For example, `ImageButton` has the same effect that a `Button` so it's prefix is `bt_`. Also, for `<item/>` in `<menu/>`, the prefix is `action_`.

#### 2.3.2.2 Strings, colors and dimens

String names start with a prefix that identifies the section they belong to and should not contain abbreviations. For example `registration_email_hint` or `registration_name_hint`. If a string __doesn't belong__ to any section, then you should follow the rules below:

| Prefix               | Description                           |
| ---------------------| --------------------------------------|
| `error_`             | An error message                      |
| `message_`           | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| none                 | An action such as "Save" or "Create"  |

Moreover, sections need to be separate by a newline (example below).

For colors and dimens, it is best to have a general element used in multiple elements rather than redefining each time the element.

This is good :

```xml
<color name="primary_button_text">#ffffff</color>

<color name="log_in_button_next_text">@color/primary_button_text</color>

<color name="participant_button_remove_text">@color/primary_button_text</color>
```

This is __bad__:

```xml
<color name="log_in_button_next_text">#ffffff</color>

<color name="participant_button_remove_text">#ffffff</color>
```

#### 2.3.2.3 Styles and Themes

Unless the rest of resources, style names are written in __UpperCamelCase__.

### 2.3.3 Attributes ordering

As a general rule you should try to group similar attributes together. A good way of ordering the most common attributes is:

1. xmlns attributes
2. View Id
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically (except style)
6. Style

### 2.3.4 XML values

For all values that can be put in a value file, it should be done. For example:

`example.xml`
```xml
<Button android:id="@+id/bt_next"
    android:layout_width="@dimen/log_in_button_next_width"
    android:layout_height="@dimen/log_in_button_next_height"
    android:text="@string/next"
    android:textColor="@color/log_in_button_next_text"
    android:textSize="@dimen/log_in_button_next_text" />
```

`colors.xml`
```xml
<color name="log_in_button_next_text">#ffffff</color>
```

`dimens.xml`
```xml
<dimen name="log_in_button_next_width">100dp</dimen>
<dimen name="log_in_button_next_height">42dp</dimen>
<dimen name="log_in_button_next_text">24sp</dimen>
```

`strings.xml`
```xml
<string name="next">Next</string>
```

### 2.3.5 `tools` attibutes

For every layout, if applicable, you should add `tools` attributes for a better visualization and understanding of the layout. For example :

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
    
    <TextView android:id="@+id/tv_participant_name"
    	android:layout_width="match_parent"
	android:layout_height="wrap_content"
	tools:text="Richard" />
    
</LinearLayout>
```

__IMPORTANT:__ `tools` attributes values should __not be created__ in values files __if only__ for `tools` attributes. 

### 2.3.6 `style`

Do not forget that you can modify the main style of your activity.

Create other styles only if necessary.

## 2.4 Unit tests guidelines

Test classes should match the name of the class the tests are targeting, followed by `Test`. For example, if we create a test class that contains tests for the `DatabaseHelper`, we should name it `DatabaseHelperTest`.

Test methods are annotated with `@Test` and should generally start with the name of the method that is being tested, followed by a precondition and/or expected behaviour.

* Template: `@Test void methodNamePreconditionExpectedBehaviour()`
* Example: `@Test void signInWithEmptyEmailFails()`

Precondition and/or expected behaviour may not always be required if the test is clear enough without them.

Sometimes a class may contain a large amount of methods, that at the same time require several tests for each method. In this case, it's recommendable to split up the test class into multiple ones. For example, if the `DataManager` contains a lot of methods we may want to divide it into `DataManagerSignInTest`, `DataManagerLoadUsersTest`, etc. Generally you will be able to see what tests belong together because they have common [test fixtures](https://en.wikipedia.org/wiki/Test_fixture).
