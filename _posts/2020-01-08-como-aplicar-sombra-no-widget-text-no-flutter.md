---
title: "Como aplicar Sombra no Widget Text no Flutter"
date: "2020-01-08"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.webp"
---

O `Widget Text` é uma parte essencial de qualquer aplicativo Flutter e o uso de sombra pode torná-lo muito elegante.

A sombra auxilia a melhorar o contraste do texto e permite aplicar efeitos bonitos no seu app.

Como você pode ver, adicionei a propriedade `textAlign` ao `Widget Text` com um valor de `TextAlign.center`. Isso alinhará o texto no centro da tela.

Também adicionei o `[TextStyle](https://api.flutter.dev/flutter/dart-ui/Shadow-class.html)` com a propriedade `shadows`:

```dart
Text('Text Shadows in Flutter',
    textAlign: TextAlign.center,
    style: TextStyle(
        shadows: [
            Shadow(
                blurRadius: 10.0,
                color: Colors.blue,
                offset: Offset(5.0, 5.0),
                ),
            ],
        ),
    ),
```

A sombra adicionada na propriedade do `TextStyle` é baseada na [classe](https://api.flutter.dev/flutter/dart-ui/Shadow-class.html) `Shadow` do Flutter.

A propriedade da `fontFamily` determina o nome da fonte, que você pode usar no argumento `fontFamily`. A propriedade do ativo é um caminho para o arquivo de fonte, relativo ao arquivo [pubspec.yaml](https://www.luizeof.com.br/pubspec-yaml-usando-pacotes-dart-com-o-flutter/).

A propriedade weight especifica o peso dos contornos do glifo no arquivo como um número inteiro múltiplo de 100 entre 100 e 900.

Isso corresponde à classe `FontWeight` e pode ser usado no argumento `fontWeight`. A propriedade style especifica se os contornos do arquivo estão em itálico ou normal. Esses valores correspondem à classe `FontStyle` e podem ser usados no argumento `fontStyle`.

A cor base é `material.Colors` e `Color.withOpacity` pode ser usado para criar uma cor derivada com a opacidade desejada.

![Como aplicar Sombra no Widget Text no Flutter](/assets/images/Flutter-Text-Shadow.webp)

A sombra padrão é preta com `offset` `0` e `blur` `0`.

A transparência deve ser ajustada através do `alpha` na propriedade `color`. A ordem das sombras é importante porque a composição de vários objetos translúcidos não é comutativa.

As propriedades do `Shadow` mais comuns são: `blurRadius`, `color` e `offset`.

A propriedade `blurRadius` definirá o comprimento da sombra e a que distância ela ficará desfocada, um número maior sendo um raio maior.

A propriedade `color` definirá a cor da sombra e a propriedade offset definirá o deslocamento da sombra do elemento de projeção (o texto).

A propriedade `offset` terá as coordenadas `x` e `y`; coordenadas positivas moverão a sombra para a direita e para baixo, enquanto coordenadas negativas moverão a sombra para a esquerda e para cima.

É bem simples aplicar sombras em texto no Flutter!

[Não deixe de ler o artigo sobre Como adicionar uma imagem no Flutter com o Widget Image.](https://www.luizeof.com.br/image-widget-como-adicionar-uma-imagem-no-flutter/)
