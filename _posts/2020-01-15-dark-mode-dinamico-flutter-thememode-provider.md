---
title: "Como fazer Dark Mode no Flutter com ThemeMode e Provider"
description: "My review of Inception movie. Acting, plot and something else in this short description."
author: luizeof
date: "2020-01-15"
categories: [ flutter ]
tags: [flutter, ui]
rating: 5
lang: "pt-BR"
---

Neste artigo vou apresentar um método de implementar o Dark Mode dinâmico no Flutter usando o `ThemeMode` e o plugin `provider`.

Existem muitas maneiras de aplicar o tema dinâmico no Flutter, mas o plugin `provider` é uma das maneiras melhores e mais eficientes e estou usando no [FluCast](https://www.luizeof.com.br/br/flucast-player-podcast-open-source/).

O plugin `provider` é uma mistura entre _**Injeção de Dependência (DI)**_ e gerenciamento de estado, construído com widgets para widgets.

Ele usa propositalmente widgets para gerenciamento de DI / estado, em vez de classes Dart, como `Stream`. A razão é que os widgets são muito simples, mas robustos e escaláveis e pra quem usa o [Flutter](https://www.luizeof.com.br/br/flutter/) é mais natural.

Ao usar widgets para gerenciamento de estado, o provedor pode garantir:

- Manutenção, através de um fluxo de dados unidirecional;
- Testabilidade / composição, pois sempre é possível `mock` / `override` um valor;
- Robustez, pois é mais difícil esquecer de lidar com o cenário de atualização de um `model` ou `widget`.

Para ler mais sobre o `provider`, [consulte a documentação](https://pub.dev/packages/provider)

## Resumo do Exemplo do Dark Mode

Nesse exemplo vamos usar o `ThemeMode` nativo do Flutter para controlar o **Dark Mode** e **Light Mode** do aplicativo que você possui em alguns aplicativos, como a leitura de aplicativos onde o _**Light Mode**_ é para o dia e o **_Dark Mode_** é para a noite.

Você também deve ter notado que o Google Maps possui o mesmo recurso para os modos noturno e diurno, que são ligados e desligados automaticamente de acordo com a luz. É um recurso cada vez mais comum hoje em dia.

Adicione o plugin `provider` no arquivo [**_pubspec.yaml_**](https://www.luizeof.com.br/br/flutter/pubspec-yaml-usando-pacotes-dart-com-o-flutter/):

```yaml
dependencies:  
 Flutter:  
   sdk: Flutter  
 provider: ^4.0.1
```

Agora vamos criar um novo arquivo para implementação do `ThemeProvider`. Eu criei um arquivo chamado `theme.dart` onde ficará a classe `DynamicDarkMode`:

```dart
import 'package:flutter/foundation.dart';

/// Provider para o Dark Mode
class DynamicDarkMode with ChangeNotifier {
  /// Por padrão o App Começa com o modo Light Mode
  /// Você pode configurar um método de persistir o valor de
  /// [_isDarkMode] para que ele seja preservado quando o app for fechado
  bool _isDarkMode = false;

  /// Verifica se o App está em Dark Mode
  get isDarkMode => this._isDarkMode;

  /// Aplica o Dark Mode
  void setDarkMode() {
    this._isDarkMode = true;
    notifyListeners();
  }

  /// Aplica o Light Mode
  void setLightMode() {
    this._isDarkMode = false;
    notifyListeners();
  }
}
```

Agora, abra o arquivo `main.dart` e implemente o `ChangeNotifier` para todo o aplicativo, para que possamos alterar o tema a partir de qualquer ponto do aplicativo.

Basicamente você precisa alterar o método `main()` para ficar desse jeito:

```dart
void main() async => runApp(
      // Aqui rodamos o app dentro do provider
      ChangeNotifierProvider<DynamicDarkMode>(
        create: (_) => DynamicDarkMode(),
        child: MyHomePage(),
      ),
    );
```

Eu implementei uma opção para alternar entre o modo escuro e claro do tema. A seguir está a implementação da programação.

## Lendo os dados do Provider

O plugin `provider` oferece o método `Provider.of(context)` que permite ler os dados do nosso [MaterialApp](https://www.luizeof.com.br/br/flutter/materialapp-usando-widgets-material-design-no-flutter/) e pode ser acessado em qualquer lugar:

```dart
// Aqui capturamos os dados do nosso ThemeProvider
final themeProvider = Provider.of<DynamicDarkMode>(context);
```

## Código Final do Dark Mode

Veja como que ficará o app com o método `main()` modificado e o `build()` lendo os dados do `provider`.

[O app de exemplo está disponível no github.](https://github.com/luizeof/flutter-darkmode-provider-example)

```dart
import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'theme.dart';

void main() async => runApp(
      // Aqui rodamos o app dentro do provider
      ChangeNotifierProvider<DynamicDarkMode>(
        create: (_) => DynamicDarkMode(),
        child: MyHomePage(),
      ),
    );

class MyHomePage extends StatefulWidget {
  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    // Aqui capturamos os dados do nosso ThemeProvider
    final themeProvider = Provider.of<DynamicDarkMode>(context);
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      // Habilita o suporte ao Dark Mode no MaterialApp
      darkTheme: ThemeData.dark(),
      // Informa o status do Dark Mode ap MaterialApp
      themeMode: themeProvider.isDarkMode ? ThemeMode.dark : ThemeMode.light,
      home: Scaffold(
        appBar: AppBar(
          title: Text(
            themeProvider.isDarkMode
                ? 'Dark Mode Habilitado'
                : 'Light Mode Habilitado',
          ),
          actions: <Widget>[
            // action button
            IconButton(
              icon: Icon(Icons.brightness_4),
              onPressed: () {
                // Aqui alteramos o status do Dark Mode
                // e o Provider se encarrega de avisar ao MaterialApp
                setState(
                  () {
                    themeProvider.isDarkMode
                        // Configura como Light Mode
                        ? themeProvider.setLightMode()
                        // Configura como Dark Mode
                        : themeProvider.setDarkMode();
                  },
                );
              },
            ),
          ],
        ),
        body: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: <Widget>[
              Spacer(),
              Text('Texto Padrão'),
              Spacer(),
              Icon(
                Icons.wb_sunny,
                size: 36.0,
              ),
              Spacer(),
              RaisedButton(
                child: Text('RaisedButton Padrão'),
                onPressed: () {},
              ),
              Spacer(),
              Text('Dark Mode Dinâmico'),
              Spacer(),
            ],
          ),
        ),
      ),
    );
  }
}
```

[Execute o projeto no dispositivo e teste o aplicativo com o código que está no Github](https://github.com/luizeof/flutter-darkmode-provider-example). Você pode personalizar o tema conforme sua exigência.

![Como fazer Dark Mode no Flutter com ThemeMode e Provider](/assets/images/flutter-light-mode.png.webp)

App Light Mode

![Como fazer Dark Mode no Flutter com ThemeMode e Provider](/assets/images/flutter-dark-mode.png.webp)

App Dark Mode

## Conclusão

Neste artigo, aprendemos como implementar o tema dinâmico no Flutter usando o provedor.

Este exemplo é básico e fácil de entender e você pode torná-lo totalmente customizado implementando outras propriedades do tema no arquivo do provedor. Você também pode armazenar as configurações do tema em preferências compartilhadas para manter as configurações ativas após fechar e reabrir o aplicativo.
