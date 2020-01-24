---
title: "Como fazer site em outro idioma com o Plugin Polylang"
date: "2020-01-06"
categories: [ wordpress ]
tags: [plugins]
---

O Polylang permite criar um site [WordPress](https://www.luizeof.com.br/br/wordpress/) bilíngue ou multilíngue. Você escreve postagens, páginas e cria categorias e tags de postagem como de costume e depois define o idioma para cada uma delas.

O **Polylang** é disponível gratuitamente no [diretório de plugins do Wordpress](https://br.wordpress.org/plugins/polylang/).

A tradução de uma postagem, seja no idioma padrão ou não é opcional.

## Principais recursos do plugin Polylang

- Você pode usar quantos idiomas desejar. Scripts de linguagem RTL são suportados. Os pacotes de idiomas do WordPress são baixados e atualizados automaticamente.
- Você pode traduzir postagens, páginas, mídia, categorias, tags, menus, widgets …
- Tipos de postagem personalizados, taxonomias personalizadas, postagens e formatos de postagens, feeds RSS e todos os widgets padrão do WordPress são suportados.
- O idioma é definido pelo conteúdo ou pelo código do idioma no URL, ou você pode usar um subdomínio ou domínio diferente por idioma
- Categorias, tags de postagem e outras metas são copiadas automaticamente ao adicionar uma nova postagem ou tradução de página
- Um alternador de idioma personalizável é fornecido como um widget ou no menu de navegação

### Se você deseja migrar do WPML, pode usar o plug-in WPML para Polylang

Se você deseja usar um serviço de tradução profissional ou automática, pode instalar o Lingotek Translation, como um complemento do Polylang.

A [Lingotek](https://polylang.pro/) oferece um sistema completo de gerenciamento de traduções que fornece serviços como memória de tradução ou processos de tradução semi-automatizada (por exemplo, tradução automática > tradução humana > revisão legal).

## Tradução de Texto

![Tradução de Texto com o Polylang](/assets/images/screenshot-2.png)

Tradução de Posts e Páginas com Polylang

![Tradução de Posts e Páginas com Polylang](/assets/images/screenshot-4.png)

## Funções disponíveis para Temas e Plugins

**IMPORTANTE:** ao usar uma ou mais dessas funções, você **deve** verificar sua existência antes de usá-la; caso contrário, seu site sofrerá um erro fatal na próxima atualização do Polylang (pois o WordPress exclui o plug-in ao atualizá-lo).

### Função `pll_the_languages($args);`

Exibe um alternador de idiomas que pode ser usado em qualquer parte do site.

`$args` é um parâmetro opcional da matriz. As opções são:

- `"dropdown" =>` exibe uma lista se definida como `0` ou uma lista suspensa se definida como `1` (padrão: `0`)
- `‘show_names '=>` exibe nomes de idiomas se definido como 1 (padrão: 1)
- `'display_names_as' =>` `'name'` ou `'slug'` (padrão: `'name'`)
- `"show_flags" =>` exibe sinalizadores se definido como `1` (padrão: `0`)
- `"hide_if_empty" =>` oculta idiomas sem postagens (ou páginas) se definido como 1 (padrão: `1`)
- `'force_home' =>` força o link para a página inicial se definido como `1` (padrão: `0`)
- `'echo' =>` renderiza se definido como `1`, retorna uma string se definido como 0 (padrão: `1`)
- `"hide_if_no_translation" =>` oculta o idioma se não houver tradução, se definido como 1 (padrão: `0`)
- `"hide_current" =>` oculta o idioma atual se definido como `1` (padrão: `0`)
- `'post_id' =>` se definido, exibe links para traduções da postagem (ou página) definida por `post_id` (padrão: `null`)
- `"raw" =>` use isso para criar seu próprio alternador de idioma personalizado (padrão: `0`)

**Importante:** Ao usar o alternador de idiomas como uma lista suspensa, você deve fornecer a ação anexada à alternância de idiomas. Se você deseja o mesmo comportamento fornecido pelo widget, pode encontrar o código correspondente no arquivo `polylang/include/widget.php`. Você deve exibir as tags `ul` se não usar a opção suspensa.

Exemplo de uso:

```php
<?php // gera uma lista de nomes de idiomas ?>
<ul><?php pll_the_languages(); ?></ul>
<?php // gera uma lista de bandeiras (sem nomes de idiomas) ?>
<ul><?php pll_the_languages(array('show_flags'=>1,'show_names'=>0)); ?></ul>
<?php // gera uma lista suspensa de nomes de idiomas ?>
<?php pll_the_languages(array('dropdown'=>1));  ?>
```

Se as opções não forem suficientes, você poderá criar seu próprio alternador de idioma personalizado usando o argumento "raw":

```php
$translations = pll_the_languages(array('raw'=>1));
```

A função retornará uma array de arrays, uma array por idioma com as seguintes entradas:

- `[id] =>` ID do idioma
- `[slug] =>` código do idioma usado nos URLs
- `[name] =>` nome do idioma
- `[url] =>` url da tradução
- `[flag] =>` URL da bandeira
- `[current_lang] =>` `true` se esse for o idioma atual, `false` caso contrário
- `[no_translation] =>` `true` se não houver tradução disponível, `false` caso contrário

### Função `pll_current_language($value);`

Retorna o nome completo ou a localidade do WordPress (assim como a função principal do WordPress `get_locale()` ou a `slug` (código de duas letras) do idioma atual, por exemplo `'br'` para Brasil.

Exemplo de uso:

```php
<?php $locale = pll_current_language($value); ?>
```

`$value` é um parâmetro opcional que pode ser `'name'` ou `'locale'` ou `'slug'`, o padrão é `'slug'`.

### Função `pll_default_language($value);`

Retorna o nome completo ou a localidade do WordPress (assim como a função principal do WordPress `get_locale()` ou a `slug` (código de duas letras) do idioma padrão do Polylang, por exemplo `'br'` para Brasil.

Exemplo de uso:

```php
<?php $locale = pll_default_language($value); ?>
```

`$value` é um parâmetro opcional que pode ser `'name'` ou `'locale'` ou `'slug'`, o padrão é `'slug'`.

### Função `pll_get_post($post_id, $slug);`

Retorna o `ID` do `post` ou `page` traduzida como `integer`.

```php
<?php pll_get_post($post_id, $slug); ?>
```

`$post_id` **_(obrigatório)_** é o `ID` do `post` ou `page` que você deseja a tradução e o `$slug` **_(opcional)_** é o código de duas letras do idioma, o padrão é o idioma atual.

### Função `pll_home_url($slug);`

Retorna o URL da página inicial no idioma solicitado, como uma string.

```php
<php pll_home_url($slug); ?>
```

O parâmetro `$slug` é um código de duas letras do idioma. O parâmetro é opcional e o padrão é o idioma atual se a função for chamada no frontend.

### Função `pll_register_string($name, $string, $group, $multiline);`

Permite que os plugins adicionem suas próprias strings no painel "Tradução de Strings". A função deve ser chamada no lado do administrador (o arquivo `functions.php` para temas).

É possível registrar `strings` vazias (por exemplo, quando elas vêm de opções), mas elas não aparecem na tabela da lista.

```php
<?php pll_register_string($name, $string, $group, $multiline); ?>
```

- `$name =>` (obrigatório) nome fornecido para facilitar a classificação (por exemplo: `'meutema'`);
- `$string =>` (obrigatório) a string a ser traduzida;
- `$group =>` (opcional) o grupo no qual a string está registrada, o padrão é `'polylang'`;
- `$multiline =>` (opcional) se definido como `true`, o campo de texto da tradução será multiline, o padrão será `false`.

### Função `pll__($string);`

Traduz e retorna uma string anteriormente registrada com `pll_register_string`.

```php
<?php $traducao = pll__($string); ?>
```

O parâmetro `$string` é a string a ser traduzida.
