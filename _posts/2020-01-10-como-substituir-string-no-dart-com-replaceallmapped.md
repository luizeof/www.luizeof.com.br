---
title: "Como substituir string no Dart com  replaceAllMapped"
date: "2020-01-10"
categories: [ flutter ]
tags: [flutter, ui]
---

O Dart fornece um [método](https://api.dart.dev/stable/2.7.0/dart-core/String/replaceAllMapped.html) chamado _replaceAllMapped_ para substituir facilmente qualquer parte de uma substring em uma [String](https://www.luizeof.com.br/br/flutter/os-tipos-de-dados-do-dart-e-flutter/) usando expressões regulares que oferece mais opções que o método _replaceAll_.

## O replaceAll() ajuda a substituir strings simples

No Dart, temos um [método](https://api.dart.dev/stable/2.7.0/dart-core/String/replaceAll.html) chamado _replaceAll()_ que permite substituir uma sequência simples de texto. Veja a definição:

```dart
String replaceAll(Pattern from, String replace);
```

Esse método suporta dois parâmetros; o `from` deve ser uma sequência com a seção de texto dentro da sequência de texto que você deseja substituir e o segundo parâmetro `replace` é o texto que você deseja substituir, veja um exemplo:

```dart
newString = 'O que é?';
newString.replaceAll('?', '*');
// it returns the string 'o que é*'
```

Este método auxilia grande parte das substituições, porém ele troca apenas o trecho fixo `from` pelo `replace`.

Caso você queira mais opções de substituição - como expressões regulares por exemplo - existe o método _replaceAllMapped_ que é muito mais poderoso.

## Entendendo o replaceAllMapped()

A definição do método _replaceAllMapped_ é:

Basicamente o _replaceAllMapped_ substitui todas as substrings que correspondem ao parâmetro `from` por uma nova `String` a partir da correspondência `match`.

```dart
String replaceAllMapped(Pattern from, String replace(Match match));
```

Isso pode ser usado para substituir `Strings` por novo conteúdo que depende da `match`, ao contrário de `replaceAll`, onde a sequência de substituição é sempre a mesma.

Esse método pode ser usado para descobrir tipos complexos e diferentes de substrings usando uma `regex`. Isso é melhor que o método `replaceAll`, no qual apenas uma substring específica pode ser transmitida.

## Exemplo de uso do método replaceAllMapped

No exemplo abaixo, estamos substituindo todas as vogais minúsculas em uma sequência por \*.

```dart
import "dart:core";

void main() {
  final original = "abcdefghijklmnopqrstuvwxyz";
  final resultado =
      givenString.replaceAllMapped(new RegExp(r'[aeiou]'), (match) {
    return '*';
  });
  print(resultado);
}
```

Ele imprimirá a saída abaixo:

```
*bcd*fgh*jklmn*pqrst*vwxyz
```

Desse modo fica fácil criar padrões de substituição de Strings no Dart usando um simples `regex`.
