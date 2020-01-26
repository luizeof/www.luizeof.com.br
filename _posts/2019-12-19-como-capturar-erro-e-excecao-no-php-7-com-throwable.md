---
title: "Como capturar Erro e Exceção no PHP 7 com Throwable"
date: "2019-12-19"
categories: [ php ]
lang: "pt-BR"
image: "assets/images/category/php-full.png"
---

Capturar e tratar com erros fatais no PHP 5.6 era quase impossível. Um erro fatal não chamaria o manipulador de exceção definido por `set_error_handler()` em um bloco `try` .. `catch` e simplesmente interromperia a execução do script.

No PHP 7+, uma exceção será lançada quando ocorrer um erro fatal e recuperável (`E_ERROR` e `E_RECOVERABLE_ERROR`), em vez de interromper a execução do script.

[https://trowski.com/2015/06/24/throwable-exceptions-and-errors-in-php7/](https://trowski.com/2015/06/24/throwable-exceptions-and-errors-in-php7/)

Erros fatais ainda existem para determinadas condições, como ficar sem memória, e ainda se comportam como antes, interrompendo imediatamente a execução do script. Uma exceção não detectada também continuará sendo um erro fatal no PHP 7.

Em modos gerais isso significa que, se uma exceção gerada por um erro fatal no PHP 5.x não for detectada, ainda será um erro fatal no [PHP 7](https://www.luizeof.com.br/).

> Observe que outros tipos de erros, como avisos e avisos, permanecem inalterados no PHP 7. Apenas erros fatais e recuperáveis lançam exceções.

Exceções lançadas através de erros fatais e recuperáveis não estendem a classe `Exception`. Essa separação foi feita para impedir que o código PHP 5.x existente capte exceções geradas por erros que costumavam interromper a execução do script.

Exceções lançadas de erros fatais e recuperáveis são instâncias de uma classe de exceção nova e separada no PHP 7: `Error`. Como qualquer outra exceção, o `Error` pode ser capturado e manipulado e permitirá que qualquer bloco final seja executado.

## Entendendo o Throwable no PHP 7

Para unir os dois ramos de exceção, `Exception` e `Error` implementam uma nova interface: `Throwable`.

```php
interface Throwable
{
    public function getMessage(): string;
    public function getCode(): int;
    public function getFile(): string;
    public function getLine(): int;
    public function getTrace(): array;
    public function getTraceAsString(): string;
    public function getPrevious(): Throwable;
    public function __toString(): string;
}
```

A interface `Throwable` especifica métodos quase idênticos aos de `Exception`. A única diferença é que `Throwable::getPrevious()` pode retornar qualquer instância de `Throwable` em vez de apenas uma `Exception`. Os construtores de `Exception` e `Error` aceitam qualquer instância de `Throwable` como a exceção anterior.

`Throwable` pode ainda ser usado nos blocos `try` / `catch` para capturar os objetos `Exception` e `Error` (ou qualquer possível tipo de exceção futura).

Lembre-se de que é uma prática recomendada capturar classes de exceção mais específicas e manipular cada uma de acordo.

No entanto, algumas situações justificam a captura de qualquer exceção (como para registro ou tratamento de erros da estrutura). No PHP 7, esses blocos catch-all devem pegar `Throwable` em vez de `Exception`.

```php
try {
    // Gerando uma Exceção
} catch (Throwable $t) {
    // Tratando error genéricos
}
```

## A classe Error

Praticamente todos os erros no PHP 5.x que eram erros fatais ou erros fatais recuperáveis agora geram instâncias do `Error` no PHP 7. Como qualquer outra exceção, os objetos `Error` podem ser capturados usando um bloco `try` / `catch`.

## Código para suportar as exceções no PHP 5.6 e PHP 7

Para capturar qualquer erro no PHP 5.6 e 7 com o mesmo código, vários blocos de captura podem ser usados, capturando `Throwable` primeiro e depois `Exception`. Uma vez que o suporte ao PHP 5.x não é mais necessário, o bloco capturando `Exception` pode ser removido com o tempo.

```php
try {
    // Code that may throw an Exception or Error.
} catch (Throwable $t) {
    // Executed only in PHP 7, will not match in PHP 5.x
} catch (Exception $e) {
    // Executed only in PHP 5.x, will not be reached in PHP 7
}
```

Um abraço!
