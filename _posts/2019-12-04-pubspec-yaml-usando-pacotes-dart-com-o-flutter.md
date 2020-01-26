---
title: "pubspec.yaml: Usando pacotes Dart com o Flutter"
date: "2019-12-04"
categories: [ flutter ]
lang: "pt-BR"
image: "assets/images/category/flutter-full.png"
---

O SDK do [Flutter](https://www.luizeof.com.br/br/flutter/) suporta o uso de [pacotes compartilhados](https://flutter.dev/docs/development/packages-and-plugins/using-packages) contribuídos por outros desenvolvedores para os ecossistemas Flutter e Dart através da declaração no arquivo _pubspec.yaml_.

Isso permite criar rapidamente um aplicativo sem precisar desenvolver tudo do zero.

Os pacotes existentes permitem muitos casos de uso, por exemplo, solicitações de rede `http`, navegação personalizada / tratamento de rotas `fluro`, integração com APIs de dispositivos (`url_launcher` e `battery`) e uso de SDKs de plataformas de terceiros como Firebase (`FlutterFire`).

## A Estrutura básica do arquivo pubspec.yaml

Abaixo segue a estrutura padrão do arquivo `pubspec.yaml` usado no exemplo [hello\_word](https://github.com/flutter/flutter/blob/master/examples/hello_world/pubspec.yaml).

```yaml
name: hello_world

environment:
  # The pub client defaults to an <2.0.0 sdk constraint which we need to explicitly overwrite.
  sdk: ">=2.0.0-dev.68.0 <3.0.0"

assets:
  - lib/assets/logo.png
  - lib/assets/icon.png

dependencies:
  flutter:
    sdk: flutter
  css_colors: 1.14.11

dev_dependencies:
  flutter_test:
    sdk: flutter
```

Existem basicamente 4 seções no arquivo `pubspec.yaml`, sendo:

### Seção `name` do arquivo pubspec.yaml

Todo app precisa de um nome. O nome deve estar em letras minúsculas, com sublinhados para separar as palavras, `apenas_como_este`.

Use apenas letras latinas básicas e dígitos árabes: `[a-z0-9_]`. Além disso, verifique se o nome é um identificador de dardo válido, que não começa com dígitos e não é uma palavra reservada.

### Seção `assets` do arquivo pubspc.yaml

A subseção de `assets` da seção flutter especifica os arquivos que devem ser incluídos no aplicativo.

Cada ativo é identificado por um caminho explícito (relativo ao arquivo `pubspec.yaml`) em que o arquivo do ativo está localizado. A ordem na qual os ativos são declarados não importa.

Durante o build do seu app, o Flutter coloca os ativos em um arquivo especial chamado pacote de ativos **_(asset bundle)_**, do qual os aplicativos podem ler em tempo de execução.

> **Artigo Relacionado:** Como adicionar imagens ao Flutter usando o Widget Image e Assets.

### Seção `environment` do arquivo pubspec.yaml

Um app pode indicar quais versões de suas dependências ele suporta, mas os pacotes têm outra dependência implícita: a própria plataforma Dart.

A plataforma `Dart` evolui com o tempo e um pacote pode funcionar apenas com determinadas versões da plataforma.

Um pacote pode especificar essas versões usando uma restrição do SDK. Essa restrição entra em um campo de `environment` de nível superior separado no `pubspec` e usa a mesma sintaxe de restrição de versão das dependências.

### Seção `dependencies` e `dev_dependencies` do arquivo pubspec.yaml

Dependências são a razão de ser do arquivo `pubspec.yaml.` Nesta seção, você lista cada pacote que seu app precisa para funcionar.

As dependências se enquadram em um dos dois tipos:

- Dependências regulares estão listadas em `dependencies`: - são pacotes dos quais qualquer pessoa que utilize o seu pacote também precisará.
- Dependências que são necessárias apenas na fase de desenvolvimento do próprio pacote estão listadas em `dev_dependencies`.

Durante o processo de desenvolvimento, pode ser necessário substituir temporariamente uma dependência. Você pode fazer isso usando `dependency_overrides`.

## Procurando pacotes Flutter e Dart

Por padrão os pacotes [Flutter](https://flutter.dev/) e [Dart](https://dart.dev/) são publicados no website [pub.dev](https://pub.dev/). Este é o gerenciador padrão de pacotes do Flutter e funciona no **Windows** e no **MacOS**.

A página de entrada do Flutter no pub.dev exibe os principais pacotes que são compatíveis com o Flutter (aqueles que declaram dependências geralmente compatíveis com o Flutter) e oferecem suporte à pesquisa entre todos os pacotes publicados.

## Adicionando uma dependência de pacote no pubspec.yaml

Para adicionar um novo pacote no seu projeto, abra o arquivo `pubspec.yaml` localizado dentro da pasta do aplicativo e adicione por exemplo o pacote `localstorage` dentro da seção `dependences`:.

- Instale o pacote No Android Studio / IntelliJ: Clique em Packages para entrar na faixa de ações na parte superior de pubspec.yaml.
- Importá-lo
- Adicione uma declaração de importação correspondente no código Dart.
- Pare e reinicie o aplicativo, se necessári

Se o pacote incluir código específico da plataforma (Java / Kotlin para Android, Swift / Objective-C para iOS), esse código deverá ser incorporado ao seu aplicativo.

O **Hot Reload** e o **Hot Restart** atualizam apenas o código **Dart**, portanto, pode ser necessária uma reinicialização completa do aplicativo para evitar erros como `MissingPluginException` ao usar o pacote.

Para um exemplo completo, consulte o exemplo `localstorage` abaixo.

[![Adicionando uma dependência de pacote no pubspec.yaml](images/dependencies-android-studio.png)](https://luizeof.com.br/wp-content/uploads/2019/12/dependencies-android-studio.png)

## Gerenciando versões de pacote no Flutter

Todos os pacotes têm um número de versão, especificado no arquivo `pubspec.yaml` do pacote. A versão atual de um pacote é exibida ao lado de seu nome (por exemplo, consulte o pacote `url_launcher`), bem como uma lista de todas as versões anteriores (versões `url_launcher`).

Quando um pacote é adicionado ao `pubspec.yaml`, o formato abreviado plugin1: significa que qualquer versão do pacote plugin1 pode ser usada.

Para garantir que o aplicativo não seja interrompido quando um pacote for atualizado, especifique um intervalo de versões usando um dos seguintes formatos:

Restrições de intervalo: especifique uma versão mínima e máxima. Por exemplo:

```yaml
dependencies:
  url_launcher: '> = 0.1.2 <0.2.0'
```

As restrições de intervalo com sintaxe de sinal de intercalação são semelhantes às restrições regulares de intervalo:

```yaml
dependencies:
  coleção: '^ 0.1.2'
```

Desse modo os pacotes serão adicionados ao seu app.

## Instalando os Pacotes na máquina de desenvolvimento

Após declarar os pacotes que seu app necessita no arquivo pubspec.yaml, você deve executar o comando abaixo para que eles sejam baixados e instalados no computador local:

```bash
flutter pub get
```

[![Instalando os Pacotes na máquina de desenvolvimento do pubspec.yaml](images/flutter-pub-get.png)](https://luizeof.com.br/wp-content/uploads/2019/12/flutter-pub-get.png)

Você também pode instalar os plugins através do Android Studio ou do Visual Studio Code. Veja um exemplo:

![Instalando os Pacotes na máquina de desenvolvimento](images/android-studio-pub-get.png)

Ao executar o `flutter pub get` (que pode ser automático no Android Studio e no VS Code) pela primeira vez após adicionar um pacote, o Flutter salva a versão concreta do pacote encontrada no arquivo de bloqueio pubspec.lock.

Isso garante que você obtenha a mesma versão novamente se você, ou outro desenvolvedor de sua equipe, executar o `flutter pub get`. Isso garante que seus pacotes não quebrem durante as atualizações de bibliotecas.

Para atualizar para uma nova versão do pacote, por exemplo, para usar novos recursos nesse pacote, execute a `flutter pub upgrade` para recuperar a versão mais nova disponível do pacote, permitida pela restrição de versão especificada em `pubspec. yaml`.

[![Atualizando os Pacotes na máquina de desenvolvimento](images/flutter-pub-upgrade.png)](https://luizeof.com.br/wp-content/uploads/2019/12/flutter-pub-upgrade.png)

## Finalizando

O _pubspec.yaml_ é crucial para apps Flutter e também para o Dart, caso você queira montar seus próprios packages terá que dominar o uso deste arquivo.
