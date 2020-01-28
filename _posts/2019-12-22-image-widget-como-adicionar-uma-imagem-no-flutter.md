---
title: "Image Widget: Como adicionar uma imagem no Flutter"
date: "2019-12-22"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.webp"
---

Adicionar uma imagem no Flutter é bem simples usando o Widget Image e copiando a imagem para o seu projeto.

## Estrutura do Projeto

Primeiro você precisa organizar suas imagens em uma pasta padrão que será usada pelo Flutter para encontrar suas imagens.

Recomendo usar a seguinte estrutura:

```bash
lib/
lib/assets/
lib/assets/icons
lib/assets/images
```

## Adicionando a imagem ao pubspec.yml

Se o seu objetivo é adicionar imagens locais no seu app, como ícones, avatares, etc, será necessário declarar essas imagens como ativos no seu `pubspec.yml`.

> Não deixe de ler o artigo sobre o pubspec.yaml.

Desse modo as imagens serão copiadas para dentro do app e estarão disponíveis para uso. Veja como declarar imagens locais no `pubspec.yml` do Flutter:

```yaml
# A seção a seguir é específica para o Flutter.
flutter:

  # Para adicionar ativos ao seu aplicativo, adicione uma seção de ativos, como esta:
  assets:
    - lib/assets/logo.png
    - lib/assets/customer.jpg
```

## Adicionando Imagem local no Flutter com Image Widget

```dart
Image(
  image: AssetImage('lib/assets/logo.png'),
  fit: BoxFit.fitHeight,
);
```

Veja que é possível utilizar o parâmetro `fit` para ajustar o tamanho da imagem dentro do Widget usando o [`BoxFit`](https://api.flutter.dev/flutter/painting/BoxFit-class.html). Você pode usar os seguintes valores:

`contain`: Mantém a imagem a maior possível, enquanto ainda contém a fonte inteiramente dentro do widget de destino.

![contain: Mantém a imagem a maior possível, enquanto ainda contém a fonte inteiramente dentro do widget de destino.](/assets/images/box_fit_contain.png)

`cover`: O menor possível, enquanto ainda cobre toda o widget de destino.

![cover: O menor possível, enquanto ainda cobre toda o widget de destino.](images/box_fit_cover.png)

`fill`: Preenche o widget de destino distorcendo a proporção da imagem.

![box_fit_fill](/assets/images/box_fit_fill.png)

`fitHeight`: Verifica se a altura total da imagem é exibida, independentemente disso significar que a imagem exceda o widget de destino horizontalmente.

![box_fit_fitHeight](/assets/images/box_fit_fitHeight.png)

`fitWidth`: Verifique se a largura total da fonte é mostrada, independentemente disso significar que a fonte excede a caixa de destino na vertical.

![box_fit_fitWidth](/assets/images/box_fit_fitWidth.png)

`none`: Alinhe a fonte na caixa de destino (por padrão, centralizado) e descarta todas as partes da imagem que estiverem fora da widget.

![box_fit_none](/assets/images/box_fit_none.png)

`scaleDown`: Alinha a imagem no widget de destino (por padrão, centralizado) e, se necessário, reduz a fonte para garantir que ela se encaixe na caixa.

É o mesmo que `contain` se isso reduzir a imagem, caso contrário, é o mesmo que `none`.

![box_fit_scaleDown](/assets/images/box_fit_scaleDown.png)

## Adicionando Imagem remota no Flutter com Image Widget

```dart
Image(
  image: NetworkImage('https://flutter.github.io/assets-for-api-docs/assets/widgets/owl.jpg'),
  fit: BoxFit.fitHeight,
);
```

Os construtores Image.asset, Image.network, Image.file e Image.memory permitem que um tamanho de decodificação personalizado seja especificado através dos parâmetros cacheWidth e cacheHeight. O mecanismo decodificará a imagem no tamanho especificado, cujo objetivo principal é reduzir o uso de memória do ImageCache.

No caso em que uma imagem de rede é usada na plataforma da Web, os parâmetros cacheWidth e cacheHeight são ignorados, pois o mecanismo da Web delega a decodificação de imagens de imagens de rede para a Web, que não suporta tamanhos de decodificação personalizados.

[Documentação](https://api.flutter.dev/flutter/widgets/Image-class.html) Oficial do Widget Image.
