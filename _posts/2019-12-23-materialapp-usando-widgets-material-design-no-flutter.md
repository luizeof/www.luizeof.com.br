---
title: "MaterialApp: Usando Widgets Material Design no Flutter"
date: "2019-12-23"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.png"
---

MaterialApp é um [widget que agrupa vários outros widgets](https://api.flutter.dev/flutter/material/MaterialApp-class.html) geralmente necessários para apps com funcionalidade específicas baseados em Material Design.

Aplicativos móveis normalmente revelam seu conteúdo por meio de elementos de tela cheia chamados "telas" ou "páginas".

No Flutter, esses elementos são chamados de rotas e são gerenciados por um widget Navigator.

O navegador gerencia uma pilha de objetos Route e fornece métodos para gerenciar a pilha, como Navigator.push e Navigator.pop.

Quando sua interface com o usuário se encaixa nesse paradigma de pilha, em que o usuário deve poder voltar para um elemento anterior na pilha, o uso de rotas e o Navegador são apropriados.

Em certas plataformas, como Android, a interface do usuário do sistema fornecerá um botão voltar (fora dos limites do seu aplicativo) que permitirá ao usuário navegar de volta às rotas anteriores na pilha do seu aplicativo.

Em plataformas que não possuem esse mecanismo de navegação embutido, o uso de um AppBar (normalmente usado na propriedade Scaffold.appBar) pode adicionar automaticamente um botão voltar para a navegação do usuário.

O MaterialApp configura um [`Navigator`](https://api.flutter.dev/flutter/widgets/Navigator-class.html) de nível superior para procurar rotas na seguinte ordem:

1. Para a rota /, a propriedade `home` será usada.
2. Caso contrário, a tabela de rotas será usada, se houver uma entrada para a rota.
3. Caso contrário, `onGenerateRoute` é chamado, se fornecido. Ele deve retornar um valor não nulo para qualquer rota válida não tratada pela home e pelas rotas.
4. Finalmente, se tudo mais falhar, o `onUnknownRoute` é chamado.

Se um Navegador for criado, pelo menos uma dessas opções deve manipular a rota /, já que é usada quando um `initialRoute` inválido é especificado na inicialização.

Este widget também configura o observador do Navegador de nível superior (se houver) para executar animações de transição.

Basicamente o MAterialApp possui a seguinte estrutura:

```dart
MaterialApp(
  home: Scaffold(
    appBar: AppBar(
      title: const Text('Home'),
    ),
  ),
)
```

Este exemplo mostra como criar um MaterialApp que usa o mapa de rotas para definir a rota "**home**" e uma rota "**about**".

```dart
MaterialApp(
  routes: <String, WidgetBuilder>{
    '/': (BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: const Text('Home Route'),
        ),
      );
    },
    '/about': (BuildContext context) {
      return Scaffold(
        appBar: AppBar(
          title: const Text('About Route'),
        ),
      );
     }
   },
)
```
