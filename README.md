# pg_timeline

## WIP

This project takes a PostgreSQL server log and extrapolates start/end times based on the duration.  This will be useful in cases where a log file is available, and `pg_stat_activity` output is not available.

### Requirements
`log_line_prefix` needs to have at minimum the following escapes: `%m (milliseconds)`, `%p (PID)`
`log_min_duration_statement` needs to be set to a non-negative value so that durations are printed when a query is done with execution

### Proposed structure
An extraction script will parse the lines out of the log and convert it to the following JSON format:
```
  [
    {
      "start":    < epoch, in milliseconds, calculated from ${end} - ${duration} >,
      "end":      < epoch, in milliseconds, calculated from the %m timestamp-with-milliseconds >,
      "duration": < number of milliseconds, as printed in log file >,
      "pid":      < PID, as printed in log file >,
      "query":    < query text, as printed in log file >
    }
    ...
  ]
```

A sample pg_timeline.html visualization is provided in inital commit -- will need to expand on this and hopefully incorporate new features:
- Ability to zoom in/out
- Reference marker to help with visualization of other activities at the same moment of time
- Ability to customize/shorten labels
