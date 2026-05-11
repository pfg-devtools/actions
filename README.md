# actions

Shared GitHub Actions used by PFG Dev Tools repositories and by services
scaffolded from [`service-templates`](https://github.com/pfg-devtools/service-templates).

Pin composites at `@main` while iterating; pin a commit SHA in production
workflows.

## Composite actions

| Action | What it does |
|--------|----------------|
| [`setup-go`](setup-go/action.yml) | `actions/setup-go` with module cache |
| [`setup-python`](setup-python/action.yml) | `actions/setup-python` with pip cache |
| [`setup-terraform`](setup-terraform/action.yml) | HashiCorp `setup-terraform` (default 1.7.x) |

### setup-go

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pfg-devtools/actions/setup-go@main
        with:
          go-version: "1.23"
      - run: go test ./...
```

Inputs: `go-version` (default `1.23`).

### setup-python

```yaml
- uses: pfg-devtools/actions/setup-python@main
  with:
    python-version: "3.12"
```

### setup-terraform

```yaml
- uses: pfg-devtools/actions/setup-terraform@main
  with:
    terraform-version: "1.7.5"
- run: terraform fmt -check -recursive
```

## Layout

```
setup-go/           composite action
setup-python/
setup-terraform/
docs/               short consumer notes
Makefile            local lint entrypoint
```

## Consuming from another repo

1. Reference `pfg-devtools/actions/<name>@<ref>` from a workflow in the
   consumer repo.
2. Keep secrets in the consumer. Nothing in this repo should need org
   secrets.
3. Open a PR here if an input default needs to change; do not fork the
   composite into application repos.

Related: [`tf-github`](https://github.com/pfg-devtools/tf-github) for
branch protection, [`pre-commit-hooks`](https://github.com/pfg-devtools/pre-commit-hooks)
for local checks.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Report secrets out of band
([SECURITY.md](SECURITY.md)).
