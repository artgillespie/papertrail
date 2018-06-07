> almost all failures only require 3 or fewer nodes to reproduce

This suggests that failures are not typically hardware or network related and
instead are "just" bugs.

I like the taxonomy of fault, error, and failure:

    * fault: initial root cause, hardware failure, bug, configuration
    * error: abnormal behavior, non-nil err, exceptions
    * failure: user-visible malfunction

> almost all (92%) of the catastrophic system failures
are the result of incorrect handling of non-fatal errors explicitly signaled in
software.

> While it is well-known that error handling code is often buggy...

An understatement.

> ... it's sheer prevalence in the causes of catastrophic failures is still surprising.

In fact, in 35% of the catastrophic failures, the faults in the error handling code fall into three trivial patterns: (i) the error handler is simply empty or only contains a log printing statement, (ii) the error handler aborts the cluster on an overly-general exception, and (iii) the error handler contains expressions like “FIXME” or “TODO” in the comments.  These faults are easily detectable by tools or code reviews without a deep understanding of the runtime context. In another 23% of the catastrophic failures, the error handling logic of a non-fatal error was so wrong that any statement coverage testing or more careful code reviews by the developers would have caught the bugs.

> ...we show that most failures require no
more 3 nodes and no more than 3 input events to reproduce, and most failures
are deterministic. In fact, most of them can be reproduced with unit tests.

> In the end, testing is a probabilistic exercise.

The thing I find fascinating (and sobering) about this paper is that I often feel like complexity produces a lot of non-deterministic behavior (how do we test for these things) but given this study, most of our failures aren't just deterministic, the underlying faults are simple and easy to detect.

Only a relatively small number of the failures stem from the black arts we
usually associate with hairy bugs-namely concurrency, atomicity, deadlocks,
etc.

Oooh, previous study on non-distributed systems, including PostgreSQL.

>  It would be helpful if
existing log analysis techniques [5, 6, 48, 64] and tools
were extended so they can infer the relevant error and
input event messages by filtering out the irrelevant ones.

Interesting. Suggests a 'smart' log analysis tool that recognizes patterns.


Most failures can be reproduced by unit test.

> none of the studied failures required specific values of user's data contents

Interesting. I suppose this makes sense at the system level (e.g., data
storage) where payloads are essentially opaque, but that faults or errors or
degradation become increasingly content and path-dependent in more "business
logic" systems built on top of these lower-level distributed systems.


