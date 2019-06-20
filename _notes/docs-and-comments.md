# General

Be brief.  After writing docs or comments, edit to remove unnecessary words.


# Docstrings

### Style

Docstrings address the user of a function or class.  They document usage, not
implementation.  Explain the item's purpose and responsibilities.

Don't repeat what is obvious from the item's name:
```py
# BAD
def shut_down(service):
    """
    Shuts down the service.
    """
```

Some cases the functionality is more subtle.
```py
def shut_down(service):
    """
    Schedules `service` to shut down after pending requests are complete.
    """
```


### Functions

For functions, document parameters and return values, except when these are
obvious from names.

Use the third person present indicative to describe what the function or class
does.  Generally, the subject is implicitly the function being documented.
Write in complete sentences, other than omitted subject.

```py
def search(items, value):
    """
    Returns the insertion index of `value` in sorted `items`.
    
    If there are multiple valid insertion points, for example if `value` appears
    in `items` one or more times, returns the lowest insertion index.
    """
```

In a longer comment, the first sentence&mdash;ideally a single
line&mdash;summarizes the purpose.


### Types

For types, phrase the summary as a description, without a subject (implicitly,
"an instance of this type") or verb (implicitly, "is").  Write in complete
sentences, in the present indicative.

```py
class AccountNumber:
    """
    Unique 16-digit account identifier with checksum.
    
    The last digit is sum mod 9 of the other digits.
    """
```


# Comments

Code addresses the interpreter, and is styled after the present imperative.
Comments are for other programmers reading the code, but use the same present
imperative, as if addressing the interpreter.  In some cases, comments also
describe a situation, especially in flow control.

```py
try:
    match = next(db.query(predicate))
except StopIteration:
    # No matches.  Use the default instead.
    return DEFAULT_RESULT
else:
    return match
```

