# Comando `apt-file`

Uso:

- `apt-file [opções] ação [padrão]`
- `apt-file [opções] -f ação <arquivo>`
- `apt-file [opções] -D ação <debfile>`

Opções de padrões:


| Opção longa         | Opção curta | Descrição                                                      |
| ------------------- | :---------: | -------------------------------------------------------------- |
| `--fixed-string`    |    `-F`     | Do not expand padrão                                           |
| `--from-deb`        |    `-D`     | Use arquivo list of .deb package(s) as padrãos; implies -F     |
| `--from-file`       |    `-f`     | Read padrãos from arquivo(s), one per line (use '-' for stdin) |
| `--ignore-case`     |    `-i`     | Ignore case distinctions                                       |
| `--regexp`          |    `-x`     | padrão is a regular expression                                 |
| `--substring-match` |    `  `     | padrão is a substring (no glob/regex)                          |

```
Search filter opções:
======================

    --architecture     -a  <arch>       Use specific architecture [L]
    --index-names      -I  <names>      Only search indices listed in <names> [L]
    --filter-suites        <suites>     Only search indices for the listed <suites> [L]
                                        (E.g. "unstable")
    --filter-origins       <origins>    Only search indices from <origins> [L]
                                        (E.g. "Debian")

Other opções:
==============

    --config           -c <arquivo>        Parse the given APT config arquivo [R]
    --option           -o <A::B>=<V>    Set the APT config option A::B to "V" [R]
    --package-only     -l               Only display packages name
    --verbose          -v               run in verbose mode [R]
    --help             -h               Show this help.
                       --               End of opções (necessary if padrão
                                        starts with a '-')

[L]: Takes a comma-separated list of values.
[R]: The option can be used repeatedly

ação:
    list|show          <padrão>        List files in packages
    list-indices                        List indices configured in APT.
    search|find        <padrão>        Search files in packages
    update                              Fetch Contents files from apt-sources.
```