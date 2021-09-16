# Comando `apt remove`

O comando `apt remove` remove um ou mais pacotes especificados via [regex](../man/regex.md), [glob](../man/glob.md) ou por correspondência exata.

Remover um pacote remove todos os dados empacotados, mas deixa ficar arquivos (modificados) de configuração do usuário geralmente pequenos, para o caso da remoção ter sido um acidente. Apenas fazer um pedido de instalação para o pacote removido acidentalmente irá restaurar a sua função como estava anteriormente. Por outro lado você pode ver-se livre desses restos ao chamar **purge** mesmo em pacotes já removidos. Note que isto não afeta nenhuns dados ou configurações armazenados no seu directório home pessoal.

## Exemplos

Remover um pacote:

- `apt remove git`

No exemplo anterior, o arquivo `/etc/bash_completion.d/git-prompt` foi mantido no sistema.

Remover vários pacotes:

- `apt remove ipython3 bpython3`

Remover um pacote e expurgar (*purge* em inglês) seu(s) arquivo(s) de configuração:

- `apt purge git`

## Seus exemplos

Deixe seus exemplos na área de comentários.