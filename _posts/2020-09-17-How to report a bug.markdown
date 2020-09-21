Typically, goal of reporting a bug is to get someone to help you fix it.  To
maximize the chance of this outcome, provide as much information as possible.

**Show** what the system is doing, don't **tell** what you think is wrong.

Worst:
```
Did something change with the database?

Thanks,
Alex
```

That's not a bug report at all.  The answer is yes, of course something changed,
that's what computers are for.

Bad:
```
The database is down.  Can you please take a look?

Thanks,
Alex
```

Which database?  What does "down" mean?  Can't connect?  Can't log in?  Access
denied?  Missing table?  No data?  What were you trying to do?  Do we _have_ to
play Twenty Questions?

Also bad:
```
I think someone has messed up permissions on the database.  Can you please take
a look?

Thanks,
Alex
```

Speculate about the problem if you want, but only _after_ you've included all
the relevant facts.

Much better:
```
I'm trying to get the current number of registered users.

host123:~/dev/tools> ./query "SELECT COUNT(*) FROM users"
...

Can you please take a look?

Thanks,
Alex
```

This doesn't take any longer to write.  Just cut and paste.  By simply
**pasting the _full_ terminal transcript**, you've communicated all sorts of
information that might be useful:
- the exact command you ran
- which computer you're running on
- your current working directory
- the full error message
- the full stack trace
- logging, including timestamps

Don't worry if the transcript seems a little long.  The computer can handle it.

```
I can connect connect and run some other stuff, but I can't run that query
either.  I'll take a look. 

The query seems to work on the backup server.  Use that for now
(`db-backup:5432`) and I'll get back to you as soon as I find out anything.

Kel
```

What you _should_ tell is what you were trying to accomplish, and in case it's
not clear, what you expected to happen.  Occasionally, your approach to solving
your problem isn't ideal, and you may find out a better approach.

```
Dear Alex,

The old `query` script was removed two weeks ago; please `git pull`.  Use the
new status dashboard for this.  Docs here: https://wiki.internal.com/dashboard.

Thanks,
Alice
```

