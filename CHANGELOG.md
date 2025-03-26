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



## JAVA

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
