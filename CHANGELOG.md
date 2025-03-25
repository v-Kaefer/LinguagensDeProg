Para a realização do trabalho, foram escolhidas as linguagens Java (20) & C.
Foi utilizado a plataforma ANTLR LAB em conjunto a IDE Visual Studio Code.

Para a análise e comparação dos lexers, foram escolhidas as regras de reconhecimento de TEXTO, INTEIRO e REAL.







## C

Para identificador de texto, é utilizado a regra:

StringLiteral
    : EncodingPrefix? '"' SCharSequence? '"'
    | EncodingPrefix? 'R' RawStringLiteral
    ;

Ou seja, a regra aceita dois formatos de strings.
1. Aceita Strings normais, entre aspas ("String Normal").
2. E aceita Raw Strings (R"Raw String"), que e usado para evitar o escape de caracteres.

