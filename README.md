# Hyperfocal back end

Hyperfocal use this back end API to abstract away from handling camera devices, not the best API in the world, but it works.

## Expected behavior
Hyperfocal back ends are comprised of 4 executable files:
```
list_all
get_info
setup
cleanup
```
These files need to be in a single directory, that directory will be considered as the back end and it's location. E.g.
```bash
$ tree example
example/
├── cleanup
├── get_info
├── list_all
└── setup

0 directories, 4 files
```

Then to use this back end in any Hyperfocal front end you need to set `"backend_dir"` to the full path of this back end directory in Hyperfocal's config (but you can also do that from Hyperfocal's settings)

---

Following sections explain expected behavior from the back end members

#### `list_all`

Lists all unique device IDs recognized by the back end, the IDs are back end specific, if no devices are found it should set the return code to `-ENODEV` (if any other error occurs, set the return code accordingly)

```bash
$ ./list_all
0
1
2
3
$ echo $?
0  # success
$
```

#### `get_info`

This one does double dutty, if no arguments are supplied it should to print a json string of back end info to std out, if an argument is supplied it should assume it's a device specific index and it needs to print a json string with that device's settings to std out, if any errors are encountered it should return the appropriate POSIX error
```bash
$ ./get_info
{
    "name": "lazy-back-end-example",
    "version": "0.1",
    "description": "A lazy back end example, long description, blah blah blah blah blah blah blah, etc.",
    "author": "Oleg Vorobiov <oleg.vorobiov@somainline.org>",
    "maintainer": "Oleg Vorobiov <oleg.vorobiov@somainline.org>"
}
$ echo $?
0 # success
$ ./get_info 0 # '0' - back end specific ID
{"rotate": 0, "flipX": false, "flipY": false, "scale": 1.0}
$ echo $?
0 # success
$ ./get_info 5 # '5' - index out of bounds according to list_all
$
$ echo $?
56 # EBADRQC - this backend returns EBADRQC on bad IDs
$
```

#### `setup`

Prepares a back end specific device for v4l2 video capture by ID, should return the v4l2 descriptor ID to std out if successful, if not it should return the appropriate POSIX error

Example:
```bash
$ ./setup 0  # '0' is one of the IDs from the result of ./list_all
1  # corresponds to /dev/video1, does not have to be equal with back end ID
$ echo $?
0  # success
$
```

#### `cleanup`

Cleans up a back end specific device using it's ID, if successful should return `0`, if not it should return the appropriate POSIX error

```bash
$ ./cleanup 0  # '0' - previously set up device, which corresponds to /dev/video1
$ echo $?
0  # success
$
```

## More examples
Example back end implementations can be found in `backend_examples/`
