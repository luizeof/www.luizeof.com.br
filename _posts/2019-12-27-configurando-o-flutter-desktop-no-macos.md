---
title: "Configurando o Flutter Desktop no MacOS"
date: "2019-12-27"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.png"
---

Estão em andamento trabalhos para estender o Flutter para oferecer suporte à área de trabalho, permitindo que os desenvolvedores criem aplicativos macOS, Windows e Linux com o Flutter Desktop.

Atualmente o Flutter 1.3 Alpha permite compilar o código-fonte do Flutter para um aplicativo nativo do macOS.

O suporte ao desktop do Flutter também se estende aos [plugins](https://www.luizeof.com.br/tag/flutter-plugins/) - você pode instalar plug-ins existentes que suportam a plataforma macOS ou criar seus próprios plugins.

**IMPORTANTE:** As APIs da área de trabalho do Flutter ainda estão nos estágios iniciais de desenvolvimento e estão sujeitas a alterações sem aviso prévio. Nenhuma compatibilidade com versões anteriores, API ou ABI, será fornecida pela equipe de desenvolvimento durante o estágio Alpha.

Espere que qualquer código usando essas bibliotecas precise ser atualizado e recompilado após qualquer atualização do Flutter.

## MacOS apps com Flutter

Essa é a plataformas de desktop que está em estágio mais avançado (por várias razões, incluindo a proximidade com o iOS, que já é suportado).

A camada da API do Objective-C é bastante estável neste momento, portanto, as alterações mais recentes devem ser raras e você já pode testar no Flutter SDK 1.3.

## Windows apps com Flutter

O shell do Windows está nos estágios iniciais. É baseado no Win32, mas é esperado explorar o suporte à UWP no futuro.

## Linux apps com Flutter

O shell atual do Linux é um espaço reservado para GLFW, para permitir experimentação antecipada.

A Equipe do Flutter gostaria de criar uma biblioteca que permita incorporar o Flutter, independentemente de você estar usando GTK +, Qt, wxWidgets, Motif ou outro kit de ferramentas arbitrário para outras partes do aplicativo, mas ainda não determinou uma boa maneira de fazer isso.

O plano atual é oferecer suporte imediato ao GTK +, de maneira que seja fácil adicionar suporte a outros kits de ferramentas.

## Requisitos Mínimos para o Flutter Desktop

Para criar um aplicativo Desktop com o Flutter você precisa do seguinte software:

- Flutter [SDK 1.3 Alpha](https://flutter.dev/docs/development/tools/sdk/releases?tab=macos) ou superior para MacOS.
- Opcional: Um IDE que suporta Flutter como o Android Studio ou o VS Code.

## Configurando o Flutter SDK

Você deve estar no canal de versão **master** do **flutter build** e ativar o recurso da plataforma de desktop macOS digitando esses comandos no Terminal:

```bash
flutter channel master
flutter upgrade
flutter config --enable-macos-desktop
```

## Rodando o Projeto com Flutter Desktop

Para rodar um projeto Desktop com o Flutter você deve passar o parâmetro `-d macOS` para o compilador:

```bash
flutter run -d macOS
```

O modo Desktop do Flutter inclui os comandos `create` e `build`, além do comando executar com o modo de debug, modo de release, modo de profile e hot reload.

No Android Studio você poderá ver o _**MacOS**_ como Device:

![No Android Studio você poderá ver o Flutter Desktop como target](images/flutter-desktop-android-studio.png)

## Adicionar desktop a um projeto existente

Para adicionar suporte de área de trabalho a um projeto existente, execute o seguinte comando em um terminal no diretório raiz do projeto:

```bash
flutter create .
```

Isso vai atualizar o seu projeto com um novo diretório macOS e cria todos os arquivos de configuração necessários.

## Suporte de plug-in no Flutter Desktop

O modo Desktop suporta o uso e a criação de plugins. Para usar um plugin compatível com macOS, siga as etapas para plugins usando pacotes.

O Flutter adiciona automaticamente o código nativo necessário ao seu projeto, como no iOS ou Android.
