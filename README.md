# PSpawn

It's a simple LXC wrapper that allows me to have thumb drive with a single LXC container anywhere, where I have a linux machine with LXC 1.0 installed. It uses it's own directory to store LXC container.

## Dependencies

It depends only on LXC 1.0.

## Usage

Since it's a LXC wrapper it wraps all `lxc-*` commands. For example `./pspawn start` actually executes `lxc-start -P $PSPAWN_DIR -n pspawn-lxc`.

Some examples:

    $ ./pspawn start
    $ ./pspawn stop
    $ ./pspawn create
    $ ./pspawn destroy
    $ ./pspawn attach -- env TERM=$TERM mc
    $ ./pspawn execute -- /bin/bash
    $ ./pspawn info
    $ PSPAWN_PRIVILEGED="y" ./pspawn create
    $ PSPAWN_PRIVILEGED="y" ./pspawn start

There is also a Midnight Commander local menu file, that contains most used shortcuts.

### Default configuration environment variables values

    PSPAWN_PRIVILEGED="n"
    PSPAWN_LXC_NAME="pspawn-lxc"
    PSPAWN_LXC_START_ARGS_EXTRA=""
    PSPAWN_DIR="$(dirname $(realpath $0))"
