# Coding Style Guide

This guide extends and expands on [GMSR-0], the basic coding standard.

The intent of this guide is to reduce cognitive friction when scanning code
from different authors. It does so by enumerating a shared set of rules and
expectations about how to format GML code.

The style rules herein should be derived from commonalities among the various member
projects - currently its just me. When various authors collaborate across multiple projects, 
it helps to have one set of guidelines to be used among all those projects. Thus, the
benefit of this guide is not in the rules themselves, but in the sharing of
those rules.

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
interpreted as described in [RFC 2119].

[RFC 2119]: http://www.ietf.org/rfc/rfc2119.txt
[PSR-0]: https://github.com/RobQuistNL/GameMakerStandards/blob/master/accepted/GMSR-0-basic-coding-standard.md


## 1. Overview

- Code MUST follow the "Basic Coding Standard" GMSR [[GMSR-0]].

- Code MUST use 4 spaces for indenting, not tabs.

- There MUST NOT be a hard limit on line length; the soft limit MUST be 120
  characters; lines SHOULD be 80 characters or less.

- Control structure keywords MUST have one space after them; method and
  function calls MUST NOT.

- Opening braces for control structures MUST go on the same line, and closing
  braces MUST go on the next line after the body.

- Opening parentheses for control structures MUST NOT have a space after them,
  and closing parentheses for control structures MUST NOT have a space before.

### 1.1. Example

This example encompasses some of the rules below as a quick overview:

**obj_player**:*Step event*
~~~gml
var nearestPickup = instance_nearest(x, y, obj_pickup);

if (nearestPickup != noone && keyboard_check_pressed(ord("Q"))) {
    if (point_distance(x, y, nearestPickup.x, nearestPickup.y) < 48) {
        pickupItem(nearestPickup);
    } else {
        audio_play_sound(snd_miss_pickup, 1, false);
    }
}
~~~

## 2. General

### 2.1. Lines

There MUST NOT be a hard limit on line length.

The soft limit on line length MUST be 120 characters; automated style checkers
MUST warn but MUST NOT error at the soft limit.

Lines SHOULD NOT be longer than 80 characters; lines longer than that SHOULD
be split into multiple subsequent lines of no more than 80 characters each.

There MUST NOT be trailing whitespace at the end of non-blank lines.

Blank lines MAY be added to improve readability and to indicate related
blocks of code.

There MUST NOT be more than one subsequent blank lines. 

There MUST NOT be more than one statement per line.

### 2.2. Indenting

Code MUST use an indent of 4 spaces, and MUST NOT use tabs for indenting.

> N.b.: Using only spaces, and not mixing spaces with tabs, helps to avoid
> problems with diffs, patches, history, and annotations. The use of spaces
> also makes it easy to insert fine-grained sub-indentation for inter-line
> alignment.

### 2.3. Keywords

GML [keywords] MUST be in lower case.

The GML constants `true`, `false`, `undefined`, `noone`, `other`, `all`  MUST be in lower case.

The GML constant `self` MUST NOT be used. Use `id` instead.

[keywords]: https://docs2.yoyogames.com/source/_build/3_scripting/3_gml_overview/5_keywords.html

## 3. Objects and Properties

The term "objects" refers to object type assets.

### 3.1. Properties

Visibility MUST be declared on all properties.

There MUST NOT be more than one property declared per statement.

Property names SHOULD be prefixed with a single underscore to indicate
protected or private visibility.

A property declaration looks like the following.

**obj_controller**:*Create event*
~~~gml
showGui = true;
debugMode = true;
_debugLines = ds_list_create();
~~~

### 4. Scripts

Internal scripts MUST be prefixed with an underscore.

Scripts SHOULD contain a description in a JSDoc Style comment block at the first line.

If the description spans over multiple lines, all lines apart from the first MUST be indented with 4 spaces.

Scripts MUST contain the parameters in a JSDoc Style comment block, at the first line, or after the description.

Optional parameters MUST be enclosed in `[]`.

There MUST be a blank line under the JSDoc block.

A proper script looks like the following. Note the placement of parentheses, commas, spaces, braces, comments:

**is_same**
~~~gml
/// @description Compare an instance object index with that of another.
    /// Strict checking will make sure they are the same instance.
/// @param {real} a The instance to match
/// @param {real} b The second instance to match
/// @param {bool} [strict] Make sure its the same instance instead of only type

var _a = arugment0;
var _b = arugment1;
var _strict = arugment2;

if (!instance_exists(_a) || !instance_exists(_b)) {
    return false;
}

if (_a.object_index == _b.object_index) {
    if (_strict && (_a.id != _b.id)) {
        return false;
    }
    return true;
}
return false;
~~~

## 5. Control Structures

The general style rules for control structures are as follows:

- There MUST be one space after the control structure keyword
- There MUST NOT be a space after the opening parenthesis
- There MUST NOT be a space before the closing parenthesis
- There MUST be one space between the closing parenthesis and the opening
  brace
- The structure body MUST be indented once
- The closing brace MUST be on the next line after the body

The body of each structure MUST be enclosed by braces. This standardizes how
the structures look, and reduces the likelihood of introducing errors as new
lines get added to the body.


### 5.1. `if`, `elseif`, `else`

An `if` structure looks like the following. Note the placement of parentheses,
spaces, and braces; and that `else` and `elseif` are on the same line as the
closing brace from the earlier body.

~~~gml
if (expression1) {
    // if body
} else if (expression2) {
    // elseif body
} else {
    // else body;
}
~~~

### 5.2. `switch`, `case`

A `switch` structure looks like the following. Note the placement of
parentheses, spaces, and braces. The `case` statement MUST be indented once
from `switch`, and the `break` keyword (or other terminating keyword) MUST be
indented at the same level as the `case` body. There MUST be a comment such as
`// no break` when fall-through is intentional in a non-empty `case` body.

~~~gml
switch (expression) {
    case 0:
        draw_text(0, 0, "First case, with a break");
        break;
    case 1:
        draw_text(0, 0, "Second case, which falls through");
        // no break
    case 2:
    case 3:
    case 4:
        draw_text(0, 0, "Third case, return instead of break");
        return;
    default:
        draw_text(0, 0, "Default case");
        break;
}
~~~


### 5.3. `while`, `do while`

A `while` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

~~~gml
while (expression) {
    // structure body
}
~~~

Similarly, a `do while` statement looks like the following. Note the placement
of parentheses, spaces, and braces.

~~~gml
do {
    // structure body;
} while (expression);
~~~

### 5.4. `for`

A `for` statement looks like the following. Note the placement of parentheses,
spaces, and braces.

~~~gml
for (var _i = 0; _i < 10; _i++) {
    // for body
}
~~~

### 5.5. `repeat`

A `repeat` statement looks like the following. Note the placement of
parentheses, spaces, and braces.

~~~gml
repeat (10) {
    // repeat body
}
~~~