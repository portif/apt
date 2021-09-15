# REGEX

## NOME


regex - expressões regulares do POSIX 1003.2

## DESCRIÇÃO


Expressões regulares (ERs), como definidas no POSIX 1003.2, vêm em duas
formas: REs modernas (grosseiramente, aquelas de *egrep*; o 1003.2 chama
essas de ERs "estendidas") e ERs obsoletas (grosseiramente,
aquelas de **ed**(1); REs "básicas" do 1003.2). REs obsoletas existem
principalmente por causa de compatibilidade retrógrada em alguns
programas antigos; eles serão discutidos no final. O 1003.2 deixa alguns
aspectos da sintaxe e da semântica das ERs em aberto; portáveis para
outras implementações do 1003.2.

Uma (moderna) ER é uma† ou mais† *ramificações*† não-vazias† , separadas
por '\|'. Ele encontra tudo o que casa com uma das ramificações.

Uma ramificação é um† ou mais *pedaços*, concatenados. Ele encontra um
casamento para o primeiro, seguido por um casamento para o segundo, etc.

Um pedaço é um *átomo* possivelmente seguido por um † '\*', '+', Um
átomo seguido por '\*' encontra uma sequência de 0 ou mais casamentos do
átomo. Um átomo seguido por '+' encontra uma sequência de 1 ou mais
casamentos do átomo. Um átomo seguido por '?' encontra uma sequência de
0 ou 1 casamento do átomo.

Uma composição é '{' seguido por um inteiro decimal sem sinal,
possivelmente seguido por ',' , possivelmente seguido por outro inteiro
decimal sem sinal, sempre seguido por '}'. Os inteiros devem estar entre
0 e RE\_DUP\_MAX (255†) inclusive, e se houver dois deles, o primeiro e
não pode exceder o segundo. Um átomo seguido por uma composição contendo
um inteiro *i*, sem vírgula, encontra uma sequência de *i* ou mais
casamentos do átomo. Um átomo seguido por uma composição contendo dois
inteiros *i* encontra uma sequencia de um mais *i* casamentos do átomo.
Um átomo seguido por uma composição contendo dois inteiros *i* e *j*
encontra uma sequência de *i* até *j* (inclusive) casamentos do átomo.

Um átomo é uma expressão regular englobada em '()' (encontrando um
casamento para a expressão regular), um conjunto vazio de '()'
(encontrando a string nula)†, uma *expressão agrupada* (ver abaixo), '.'
(encontrando qualquer caractere simples), '\^' (encontrando a string
nula no começo de uma linha), '\$' (encontrando a string nula no fim de
uma linha), um '\\' seguido de um dos caracteres (encontrando aquele
caractere tomado como um caractere ordinário), um '\\' seguido por
qualquer outro caractere† (encontrando aquele caractere tomado como um
caractere ordinário, como se o '\\' não estivesse presente†), ou um
caractere simples se outro significado (encontrando qualquer caractere).
Um '{' seguido por um caractere diferente de um dígito é um caractere
ordinário, não o início de uma composição†. É ilegal terminar uma ER com
'\\'.

Uma *expressão agrupada* é uma lista de caracteres englobados por um
'\[\]'. Ele normalmente encontra qualquer caractere simples da lista
(mas veja abaixo). Se a lista começa com '\^', ele encontra qualquer
caractere simples (mas veja abaixo) que não venha do resto da lista. Se
dois caracteres na lista são separados por '-', isto é uma abreviação
para a *range* completa de caracteres entre aqueles dois (inclusive) na
sequência de combinação, por exemplo, '\[0-9\]' em ASCII encontra
qualquer dígito decimal. É ilegal † que duas faixas compartilhem um
ponto final, por exemplo, 'a-c-e'. As faixas são muito dependentes de
sequência de combinação, e programas portáveis devem evitar confiar
nelas.

Para incluir um literal '\]' na lista, torne-o o primeiro caractere
(seguindo um possível '\^'). Para incluir um literal '-', torne-o o
primeiro ou o último caractere, ou o segundo ponto final da faixa. Para
usar um literal '-' como o primeiro ponto final da faixa, englobe-o
entre '\[.' e '.\]' para torná-lo um elemento de combinação (veja
abaixo). Com a exceção destas e algumas combinações usando '\[' (veja os
próximos parágrafos), todos os outros caracteres especiais, incluindo
'\\', perdem seu significado especial dentro de uma expressão agrupada.

Dentro de uma expressão agrupada, um elemento de combinação (um
caractere, uma sequência multi-caractere que combina como se fosse um
caractere simples, ou um nome de sequência de combinação se for o caso)
englobado entre '\[.' e '.\]' significa a sequência de caracteres
daquele elemento de combinação. A sequência é um elemento simples de uma
lista de expressões agrupada. Uma expressão agrupada contendo um
elemento de combinação multi-caractere pode, portanto, encontrar mais de
um caractere, por exemplo, se a sequência de combinação inclui um
elemento de combinação 'ch', então a ER '\[\[.ch.\]\]\*c' encontra os
primeiros cinco caracteres de 'chchcc'.

Dentro de uma expressão agrupada, um elemento de combinação englobado por '\[=' e '=\]' é uma classe equivalente, que significa uma sequência de caracteres com todos os elementos de combinação equivalentes a aquele, incluindo ele mesmo. (Se não houver outro elemento de combinação equivalente, o tratamento é como se os delimitadores fossem '\[.' e '.\]'.) Por exemplo, se o e \^ são os membros de uma classe equivalente, então '\[\[=o=\]\]', '\[\[=\^=\]\]', e '\[o\^\]' são todos sinônimos. Uma classe equivalente não pode † ser um ponto final de uma faixa.

