Golang notes,


### go run
Executes the main program, if it main package has multiple files, remember
that, simply running the main.go will not work.

Read the go run help page
```
  go help run
```
Use, the import path, or pattern or file path or simply the list of go files
in the package
```
 go run .
 go run *.go
```
Again, read the documentation!


### go importpath, gopath, packages
The help documents for the above commands must be read.

### internal packages
Understand the internal/ packages directory, who is allowed to import the internal
packages
```bash
go help gopath
```
The internal packages can only be imported from the packages, which are rooted as the
parent of internal directory
