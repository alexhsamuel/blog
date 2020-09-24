Python lets you use either single quotes (`'string'`) or double quotes
(`"string"`) for strings, and they're almost identical.  The only difference is
that in a single-quoted string, one must escape single quotes, while conversely
double quotes in a double-quoted string.

I generally use double quotes.  People sometimes ask me why.


### Advantages of single quotes:

- Easier to type.  Single quotes are on the home row, while double quotes
  require a shift chord.  On ISO keyboards, double quotes is Shift-2, on the
  top row.

- Consistent with how Python renders strings: `repr("foo")` is `'foo'`


### Advantages of double quotes:

- We use the same character for a single quote and an apostrophe.  In computer
  English, apostrophes appear much more often than double quotes.
   
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

- Double quotes stand out visually in dense code.

- Double quotes are visually distinct from left ticks, which are often used to
  specify code formatting in Sphinx, Markdown, and other doc styles.

- Some people are used to other common languages that accept double quotes only
  for strings, such as C-like languages, many LISPs, and JSON.


# Conclusion

I think the advantages for double quotes, particularly with apostrophes,
outweigh the alternative.

I attempt to be consistent within a source file: if the rest of a file already
uses single quotes, I try to do the same.