Dentro de uma expressão agrupada, o nome de uma *classe de caractere* englobado por '\[:' e ':\]' significa a lista de todos os caracteres pertencente àquela classe. Os nomes padrão de classes de caracteres são:

| classe | classe | classe |
| ------ | ------ | ------ |
| alnum  | digit  | punct  |
| alpha  | graph  | space  |
| blank  | lower  | upper  |
| cntrl  | print  | xdigit |


Isto vale para as classes de caracteres definidas em **ctype**(3). Um
locale pode fornecer outros. Uma classe de caracteres não pode ser usada
como um ponto final de uma faixa.

Há dois casos especiais de expressões de colchetes: as expressões
'\[\[:\<:\]\]' e '\[\[:\>:\]\]' casam com a string nula no início e no
fim de uma palavra, respectivamente. Uma palavra é definida como uma
sequência de caracteres de palavras que não é precedida e nem seguida
por caracteres de palavra. Um caractere de palavra é um caractere
*alnum* (como definido por **ctype**(3)) ou um sublinhado. Esta é uma
extensão, compatível mas não específica do POSIX 1003.2, e deve ser
usada com cautela em softwares que pretendem ser portáveis para outros
sistemas.

Em um evento em que uma ER encontraria mais de uma substring de uma
string dada, a ER encontra aquela que inicia mais próxima do início da
string. Se a ER pode encontrar mais que uma substring começando naquele
ponto, ela encontra a mais longa. Sub-expressões também podem encontrar
a substring mais longa possível, sujeitando-se à limitação de o
casamento todo ser tão longo quanto possível, com sub-expressões
iniciando primeiro na ER tendo prioridade sobre aquelas iniciando
posteriormente. Note que sub-expressões de nível mais alto, portanto,
têm prioridade sobre suas sub-expressões componentes de nível mais
baixo.

Os comprimentos dos casamentos são medidos em caracteres, e não em
elementos de combinação. Uma string nula é considerada mais longa do que
um caso de não se encontrar nada. Por exemplo, quando '(.\*).\*' é
aplicado em 'abc' a sub-expressão entre parênteses casa todos os três
caracteres, e quando '(a\*)\*' é aplicado em 'bc' ambas as ERs inteiras
e a sub-expressão entre parênteses encontram a string nula.

Se casamento independente da caixa é especificado, o efeito é tal como
se todas as distinções de caixa sumissem do alfabeto. Quando um
alfabético que existe em múltiplos casos aparece como um caractere
ordinário fora de uma expressão de colchete, é transformado efetivamente
em uma expressão de colchetes contendo ambos os casos, por exemplo, 'x'
se torna '\[\^xX\]'. Quando ele aparece dentro de uma expressão de
colchetes, todos os casos equivalentes a ele são acrescentados à
expressão de colchetes, de forma que (por exemplo) '\[x\]' se torna
'\[xX\]' e '\[\^xX\]'.

Nenhum limite particular é imposto sobre o comprimento das ERs†.
Programas que pretendem ser portáveis não devem empregar ERs maiores de
256 bytes, pois uma implementação pode negar-se a aceitar tais ERs para
continuar compatível com o POSIX.

Expressões regulares obsoletas ("básicas") diferem em vários aspectos.
Os sinais '+', '\|' e '?' são caracteres ordinários e não há
equivalentes para suas funcionalidades. Os delimitadores para composição
são '\\{' and '\\}', com '{' e '}' por eles mesmos caracteres
ordinários. Os parênteses para sub-expressões aninhadas são '\\(' e
'\\)', com '(' e ')', por eles mesmos caracteres ordinários. O '\^' é um
caractere ordinário, exceto no começo de uma ER ou † no começo de uma
sub-expressão com parênteses, o '\$' é um caractere ordinário, exceto no
fim da ER ou † no fim da sub-expressão com parênteses, e '\*' é um
caractere ordinário se ele aparece no começo da ER ou no começo de uma
sub-expressão com parênteses (depois de um possível '\^' dianteiro).
Finalmente, há um novo tipo de átomo, uma *referência para trás*: casa
com a mesma sequência de caracteres casada pela sub-expressão de
parênteses (numerando sub-expressões pelas posições dos seus parênteses
abertos, da esquerda para a direita), tal que (por exemplo)
'\\(\[bc\]\\)\\1' case com 'bb' ou 'cc', mas não com 'bc'.

## VEJA TAMBÉM


**regex**(3)

POSIX 1003.2, seção 2.8 (Notação de Expressão Regular).

## PROBLEMAS


Ter dois tipos de ERs é uma devastação.

A especificação corrente do 1003.2 diz que ')' é um caractere ordinário
na ausência de um '(' não casado; este era um resultado não-intencional
de um erro de palavreamento, e mudanças são parecidas. Evite confiar
nela.

Referências para trás são uma destruição terrível, causando problemas
sérios em implementações eficientes. Elas também são definidas um pouco
vagamente (fazendo Evite usá-las).

A especificação 1003.2 para casamentos independentes de caixa é vaga. A
definição "um caso implica em todos os casos" dada acima é um consenso
corrente entre implementadores como a interpretação correta.

A sintaxe para os limites de palavra é incrivelmente feia.

## AUTOR


Esta página foi tomada do pacote regex de Henry Spencer.

## TRADUZIDO POR LDP-BR em 21/08/2000

- Rubens de Jesus Nogueira <darkseid99\@usa.net> (tradução) 
- André L. Fassone Canova <lonelywolf\@blv.com.br> (revisão)

