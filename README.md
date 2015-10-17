# PHP bindings for Go

This package implements bindings for calling PHP scripts and binding Go variables for use in PHP contexts.

## Building

Building this package requires that you have PHP built and installed as a library. For most Linux systems, this would be a file under `/usr/lib/libphp5.so`, and can usually be found in the `php5-embed` package, or variations thereof.

Once the PHP library is available, the bindings can be compiled with `go build` and are `go get`-able.

## Usage

Executing a script is very simple:

```go
package main

import (
    "os"
    php "github.com/deuill/go-php"
)

func main() {
    engine, _ := php.New()
    context, _ := engine.NewContext(os.Stdout)

    context.Exec("index.php")

    engine.Destroy()
}
```

The above will execute script file located in `index.php` and write any output to the `io.Writer` passed to `php.NewContext` (in this case, the standard output).

By executing the PHP context in a Goroutine, one can read from the writer as the output is generated by the script, and is useful for long-running scripts which output information during execution.

## Status

Currently, execution of script files as well as code snippets has been implemented, as well as binding of most base types from Go to PHP contexts.

Calling Go functions and creating classes from Go method receivers is currently a work in progress.

A test-suite is included, which covers most common uses of this library.

## Caveats

Be aware that, by default, PHP is **not** designed to be used in multithreaded environments (which severely restricts the use of these bindings with Goroutines) if not built with [ZTS support](https://secure.php.net/manual/en/pthreads.requirements.php).

Unfortunately, most distributions do not ship ZTS-enabled versions of PHP by default, and very few offer pre-compiled packages, so you're mostly left to compile one yourself.

## License

All code in this repository is covered by the terms of the MIT License, the full text of which can be found in the LICENSE file.