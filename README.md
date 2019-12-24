# Introduction
ToolChain Switcher (TCS) is a simple utility that help you change your default gcc from a bunch of toolchains.

Suppose you are a toolchain maintainer or developer and often build and test a bunch of toolchains.
TCS can change, examine and rollback your deault gcc by modifing the shell variable $PATH.

## Install
First put the file 'tcs.py' to the directory ${HOME}/opt/lib/python/ where tcs searchs its library by default.

Second put the file '.cclists' to your home root ${HOME}/ and you have to write down all toolchains' path you might
use daily to this file. You can refer to the section '.cclists format' to know how to write your own '.cclist' file.

Finally make sure the bash file 'tcs_init' be sourced everytime you invoked a new shell.
I usually wrote one line like following example in my '.bashrc'

```sh
source ${HOME}/tcs_init
```

## usage
Suppose all are settled down and you can type 'tcs -h' simply to see how to use it.

```sh
$ tcs -h

usage: tcs [-h] [-p] [-l] [-c [CCNUM]] [-a] [--lsalias] [-r]

ToolChain Switcher utility

optional arguments:
  -h, --help            show this help message and exit
  -p, --lsp             list PATH environment variable
  -l, --lscc            list the available toolchain from .cclists
  -c [CCNUM], --usecc [CCNUM]
                        Use which CC. If no args was specified, restore the
                        PATH to its original value
  -a, --lsvar           list the current defined alias from .cclists
  --lsalias             same as -a, --lsvar
  -r, --recover         restore the PATH to its original value

Author: Li-Hang Lin <lihang.lin@gmail.com>
```

Usually you can type 'tcs -l' to show how many toolchains you have

```sh
$ tcs -l
[ 0]  /home/flin/x-tools/x86_64-w64-mingw32/bin
[ 1]  /opt/riscv/linux/bin (No such file or directory)
```

TCS would show all toolchains from your '.cclists' row by row and check existence.

We can switch any one of them by

```sh
$ tcs -c 0
Update PATH: /home/flin/x-tools/x86_64-w64-mingw32/bin:/home/flin/opt/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

TCS would prepend the toolchain path (numbered by 0) to your environment variable ${PATH}.
After doing command above, typing 'x86_64-w64-mingw32-gcc' would let shell find this gcc from the path '/home/flin/x-tools/x86_64-w64-mingw32/bin'.

Ofcourse you can recover your ${PATH} by

```sh
$ tcs -r
Update PATH: /home/flin/opt/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

## .cclists format
It is actually a python file and includes two list. One is CC and another is ALIAS.

```python
CC = [
    "/home/flin/x-tools/x86_64-w64-mingw32/bin",
    "/opt/riscv/linux/bin",
]

ALIAS = [
    'arm="ARCH=arm CROSS_COMPILE=arm-linux-androideabi-"',
    'arm64="ARCH=arm64 CROSS_COMPILE=aarch64-linux-android-"',
    'rv64="ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu-"',
]
```

Put all your toolchains' path row by row in CC list.
ALIAS list can have any bash alias and they would be sourced when tcs_init is sourcing.
It can help you manage your bunch of alias.

e.g.

```sh
$ make rv64 Image
```

And bash simply replaced 'rv64' with 'ARCH=riscv CROSS_COMPILE=riscv64-unknown-linux-gnu-' before invoking 'make'

