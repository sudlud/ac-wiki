# Linux Core Installation

| Installation Guide | |
| :- | :- |
| This article is a part of the Installation Guide. You can read it alone or click on the previous link to easily move between the steps. |
| [<< Step 1: Requirements](linux-requirements) | [Step 3: Server Setup >>](server-setup) |

## Required software

See [Requirements](linux-requirements) before you continue.

## Getting the source code

Choose **ONE** of the following method, run one of the below `git ...` commands in your terminal.


1. Clone only the master branch + full history (smaller size - recommended):

    ```sh
    git clone https://github.com/azerothcore/azerothcore-wotlk.git --branch master --single-branch azerothcore
    ```

1. Clone only the master branch + no previous history (smallest size):

    ```sh
    git clone https://github.com/azerothcore/azerothcore-wotlk.git --branch master --single-branch azerothcore --depth 1
    ```

    Note: If you want to get the full history back, use `git fetch --unshallow`.

1. Clone all branches and all history:

    ```sh
    git clone https://github.com/azerothcore/azerothcore-wotlk.git azerothcore
    ```

This will create an `azerothcore-wotlk` directory containing the AC source files.

## Compiling the source code

### Creating the build-directory

To avoid issues with updates and colliding source builds, we create a specific build-directory, so we avoid any possible issues due to that (if any might occur)

```sh
cd azerothcore
mkdir build
cd build
```

### Configuring for compiling

Before running the CMake command, replace `$HOME/azeroth-server/` with the path of the server installation (where you want to place the compiled binaries).

Parameter explanation for advanced users [CMake options](cmake-options).

At this point, you must be in your "build/" directory.

**Note**: in the following command the variable `$HOME` is the path of the **current user**, so if you are logged as root, $HOME will be "/root". You can check the state of the environment variable, as follows:

```sh
echo $HOME
```

**Note**: in case you use a non-default package for `clang`, you need to replace it accordingly. For example, if you installed `clang-6.0` then you have to replace `clang` with `clang-6.0` and `clang++` with `clang++-6.0`

```sh
cmake ../ -DCMAKE_INSTALL_PREFIX=$HOME/azeroth-server/ -DCMAKE_C_COMPILER=/usr/bin/clang -DCMAKE_CXX_COMPILER=/usr/bin/clang++ -DWITH_WARNINGS=1 -DTOOLS_BUILD=all -DSCRIPTS=static -DMODULES=static
```

To know the amount of cores available.
You can use the following command

```sh
nproc --all
```

Then, replacing `6` with the number of threads that you want to execute, type:

```sh
make -j 6
make install
```

<br>

## Services
systemd services might help you with managing your AzerothCore server. `/srv/azerothcore-wotlk` is intallation directory in following examples.

### authserver.service
```sh
[Unit]
Description=AzerothCore Authserver
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=azerothcore
WorkingDirectory=/srv/azerothcore-wotlk
ExecStart=/srv/azerothcore-wotlk/acore.sh run-authserver

[Install]
WantedBy=multi-user.target
```
### worldserver.service
```sh
[Unit]
Description=AzerothCore Worldserver
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=always
RestartSec=1
User=azerothcore
WorkingDirectory=/srv/azerothcore-wotlk
ExecStart=/bin/screen -S worldserver -D -m /srv/azerothcore-wotlk/acore.sh run-worldserver

[Install]
WantedBy=multi-user.target
```

After you reload systemctl, you can start AzerothCore like this
```sh
sudo service worldserver start
sudo service authserver start
```
Or stop it
```sh
sudo service worldserver stop
sudo service authserver stop
```

## Help

If you are still having problems, check:

* [FAQ](faq)

* [Common Errors](common-errors)

* [How to ask for help](how-to-ask-for-help)

* [Join our Discord Server](https://discord.gg/gkt4y2x), but it is not a 24/7 support channel. A staff member will answer you whenever they have time.

| Installation Guide | |
| :- | :- |
| This article is a part of the Installation Guide. You can read it alone or click on the previous link to easily move between the steps. |
| [<< Step 1: Requirements](linux-requirements) | [Step 3: Server Setup >>](server-setup) |
