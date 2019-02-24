# Naming

### Hierarchical names

Use packages and modules to organize names.  Do not use "typographical
namespaces", i.e. organization using punctuation.
```py
http.connect
terminal.ansi.Color
```

This is preferable to `http_connect` or `AnsiTerminalColor`.

Similarly, don't repeat hierarchial names in the final name!  So, no
`http.http_connect` or `terminal.ansi.AnsiTerminalColor`.

You can always rename in an `import` statement, if your module has multiple
`connect()` functions or `Color` types.
```py
from http import connect as http_connect
```


### Functions

Name a function with an imperative verb or verb phrase.

```py
run(argv, cwd, env)
get_config(instance)
load_from_file(path)
```

Likewise for a method, and for any object that is primarily used as a callable.

As an exception, name, a functions that computes something, particularly functions of
a mathematical bent, after what it computes.

```py
sqrt(x)
atan(y, x)
```

Such functions are typically pure functional; those that access external state
or have side effects usually have verb names, like `get_next_id()`.  This isn't
a hard and fast rule though; for instance, we usually write `now()` instead of
`get_current_time()`.


### Types



