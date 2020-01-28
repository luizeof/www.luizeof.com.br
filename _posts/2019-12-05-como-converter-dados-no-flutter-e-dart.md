---
title: "Como converter dados no Flutter e Dart"
date: "2019-12-03"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.webp"
---

Durante o desenvolvimento é comum converter dados de um tipo para outro para efetuar as mais diversas operações e o **Dart** oferece uma série de métodos para realizar as conversões.

## Converter String em int

```dart
var strtoint = int.parse("999");
```

O método [int.parse](https://api.dartlang.org/stable/dart-core/int/parse.html) analisa a origem como um literal inteiro e retorna o valor.

A origem deve ser uma sequência não vazia de dígitos de base de base, opcionalmente prefixados com um sinal de menos ou mais ('-' ou '+'). Não deve ser `null`.

Se a cadeia de origem não contiver um literal inteiro válido, opcionalmente prefixado por um sinal, será lançada uma `FormatException`.

## Converter String em double

```dart
var strtodouble = double.parse('1.1');
```

O método `[double.parse()](https://api.dartlang.org/stable/dart-core/double/parse.html)` aceita um sinal opcional (+ ou -) seguido pelos caracteres "`Infinity`", "`NaN`" ou uma representação de ponto flutuante.

Uma representação de ponto flutuante é composta por uma [mantissa](https://pt.wikipedia.org/wiki/Significando) e uma parte opcional do expoente.

> A [mantissa](https://pt.wikipedia.org/wiki/Significando) é um ponto decimal (.) Seguido por uma sequência de dígitos (decimais) ou uma sequência de dígitos opcionalmente seguida por um ponto decimal e, opcionalmente, mais dígitos.

A parte do expoente (opcional) consiste no caractere "**e**" ou "**E**", um sinal opcional e um ou mais dígitos. A `String` não deve ser `null`.

Os espaços em branco à esquerda e à direita são ignorados.

Se a `String` de origem não for um literal `double` válido, o `onError` será chamado com a fonte como argumento e seu valor de retorno será usado. Se nenhum `onError` for fornecido, uma `FormatException` será lançada.

## Converter int para String

O Dart oferece um método especial chamado [`toString()`](https://api.dartlang.org/stable/dart-core/int/toString.html) para converter números inteiros em string.

```dart
String inttostr = 1.toString();
```

`int.toString()` retorna a representação de um literal numérico inteiro.

## Converter double para String

O Dart oferece um método especial chamado [`toString()`](https://api.dartlang.org/stable/dart-core/double/toString.html) para converter números double em string.

```dart
String doubletostr = 1.9.toString();
```

`double.toString()` retorna a representação de um literal numérico, de modo que o valor `double` mais próximo do valor matemático da representação seja esse `double`.
