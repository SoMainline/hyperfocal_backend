# Hyperfocal back end
## Expected structure
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
$
```

#### `get_info`

Returns device specific info (e.g. format, resolution, fps, etc.) in form of json, further details will be provided later

#### `setup`

Prepares a back end specific device using it's ID, if successful should return `0`, if the setup process is interrupted or impossible to complete it should return the appropriate POSIX error, if the requested ID is not present in the list of back end's devices it should return `-ENODEV`

Example:
```bash
$ ./setup 0  # '0' is one of the IDs from the result of ./list_all
$ echo $?
0  # success
$
```

#### `cleanup`

Cleans up a back end specific device using it's ID, if successful should return `0`, if not it should return the appropriate POSIX error

```bash
$ ./cleanup 0  # '0' - previously set up device
$ echo $?
0  # success
$
```