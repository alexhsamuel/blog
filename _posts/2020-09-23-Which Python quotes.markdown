Python lets you use either single quotes (`'string'`) or double quotes
(`"string"`) for strings.  The only difference is that in a single-quoted
string, one must escape single quotes, while conversely double quotes in a
double-quoted string.

I generally use double quotes.  People sometimes ask me why.


### Advantages of single quotes:

- Easier to type.  Single quotes are on the home row.  On ANSI keyboards, double
  quotes are on the same key but shifted.  On ISO keyboards, double quotes are
  Shift-2, on the top row.

- Consistent with how Python renders strings: `repr("foo")` is `'foo'`.


### Advantages of double quotes:

- We use a single quote as an apostrophe.  Messages inteded for users frequently
  contain apostrophes, and it is clumsy to escape them.
   
  Thus,
  ```py
  logging.error("The operation didn't work.")
  ```
  rather than,
  ```py
  logging.error('The operation didn\'t work.')
  ```

  Some people work around this by avoiding contractions (`'did not'`) or
  by misspelling the contraction deliberately (`'didnt'`).

- Double quotes stand out visually in dense code.  Double quotes are also
  visually distinct from left ticks, which are often used to specify code
  formatting in Sphinx, Markdown, and other doc styles.

- American English uses double quotes primarily.  Single quotes are for nested
  quotes only.

- Some people are used to other programming languages that use double quotes
  for strings, such as C-like languages, many LISPs, and JSON.


# Conclusion

I think the advantages for double quotes, particularly with apostrophes,
outweigh the alternative.

I attempt to be consistent within a source file: if the rest of a file already
uses single quotes, I try to do the same.

