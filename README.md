# go-pathspec

<div align="left">
    <a href="https://github.com/karagenc/go-pathspec/actions/workflows/tests.yml"><img src="https://github.com/karagenc/go-pathspec/actions/workflows/tests.yml/badge.svg"></img></a>
    <a href="https://coveralls.io/github/karagenc/go-pathspec"><img src="https://coveralls.io/repos/github/karagenc/go-pathspec/badge.svg"></img></a>
    <a href="https://pkg.go.dev/github.com/karagenc/go-pathspec"><img src="https://pkg.go.dev/badge/github.com/karagenc/go-pathspec"></img></a>
</div><br>

go-pathspec is a library that implements gitignore-style pattern matching for paths and is fully compatible with Git's pathspec. Pathspec is a syntax used to specify a pattern for matching file paths in a command-line interface or a script.

As of writing, this is the only Go package that fully implements pathspec. For Python implementation, please refer to [python-pathspec](https://github.com/cpburnz/python-pathspec)

## Usage

`go get github.com/karagenc/go-pathspec`

```go
import "github.com/karagenc/go-pathspec"

p, _ := pathspec.FromLines(...)

p, _ := pathspec.FromFile("path/to/.gitignore")

r := bytes.NewBufferString("...")
p, _ := pathspec.FromReader(r)
```

### Match

**Note:** Append `/` to directories. Otherwise, patterns that end with `/` won't match — a pattern that ends with a slash indicates that it only matches with directories. It doesn't matter whether the path begins with `/` or `./`; they are trimmed.

```go
file := "main.exe"
if p.Match(file) {
    // This file is ignored.
}

dir := "build/"
if p.Match(dir) {
    // This directory is ignored.
}

err := filepath.WalkDir(path, func(path string, d fs.DirEntry, err error) error {
    if err != nil {
        return err
    }
    // By default `path` doesn't have a slash prefix.
    // If it's a directory, append it.
    if d.Type().IsDir() {
        path += "/"
    }
    if p.Match() {
        // This file/directory is ignored.
    }
    return nil
})
```

## Authors

- karagenc (https://github.com/karagenc)
- Sander van Harmelen (<sander@vanharmelen.nl>)
- Christian Rebischke (<chris@shibumi.dev>)
