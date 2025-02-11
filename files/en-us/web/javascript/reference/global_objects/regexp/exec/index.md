---
title: RegExp.prototype.exec()
slug: Web/JavaScript/Reference/Global_Objects/RegExp/exec
tags:
  - JavaScript
  - Method
  - Prototype
  - Reference
  - RegExp
  - Regular Expressions
browser-compat: javascript.builtins.RegExp.exec
---
{{JSRef}}

The **`exec()`** method executes a
search for a match in a specified string. Returns a result array, or
[`null`](/en-US/docs/Web/JavaScript/Reference/Operators/null).

JavaScript {{jsxref("RegExp")}} objects are **stateful** when they have
the {{jsxref("RegExp.global", "global")}} or {{jsxref("RegExp.sticky", "sticky")}} flags
set (e.g. `/foo/g` or `/foo/y`). They store a
{{jsxref("RegExp.lastIndex", "lastIndex")}} from the previous match. Using this
internally, `exec()` can be used to iterate over multiple matches in a string
of text (with capture groups), as opposed to getting just the matching strings with
{{jsxref("String.prototype.match()")}}.

A newer function has been proposed to simplify matching multiple parts of a string
(with capture groups): {{jsxref("String.prototype.matchAll()")}}.

If you are executing a match to find `true` or `false`, use
{{jsxref("RegExp.prototype.test()")}} method instead.

If you are executing a match to find its index position in the string, use
{{jsxref("String.prototype.search()")}} method instead.

{{EmbedInteractiveExample("pages/js/regexp-prototype-exec.html")}}

## Syntax

```js
exec(str)
```

### Parameters

- `str`
  - : The string against which to match the regular expression.

### Return value

If the match succeeds, the `exec()` method returns an array (with extra
properties `index`, `input`, and if the `d` flag is
set, `indices`; see below) and updates the
{{jsxref("RegExp.lastIndex", "lastIndex")}} property of the regular expression object.
The returned array has the matched text as the first item, and then one item for each
parenthetical capture group of the matched text.

If the match fails, the `exec()` method returns [`null`](/en-US/docs/Web/JavaScript/Reference/Operators/null), and sets
{{jsxref("RegExp.lastIndex", "lastIndex")}} to `0`.

## Description

Consider the following example:

```js
// Match "quick brown" followed by "jumps", ignoring characters in between
// Remember "brown" and "jumps"
// Ignore case
const re = /quick\s(brown).+?(jumps)/igd;
const result = re.exec('The Quick Brown Fox Jumps Over The Lazy Dog');
```

The following table shows the state of `result` after running this script:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Property/Index</th>
      <th scope="col">Description</th>
      <th scope="col">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>[0]</code></td>
      <td>The full string of characters matched</td>
      <td><code>"Quick Brown Fox Jumps"</code></td>
    </tr>
    <tr>
      <td><code>[1], …[<var>n</var>]</code></td>
      <td>
        <p>The parenthesized substring matches, if any.</p>
        <p>The number of possible parenthesized substrings is unlimited.</p>
      </td>
      <td>
        <p><code>result[1] === "Brown"</code></p>
        <p><code>result[2] === "Jumps"</code></p>
      </td>
    </tr>
    <tr>
      <td><code>index</code></td>
      <td>The 0-based index of the match in the string.</td>
      <td><code>4</code></td>
    </tr>
    <tr>
      <td><code>indices</code></td>
      <td>
        An array where each entry represents a substring match. Each substring
        match itself is an array where the first entry represents its start
        index and the second entry its end index.<br />The
        <code>indices</code> array additionally has a
        <code>groups</code> property which holds an object of all named
        capturing groups. The keys are the names of the capturing groups and
        each value is an array with the first item being the start entry and the
        second entry being the end index of the capturing group. If the regular
        expression doesn't contain any capturing groups, <code>groups</code> is
        <code>undefined</code>.
      </td>
      <td>
        <p><code>indices[0] === Array [ 4, 25 ]</code></p>
        <p><code>indices[1] === Array [ 10, 15 ]</code></p>
        <p><code>indices[2] === Array [ 20, 25 ]</code></p>
        <p><code>indices.groups === undefined</code></p>
        <p><code>indices.length === 3</code></p>
      </td>
    </tr>
    <tr>
      <td><code>input</code></td>
      <td>The original string that was matched against.</td>
      <td><code>The Quick Brown Fox Jumps Over The Lazy Dog</code></td>
    </tr>
  </tbody>
