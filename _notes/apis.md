# Conversions

In a dynamic, interactive language like Python, it's nice to make APIs as
lenient about argument types as performance will reasonably allow.

To do this, simply convert the arguments to the necessary type on entry.
```py
def number_of_words(string):
    string = str(string)
    words = string.split()
    return len(words)

```


Often, the type itself will perform all reasonable conversions.  If not, create
a standard function that does this.
```py
def days_until(date):
    date = to_date(date)
    return (today() - date).total_days()

```

Make `to_date()` as flexible as possible, and use it consistently in any
function that takes a date.  This makes your APIs easy to learn and easy to use
interactively.
```py
>>> days_until("2019-07-04")
113
```

The downside of this pattern is that it encourages careless use of types within
larger programs.  Resist the temptation to use strings, integers, and other _ad
hoc_ representations for dates within code; use a `date` object instead.
Lenient conversions are there for interactive use, and in other places where
proper types are impossible or impractical, such as command line arguments and
configuration files.  Convert to the proper type at the API surface (scout's
honor).



# Smells

## Decomposition

Code smells that suggest functions or classes should be decomposed.

### Boolean feature flags

Don't add a boolean parameter that enables additional behavior when true.  This
suggests that the function should be decomposed into two, for the base and the
additional operation.

Instead of,
```py
x = load_data(url, normalize=True)
```

use,
```py
x = normalize(load_data(url))
```

### "And" in a name

This strongly suggests the function should be split.
```py
save_and_quit()
```

should quite cliearly be,
```py
save()
quit()
```

