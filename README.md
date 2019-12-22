# Introduction
ToolChain Switcher (TCS) is a simple utility that help you change your default gcc or clang from a bunch of toolchains. 

Suppose you are toolchain maintainer or developer and often build and test a bunch of toolchains. TCS can change, examine and restore your $PATH an environment variable that shell look for commands quickly and easily.

## usage
Suppose we are in bash then type
```sh
$ tcs -l

```
It would list all toolchains from file ~/.cclist.
We can switch any one of them by
```sh
$ tcs -c 3
```
TCS would put the toolchain path (numbered by 3) in front of your $PATH environment variable.
After switching to the default gcc, typing gcc would let shell found one from xxxx


# How to install?

1. put .cclist to your home ~/
2. make sure your bashrc can source tcs_init
3. put tcs.py to your favor path. By default it is in ~/opt/lib/python/tcs.py
4. that's all

# How to use it?

just type 'tcs -h' to show the usage.
