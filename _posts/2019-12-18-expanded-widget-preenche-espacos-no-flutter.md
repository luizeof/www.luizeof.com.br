---
title: "Expanded Widget preenche espaços no Flutter"
date: "2019-12-18"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.webp"
---

O Expanded Widget expande um `child` de uma `Row`, `Column` ou `Flex` para que o `child` [preencha o espaço disponível](https://api.flutter.dev/flutter/widgets/Expanded-class.html) na estrutura de layout do [Flutter](https://www.luizeof.com/).

O uso de um widget `Expanded` faz com que um child de uma `Row`, `Column` ou `Flex` se expanda para preencher o espaço disponível ao longo do eixo principal (por exemplo, horizontalmente para uma Linha ou verticalmente para uma Coluna). Se vários filhos forem expandidos, o espaço disponível será dividido entre eles de acordo com o fator [Flex](https://api.flutter.dev/flutter/widgets/Flexible/flex.html).

Um widget `Expanded` deve ser um descendente de uma `Row`, `Column` ou `Flex` e o hierarquia do widget `Expanded` para sua `Row`, `Column` ou `Flex` deve conter apenas `StatelessWidgets` ou `StatefulWidgets` (não outros tipos de widgets, como [RenderObjectWidgets](https://api.flutter.dev/flutter/widgets/RenderObjectWidget-class.html) tipo [ConstrainedLayoutBuilder](https://api.flutter.dev/flutter/widgets/ConstrainedLayoutBuilder-class.html)[, LeafRenderObjectWidget](https://api.flutter.dev/flutter/widgets/LeafRenderObjectWidget-class.html)[, ListWheelViewport,](https://api.flutter.dev/flutter/widgets/ListWheelViewport-class.html) [MultiChildRenderObjectWidget,](https://api.flutter.dev/flutter/widgets/MultiChildRenderObjectWidget-class.html) [RenderObjectToWidgetAdapter,](https://api.flutter.dev/flutter/widgets/RenderObjectToWidgetAdapter-class.html) [SingleChildRenderObjectWidget,](https://api.flutter.dev/flutter/widgets/SingleChildRenderObjectWidget-class.html) [SliverWithKeepAliveWidget](https://api.flutter.dev/flutter/widgets/SliverWithKeepAliveWidget-class.html) e [Table](https://api.flutter.dev/flutter/widgets/Table-class.html)).

Para este artigo vou usar o seguinte widget customizado chamado `MyWidget`:

```dart
Widget MyWidget(_title) {
  return Card(
    color: Colors.red,
    child: Padding(
      padding: EdgeInsets.all(25),
      child: Text(
        _title.toString(),
        textAlign: TextAlign.center,
        style: TextStyle(
          color: Colors.white,
          fontSize: 25,
          fontWeight: FontWeight.bold,
        ),
      ),
    ),
  );
},
```

## Exemplos de uso do Expanded Widget

Se você tem alguns widgets em uma linha ou coluna, pode abraçar os filhos assim:

```dart
Row(
  children: [
    MyWidget(1),
    MyWidget(2),
    MyWidget(3),
  ],
),
```

que vai gerar algo desse tipo:

![Row com 3 Widgets sem o Expanded](/assets/images/widget-expanded-1-576x1024.png)

Para alinhar os itens da linha, você pode usar a propriedade `mainAxisAlignment` do `Row`, com o valor `MainAxisAlignment.spaceBetween` que vai deixar todos os widgets com o mesmo espaço entre eles:

```dart
Row(
  mainAxisAlignment: MainAxisAlignment.spaceBetween,
  children: [
    MyWidget(1),
    MyWidget(2),
    MyWidget(3),
  ],
),
```

![Row com 3 Widgets sem o Expanded e com Alinhamento Igual](/assets/images/widget-expanded-2-576x1024.png)

É aí que entra o `Expanded` Widget, pois se você deseja que um dos widgets se expanda para preencher o espaço extra na linha ou coluna, é possível agrupá-lo facilmente com um widget `Expanded`:

```dart
Row(
  children: [
    Expanded(
      child: MyWidget(1),
    ),
    Expanded(
      child: MyWidget(2),
    ),
    Expanded(
      child: MyWidget(3),
    ),
  ],
),
```

Vai gerar o seguinte resultado:

![Row com 3 Widgets com o Expanded](/assets//widget-expanded-3-576x1024.png)

Você pode ainda mixar os Widgets com e sem o Expanded para criar layouts mais atraentes:

```dart
Row(
  children: [
    MyWidget(1),
    Expanded(
      child: MyWidget(2),
    ),
    MyWidget(3),
  ],
),
```

O snippet acima vai resultar em:

![Row com 3 Widgets sem o Expanded e Flex](/assets/images/widget-expanded-4-576x1024.png)

E se você possui alguns widgets que deseja compartilhar o espaço extra (mas não igualmente), pode definir a propriedade `flex` do Widget `Expanded`.:

```dart
children: [
    MyWidget(1),
    Expanded(
      flex: 2,
      child: MyWidget(2),
    ),
    Expanded(
      flex: 5,
      child: MyWidget(3),
    ),
  ],
),
```

No exemplo acima o primeiro e o terceiro widgets têm um tamanho fixo. O segundo e o quarto widget da `Row` compartilham o espaço extra porque são agrupados com o `Expanded`. No entanto, como os fatores `flex` estão sendo usados, o segundo widget obtém o dobro da largura do quarto. O resultado é esse:

![Row com 3 Widgets sem o Expanded e Flex diferentes](/assets/images/widget-expanded-5-576x1024.png)

O uso do Widget Expanded é fundamental para que você possa preencher os espaços no Layout dos apps desenvolvidos no Flutter.

Um grande abraço!
