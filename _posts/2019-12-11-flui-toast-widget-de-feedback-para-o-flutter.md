---
title: "FLUI Toast: Widget de feedback para o Flutter"
date: "2019-12-11"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.png"
---

Toast é um [widget de feedback](https://www.flui.xin/en/widgets/toast.html) da suíte FLUI Uma classe `FLToastDefaults` suporta a configuração do estilo de exibição de Toast, posição, duração da exibição, modo escuro e outros comportamentos.

Um Toast é a melhor maneira de você notificar os usuários sobre alterações ou atualizações realizadas após alguma interação, por exemplo, salvar informações no banco de dados, uma nova mensagem ou a resposta de algum comando assíncrono longo.

Esses Feedbacks visuais são importantes para que o usuário compreenda o funcionamento do aplicativo e são menos invasivos do que as notificações tradicionais do Android e iOS.

![FLUI Toast: Widget de feedback para o Flutter](images/2019-12-11-toast.gif)

Para facilitar a vida do desenvolvedor o FLUI oferece o `FLToastDefaults`, onde é possível definir o padrão visual e de comportamento das mensagens exibidas pelo Widget `Toast`, como a duração da exibição da mensagem com a propriedade `showDuration`, o padrão de cores com `darkColor` e `darkBackgroundColor`, a opacidade da mensagem com `backgroundOpacity` e a posição com `position`. é um Widget muito personalizável.

A configuração padrão do `FLToastDefaults` é a seguinte:

```dart
const FLToastDefaults({
    this.showDuration = const Duration(milliseconds: 1500),
    this.darkColor = Colors.white,
    this.darkBackgroundColor = Colors.black87,
    this.backgroundOpacity = 0.8,
    this.lightColor = const Color(0xFF2F2F2F),
    this.lightBackgroundColor = const Color(0xFFE0E0E0),
    this.position = FLToastPosition.center,
    this.style = FLToastStyle.dark,
    this.dismissOtherToast = true,
    this.hideWithTap = true,
    this.textDirection = TextDirection.ltr,
    this.topOffset = kToolbarHeight + 10,
    this.bottomOffset = 10,
});
```

O `FLToastProvider` fornece a capacidade de exibir Toast na árvore de sub-widget. Geralmente, atua como o nó pai do MaterialApp.

```dart
class _MyAppState extends State<MyApp> {
  FLToastDefaults _toastDefaults = FLToastDefaults();
  
  @override
  Widget build(BuildContext context) {
    return FLToastProvider(
      defaults: _toastDefaults,
      child: MaterialApp(
        title: 'FLUI',
        theme: $YOUR_THEME
        routes: $ROUTES
      )
   );
}
```

Para mostrar o `Toast`, você precisa usar os métodos estáticos do `FLToast`.

## Exibindo Toast de Texto

Esta é uma ótima opção para alertas de comandos que rodam assíncronos ou operações longas. Basicamente exibe uma mensagem no centro da tela:

```dart
FLToast.text(text: 'Here is text');
/// or
FLToast.showText(text: 'Here is text', position: FLToastPosition.xxx, duration: Duration(seconds: 5), style: FLToastStyle.xxx);
/// shortcut
FLToast.showText(text: 'Here is text');
```

## Exibindo Toast com Carregamento

Esta é uma boa opção para usar com Future, pois permite ao usuário acompanhar o carregamento de informações, etc.

```dart
var dismiss = FLToast.loading(text: 'Loading...');
/// do something...
Future.delayed(Duration(seconds: 2), () {
    /// hide toast
    dismiss();
});
```

## Exibindo Toast de Alerta, Erro e Sucesso

As tês opções são: `info`, `error` e `success`. Basicamente a mensagem fornecida em `text` é exibida com um ícone na parte superior.

```dart
/// info
FLToast.info(text: 'Some info');
/// success
FLToast.success(text: 'Fetch success');
/// error
FLToast.error(text: 'Something was wrong');
```

## Finalizando

O [FLUI](https://www.luizeof.com.br/flutter/flui-e-um-conjunto-de-widgets-para-o-flutter/) é excelente e possui widgets fantásticos como o Toast. A cada nova versão os Widgets ficam mais estáveis e completos e permitem que você use componentes visuais que se encaixam perfeitamente nos seus apps.

Recomendo ;)

Um braço!
