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
- `bind`
- `connect`

POSIX functions:

- `write`
- `read`

## Known issues

Doesn't check errors.

## Running

1. Get a running `ddc`.

    Currently requires a patched `ddc`:

    ```
    diff --git a/src/s1/ddc-build/DDC/Build/Builder.hs b/src/s1/ddc-build/DDC/Build/Builder.hs
    index f95628bd2..27bd02c9a 100644
    --- a/src/s1/ddc-build/DDC/Build/Builder.hs
    +++ b/src/s1/ddc-build/DDC/Build/Builder.hs
    @@ -457,6 +457,7 @@ builder_X8664_Linux config host
                     [ "gcc -m64"
                     , "-o", binFile
                     , intercalate " " oFiles
    +                , "get_errno.o"
                     , builderConfigBaseLibDir config
                             </> "ddc-runtime" </> "build"
                             </> builderConfigLibFile config
    ```

    This is required to link the C module. This will not be needed when `ddc`
    gains the ability to link user-supplied `.o` files.

2. Compile the C module: `cc -c get_errno.c`

3. Compile the example: `ddc Main.ds`

3. Start a server (in another terminal): `nc -l -p 1234`

4. Run the example:

    ```
    ./Main
    ```

    The program will write `Hello, World!` to the socket, and read from it. Type
    something into the server terminal, and the program should respond with
    `Read N bytes`, where N is the length of the response.

    Expected output:

    ```
    Connected
    Written 14 bytes
    Read 5 bytes
    ```

    When something goes wrong, it may be helpful to run the program under
    `strace`.
