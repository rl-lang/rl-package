# rl-package

Packages a `.rl` file into self-contained binaries (Linux/macOS/Windows matrix in your workflow).

```yaml
- uses: rl-lang/rl-package@main
  with:
    file: src/main.rl
    output: program
```

| Input | Default |
|---|---|
| `version` | `latest` |
| `file` | *(required)* |
| `output` | `program` |
| `vm` | `false` |
