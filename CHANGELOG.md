Para a realização do trabalho, foram escolhidas as linguagens Java (20) & C.
Foi utilizado a plataforma ANTLR LAB em conjunto a IDE Visual Studio Code.

Para a análise e comparação dos lexers, foram escolhidas as regras de reconhecimento de TEXTO, INTEIRO e REAL.





## C

### Para identificador de texto, é utilizado a regra:

StringLiteral
    : EncodingPrefix? '"' SCharSequence? '"'
    ;

fragment EncodingPrefix
    : 'u8'
    | 'u'
    | 'U'
    | 'L'
    ;

fragment SCharSequence
    : SChar+
    ;

fragment SChar
    : ~["\\\r\n]
    | EscapeSequence
    | '\\\n'   // Added line
    | '\\\r\n' // Added line
    ;

/* Feito mais cedo, ajustar

StringLiteral
    : EncodingPrefix? '"' SCharSequence? '"'
    | EncodingPrefix? 'R' RawStringLiteral
    ;

Ou seja, a regra aceita dois formatos de strings.
1. Aceita Strings normais, entre aspas ("String Normal").
2. E aceita Raw Strings (R"Raw String"), que e usado para evitar o escape de caracteres.
*/

### INTEGER

Constant
    : IntegerConstant
    | FloatingConstant
    //|   EnumerationConstant
    | CharacterConstant
    ;


### REAL 



## JAVA (20)

### Para identificador de texto, é utilizado a regra:

// §3.10.5 String Literals

StringLiteral: '"' StringCharacters? '"';

fragment StringCharacters: StringCharacter+;

fragment StringCharacter: ~["\\\r\n] | EscapeSequence;

TextBlock: '"""' [ \t]* [\n\r] [.\r\b]* '"""';


### INTEGER

// §3.10.1 Integer Literals

IntegerLiteral:
    DecimalIntegerLiteral
    | HexIntegerLiteral
    | OctalIntegerLiteral
    | BinaryIntegerLiteral
;

fragment DecimalIntegerLiteral: DecimalNumeral IntegerTypeSuffix?;

fragment HexIntegerLiteral: HexNumeral IntegerTypeSuffix?;

fragment OctalIntegerLiteral: OctalNumeral IntegerTypeSuffix?;

fragment BinaryIntegerLiteral: BinaryNumeral IntegerTypeSuffix?;

fragment IntegerTypeSuffix: [lL];


### REAL 




----------------



# 1. Análise e Comparação das Regras
A. Regra para Identificador de Texto (StringLiteral)
### Lexer em C:

Definição:
A regra é definida como:

StringLiteral
    : EncodingPrefix? '"' SCharSequence? '"'
    ;


e possui fragmentos que definem:

EncodingPrefix: Pode ser u8, u, U ou L, permitindo suportar diferentes codificações (útil para Unicode ou tipos largos).

SCharSequence: Sequência de caracteres que pode ser composta por caracteres válidos (qualquer caractere, exceto aspas, barra invertida, retorno de carro ou nova linha) ou sequências de escape.

Observações:

Existe um comentário indicando que também se pretendia aceitar “Raw Strings” (por exemplo, R"Raw String") para evitar o tratamento de escapes.

Exemplos:

Aceitos:

"Hello World"

u8"Olá, mundo!"

Rejeitados:

"String sem fechamento (falta a aspa de fechamento)

"String com caractere inválido \n" (se a quebra de linha não for tratada ou se houver erro na sequência de escape)


### Lexer em Java (versão 20):

Definição:
A regra para String Literals é:

StringLiteral: '"' StringCharacters? '"';

fragment StringCharacters: StringCharacter+;

fragment StringCharacter: ~["\\\r\n] | EscapeSequence;

TextBlock: '"""' [ \t]* [\n\r] [.\r\b]* '"""';


Aqui, StringLiteral reconhece cadeias de caracteres delimitadas por aspas duplas e com a possibilidade de ter sequências de escape.

Além disso, o Java (a partir de versões recentes) permite o TextBlock, que é delimitado por três aspas duplas ("""), facilitando a escrita de textos com múltiplas linhas sem a necessidade de escapes.

Observações:

Diferentemente do lexer em C, não há um prefixo para codificação.

O TextBlock amplia as possibilidades para representar strings multilinha.

Exemplos:

Aceitos:

"Hello World"

"""Este é um text block com várias linhas e "aspas" sem escapes"""

Rejeitados:

"Texto sem fechamento

"Texto com quebra de linha direta, sem usar escape ou text block"


## Comparação:

Semelhanças:

Ambos os lexers reconhecem strings delimitadas por aspas duplas e suportam sequências de escape.

Ambos evitam permitir caracteres não desejados (como quebra de linha não escapada) dentro das aspas.

Diferenças:

Prefixo de Codificação: O lexer em C permite um prefixo (u8, u, U, L), enquanto o lexer Java não o possui.

Raw String / TextBlock:

Em C, há uma tentativa de suportar raw strings com uma regra alternativa (comentada) para R"Raw String".

Em Java, há uma regra específica para TextBlocks, facilitando a inclusão de múltiplas linhas sem escapes.


# B. Regra para Inteiros (IntegerLiteral ou Constant)
## Lexer em C:

Definição:
A regra apresentada é parte da regra Constant:

Constant
    : IntegerConstant
    | FloatingConstant
    | CharacterConstant
    ;


Aqui, o IntegerConstant é utilizado para representar números inteiros.

A especificação completa do IntegerConstant não foi mostrada, mas normalmente ela engloba números decimais, octais e hexadecimais, com possíveis sufixos (como U, L etc.).

Exemplos:

Aceitos:

123

0x1A3F

Rejeitados:

0129 (se considerado inválido como octal, dependendo da implementação)

123abc (contém caracteres inválidos para um literal numérico)

## Lexer em Java (versão 20):

Definição:
A regra é definida de forma mais detalhada:

IntegerLiteral:
    DecimalIntegerLiteral
    | HexIntegerLiteral
    | OctalIntegerLiteral
    | BinaryIntegerLiteral
;

fragment DecimalIntegerLiteral: DecimalNumeral IntegerTypeSuffix?;

fragment HexIntegerLiteral: HexNumeral IntegerTypeSuffix?;

fragment OctalIntegerLiteral: OctalNumeral IntegerTypeSuffix?;

fragment BinaryIntegerLiteral: BinaryNumeral IntegerTypeSuffix?;

fragment IntegerTypeSuffix: [lL];


São explicitamente tratadas as diferentes bases: decimal, hexadecimal, octal e binária.

O sufixo para designar o tipo do literal (por exemplo, L ou l para long) é opcional.

Exemplos:

Aceitos:

123

0b1010

Rejeitados:

0xGHI (contém caracteres inválidos para hexadecimal)

0129 (se os dígitos para octal não forem válidos)

# Comparação:

Semelhanças:

Ambos os lexers têm regras para reconhecer inteiros e, em ambos os casos, é possível reconhecer literais em diferentes bases (ainda que o lexer Java explicite mais as bases suportadas, inclusive a binária).

Diferenças:

Detalhamento:

O lexer Java define regras fragmentadas específicas para cada base, enquanto o lexer C, conforme apresentado, trata o inteiro como parte de uma regra geral (IntegerConstant) dentro de Constant.

Sufixos:

Em Java, o sufixo para literais (por exemplo, L) é definido de forma explícita.

Em C, os sufixos podem ser mais variados (como U, L, UL, etc.) e a regra completa provavelmente engloba esses detalhes, mas não foi mostrada completamente no exemplo.


