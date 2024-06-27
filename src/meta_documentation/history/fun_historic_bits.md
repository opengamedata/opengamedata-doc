# Fun Historical Bits

This document just holds bits of historic trivia and code snippets that I find fun in looking back on the development of OpenGameData.

## Model Class Comment

Found this comment left-over from the "Model" class used in the original teacher dashboard, or a near descendant of that class.

```python
## @class Model
#  Abstract base class for session-level Wave features.
#  Models only have one public function, called Eval.
#  The Eval function takes a list of row data, computes some statistic, and returns a list of results.
#  If the model works on features from session data, it should calculate one result for each row (each row being a session).
#  If the model works on a raw list of recent events, it should calculate a single result (each row being an event).
```

Found it really interesting that at the time, it had the option of taking in rows of feature data.
Not sure if we ever really used this or not, but it's cool that we had some version of "second-order features" before we'd ever formalized that idea as something to be done as a main part of our "miniextractors" architecture.