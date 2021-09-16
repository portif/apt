# Comando `apt list`

O comando `apt list` mostra uma lista de pacotes que satisfaçam certos critérios. Suporta padrões **glob**(7) para coincidir com nomes de pacotes assim como opções para listar instalados (`--installed`), atualizáveis (`--upgradeable`) ou todas as versões disponíveis (`--all-versions`).

## Exemplos

Nome de pacote específico:

- `apt list  markdown`

Nome de pacotes que terminam com *markdown*:

- `apt list *markdown`

Nome de pacotes que começam com *markdown*:

- `apt list markdown*`


Nome de pacotes que tenham *markdown* no início, meio ou fim:

- `apt list *markdown*`

## Seus exemplos

Deixe seus exemplos na área de comentários.