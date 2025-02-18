## Emacs flymake-mode for the Go programming language

The `goflymake` program is a wrapper around the `go` tool to provide
Emacs flymake style syntax checking for golang source files within
multi-file packages and _test.go files.  Support for os/arch specific
*cgo* files is included thanks to the standard *go/build* package.

With `goflymake` we get not only a syntax check, but a full compile and link check.
Since both *flymake* and *flycheck* generate a temporary file on disk, we need
to filter out the original file.  For example, consider a package where we have
two files, *foo.go* and *bar.go*.  In order to compile and link, `go build`
needs both files as input.  However, while editing *foo.go* we want flymake to
use *flymake_foo.go* and *bar.go* as input, excluding *foo.go* to avoid
duplicate symbol errors.

The same filtering applies when editing test files ending in *_test.go*.

In addition to file filtering, `goflymake` dispatches to the appropriate `go`
tool command.  When editing files that end in *_test.go*, to `go test -c`.
Otherwise to the `go build` command.

### Setup

 1. If needed, update your **${PATH}** to include Go installed binaries, for example:

    `export PATH=${PATH}:${GOPATH}/bin`

    Depending on your Emacs workflow (e.g., windowing system environment), it may be required to explicitly set
the following items:
    1. ``(setenv "GOPATH" "/path/to/gopath")``
    2. ``(setenv "PATH" (concat (getenv "PATH") ":" "/extra/path/element"))``
    3. ``(setq exec-path (append exec-path (list (expand-file-name "/another/thing"))))``

 2. Install goflymake:

    `go install github.com/fredericlepied/goflymake@latest`

### Emacs setup

 1. Install go-mode.el if you haven't already

 2. Add these lines to your **.emacs** or similar:

   * **flymake**

            (add-to-list 'load-path "~/go/pkg/mod/github.com/fredericlepied/goflymake@v0.0.0-<date+sha1>")
            (require 'go-flymake)

   * **flycheck**

            (add-to-list 'load-path "~/go/pkg/mod/github.com/fredericlepied/goflymake@v0.0.0-<date+sha1>")
            (require 'go-flycheck)


### ToDo

We probably shouldn't need the `goflymake` program, the `go` tool could
be tweaked to support the flymake style of syntax checking.
Maybe there is already a better way, but I couldn't find one.

### Troubleshooting

The ``goflymake`` command includes forensic information to assist in debugging
anomalies, which will assist you in tracking down the problem.

If worst comes to worst, the [Flymake Troubleshooting Guide](http://www.gnu.org/software/emacs/manual/html_node/flymake/Troubleshooting.html)
is definitely helpful.
