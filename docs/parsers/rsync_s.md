[Home](https://kellyjonbrazil.github.io/jc/)
<a id="jc.parsers.rsync_s"></a>

# jc.parsers.rsync\_s

jc - JSON CLI output utility `rsync` command output streaming parser

> This streaming parser outputs JSON Lines

Supports the `-i` or `--itemize-changes` options with all levels of
verbosity.

Will also process the rsync log file generated with the `--log-file`
option.

Usage (cli):

    $ rsync -i -a source/ dest | jc --rsync-s

Usage (module):

    import jc
    # result is an iterable object (generator)
    result = jc.parse('rsync_s', rsync_command_output.splitlines())
    for item in result:
        # do something

    or

    import jc.parsers.rsync_s
    # result is an iterable object (generator)
    result = jc.parsers.rsync_s.parse(rsync_command_output.splitlines())
    for item in result:
        # do something

Schema:

    {
      "type":                           string,       # 'file' or 'summary'
      "date":                           string,
      "time":                           string,
      "process":                        integer,
      "sent":                           integer,
      "received":                       integer,
      "total_size":                     integer,
      "matches":                        integer,
      "hash_hits":                      integer,
      "false_alarms":                   integer,
      "data":                           integer,
      "bytes_sec":                      float,
      "speedup":                        float,
      "filename":                       string,
      "date":                           string,
      "time":                           string,
      "process":                        integer,
      "metadata":                       string,
      "update_type":                    string/null,  [0]
      "file_type":                      string/null,  [1]
      "checksum_or_value_different":    bool/null,
      "size_different":                 bool/null,
      "modification_time_different":    bool/null,
      "permissions_different":          bool/null,
      "owner_different":                bool/null,
      "group_different":                bool/null,
      "acl_different":                  bool/null,
      "extended_attribute_different":   bool/null,
      "epoch":                          int,          [2]

      # Below object only exists if using -qq or ignore_exceptions=True

      "_jc_meta":
        {
          "success":    boolean,     # false if error parsing
          "error":      string,      # exists if "success" is false
          "line":       string       # exists if "success" is false
        }
    }

    [0] 'file sent', 'file received', 'local change or creation',
        'hard link', 'not updated', 'message'
    [1] 'file', 'directory', 'symlink', 'device', 'special file'
    [2] naive timestamp if time and date fields exist and can be converted.

Examples:

    $ rsync | jc --rsync-s
    {example output}
    ...

    $ rsync | jc --rsync-s -r
    {example output}
    ...

<a id="jc.parsers.rsync_s.parse"></a>

### parse

```python
@add_jc_meta
def parse(data: Iterable[str], raw: bool = False, quiet: bool = False, ignore_exceptions: bool = False) -> Union[Iterable[Dict], tuple]
```

Main text parsing generator function. Returns an iterator object.

Parameters:

    data:              (iterable)  line-based text data to parse
                                   (e.g. sys.stdin or str.splitlines())

    raw:               (boolean)   unprocessed output if True
    quiet:             (boolean)   suppress warning messages if True
    ignore_exceptions: (boolean)   ignore parsing exceptions if True

Yields:

    Dictionary. Raw or processed structured data.

Returns:

    Iterator object

### Parser Information
Compatibility:  linux, darwin, freebsd

Version 1.1 by Kelly Brazil (kellyjonbrazil@gmail.com)