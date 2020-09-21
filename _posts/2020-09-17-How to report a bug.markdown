Typically, goal of reporting a bug is to get someone to help you fix it.  To
maximize the chance of this outcome, provide as much information as possible.

**Show** what the system is doing, don't **tell** what you think is wrong.

Worst:

> Did something change with the database?
> 
> Thanks,
> Alex

That's not a bug report at all.  The answer is yes, of course something changed,
that's what computers are for.

Bad:

> The database is down.  Can you please take a look?
> 
> Thanks,
> Alex

Which database?  What does "down" mean?  Can't connect?  Can't log in?  Access
denied?  Missing table?  No data?  What were you trying to do?  Do we _have_ to
play Twenty Questions?

Also bad:

> I think someone has messed up permissions on the database.  Can you please fix
> them?
> 
> Thanks,
> Alex

Speculate about the problem if you want, but only _after_ you've included all
the relevant facts.

Much better:

> I'm trying to get the current number of registered users.
> 
> ```
> host123:~/dev/tools> ./query-users "SELECT COUNT(*) FROM users"
> ...
> ```
> 
> Can you please take a look?
> 
> Thanks,
> Alex

This doesn't take any longer to write.  Just cut and paste.  By simply
**pasting the _full_ terminal transcript**, you've communicated all sorts of
information that might be useful:
- the exact command you ran
- which computer you're running on
- your current working directory
- the full error message
- the full stack trace
- logging, possibly including timestamps

Don't worry if the transcript seems a little long.  The computer can handle it.

This maximizes your chances of getting the response you want:

> I can connect connect and run some other stuff, but I can't run that query
> either.  I'll take a look. 
> 
> The query seems to work on the backup server.  Use that for now
> (`--db backup:5432`) and I'll get back to you.
> 
> Alice

What you _should_ tell is what you were trying to accomplish ("I'm trying to get
the current number of registered users").  In case it's not obvious, also
describe what you expected to happen.  Occasionally, your approach to solving
your problem isn't ideal, or your expectations aren't accurate.

> Dear Alex,
> 
> The old `query-users` script, and the table behind it, was removed two weeks
> ago; please `git pull` your checkout.
> 
> You can use the new user API for this, instead.  Docs are here: 
> https://wiki.internal.com/user-api.
> 
> Thanks,
> Alice

