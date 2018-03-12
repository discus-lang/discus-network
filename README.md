# discus-network

A network library for [Discus](http://discus-lang.org/), loosely based on the Haskell
[network](https://hackage.haskell.org/package/network) package.

Wrapper around BSD sockets.

## What is implemented?

Almost nothing.

Address families (`struct sockaddr` encoding):

- `AF_INET` (`struct sockaddr_in`)

BSD sockets functions:

- `socket`
- `bind` (doesn't work)
- `connect`

POSIX functions:

- `write`

## Known issues

Doesn't check errors.

## Running

1. Get a running `ddc`.

2. Compile the example: `ddc Main.ds`

3. Start a server (in another terminal): `nc -l -p 1234`

4. Run the example:

    ```
    strace ./Main
    ```

    Currently the most convenient method of inspecting behavior of the program is `strace`.
    Expected output:

    ```
    execve("./Main", ["./Main"], 0x7fff50a7d430 /* 51 vars */) = 0
    ... irrelevant junk ...
    socket(AF_INET, SOCK_STREAM, IPPROTO_IP) = 3
    connect(3, {sa_family=AF_INET, sin_port=htons(1234), sin_addr=inet_addr("0.0.0.0")}, 16) = 0
    write(3, "Hello World!\n\0", 14)        = 14
    exit_group(0)                           = ?
    +++ exited with 0 +++
    ```
