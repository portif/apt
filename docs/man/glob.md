GLOB {#glob align="center"}
====

[NOME](#NOME)\
[DESCRIÇÃO](#DESCRIÇÃO)\
[CASAMENTO DE CORINGAS](#CASAMENTO%20DE%20CORINGAS)\
[NOMES DE CAMINHOS](#NOMES%20DE%20CAMINHOS)\
[LISTAS VAZIAS](#LISTAS%20VAZIAS)\
[NOTAS](#NOTAS)\
[VEJA TAMBÉM](#VEJA%20TAMBÉM)\
[TRADUZIDO POR LDP-BR em
21/08/2000.](#TRADUZIDO%20POR%20LDP-BR%20em%2021/08/2000.)\

------------------------------------------------------------------------

NOME []{#NOME}
--------------

glob - Caminhos de diretórios de englobamento

DESCRIÇÃO []{#DESCRIÇÃO}
------------------------

Muito tempo atrás, no Unix V6, havia um programa */etc/glob* que poderia
expandir os padrões de coringas. Logo em seguida, isso se tornaria
embutido no interpretador de comandos.

Nos dias de hoje, também há uma rotina de biblioteca **glob**(3) que
realizará esta função para um programa de usuário.

As regras são as que seguem (POSIX 1003.2, 3.13).

CASAMENTO DE CORINGAS []{#CASAMENTO DE CORINGAS}
------------------------------------------------

Uma cadeia de caracteres é um padrão de coringas se contiver um ou mais
caracteres '?', '\*' ou '\['. Englobamento é a operação que expande um
padrão de coringas para uma lista de nomes de caminhos que casam com o
padrão. Casamento é definido por:

Um '?' (não entre colchetes) casa com qualquer caractere unitário.

Um '\*' (não entre colchetes) casa com qualquer string, incluindo uma
cadeia de caracteres vazia.

**Classes de caracteres**\
Uma expressão '\[\...\]' onde o primeiro caractere depois do primeiro
'\[' não é um '!' que casa com um caractere unitário, desde que seja um
dos caracteres de dentro dos colchetes. A string cercada pelos colchetes
não pode estar vazia: portanto '\]' é um caractere permitido entre os
colchetes, desde que seja o primeiro caractere. Portanto, '\[\]\[!\]'
casa com os três caracteres '\[', '\]' e '!'.)

**Faixas**\
Há uma convenção especial: dois caracteres separados por '-' denotam uma
faixa. (Portanto, '\[A-Fa-f0-9\]' é equivalente a
'\[ABCDEFabcdef0123456789\]'.) É possível incluir '-' com seu
significado literal ao colocá-lo em primeiro ou em último entre os
caracteres dentro dos colchetes. (Portanto, '\[\]-\]' casa apenas com os
dois caracteres '\]' e '-', e '\[\--/\]' casa com os três caracteres
'-', '.', '/'.)

**Complementação**\
Uma expressão '\[!\...\]' casa com um caractere unitário, desde que seja
um caractere não presente na expressão obtida pela remoção do primeiro
'!'. (Portanto, '\[!\]a-\]' casa com qualquer caractere unitário, exceto
'\]', 'a' e '-'.)

É possível remover o significado especial de '?', '\*' e '\['
precedendo-os por uma barra invertida, ou, caso seja parte de uma linha
de comando do shell, cercando-os com aspas. Entre colchetes, estes
caracteres respondem por eles mesmos. Portanto, '\[\[?\*\\\]' casa com
os quatro caracteres '\[', '?', '\*' e '\\'.

NOMES DE CAMINHOS []{#NOMES DE CAMINHOS}
----------------------------------------

Englobamento é a aplicação de cada um dos componentes de um nome de
caminho separadamente. Um '/' em um nome de caminho não pode casar com
um coringa '?' ou '\*', ou com uma faixa como '\[.-0\]'. Uma faixa não
pode conter um caractere '/' explícito; isto levaria a um erro de
sintaxe.

Se um nome de arquivo começa com um '.', este caractere deve ser casado
explicitamente. (Portanto, 'rm \*' não removerá .profile, e 'tar c \*'
não arquivará todos os seus arquivos: 'tar c .' é melhor.)

LISTAS VAZIAS []{#LISTAS VAZIAS}
--------------------------------

A bela e simples regra dada acima: 'expanda um padrão de coringas na
lista de caminhos de diretório de casamento' foi a definição padrão do
Unix. Ela permite padrões que se expandam para uma lista vazia, como em

+-----------------------+-----------------------+-----------------------+
|                       |                       | xv -wait 0 \*.gif     |
|                       |                       | \*.jpg                |
+-----------------------+-----------------------+-----------------------+

onde talvez nenhum arquivo \*.gif esteja presente (e isto não é um
erro). Porém, o POSIX requer que um padrão de coringas seja deixado
inalterado quando estiver sintaticamente incorreto, ou a lista de nomes
de caminhos esteja vazia. Com *bash* pode-se forçar o comportamento
clássico, setando-se *allow\_null\_glob\_expansion=true*.

(Problemas similares ocorrem em toda a parte. Por exemplo, onde há em
scripts antigos

+-----------------------------------+-----------------------------------+
|                                   | rm 'find . -name \"\*\~\"'        |
+-----------------------------------+-----------------------------------+

os novos scripts requerem

+-----------------------------------+-----------------------------------+
|                                   | rm -f nosuchfile 'find . -name    |
|                                   | \"\*\~\"'                         |
+-----------------------------------+-----------------------------------+

para evitar mensagens de erro de *rm* chamado com uma lista de
argumentos vazia.)

NOTAS []{#NOTAS}
----------------

**Expressões regulares**\
Note que padrões de coringas não são expressões regulares, apesar de que
são um pouco similares. Primeiramente, eles casam com nomes de arquivos
em vez de texto, e em segundo lugar, as convenções não são as mesmas:
por exemplo, em uma expressão regular '\*' significa zero ou mais cópias
da coisa precedente.

Agora que as expressões regulares têm expressões com colchetes, onde a
negação é indicada por um '\^', o POSIX declarou que o efeito de um
padrão de coringa '\[\^\...\]' é indefinido.

**Classes de caracteres e Internationalização**\
Obviamente, faixas significavam originalmente as faixas ASCII, de forma
que '\[ -%\]' significa '\[ !\"\#\$%\]' e '\[a-z\]' significa \"qualquer
letra minúscula\". Algumas implementações Unix generalizaram isso, de
tal forma que que uma faixa X-Y significa o conjunto de caracteres com
código entre o código de X e o de Y. Porém, isso requer que o usuário
saiba o código do caractere em uso no sistema local, e além disso, não é
conveniente se a seqüência de conferência para o alfabeto local difere
da ordenação dos códigos de caractere. Portanto, POSIX estendeu
grandemente a notação de colchetes, tanto nos padrões de coringas quanto
nas expressões regulares. Anteriormente, nós vimos três tipos de itens
que podem ocorrer em uma expressão em colchetes: (i) a negação, (ii)
caracteres unitários explicitados e (iii) faixas. POSIX especifica
faixas de uma forma internacionalmente mais útil, e acrescenta mais três
tipos:

\(iv) Faixas X-Y compreendem todos os caractees que caem entre X e Y
(inclusive) na seqüência de conferência corrente, como definido pela
categoria LC\_COLLATE no locale corrente.

\(iv) Classes nomeadas de caracteres, como\
\[:alnum:\] \[:alpha:\] \[:blank:\] \[:cntrl:\]\
\[:digit:\] \[:graph:\] \[:lower:\] \[:print:\]\
\[:punct:\] \[:space:\] \[:upper:\] \[:xdigit:\]\
, de forma que se pode dizer '\[\[:lower:\]\]' (minúsculo) em vez de
'\[a-z\]', e funciona na Dinamarca também, onde há três letras depois do
'z' no alfabeto. Essas classes de caracteres são definidas pela
categoria LC\_CTYPE na localização atual.

\(v) Símbolos de conferência, como '\[.ch.\]' ou '\[.a-acute.\]', onde a
string entre '\[.' e '.\]' é um elemento de conferência definido na
localização atual. Note que este pode ser um elemento multi-caractere.

\(vi) Expressões de classes de equivalência, como '\[=a=\]', onde a
string entre '\[=' e '=\]' é um elemento de conferência qualquer da sua
classe de equivalência, como é definido no locale corrente. Por exemplo,
'\[\[=a=\]\]' deve ser equivalente a '\[aáàäâ\]' (cuidado: Latin-1
aqui), ou seja, a
'\[a\[.a-acute.\]\[.a-grave.\]\[.a-umlaut.\]\[.a-circumflex.\]\]'.

VEJA TAMBÉM []{#VEJA TAMBÉM}
----------------------------

**sh**(1), **glob**(3), **fnmatch**(3), **locale**(7), **regex**(7)

TRADUZIDO POR LDP-BR em 21/08/2000. []{#TRADUZIDO POR LDP-BR em 21/08/2000.}
----------------------------------------------------------------------------

Rubens de Jesus Nogueira \<darkseid99\@usa.net\> (tradução) André L.
Fassone Canova \<lonelywolf\@blv.com.br\> (revisão)

------------------------------------------------------------------------
