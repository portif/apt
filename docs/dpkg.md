# dpkg

Abreviação para *Debian PacKaGe*.

Sistema gerenciador de baixo nível para manipulação de pacotes. Comandos básicos:


| Ação                              | Comando                         |
| --------------------------------- | ------------------------------- |
| Instalação                        | `dpkg -i <arquivo.deb>`         |
| Informação                        | `dpkg -I <arquivo.deb>`         |
| Remoção                           | `dpkg -r <pacote>`              |
| Listagem de pacotes               | `dpkg -l`                       |
| Listagem de pacotes               | `dpkg --get-selections`         |
| Listagem do conteúdo de um pacote | `dpkg -L <pacote>`              |
| Pacote de um arquivo              | `dpkg -S /caminho/para/arquivo` |

