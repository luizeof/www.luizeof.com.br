---
title: "Conheça os Tipos de Dados do Dart e Flutter"
date: "2019-12-08"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.png"
---

Uma das características mais fundamentais de uma linguagem de programação é o conjunto de tipos de dados [que ela suporta](https://dart.dev/guides/language/language-tour#built-in-types). Estes são os tipos de dados que podem ser representados e manipulados em uma linguagem de programação como o Dart que é a base do [Flutter](https://www.luizeof.com.br/).

O Dart possui suporte especial para os seguintes tipos de dados:

- numbers
- strings
- booleans
- lists (ou _arrays_)
- sets
- maps

Você pode inicializar um objeto de qualquer um desses tipos especiais usando um literal. Por exemplo, `'esta é uma string'` é uma literal de string e `true` é um literal booleano.

Como todas as variáveis no Dart se referem a um objeto - uma instância de uma classe - você geralmente pode usar construtores para inicializar variáveis.

Alguns dos tipos internos têm seus próprios construtores. Por exemplo, você pode usar o construtor `Map()` para criar um mapa.

## Tipos de Dados no Dart

### O Tipo de Dados `int`

Valores inteiros não maiores que 64 bits, dependendo da plataforma. Na VM do Dart, os valores podem ser de -263 a 263 - 1. O Dart compilado no JavaScript usa números JavaScript, permitindo valores de -253 a 253 - 1.

### O Tipo de Dados `double`

Números de ponto flutuante de 64 bits (precisão dupla), conforme especificado pelo padrão IEEE 754.

O `int` e `double` são subtipos de `num`. O tipo `num` inclui operadores básicos como `+`, `-`, `/` e `*`, e também é onde você encontrará `abs()`, `ceil()` e `floor()`, entre outros métodos.

### O Tipo de Dados `String`

Uma `String` é uma sequência de unidades de código UTF-16. Você pode usar aspas simples ou duplas para criar uma sequência de caracteres.

### O Tipo de Dados `bool`

Para representar valores booleanos, o Dart tem um tipo chamado `bool`. Apenas dois objetos têm o tipo bool: os literais booleanos `true` e `false`, que são constantes em tempo de compilação.

### O Tipo de Dados `List`

Talvez a coleção mais comum em quase todas as linguagens de programação seja a matriz ou o grupo ordenado de objetos. No Dart, arrays são objetos `List`.

Os literais da lista Dart se parecem com literais da matriz JavaScript.

### O Tipo de Dados `Set`

Um `Set` no Dart é uma coleção não ordenada de itens exclusivos. O suporte ao Dart para conjuntos é fornecido pelos literais de conjunto e pelo tipo de conjunto.

### O Tipo de Dados `Map`

Em geral, um `map` é um objeto que associa chaves e valores. Chaves e valores podem ser qualquer tipo de objeto.

Cada chave ocorre apenas uma vez, mas você pode usar o mesmo valor várias vezes.
