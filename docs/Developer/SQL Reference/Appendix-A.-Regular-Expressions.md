# Appendix A. Regular Expressions

This section explains the regular expressions supported by Altibase.

### Regular Expression Support

Regular expressions are a syntax convention for writing text patterns and consist of one or more character strings and metacharacters. Altibase supports Altibase Regular Expression Library that supports POSIX Basic Regular Expression (BRE) and Extended Regular Expression (ERE) partially, and Perl Compatible Regular Expressions (PCRE2) library. The library to be used for the regular expressions can be chosen by the user between the two.

#### Altibase Regular Expression Library

Regular expressions supported by Altibase Regular Expression Library have the following limitations and features.

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
| [:blank:]       |           | Space or tab character                                       |
| [:cntrl:]       | \\c       | Any non-printing ASCII control chracter (i.e. 127, 0~31)     |
| [:digit:]       | \\d       | Any numeric digit                                            |
| [:graph:]       |           | Characters printable in ASCII code from 32 to 126 except the space character |
| [:lower:]       | \\l       | Any alphabet in lowercase                                    |
| [:print:]       |           | Characters printable in ASCII code from 32 to 126            |
| [:punct:]       | \\p       | Characters printable in ASCII code from 32 to 126 except the space character, numeric digits and alphabetic characte |
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

The following table describes metacharacters that can be used for regular expressions in Altibase Regular Expression Library.

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

#### Perl Compatible Regular Expressions (PCRE2) Library

Perl Compatible Regular Expressions Library is supported from Altibase 7.1.0.7.7 and the Perl Compatible Regular Expressions Library version is 10.40. This library supports searching in Korean which is not supported by Altibase Regular Expression Library and new search features such as backreferences, lookaheads, etc. are added.

**Perl Compatible Regular Expressions Library Limitations** 

- This library is only supported when Altibase server character set is  US7ASCII or UTF-8.
- There are syntax differences with the Altibase Regular Expression Library.

### Syntax Differences between the two Regular Expression Libraries

The table below shows the differences in syntax between the two regular expression libraries.

<table>
  <tbody>
    <tr>
      <th>Regular Expression Syntax</th>
      <th>Example of Regular Expression Syntax of Altibase Regular Expression Library</th>
      <th>Example of Regular Expression Syntax of Perl Compatible Regular Expressions Library</th>
    </tr>
    <tr>
      <td>
        <p>POSIX character class</p>
      </td>
      <td>
SELECT REGEXP_COUNT('ABCDEFG1234567abcdefgh!@#$%^&*(','[:punct:]+');
<br\></br\>SELECT REGEXP_COUNT('ABCDEFG1234567abcdefgh!@#$%^&*(','\l+');`
      </td>
      <td>
`SELECT REGEXP_COUNT('ABCDEFG1234567abcdefgh!@#$%^&*(','[[:punct:+');
SELECT REGEXP_COUNT('ABCDEFG1234567abcdefgh!@#$%^&*(','[[:lower:+');`
      </td>
    </tr>
    <tr>
      <td rowspan="2">
        <p>POSIX collating element or equivalence class</p>
      </td>
      <td colspan="1">
`SELECT I1 FROM T1 WHERE REGEXP_LIKE( I1, '[=A=]' );`
      </td>
      <td colspan="1">Unsupported</td>
    </tr>
    <tr>
      <td colspan="1">
`SELECT I1 FROM T1 WHERE REGEXP_LIKE( I1, '[A-[.CH.' );`
      </td>
      <td colspan="1">Unsupported</td>
    </tr>
    <tr>
      <td colspan="1">
        <p>Backreference</p>
      </td>
      <td colspan="1">Unsupported</td>
      <td colspan="1">`SELECT * FROM T1 WHERE REGEXP_LIKE(I2,'(ALTI(BASE)7) DATA\2');
SELECT * FROM T1 WHERE REGEXP_LIKE(I2,'(ALTI(?<BASE>BASE)7) DATA(?P=BASE)');`
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <p>Lookahead</p>
      </td>
      <td colspan="1">Unsupported</td>
      <td colspan="1">`SELECT * FROM T1 WHERE REGEXP_LIKE(I2,'ALTI.*(?=DATABASE)');
SELECT * FROM T1 WHERE REGEXP_LIKE(I2,'ALTI.*(?!DATABASE)');`
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <p>Lookbehind</p>
      </td>
      <td colspan="1">Unsupported</td>
      <td colspan="1">`SELECT * FROM T1 WHERE REGEXP_LIKE(I2,'(?<=ALTIBASE7) DATABASE');
SELECT * FROM T1 WHERE REGEXP_LIKE(I2,'(?<!ALTIBASE7) DATABASE');`
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <p>Conditional pattern</p>
      </td>
      <td colspan="1">Unsupported</td>
      <td colspan="1">`SELECT REGEXP_SUBSTR(I2,'(?(?=ALTIBASE)ALTIBASE7|DATABASE)') FROM T1;`
      </td>
    </tr>
    <tr>
      <td colspan="1">
        <p>Character with the xx property</p>
      </td>
      <td colspan="1">Unsupported</td>
      <td colspan="1">`SELECT REGEXP_SUBSTR(I2,'\P{HANGUL}+') FROM T1;`
      </td>
    </tr>
  </tbody>
</table>


For more details about the regular expression syntax of the Perl Compatible Regular Expressions Library, please refer to the [Perl Compatible Regular Expressions Library pattern manual page](https://www.pcre.org/current/doc/html/pcre2pattern.html).

### Altering the Regular Expression Library

Since Altibase provides Altibase Regular Expression Library and Perl Compatible Regular Expressions Library, the library to be used for the regular expressions has to be chosen between the two. The default library is Altibase Regular Expression Library. Therefore, in case the user wishes to use Perl Compatible Regular Expressions Library, regular expression library must be altered with the following statements.

- Alter the regular expression library for the system when Altibase server is running

  To apply this change, the session has to be reconnected.

  `ALTER SYSTEM SET REGEXP_MODE=1;`

- Alter the regular expression library for the session when Altibase server is running

  `ALTER SESSION SET REGEXP_MODE=1;`

- Alter the regular expression library permanently

  Add REGEXP_MODE=1 in altibase.properties file and restart the Altibase server

