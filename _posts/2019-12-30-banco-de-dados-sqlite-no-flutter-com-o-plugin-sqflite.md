---
title: "Banco de Dados SQLite no Flutter com o plugin sqflite"
date: "2019-12-30"
categories: [ flutter ]
lang: "pt-BR"
featured: true
image: "assets/images/category/flutter-full.webp"
---

O SQLite é [uma biblioteca em linguagem C](https://www.sqlite.org/) que implementa um mecanismo de banco de dados SQL pequeno, rápido, independente, de alta confiabilidade e completo que pode ser usado com o Flutter através da biblioteca **_sqflite_**.

O SQLite é o mecanismo de banco de dados mais usado no mundo. O SQLite está embutido em todos os telefones celulares e na maioria dos computadores e é fornecido em inúmeras outras aplicações que as pessoas usam todos os dias.

O formato de arquivo SQLite é estável, multiplataforma e compatível com versões anteriores, e os desenvolvedores prometem mantê-lo assim pelo menos até o ano de 2050.

O [código-fonte](https://sqlite.org/src/doc/trunk/README.md) SQLite é de domínio público e é gratuito para todos, para qualquer finalidade e pode ser facilmente usado no Android ou iOS com o Flutter.

> Este artigo é uma adaptação da documentação do _sqfite_ disponível em [https://pub.dev/packages/sqflite](https://pub.dev/packages/sqflite) e [https://github.com/tekartik/sqflite](https://github.com/tekartik/sqflite).

## Sobre o plugin **_sqflite_** para Flutter

Atualmente o plugin **_sqflite_** para [Flutter](https://www.luizeof.com.br/) suporta **iOS**, **Android** e **MacOS**.

- Transações e lotes de suporte
- Gerenciamento automático de versão durante a abertura
- Auxiliares para inserir / consultar / atualizar / excluir consultas
- Operação do banco de dados executada em um encadeamento em segundo plano no iOS e Android

## Instalando o **_sqflite_** no seu projeto Flutter

No seu projeto Flutter, adicione a dependência `sqflite` no pubspec.yaml:

```dart
dependencies:
  sqflite: ^1.2.0
```

Depois de adicionar o `sqflite` no seu `pubspec.yaml`, você deve instalar pacotes a partir da linha de comando:

```bash
flutter pub obter
```

Como alternativa, seu editor pode oferecer suporte ao `flutter pub get` (Android Studio e VS Code). Verifique os documentos do seu editor para saber mais.

Agora, no seu código Dart, você deve usar o package `sqflite.dart`:

```dart
import 'package:sqflite/sqflite.dart';
```

## Localizando o caminho do banco de dados

O **_sqflite_** fornece uma estratégia básica de localização usando o caminho dos bancos de dados no **Android** e a pasta Documents no **iOS**, conforme recomendado nas duas plataformas. O local pode ser recuperado usando `getDatabasesPath()`.

```dart
var databasesPath = await getDatabasesPath();
var path = join(databasesPath, dbName);

// Make sure the directory exists
try {
  await Directory(databasesPath).create(recursive: true);
} catch (_) {}
```

## Abrindo um banco de dados _SQLite_ no Flutter

Um banco de dados **SQLite** é um arquivo no sistema de arquivos identificado por um caminho. Se relativo, esse caminho é relativo ao caminho obtido por `getDatabasesPath()`, que é o diretório de banco de dados padrão no **Android** e o diretório de _Documents_ no **iOS**.

```dart
var db = await openDatabase('my_db.db');
```

Existe um mecanismo básico de migração do **_sqflite_** para lidar com alterações de esquema durante a abertura.

Abrir um banco de dados SQLite no modo de leitura e gravação é o padrão. Pode-se especificar uma versão para executar a estratégia de migração, pode configurar o banco de dados e sua versão.

Para abrir um banco de dados SQLite em modo de somente leitura, use o seguinte método:

```dart
var db = await openReadOnlyDatabase('my_db.db');
```

### Evento _onConfigure_ do _sqlfite_

O `onConfigure` é o primeiro _callback_ opcional chamado. Permite executar a inicialização do banco de dados, como suporte à exclusão em cascata, etc.

```dart
_onConfigure(Database db) async {
  // Add support for cascade delete
  await db.execute("PRAGMA foreign_keys = ON");
}

var db = await openDatabase(path, onConfigure: _onConfigure);
```

## Usando Migrations com Flutter e SQLite

Para lidar com atualizações de banco de dados (alterações de _schema_), existe um mecanismo básico de versão semelhante à API do Android.

Embora `getVersion` e `setVersion` estejam expostos, eles não devem ser usados e, em vez disso, as migrações devem ser executadas ao abrir o banco de dados.

Veja alguns conceitos básicos das migrations do SQLite com Flutter:

- `onCreate`, `onUpgrade` e `onDowngrade` são chamados quando uma versão é especificada. Se o banco de dados não existir, o `onCreate` será chamado e então o banco de dados é criado.
- Se `onCreate` não estiver definido, `onUpgrade` será chamado com `oldVersion` com o valor `0`. Se o banco de dados existir e a nova versão for maior que a versão atual, `onUpgrade` será chamado.
- Inversamente, se a nova versão for inferior à versão atual, o `onDowngrade` será chamado. Tente evitar isso sempre incrementando a versão do banco de dados.
- Para o caso de _downgrade_, existe um retorno de chamada especial `onDatabaseDowngradeDelete` que simplesmente exclui o banco de dados e chama `onCreate` para criá-lo.

Esses três _callback_ são chamados em uma transação imediatamente antes da versão ser definida no banco de dados. Veja um exemplo:

```dart
_onCreate(Database db, int version) async {
  // Database is created, create the table
  await db.execute(
    "CREATE TABLE Test (id INTEGER PRIMARY KEY, value TEXT)");
}

_onUpgrade(Database db, int oldVersion, int newVersion) async {
  // Database version is updated, alter the table
  await db.execute("ALTER TABLE Test ADD name TEXT");
}

// Special callback used for onDowngrade here to recreate the database
var db = await openDatabase(path,
  version: 1,
  onCreate: _onCreate,
  onUpgrade: _onUpgrade,
  onDowngrade: onDatabaseDowngradeDelete);
```

## Como lidar com um bando de dados SQLite corrompido

Android e iOS lida com a corrupção de uma maneira diferente:

- no **iOS**, ele falha no primeiro acesso ao banco de dados;
- no **Android**, o arquivo existente é removido.

Ainda não sei como torná-lo consistente sem interromper o comportamento existente.

Parece que uma maneira de verificar se um arquivo é um arquivo de banco de dados válido é abri-lo em somente leitura e verificar sua versão (ou seja, sqlite / iOS falha de maneira inconsistente no primeiro acesso a um banco de dados não-sqlite).

Antes de fazer disso uma função de nível superior, seriam necessários mais testes para validar o comportamento. Veja um exemplo:

```dart
Future<bool> isValidDatabase(String path) async {
  Database db;
  bool isDatabase = false;
  try {
    db = await openReadOnlyDatabase(path);
    int version = await db.getVersion();
    if (version != null) {
      isDatabase = true;
    }
  } catch (_) {} finally {
    await db?.close();
  }
  return isDatabase;
}
```

O exemplo acima verifica se um arquivo é um de banco de dados válido. Um arquivo vazio é um arquivo sqlite vazio válido.

## Impedir problema de _database lock_

É altamente recomendável abrir um banco de dados apenas uma vez. Por padrão, um banco de dados é aberto como uma única instância `singleInstance: true`. Ou seja, reabrir o mesmo arquivo é seguro e fornecerá o mesmo banco de dados.

Se você abrir o mesmo banco de dados várias vezes usando `singleInstance: false`, poderá encontrar (pelo menos no Android) o seguinte erro:

```dart
android.database.sqlite.SQLiteDatabaseLockedException: database is locked (code 5)
```

Vamos considerar a seguinte classe auxiliar:

```dart
class Helper {
  final String path;
  Helper(this.path);
  Database _db;

  Future<Database> getDb() async {
    if (_db == null) {
      _db = await openDatabase(path);
    }
    return _db;
  }
}
```

Como o `openDatabase` é `async`, há um risco de que o `openDatabase` pode ser chamado duas vezes. Você pode corrigir isso com o seguinte:

```dart
import 'package:synchronized/synchronized.dart';

class Helper {
  final String path;
  Helper(this.path);
  Database _db;
  final _lock = new Lock();

  Future<Database> getDb() async {
    if (_db == null) {
      await _lock.synchronized(() async {
        // Check again once entering the synchronized block
        if (_db == null) {
          _db = await openDatabase(path);
        }
      });
    }
    return _db;
  }
}
```

Isso é tudo que você precisa saber sobre como abrir e inicializar um banco de dados SQLite com Flutter e a biblioteca `sqflite`.

## Fechando um banco de dados SQLite

Muitos aplicativos usam um banco de dados e nunca precisariam fechá-lo (ele será fechado quando o aplicativo for finalizado). Se você deseja liberar recursos, pode fechar o banco de dados.

```dart
await db.close();
```

## Realizando CRUD no SQLite com Flutter e sqlfite

Para exemplificar o CRUD no SQLite com Flutter vamos considerar esta classe `Todo`:

```dart
// nome da tabela
final String tableTodo = 'todo';
// nome do campo ID
final String columnId = '_id';
// nome do campo "título"
final String columnTitle = 'title';
// nome do campo "concluído"
final String columnDone = 'done';

// Representa uma tarefa
class Todo {

  // Campos do objeto [Todo] que serão mapeados
  // com a tabela [todo] do banco de dados
  int id;
  String title;
  bool done;


  // Este método mapeia o objeto [Todo] para os campos
  // da tabela [todo] do banco de dados.
  Map<String, dynamic> toMap() {
    var map = <String, dynamic>{
      columnTitle: title,
      columnDone: done == true ? 1 : 0
    };
    if (id != null) {
      map[columnId] = id;
    }
    return map;
  }

  // Construtor padrão
  Todo();

  // Construtor que mapeia o valor do banco de dados
  // para o objeto [Todo]
  Todo.fromMap(Map<String, dynamic> map) {
    id = map[columnId];
    title = map[columnTitle];
    done = map[columnDone] == 1;
  }

}
```

Além da classe `Todo`, vamos usar também uma classe chamada `TodoProvider` que será a responsável por realizar a comunicação com o banco de dados.

```dart
class TodoProvider {

  // Instância do banco de dados
  Database db;

  // Abre o banco de dados
  // - realiza a criação caso não exista
  Future open(String path) async {
    db = await openDatabase(path, version: 1,
        onCreate: (Database db, int version) async {
          await db.execute('''
            create table $tableTodo (
             $columnId integer primary key autoincrement,
             $columnTitle text not null,
             $columnDone integer not null)
          ''');
    });
  } // open

  // Realiza o insert no banco de dados
  Future<Todo> insert(Todo todo) async {
    todo.id = await db.insert(tableTodo, todo.toMap());
    return todo;
  } // insert

  // Busca um item específico no banco de dados pelo ID
  Future<Todo> getItem(int id) async {
    List<Map> maps = await db.query(tableTodo,
        columns: [columnId, columnDone, columnTitle],
        where: '$columnId = ?',
        whereArgs: [id]);
    if (maps.length > 0) {
      return Todo.fromMap(maps.first);
    }
    return null;
  } // getTodo

  // Apaga um item do banco de dados pelo ID
  Future<int> delete(int id) async {
    return await db.delete(tableTodo, where: '$columnId = ?', whereArgs: [id]);
  } // delete

  // Atualiza um item no Banco de Dados
  Future<int> update(Todo todo) async {
    return await db.update(tableTodo, todo.toMap(),
        where: '$columnId = ?', whereArgs: [todo.id]);
  } // update

  Future<List<Todo>> getAll() async {
    // Query the table for all The Dogs.
    final List<Map<String, dynamic>> maps = await db.query(tableTodo);
    // Converte a List<Map<String, dynamic> em uma List<Todo>.
    return List.generate(maps.length, (i) {
      return Todo(
        columnId: maps[i][columnId],
        columnTitle: maps[i][columnTitle],
        columnDone: maps[i][columnDone],
      );
    });
  }

  // Fecha a conexão com o banco de dados
  Future close() async => db.close();

} // TodoProvider
```

Com base nas duas classes `Todo` e `TodoProvider`, vou explicar como as coisas funcionam com a biblioteca `sqflite`.

## Executando Consultas RAW SQL

O `sqflite` permite usar comandos SQL completos usando o Flutter e o SQLite através dos comandos `rawInsert()`, `rawUpdate()`, `rawQuery()` e `rawDelete()`.

Ao fornecer uma instrução SQL RAW, você não deve tentar _sanitizar_ nenhum valor. Em vez disso, você deve usar a sintaxe de binding padrão do SQLite:

```dart
// esse é o modo correto
int recordId = await db.rawInsert(
  'INSERT INTO todo(title, done) VALUES (?, ?)',
  ['Exemplo', 1],
);
// esse modo não é recomendado
int recordId = await db.rawInsert("INSERT INTO todo(title, done) VALUES ('Exemplo', 1)");
```

O caractere `?` é reconhecido pelo SQLite como um espaço reservado para um valor a ser inserido.

O número de caracteres `?` devem corresponder ao número de argumentos. Os tipos de argumentos devem estar na lista de tipos suportados pelo SQLite.

Particularmente, listas (esperadas para o conteúdo de blob) não são suportadas. Um erro comum é esperar usar `IN (?)` E fornecer uma lista de valores. Isso não funciona. Em vez disso, você deve listar cada argumento um por um:

```dart
var list = await db.rawQuery(
  'SELECT * FROM todo WHERE id IN (?, ?, ?)',
  ['2', '7', '12'],
);
```

É importante destacar também que `NULL` é um valor especial. Ao testar `NULL` em uma consulta, você não deve fazer `'WHERE title =?', [Null]`, mas usar `WHERE title IS NULL`.

```dart
var list = await db.query(
  'todo',
  columns: ['id', 'title', 'done'],
  where: 'title IS NULL',
);
```

## Inserindo e Atualizando dados no SQLite com Flutter

O método `TodoProvider.insert()` utiliza o `db.insert` que após executar o comando SQL retorna o _ID_ do registro criado.

Já o método `TodoProvider.update()` utiliza o `db.update()` para atualizar o registro, passando o parâmetro `where` com o campo utilizado para a comparação da chave e o `whereArgs` para especificar o valor do campo.

Veja que foi utilizado o helper `toMap()` para realizar a escrita do SQL, evitando que você misture Flutter, Dart, SQL, etc.

Mas você pode utilizar o método passando os parâmetros no formato `chave : valor`, como mostra o exemplo abaixo:

```dart
int recordId = await db.insert('todo',
   {'title': 'exemplo', 'done': 1},
);
```

## Lendo resultados do banco de dados com SQLite e Flutter

O método `TodoProvider.getItem()` retorna um único registro do banco de dados baseado no ID ou `null` caso não exista passando o parâmetro `where` com o campo utilizado para a comparação da chave e o `whereArgs` para especificar o valor do campo.

O método `TodoProvider.getAll()` retorna uma `List` de objetos `Todo`, mas nos bastidores é usado o seguinte método:

```dart
final List<Map<String, dynamic>> maps = await db.query(tableTodo);
```

Os itens do `Map` resultantes são somente leitura, portanto geram uma exceção se forem modificados. Por exemplo:

```dart
// Vamos ler o primeiro registro
Map<String, dynamic> mapRead = records.first;
// Atualiza na memória ... isso lançará uma exceção
mapRead['title'] = "Exemplo";
// Erro... `mapRead` is read-only
```

Para resolver esse problema, você precisa criar um novo `Map` se quiser modificá-lo na memória:

```dart
// Vamos criar outro map
Map<String, dynamic> map = Map<String, dynamic>.from(mapRead);
// Agora pode atualizar os dados na memória
map['title'] = "Exemplo";
```

Você também pode buscar somente alguns campos em uma consulta:

```dart
var list = await db.query('todo', columns: ['id', 'title']);
```

## Removendo dados no SQLite com Flutter

O método `TodoProvider.delete()` utiliza o `db.delete` é para excluir o conteúdo de uma tabela e retorna o número de linhas excluídas.

Veja um exemplo de uso do comando `db.delete()` do `sqlfite`:

```dart
var count = await db.delete('todo', where: 'id = ?', whereArgs: ['1']);
```

Veja que é necessário usar o parâmetro `where` e `whereArgs`.

## Executando comandos no Banco de Dados com Flutter

O método `execute()` é usado para comandos sem valores de retorno.

```dart
await db.execute('CREATE TABLE my_table (id INTEGER PRIMARY KEY AUTO INCREMENT, name TEXT, type TEXT)');
```

## Transações no SQLite com Flutter

É uma boa prática usar o objeto `db` diretamente, mas apenas o objeto `Transaction` em uma transação para acessar o banco de dados

```dart
await db.transaction((txn) async {
  // Ok .. Assim você vai evitar muitos problemas ...
  await txn.execute('CREATE TABLE Test1 (id INTEGER PRIMARY KEY)');
  // Evite usar o objeto database diretamente,
  // pode causar um deadlock!
  await db.execute('CREATE TABLE Test2 (id INTEGER PRIMARY KEY)');
});
```

Uma transação é confirmada se o retorno de chamada não gerar um erro, ou seja, o commit é automático. Se um erro for lançado, a transação será cancelada. Portanto, reverter uma transação de uma maneira geral é só lançar uma exceção.

Você pode usar uma série de comandos numa transação:

```dart
await db.transaction((txn) async {
  await db.insert('todo', {'title': 'Exemplo 2', 'done': 1});
  await db.delete('todo', where: 'id = ?', whereArgs: ['5']);
});
```

Devido à maneira como a transação funciona no SQLite (threads), transações simultâneas de leitura e gravação não são suportadas.

Todas as chamadas estão atualmente sincronizadas e o bloco de transações é exclusivo. Uma maneira básica de oferecer suporte ao acesso simultâneo é abrir um banco de dados várias vezes, mas só funciona no iOS, pois o Android reutiliza o mesmo objeto de banco de dados.

## Comandos em lote no SQLite com Flutter

Para evitar ping-pong entre o Dart e o SQL, você pode usar o lotes de comandos usado o `Batch`:

```dart
batch = db.batch();
batch.insert('todo', {'title': 'Exemplo de Título'});
batch.update(
  'todo',
  {'title': 'Exemplo de Título'},
  where: 'title = ?',
  whereArgs: ['Exemplo de Título'],
);
batch.delete(
  'todo',
  where: 'title = ?',
  whereArgs: ['Exemplo de Título'],
);
results = await batch.commit();
```

É important lembrar que durante uma transação o `Batch` não será confirmado até que a transação seja confirmada (`commit`):

```dart
await database.transaction((txn) async {

  var batch = txn.batch();

  // commit, mas a confirmação real ocorrerá quando a transação for confirmada
  // no entanto, os dados estão disponíveis nesta transação
  await batch.commit();

  });
```

Por padrão, um Batch termina assim que encontra um erro (que normalmente reverte as alterações não confirmadas).

Você pode ignorar erros para que todas as operações bem-sucedidas sejam executadas e confirmadas, mesmo se uma operação falhar usando o parâmetro `continueOnError`:

```dart
await batch.commit(continueOnError: true);
```

A obtenção do resultado para cada operação tem um custo (ID para inserção e número de alterações para atualização e exclusão), especialmente no Android, onde uma solicitação SQL extra é executada.

Se você não se importa com o resultado e se preocupa com o desempenho em grandes lotes, pode usar:

```dart
await batch.commit(noResult: true);
```

## Regras de Nomes de tabela e coluna no _sqlfite_

Em geral, é melhor evitar o uso de palavras reservadas do **SQLite** para nomes de entidades. Se qualquer um dos seguintes nomes for usado:

`"add","all","alter","and","as","autoincrement","between","case","check","collate","commit","constraint","create","default","deferrable","delete","distinct","drop","else","escape","except","exists","foreign","from","group","having","if","in","index","insert","intersect","into","is","isnull","join","limit","not","notnull","null","on","or","order","primary","references","select","set","table","then","to","transaction","union","unique","update","using","values","when","where"`

O helper do **_sqflite_** aplica escape nos nomes reservados automaticamente, ou seja:

```dart
db.query('table');
```

será equivalente a adicionar manualmente aspas duplas ao redor do nome _table_:

```dart
db.rawQuery('SELECT * FROM "table"');
```

No entanto, em qualquer outra instrução RAW SQL (incluindo `orderBy`, `where`, `groupBy`), certifique-se de escapar o nome corretamente usando aspas duplas. Por exemplo, veja abaixo onde o grupo de nomes de colunas não é escapado no argumento de colunas, mas é escapado no argumento where.

```dart
db.query(
  'todo',
  columns: ['title'],
  where: '"title" = ?',
  whereArgs: ['Exemplo'],
);

```

## Tipos de Dados suportados pelo SQLite e Flutter

Ainda não há verificação de validade dos valores na API, portanto, evite tipos não suportados pelo SQLite ([Veja a documentação](https://www.sqlite.org/datatype3.html)).

O `DateTime` não é um tipo SQLite suportado. É recomendado os armazenar como `int` (`millisSinceEpoch`) ou `string` (iso8601).

O `bool` também não é um tipo SQLite suportado. Use `INTEGER` e os valores `0` e `1`.

Além disso o Flutter e o SQLite também suporta os tipos `Text`, `Real`, `Blob` e `Integer`.

## Finalizando

Espero que o **_sqflite_** e o SQLIte forneçam os recursos necessários para a persistência de dados no seu App com Flutter.

Um grande abraço!