</table>

The following table shows the state of `re` after running this script:

<table class="standard-table">
  <thead>
    <tr>
      <th scope="col">Property/Index</th>
      <th scope="col">Description</th>
      <th scope="col">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>lastIndex</code></td>
      <td>
        <p>The index at which to start the next match.</p>
        <p>If <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>g</code></a> is absent, this will always be <code>0</code>.</p>
      </td>
      <td><code>25</code></td>
    </tr>
    <tr>
      <td><code>dotAll</code></td>
      <td>
        Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>s</code></a> flag was used to let
        <code>.</code> match newlines.
      </td>
      <td><code>false</code></td>
    </tr>
    <tr>
      <td><code>hasIndices</code></td>
      <td>
        Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>d</code></a> flag was used to generate an
        <code>indices</code> property in the returned value containing start and
        end indices of the substring matches.
      </td>
      <td><code>true</code></td>
    </tr>
    <tr>
      <td><code>ignoreCase</code></td>
      <td>Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>i</code></a> flag was used to ignore case.</td>
      <td><code>true</code></td>
    </tr>
    <tr>
      <td><code>global</code></td>
      <td>Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>g</code></a> flag was used for a global match.</td>
      <td><code>true</code></td>
    </tr>
    <tr>
      <td><code>multiline</code></td>
      <td>
        Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>m</code></a> flag was used to search across multiple
        lines.
      </td>
      <td><code>false</code></td>
    </tr>
    <tr>
      <td><code>source</code></td>
      <td>The text of the pattern.</td>
      <td><code>quick\s(brown).+?(jumps)</code></td>
    </tr>
    <tr>
      <td><code>sticky</code></td>
      <td>
        Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>y</code></a> flag was used to match only from the
        index indicated by the <code>lastIndex</code> property.
      </td>
      <td><code>false</code></td>
    </tr>
    <tr>
      <td><code>unicode</code></td>
      <td>
        Indicates if the <a href="/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags"><code>u</code></a> flag was used to treat the pattern as a
        sequence of Unicode code points.
      </td>
      <td><code>false</code></td>
    </tr>
  </tbody>
</table>

## Examples

### Finding successive matches

If your regular expression uses the [`g`](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags) flag, you can use the
`exec()` method multiple times to find successive matches in the same string.
When you do so, the search starts at the substring of `str`
specified by the regular expression's {{jsxref("RegExp.lastIndex", "lastIndex")}}
property ({{jsxref("RegExp.prototype.test()", "test()")}} will also advance the
{{jsxref("RegExp.lastIndex", "lastIndex")}} property). Note that the
{{jsxref("RegExp.lastIndex", "lastIndex")}} property will not be reset when searching a
different string, it will start its search at its existing {{jsxref("RegExp.lastIndex",
  "lastIndex")}}.

For example, assume you have this script:

```js
const myRe = /ab*/g;
const str = 'abbcdefabh';
let myArray;
while ((myArray = myRe.exec(str)) !== null) {
  let msg = `Found ${myArray[0]}. `;
  msg += `Next match starts at ${myRe.lastIndex}`;
  console.log(msg);
}
```

This script displays the following text:

```plain
Found abb. Next match starts at 3
Found ab. Next match starts at 9
```

> **Warning:** **Do _not_ place the regular expression
> literal (or {{jsxref("RegExp")}} constructor) within the `while`
> condition!**
>
> It will create an infinite loop if there is a match, due to the
> {{jsxref("RegExp.lastIndex", "lastIndex")}} property being reset upon each iteration.
>
> Also, be sure that the [global (`g`) flag](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions#advanced_searching_with_flags) is set, or it will also cause
> an infinite loop.
>
> In addition, when matching zero-length characters (e.g. `/^/gm`),
> increase its {{jsxref("RegExp.lastIndex", "lastIndex")}} each time to avoid
> an infinite loop.

### Using exec() with RegExp literals

You can also use `exec()` without creating a {{jsxref("RegExp")}} object
explicitly:

```js
const matches = /(hello \S+)/.exec('This is a hello world!');
console.log(matches[1]);
```

This will log a message containing `'hello world!'`.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- [Regular Expressions](/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) chapter in the
  [JavaScript Guide](/en-US/docs/Web/JavaScript/Guide)
- {{jsxref("RegExp")}}
