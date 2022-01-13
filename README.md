# Zeedit

This terminal program uses the [lbrytools](https://github.com/belikor/lbrytools) library
to download content from various channels, cleaning older files in the process,
in order to seed those files to the LBRY network.

This script should be run periodically to constantly download new content,
and remove older files if they take too much space.

This program is released as free software under the MIT license.

## Motivation

The [LBRY Desktop application](https://github.com/lbryio/lbry-desktop)
provides basic functionality to manage downloaded claims, but at the moment
it doesn't give us advanced tools to periodically download new content
from selected channels.

The `zeedit` program allows us to define a list of channels
from which to download the newest content,
and control how much space the files take in our system.
It can be used to create a terminal or GUI-based seedbox
that requires little user intervention,
which will help strengthen the peer to peer LBRY network.

This program was inspired by tuxfoo's [lbry-seedit](https://github.com/tuxfoo/lbry-seedit) script,
which provides basic functions, and a configuration file to download and seed
claims. Initially tuxfoo's code was extended slightly but eventually an entire
library and this program were written from scratch to provide
more tools to the user.

## Installation

You must have Python installed. Most Linux distributions come with Python
ready to use; for Windows you may need to get the official package,
or a full featured distribution such as Anaconda.
In Windows, make sure the Python executable is added to the `PATH`
so that it can be launched from anywhere in your system.

You must have the LBRY Desktop application
or the standalone `lbrynet` terminal client.
Get them from [lbry.com/get](https://lbry.com/get).

### Requisites

The `requests` library is necessary to communicate
with the running LBRY daemon:
```sh
python -m pip install --user requests
python3 -m pip install --user requests  # for Ubuntu
```

You can install these and other libraries by using `pip`,
the Python package manager. If this is not installed it can be installed:
```sh
sudo apt install python-pip
sudo apt install python3-pip  # for Ubuntu
sudo pacman -S python-pip  # for Arch
```

### lbrytools

You must have the [lbrytools](https://github.com/belikor/lbrytools/tree/master/lbrytools)
library (the `lbrytools/` directory that has the `__init__.py`).

Clone this repository using Git with `--recurse-submodules`
to include `lbrytools` with the rest of the code:
```sh
git clone --recurse-submodules https://github.com/belikor/zeedit
```

After cloning you should have the following structure:
```
zeedit/
    zeedit
    zeedit_config_example.py
    lbrytools/
```

### Running in place

You can run the `zeedit` program where it is.
```sh
python zeedit
```

If you place `zeedit` somewhere else, make sure it is next to `lbrytools/`.

### Updating

To update the program's code, make sure you are in the `zeedit/` directory:
```sh
cd zeedit/
git pull
```

The [lbrytools](https://github.com/belikor/lbrytools/tree/submodule) library
is hosted in its own repository (under the `submodule` branch),
and is used in this program as a submodule.
To update this component:
```sh
git submodule update --remote --rebase lbrytools/
```

If this causes merging errors you may have to update the submodule manually:
```sh
cd zeedit/lbrytools/
git checkout submodule
git fetch
git reset --hard FETCH_HEAD
```

### System wide installation

This is optional, and only required if you want to have the libraries
available in your entire system.

Copy the `lbrytools` directory (the one with an `__init__.py`),
and place it inside a `site-packages` directory that is searched by Python.
This can be in the user's home directory,
```
/home/user/.local/lib/python3.8/site-packages/lbrytools
```

or in a system-wide directory:
```
/usr/local/lib/python3.8/dist-packages/lbrytools
/usr/lib/python3/dist-packages/lbrytools
```

Then place `zeedit` wherever you want, and run it from there.

### Environmental variables

This is optional. Instead of moving the `lbrytools` library,
simply add it to the `PYTHONPATH` environmental variable.
We must add the parent directory containing this library.

For example, if
```
/top1/pkg/
    lbrytools/
```

The variable will be
```sh
PYTHONPATH="/top1/pkg:$PYTHONPATH"
```

## Usage

Make sure the `lbrynet` daemon is running either by launching
the full LBRY Desktop application, or by starting the console `lbrynet`
program.
```
lbrynet start
```

If `lbrytools` is correctly installed in the Python path, the script can be
executed directly, or through the Python interpreter.
```sh
python zeedit [config.py]
python3 zeedit [config.py]  # for Ubuntu
```

A configuration file should be passed as the first argument.
```
python zeedit funny_config.py
python zeedit tech_channels_config.py
python zeedit cooking_conf.py
```

The configuration file specifies the channels to download content from,
the download directory, the limit in gigabytes before cleanup of older files
is started, and whether to write a summary of all downloaded claims.

Modify the [zeedit_config_example.py](./zeedit_config_example.py)
to your liking, and read the comments in it to understand what each variable
does. Only the `channels` variable is mandatory, all others have a default
value if they are missing in the configuration file.

If no argument is given, or if the provided configuration file does not exist,
it will default to loading a configuration under the name `zeedit_config.py`;
if this is not available, it will simply exit.

To keep everything contained, the `lbrytools` package can be placed
in the same directory as the `zeedit` executable and its `zeedit_config.py`.
```
top/
   zeedit
   zeedit_config.py
   lbrytools/
       __init___.py
       blobs.py
       ...
```

## Development

Ideally, this collection of tools can be merged into the official
LBRY sources so that everybody has access to them without installing separate
programs.
Where possible, the tools should also be available from a graphical
interface such as the LBRY Desktop application.
* [lbryio/lbry-sdk](https://github.com/lbryio/lbry-sdk)
* [lbryio/lbry-desktop](https://github.com/lbryio/lbry-desktop)

If you wish to support this work you can send a donation:
```
LBC: bY38MHNfE59ncq3Ch3zLW5g41ckGoHMzDq
XMR: 8565RALsab2cWsSyLg4v1dbLkd3quc7sciqFJ2mpfip6PeVyBt4ZUbZesAAVpKG1M31Qi5k9mpDSGSDpb3fK5hKYSUs8Zff
```
