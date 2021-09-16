# snap

> **Snaps** são pacotes de aplicativos para desktop, nuvem e IoT fáceis de instalar, seguros, entre plataformas e sem dependências. Os snaps podem ser descobertos e instalados na Snap Store, a loja de aplicativos para Linux com um público de milhões.
> 
> Um **snap** é um pacote de um aplicativo e suas dependências que funciona sem modificação nas distribuições Linux.
> 
> **Snapd** é o serviço de segundo plano que gerencia e mantém seus snaps, automaticamente.
> 
> O **Snap Store** fornece um local [...] para os usuários navegar e instalar o software que desejam. 
> Fonte: <https://snapcraft.io/about>

Para instalar pacotes por meio do comando `snap`, antes é necessário instalar o pacote `snapd`, o serviço (*daemon*) do `snap`.

Segue a ajuda do comando `snap`:

```
O comando snap permite instalar, configurar, atualizar e remover snaps.
Os Snaps são pacotes que funcionam em muitas distribuições Linux diferentes,
permitindo a entrega e operação seguras das aplicações e utilitários mais recentes.

Uso: snap <command> [<options>...]

Os comandos geralmente usados podem ser classificados da seguinte forma:

      Noções Básicas: find, info, install, remove, list
             ...mais: refresh, revert, switch, disable, enable, create-cohort
           Histórico: changes, tasks, abort, watch
             Daemons: services, start, stop, restart, logs
          Permissões: connections, interface, connect, disconnect
        Configuração: get, set, unset, wait
  Pseudónimos de App: alias, aliases, unalias, prefer
               Conta: login, logout, whoami
           Snapshots: saved, save, check-snapshot, restore, forget
         Dispositivo: model, reboot, recovery
     Desenvolvimento: download, pack, run, try
           ... Other: warnings, okay, known, ack, version

Para obter mais informações sobre um comando, execute 'snap help <command>'.
Para um breve resumo de todos os comandos, execute 'snap help --all'.
```