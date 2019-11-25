# PSpawn/XSpawn

PSpawn is a simple LXC wrapper that allows me to have thumb drive with a single LXC container anywhere, where I have a linux machine with LXC 1.0 installed. It uses it's own directory to store LXC container. XSpawn wraps both PSpawn and X.Org Server, which allows me to use my own portable X environment.

## Dependencies

PSpawn depends only on LXC 1.0, while XSpawn also depends on X.Org Server.

## Usage

Since it's a LXC wrapper it wraps all `lxc-*` commands. For example `./pspawn start` actually executes `lxc-start -P $PSPAWN_DIR -n pspawn-lxc`.

### Examples

Start X session
    
    $ ./xspawn

Start LXC Container

    $ ./pspawn start

Stop LXC Container

    $ ./pspawn stop

Create LXC Container (should be done once)

    $ ./pspawn create

Destroy LXC Container (should be done once)

    $ ./pspawn destroy

Execute command inside running container

    $ ./pspawn attach --keep-var TERM -- mc

Execute command inside container without starting it

    $ ./pspawn execute -- /bin/bash

Execute lxc-info

    $ ./pspawn info

Create LXC Container in privileged mode

    $ PSPAWN_PRIVILEGED="y" ./pspawn create

Start LXC Container in privileged mode

    $ PSPAWN_PRIVILEGED="y" ./pspawn start

There is also a Midnight Commander local menu file, that contains most used shortcuts.

### Default Configuration Environment Variables Values

#### PSPawn

Those values are read from `pspawn.conf` if the file is available in PSpawn directory.

    PSPAWN_PRIVILEGED="n"
    PSPAWN_LXC_NAME="pspawn-lxc"
    PSPAWN_LXC_START_ARGS_EXTRA=""
    PSPAWN_DIR="$(dirname $(realpath $0))"

#### XSpawn

Those values are read from `xspawn.conf` if the file is available in PSpawn directory.

    XSPAWN_INSTALL_SCRIPT="usermod -a -G sudo ubuntu; which startxfce4 || (apt-get -y update && apt-get -y install xfce4)"
    XSPAWN_XSESSION_SCRIPT="sudo -u ubuntu -i xfce4-session"
