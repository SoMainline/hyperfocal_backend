# Hyperfocal back end
## Expected behavior
The back end is expected to have the following executable files exposed to the front end
```
$ ls backend_example
cleanup  get_info  list_all  setup
```

The said executable files are expected to have the following behavior


#### `list_all`

Lists all unique device IDs recognized by the back end, the IDs are back end specific, the front end does not care about the specifics

Example:
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

Returns device specific info (e.g. format, resolution, fps, etc.), further details will be provided later

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