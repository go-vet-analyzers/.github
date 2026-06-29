<p align="center"><img src="https://raw.githubusercontent.com/go-vet-analyzers/brand/main/social/go-vet-analyzers.png" alt="go-vet-analyzers" width="720"></p>

# go-vet-analyzers

Pure-Go [`go vet`](https://pkg.go.dev/cmd/vet) analyzers — small, dependency-light
[`golang.org/x/tools/go/analysis`](https://pkg.go.dev/golang.org/x/tools/go/analysis)
tools that turn project conventions into checks that **fail CI** instead of
relying on human discipline.

Each analyzer ships as a `singlechecker` binary you can drop into any repo's
workflow:

```sh
go install github.com/go-vet-analyzers/<analyzer>/cmd/<analyzer>@latest
go vet -vettool="$(go env GOPATH)/bin/<analyzer>" ./...
```

## Analyzers

| Repo | What it checks |
| --- | --- |
| [`nonnil`](https://github.com/go-vet-analyzers/nonnil) | Enforces the **Null-Object invariant** — fails the build when an interface declaring `IsNull() bool` is returned, assigned, or stored (struct field / slice / map value) as a bare `nil`, the gap that lets a nil-dereference slip back in. |
| [`respondto`](https://github.com/go-vet-analyzers/respondto) | Makes **reflective method-name strings compile-checked** — flags a `RespondTo("Method")` whose string literal names a method the resolved type does not have, catching typo'd or stale dynamic sends at build time. |

## Usage

For example, to wire `nonnil` into a repo's CI:

```yaml
  - run: go install github.com/go-vet-analyzers/nonnil/cmd/nonnil@latest
  - run: go vet -vettool="$(go env GOPATH)/bin/nonnil" ./...
```

Both analyzers are deliberately conservative: they report only when they can be
certain, so false positives stay near zero.

## License

BSD-3-Clause © the go-vet-analyzers authors.
