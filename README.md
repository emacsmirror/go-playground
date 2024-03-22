<!--*- mode:markdown;mode:orgtbl;fill-column:99 -*-->
# Local Golang playground [![MELPA Stable](https://stable.melpa.org/packages/go-playground-badge.svg)](https://stable.melpa.org/#/go-playground) [![MELPA](http://melpa.org/packages/go-playground-badge.svg)](http://melpa.org/#/go-playground)

The simple mode for GNU/Emacs for setting up local [Go language](http://golang.org) playground with
features similar (and with help of `go-mode` even outperform!) service
at [play.golang.org](http://play.golang.org). You may treat it like the simple REPL for Go. This is
not a wrapper for working with play.golang.org from Emacs (it is already did by original go-mode),
this is complete alternative for setting local playground inside Emacs without using browser for
experiments with code snippets.

Web playground at play.golang.org is a nice idea. It has not require setup and it executes your
code in a restricted environment where you can't damage you system. But it brings many restrictions
too especially when you want use additional helpers for working with your code: formatters, code
completers etc. Emacs offers many such tools for Go development so it is the most comfortable play
with snippets inside Emacs instead of rude webeditor. Of course local playground requres you to
setup Go environment but if you developing in Go (or you want to do it) you are anyway would need
to setup `go-mode` and helper tools for working with code in a comfortable way.

## Features
* uses [go-mode](https://github.com/dominikh/go-mode.el) with all your plugins (like lsp, linters and so on)
* allow to import external packages
* keeps the library of snippets under a customized root path
* support multiple source filcces (or subpackages) for a snippet
* compatible with Go modules

## Install
First install `go-mode` and `gotest` — these are mandatory. Setup any additional tools you want
for Golang (see Emacs Wiki for guides or google for "emacs+golang"). Minimal way is install `goimports` that automatically add import clauses to your snippets. For a fully functional Go IDE look at `lsp` and supplementary packages.

Install `go-playground` from MELPA:

	M-x package-install RET go-playground

Or:

```
(use-package go-playground)
```

If you want to share snippets use `go-play-buffer` from `go-mode`.
Install `gist-buffer` from MELPA if you want publish gists on github.com.

## Usage

### Quick start

1. From any mode run `M-x go-playground` for start a new playground buffer filled with basic
   template for `main` package (see the picture below).
1. Add your code then press `Ctl-Return` (it bound to `go-playground-exec` command). It will save,
   compile and exec the snippet code.
1. When you played enough with this snippet just run `M-x go-playground-rm`. It will remove the
   current snippet with its directory and all files.

Mode not offers default bindings except `Ctl-Return`. It just lefts the space for you.

For easy running the template declares `main` package with defined `main()` function. Each snippet
saved to its own directory (named by timestamp by default). Remember `go-playground` runs the
compiler as `go run *.go` so any sources from the snippet directory will be included.

### List of interactive functions
| Function name            | Description                                                                |
|--------------------------|----------------------------------------------------------------------------|
| `go-playground`          | Create a new playground buffer with basic template for the `main` package. |
| `go-playground-download` | Download the snippet from the URL at play.golang.org.                      |
| `go-playground-exec`     | Save, compile and run the code of the snippet.                             |
| `go-playground-cmd`      | Save the code then prompts for the command (compile-mode used).            |
| `go-playground-upload`   | Upload the buffer to play.golang.org and return the short URL.             |
| `go-playground-rm`       | Remove the snippet with its directory with all files.                      |

### List of customizable variables
| Function name                    | Description                                                                                                                                                                             |
|----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `go-playground-ask-filename`     | Ask for the snippet filename on a new playground creation. By default it got name `snippet.go`.                                                                                         |
| `go-playground-basedir`          | Default directory where snippets dirs will placed. Each new snippet got a new dir under this. It has sense place the default directory under GOPATH. Var introduced in the release 1.1. |
| `go-playground-confirm-deletion` | Ask for confirmation before snippet deletion. It may be annoying so you can switch confirmations off. Var introduced in the release 1.2.                                                |
| `go-playground-compile-command`  | Run compiler with custom args or run any other additional commands. By default it runs two command in sequence "go mod tidy; go mod run ./..."                                          |
| `go-playground-init-command`     | Setup shell command that will be run once when a new snippet just created.                                                                                                              |
| `go-playground-compiler-args`    | That arguments should be passed for the compiler.                                                                                                                                       |
| `go-playground-pre-rm-hook`      | Hook that should be run before a snippet is removed.                                                                                                                                    |

Example screen after creation of a new snippet:

![screenshot](playground-screenshot.png)

## Using with `lsp-mode`
If you use `lsp-mode`, you can add a hook in your init file to cleanup the workspace when a snippet
is removed, for example:
```
	(defun my/go-playground-remove-lsp-workspace () (when-let ((root (lsp-workspace-root))) (lsp-workspace-folders-remove root)))
	(add-hook 'go-playground-pre-rm-hook #'my/go-playground-remove-lsp-workspace)
```
## Snippet with multiple files and subpackages

Explained here [#19](https://github.com/grafov/go-playground/issues/19).

## Similar projects

* Try [go-scratch](https://github.com/shosti/go-scratch.el) it even simplier than
  `go-playground`. But it prevents you from using many of tools because it not keeps code in files.
* [gorepl-mode](https://github.com/manute/gorepl-mode) is REPL for Go, helpful for interactive
  research of code.

## Licence

Under terms of GPL v3. See LICENSE file.

## Future plans and accepting contributions

I don't want to make this package much complex. Most of `go-playground` functions depends on other
packages around `go-mode`.  And it is good way.  But one thing I want to add is snippet list buffer
where you can easily find and edit/delete snippets of your library.  Bugfixes and small features
accepted as well.
