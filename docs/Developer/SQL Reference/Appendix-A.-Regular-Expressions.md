# Appendix A. Regular Expressions
=================

This section explains the regular expressions supported by Altibase.

### Regular Expression Support

Regular expressions are a syntax convention for writing text patterns and consist of one or more character strings and metacharacters. Altibase partly supports POSIX Basic Regular Expression (BRE) and Extended Regular Expression (ERE). Regular expressions supported by Altibase have the following limitations and features.

-   Multibyte characters are unsupported. 
-   Backreferences ( \digit) are unsupported. 
-   Lookaheads ( ?=)and lookbehinds ( ?<=) are unsupported. 
-   Conditional regular expressions (e.g., condition)B|C) are unsupported. 
-   The escape character is supported.

The following table describes character classes.

| Character class | Shorthand | Description                                                  |
| --------------- | --------- | ------------------------------------------------------------ |
| [:alnum:]       |           | Any alphanumeric character                                   |
| [:alpha:]       | \\a       | Any alphabetic character                                     |
| [:blank:]       |           | The space or tab character                                   |
| [:cntrl:]       | \\c       | Any non-printing ASCII control chracter (i.e. 127, 0~31)     |
| [:digit:]       | \\d       | Any numeric digit                                            |
| [:graph:]       |           | Any character equivalent to 'print', other than space        |
| [:lower:]       | \\l       | Any alphabet in lowercase                                    |
| [:print:]       |           | Any printing ASCII character (i.e. 32~126)                   |
| [:punct:]       | \\p       | Any punctuation character among ASCII printing characters (32~126), other than a space, numeric digit and alphabetic character. |
| [:space:]       | \\s       | Any non-printing space character (e.g., a space, carriage return, newline, vertical tab, form feed, etc.) |
| [:upper:]       | \\u       | Any alphabetic character in uppercase                        |
| [:word:]        | \\w       | The alphabetic character, numeric digit and underscore "_"   |
| [:xdigit:]      | \\x       | Any hexadecimal digit, 0-9, a-f, A-F                         |
|                 | \\A       | Any character, other than \a                                 |
|                 | \\W       | Any character, other than \w                                 |
|                 | \\S       | Any character, other than \s                                 |
|                 | \\D       | Any character, other than \d                                 |
|                 | \\X       | Any character, other than \x                                 |
|                 | \\C       | Any character, other than \c                                 |
|                 | \\P       | Any character, other than \p                                 |
|                 | \\b       | The word border                                              |
|                 | \\B       | Any character, other than \b                                 |

The following table describes metacharacters that can be used for regular expressions in Altibase, and their meanings.

<table>
<tbody>
<tr>
<th>
<p>Metacharacter</p>
</th>
<th>
<p>Description</p>
</th>
</tr>
<tr>
<td>
<p>.</p>
</td>
<td>
<p>Matches a single character, other than the newline. The punctuation character(.) of a regular expression enclosed in square brackets matches the literal
dot. For example, a.c matches “abc”, but [a.c] matches only “a”, “.”, or “c”. </p>
</td>
</tr>
<tr>
<td>
<p>[]</p>
</td>
<td>
<p>A character class expression. Matches a single character enclosed in square
brackets. For example, [abc] matches “a”, “b”, or “c”; [a-z] matches any alphabetic character in lowercase, from “a” to “z”. The format can also be
mixed: both [a-cx-z] and [abcx-z] match “a”, “b”, “c”, “x”, “y”, or “z”.
If the right square bracket (]) is the initial character to follow a circumflex (^),
it can be included in the expression enclosed in square brackets: []abc].</p>
<p>] If the circumflex (^) is the initial character enclosed in square brackets ([]), it
matches any character other than those enclosed in the square brackets ([]). For
example, [^abc]d matches “ed”, “fd”, but not “ad”, “bd” and “cd”. [^a-z]
matches any character that does not start with an alphabetic character in lowercase.</p>
</td>
</tr>
<tr>
<td>
<p>^</p>
</td>
<td>
<p>Matches the beginning character of a string.</p>
</td>
</tr>
<tr>
<td>
<p>$</p>
</td>
<td>
<p>Matches the last character of a string or the preceding character of the last
newline of a string.</p>
</td>
</tr>
<tr>
<td>
<p>*</p>
</td>
<td>
<p>Matches the preceding element for 0 or more times. For example, ab*c matches "ac", "abc", "abbbc", etc.; [xyz]* matches "", "x", "y", "z", "zx", "zyx",
"xyzzy", etc.; (ab)* matches "", "ab", "abab", "ababab", etc.</p>
</td>
</tr>
<tr>
<td>
<p>+</p>
</td>
<td>
<p>Matches the preceding character for 1 or more times.</p>
</td>
</tr>
<tr>
<td>
<p>?</p>
</td>
<td>
<p>Matches the preceding character for 0 or 1 time.</p>
</td>
</tr>
<tr>
<td>
<p>{m,n}</p>
</td>
<td>
<p>Matches the preceding element for a minimum of m, and a maximum of n
times. For example a{3,5} matches "aaa", "aaaa", and "aaaaa".</p>
</td>
</tr>
<tr>
<td>
<p>{m}</p>
</td>
<td>
<p>Matches the preceding element for m times.</p>
</td>
</tr>
<tr>
<td>
<p>{m,}</p>
</td>
<td>
<p>Matches the preceding element for m or more times.</p>
</td>
</tr>
<tr>
<td>
<p>|</p>
</td>
<td>
<p>Matches a single expression among multiple expressions.</p>
</td>
</tr>
<tr>
<td>
<p>()</p>
</td>
<td>
<p>Matches a subexpression. Multiple expressions can be grouped as a single
complex regular expression.</p>
</td>
</tr>
</tbody>
</table>
