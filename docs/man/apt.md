# APT

## NOME

apt - interface de linha de comandos

## SINOPSE

```
 apt [-h] [-o=config_string] [-c=arquivo_de_configuração] [-t=lançamento-alvo] [-a=arquitectura] {list | search | show | update |
           install pkg [{=número_de_versão_do_pacote | /lançamento-alvo}]...  | remove pkg...  | upgrade | full-upgrade | edit-sources | {-v | --version} |
           {-h | --help}}
```

## DESCRIÇÃO

O **apt** disponibiliza uma interface de linha de comandos de alto nível
para o sistema de gestão de pacotes. Destina-se a ser uma interface para
usuário final.

Muito como o próprio **apt**, o seu manual destina-se a ser uma
interface de usuário final e como tal apenas menciona os comandos
mais usados e parte das opções para não duplicar informação em múltiplos
locais e em parte para evitar saturar os leitores com demasiadas
opções e detalhes.

- **update**

    O comando `apt update` é usado para baixar informação de pacotes de todas as fontes configuradas. Outros comandos operam com estes dados para , por exemplo, executar atualizações de pacotes ou procurar e mostrar detalhes acerta de todos os pacotes disponíveis para instalação.

- **upgrade**

    O comando `apt upgrade` é usado para instalar atualizações disponíveis de todos os pacotes atualmente instalados no sistema a partir das fontes configuradas via **sources.list**(5). Se necessário para satisfazer dependências serão instalados novos pacotes, mas pacotes existentes nunca serão removidos. Se a atualização de um pacote necessitar da remoção de um pacote instalado, a atualização deste pacote não será executada.

- **full-upgrade**

    O comando `apt full-upgrade` executa a função de atualização (*upgrade*) mas irá remover pacotes atualmente instalados se tal for necessário para atualizar o sistema como um todo.

- **install**, **reinstall**, **remove**, **purge**

    Executa a ação requisitada em um ou mais pacotes especificados via **regex**(7), **glob**(7) ou por correspondência exata. A ação requisitada pode ser sobreposta para pacotes específicos ao acrescentar um mais (`+`) ao nome do pacote para instalar esse pacote ou um menos (`-`) para o remover.

    Pode ser selecionada para instalação uma versão específica de um pacote ao adicionar ao nome do pacote o símbolo igual (`=`) e a versão do pacote a selecionar. Alternativamente a versão de um lançamento específico pode ser selecionada ao adicionar ao nome do pacote uma barra de divisão (`/`) e o nome de código (buster, bullseye, sid \...) ou o nome de suite (stable, testing, unstable). Isto irá também selecionar versões a partir deste lançamento para as dependências deste pacote se necessário para satisfazer o pedido.

    Remover um pacote remove todos os dados empacotados, mas deixa ficar arquivos (modificados) de configuração do usuário geralmente pequenos, para o caso da remoção ter sido um acidente. Apenas fazer um pedido de instalação para o pacote removido acidentalmente irá restaurar a sua função como estava anteriormente. Por outro lado você pode ver-se livre desses restos ao chamar **purge** mesmo em pacotes já removidos. Note que isto não afeta nenhuns dados ou configurações armazenados no seu directório home pessoal.

- **autoremove**

    O comando `apt autoremove` é usado para remover pacotes que foram instalados automaticamente para satisfazer dependências de outros pacotes e que já não são necessários porque as dependências alteraram ou porque os pacotes que precisavam delas foram entretanto removidos.

    Você deve verificar que a lista não inclua aplicações de que passou a gostar apesar de terem sido instaladas apenas como uma dependência de outro pacote. Você pode marcar tal pacote como instalado manualmente ao usar **apt-mark**(8). Os pacotes que você instalou explicitamente via comando **install** também nunca são propostos para remoção automática.

- **satisfy**

    O comando `apt satisfy` satisfies dependency strings, as used in Build-Depends. It also handles conflicts, by prefixing an argument with \"Conflicts: \".

    Examplo: `apt satisfy "foo, bar (>= 1.0)" "Conflicts: baz, fuzz"`

- **search**

    O comando `apt search` pode ser usado para procurar por termo(s) **regex**(7) fornecidos na lista de pacotes disponíveis e apresentar correspondências. Isto pode, por exemplo, ser útil se procura pacotes com uma característica específica. Se está à procura de um pacote que inclua um arquivo específico tente o **apt-file**(1).

- **show** (**apt-cache**(8))

    O comando `apt show` mostra informação sobre o(s) pacote(s) indicados incluindo as suas dependências, tamanho de instalação e de download, fontes a partir das quais o pacote está disponível, a descrição do conteúdo dos pacotes e muito mais. Pode, por exemplo, ser útil para ver esta informação antes de permitir ao **apt**(8) remover um pacote ou enquanto procura por novos pacotes para instalar.

- **list**

    O comando `apt list` mostra uma lista de pacotes que satisfaçam certos critérios. Suporta padrões **glob**(7) para coincidir com nomes de pacotes assim como opções para listar instalados (**\--installed**), atualizáveis (**\--upgradeable**) ou todas as versões disponíveis (**\--all-versions**).

- **edit-sources**

    O comando `apt edit-sources` permite-lhe editar os seus arquivos **sources.list**(5) no seu editor de texto preferido enquanto também disponibiliza verificações básicas aos mesmos.

## UTILIZAÇÃO DE SCRIPTS E DIFERENÇAS COM OUTRAS FERRAMENTAS DO APT

A linha de comandos do **apt**(8) foi desenhada como ferramenta de
usuário final e pode variar o comportamento entre versões. Apesar de
tentar não perder a compatibilidade com versões anteriores isto não é
garantido se uma alteração parecer benéfica para uso interactivo.

Todas as funcionalidades do **apt**(8) estão também disponíveis em
ferramentas dedicadas ao APT como **apt-get**(8) e **apt-cache**(8). O
**apt**(8) apenas varia o valor predefinido de algumas opções (veja
**apt.conf**(5) e especialmente o âmbito Binário). Portanto você deverá
preferir usar estes comandos (potencialmente com algumas opções
adicionais activas) nos seus scripts pois eles mantêm compatibilidade
com versões anteriores sempre que possível.

## VEJA TAMBÉM

**apt-get**(8), **apt-cache**(8), **sources.list**(5), **apt.conf**(5),
**apt-config**(8), O guia de usuários do The APT em
/usr/share/doc/apt-doc/, **apt\_preferences**(5), o Howto do APT.

## DIAGNÓSTICO

**apt** devolve zero na operação normal, 100 decimal em erro.

## BUGS

**página de bugs do APT** [\[1\]]{.small} . Se deseja reportar um bug no
APT, por favor veja /usr/share/doc/debian/bug-reporting.txt ou o comando
**reportbug**(1).

## TRADUÇÂO

A tradução Portuguesa foi feita por Américo Monteiro
\<a\_monteiro\@netcabo.pt\> de 2009 a 2012. A tradução foi revista pela
equipa de traduções portuguesas da Debian \<traduz\@debianpt.org\>.

Note que este documento traduzido pode conter partes não traduzidas.
Isto é feito propositadamente, para evitar perdas de conteúdo quando a
tradução está atrasada relativamente ao conteúdo original.

## AUTOR

Equipe do APT.

## NOTAS

1. página de bugs do APT: <http://bugs.debian.org/src:apt>
