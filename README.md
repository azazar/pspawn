# PSpawn/XSpawn/ESpawn

PSpawn is a simple [LXC](https://linuxcontainers.org/) wrapper that allows me to have a thumb drive with a single [LXC](https://linuxcontainers.org/) container anywhere, where I have a linux machine with LXC 1.0 installed. It uses it's own directory to store LXC container. XSpawn wraps both PSpawn and [X.Org Server](https://www.x.org/), which allows me to use my own portable X environment on any linux PC with sudo privileges or Xephyr installed. ESpawn wraps both PSpawn and XSpawn to encrypt LXC container using [eCryptfs](https://ecryptfs.org/).

## Dependencies

PSpawn depends only on LXC 1.0, while XSpawn also depends on [X.Org Server](https://www.x.org/). ESpawn requires PSpawn, XSpawn and [eCryptfs](https://ecryptfs.org/) to work. It was tested only on Ubuntu 18.04.3, but I expect it to work anywhere, where sudo and LXC are available.

## Usage

Since it's a LXC wrapper it wraps all `lxc-*` commands. For example `./pspawn start` actually executes `lxc-start -P $PSPAWN_DIR -n pspawn-lxc`.

Use following commands to install everything into current directory:

    $ git clone https://github.com/azazar/pspawn
    $ cd pspawn

Create your first plain PSpawn container:

    $ ./pspawn create

And start X session:

    $ ./xspawn

Or create encrypted container:

    $ ./espawn create
    $ ./espawn p create

And start X session:

    $ ./espawn x

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

### Configuration Environment Variables Values

#### PSPawn

Those values are read from `pspawn.conf` if the file is available in `pspawn_data` directory.

    PSPAWN_PRIVILEGED="n"
    PSPAWN_LXC_NAME="pspawn-lxc"
    PSPAWN_LXC_START_ARGS_EXTRA=""
    PSPAWN_DIR="$(dirname $(realpath $0))"

#### XSpawn

Those values are read from `xspawn.conf` if the file is available in `pspawn_data` directory.

    XSPAWN_INSTALL_SCRIPT="usermod -a -G sudo ubuntu; which startxfce4 || (apt-get -y update && apt-get -y install xfce4)"
    XSPAWN_XSESSION_SCRIPT="sudo -u ubuntu -i xfce4-session"
    XSPAWN_X_SERVER_PRIVILEGED="$(groups `id -un` | grep -q '[ ^]sudo[ $]' || test "$UID" = "0" && echo y || echo n)"
    XSPAWN_X_SERVER_COMMAND="/usr/bin/Xephyr :2001 -resizeable"
    XSPAWN_X_SERVER_COMMAND_PRIVILEGED="/usr/bin/Xorg :2001 vt12 -logfile /dev/null"
