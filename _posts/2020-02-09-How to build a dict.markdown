---
layout: post
title: "How to build a dict"
date: 2020-02-09
categories: Python
---

# Adding to a dict

Obviously, you can just insert an element.
```py
my_dict["foo"] = 42
```

But of course this mutates the dict, so you wouldn't want to do this, for
instance, with a function argument.
```py
def fn(inputs):
    inputs["foo"] = 42   # BAD
    # Do things...
```

You've mutated the argument, and this change is visible to the caller after the
function returns.  This is usually underireable.

In recent versions of Python, you can use `**` easily to create a new dict with
additional keys.
```py
def fn(inputs):
    inputs = {**inputs, "foo": 42}
    # Do things...
```

Order matters.  In the code above, our value 42 for the key `"foo"` would
replace one passed in by the caller.  To give the caller's value precedence, we
just specify ours first.
```py
inputs = {"foo": 42, **inputs}
```

The mutable equivalent of this would be,
```py
inputs.setdefault("foo", 42)
```


# Removing from a dict

The mutable way is straightforward.
```py
del my_dict["foo"]
```

If you are not sure whether `"foo"` is a key, but you want to remove it if it
is, the best way is with `pop()`.  You need to pass a second argument;
otherwise, `pop()` raises an exception if the key is not present.  Since we're
not interested in the value, any second argument will do.
```py
my_dict.pop("foo", None)
```

But once again, you probably wouldn't want to do this with a function argument.
There isn't an equivalent with `**`, but you can use a dict comprehension with
an expicit filter on the keys.
```py
def fn(inputs):
    inputs = { k: v for k, v in inputs.items() if k != "foo" }
    # Do things...
```

More generally, to remove any of several keys:
```py
def fn(inputs):
    SKIP_KEYS = {"foo", "bar", "baz"}
    inputs = { k: v for k, v in inputs.items() if k not in SKIP_KEYS }
    # Do things...
```

A less fancy way to solve the same thing is simply to copy the dict first, and
then mutate it.
```py
def fn(inputs):
    inputs = dict(inputs)
    inputs.pop("foo", None)
    inputs.pop("bar", None)
    inputs.pop("baz", None)
    # Do things...
```


# Merging two dicts

Here, the `**` syntax is the clear winner.
```py
merged = {**my_dict, **more_dict}
```

Again, order matters: later keys replace earlier ones.


# Building a dict from scratch

Let's start with the fully imperative way.  This is usually the first pattern
that people learn.
```py
STOP_WORDS = {"I", "you", "he", "and", "to", "not"}

counts = {}
for word in words:
    word = str(words)
    if word is not in STOP_WORDS
        counts[word] = len(word)
```
- Pros: Nothing fancy.
- Cons: Verbose.  The most important part, where we add items to the dict, is at
  the end and deeply indented.
  
Here's the same logic as a generator.
```py
counts = { str(w): len(str(w)) for w in words if str(w) not in STOP_WORDS }
```

This fits now on one line, though it is nice to split it for readability.
```py
counts = {
    str(w): len(str(w))
    for w in words
    if str(w) not in STOP_WORDS
}
```
- Pros: Declarative.  Once you are used to dict comprehensions, it's very easy
  to understand the control flow.  (In contrast, imperative code is more
  flexible, so it requires careful reading.)
- Cons: We're calling `str(w)` three times!

In this case, we can deduplicate the `str(w)` calls by "preprocessing" the
inputs.  We use a generator comprehension, as we don't need to realize the
intermediate sequence into an explicit in-memory data structure.
```py
words = ( str(w) for w in words )
counts = { w: len(w) for w in words if w not in STOP_WORDS }
```
- Pros: Concise.
- Cons: This style of stream-oriented programming takes some getting used to.

Another technique to consider is a local function for producing keys and/or
values.
```py
def count(word):
    return len(str(word))

counts = { w: count(w) for w in words }
```
- Pros: The loop and the dict structure are separated from the inner logic.
- Cons: No way to skip items.

This doesn't work so well here, because we would like to skip words that appear
in `STOP_WORDS`, but there is no easy way for the function `count()` to indicate
that an item should be skipped.  We could use a flag value to indicate, but this
gets cumbersome.
```py
def count(word):
    word = str(word)
    return None if word in STOP_WORDS else len(word)

counts = ( (w, count(w)) for w in words )
counts = { w: c for w, c in counts if c is not None )
```

Here, we are using a generator comprehension first so that we can call `count()`
only once but use its return value twice: once to filter on the result (skip if
none), and once as the dict value.

There's a trick to combine this into one comprehension:
```py
counts = {
    w: c
    for w in words
    for c in [count(w)]
    if c is not None
}
```
- Pros: Concise.
- Cons: Tricky.

What's going on here?  Before Python 3.8, there was no syntax to assign a
temporary variable within a comprehension.  We fake it by creating a one-element
list `[count(w)]` and then iterating over it.  This lets us set the temporary
variable `c` to the result of `count(w)`.

Starting in Python 3.8, the "walrus operator" `:=` lets us assign the temporary
variable with a slightly more natural syntax.
```py
counts = {
    w: c
    for w in words
    if (c := count(w)) is not None
}
```
- Pros: Less tricky.
- Cons: Requires Python 3.8.

Note that we're not calling `str()` anymore here.  We can restore it using the
walrus operator, and in this case omit the local function entirely.
```py
counts = {
    w: len(w)
    for word in words
    if (w := str(word)) not in SKIP_WORDS
}
```

One advantage of using a local function is that you sometimes can use early
`returns` to simplify the control flow.  This is, for example, nice in functions
try a number of options or conversions.
```py
def to_word(word):
    if isinstance(word, bytes):
        return word.decode()
    word = str(word)
    if word == "":
        return "<empty>"
    if word in STOP_WORDS:
        return "<stop>"
    return word

counts = { w: len(to_word(w)) for w in words }
```

Python lets you build iterables with generator functions.  Is there some
corresponding way to generate a dictionary?  Not directly.  But we can rely on
the fact that Python can construct a dict from any iterable of (key, value)
pairs.  It doesn't even demand a sequence; a generator is fine too.  This
technique is useful when there isn't a one-to-one relation between inputs and
resulting items.  Write and call a function that generates (`yield`s) items, and
convert these to a dict.
```py
def count_items():
    for w in words:
        w = str(w)
        if w not in STOP_WORDS:
            yield w, len(w)

counts = dict(count_items())
```

If `count()` doesn't yield for a given element, no item is added to the dict.

- Pros: All the flexibility of imperative code, but doesn't mutate a dict, so
  more functional in nature.
- Cons: As verbose as imperative code.

You can also produce multiple items per input with this technique.
```py
def count_items():
    for word in words:
        word = str(word)
        if word in STOP_WORDS:
            continue
        # Maybe the word contains spaces!
        for w in word.split():
            yield w, len(w)

counts = dict(count_items())
```

