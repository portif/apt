# Comando `apt autoremove`

O comando `apt autoremove` é usado para remover pacotes que foram instalados automaticamente para satisfazer dependências de outros pacotes e que já não são necessários porque as dependências alteraram ou porque os pacotes que precisavam delas foram entretanto removidos.

## Exemplos

Simulação da remoção automática de pacotes:
- `apt -s autoremove`

Saída:
```
NOTE:   Isto é apenas uma simulação!
        o apt necessita de privilégios de root para a execução real.
        Tenha em mente que o acesso exclusivo está desabilitado,
        por isso não confie na relevância da real situação actual!
Lendo listas de pacotes... Pronto
Construindo árvore de dependências
Lendo informação de estado... Pronto
Os pacotes a seguir serão REMOVIDOS:
  git-man liberror-perl libstd-rust-1.47
0 pacotes atualizados, 0 pacotes novos instalados, 3 a serem removidos e 0 não atualizados.Remv git-man [1:2.25.1-1ubuntu3.2]
Remv liberror-perl [0.17029-1]
Remv libstd-rust-1.47 [1.47.0+dfsg1+llvm-1ubuntu1~20.04.1]
```

Remoção automática de pacotes:
- `apt autoremove`

## Seus exemplos

Deixe seus exemplos na área de comentários.