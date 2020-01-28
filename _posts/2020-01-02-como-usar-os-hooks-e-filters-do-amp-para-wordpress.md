---
title: "Como usar os Hooks e Filters do AMP para Wordpress"
date: "2020-01-02"
categories: [ wordpress ]
lang: "pt-BR"
image: "assets/images/category/wordpress-full.webp"
---

O AMP é um esforço comum que promete um melhor desempenho de carregamento de página para sites no ambiente móvel e pode ser customizado com _filters_ e _hooks_.

Porém, como você pode encontrar no [artigo sobre o AMP](https://www.luizeof.com.br/amp-para-wordpress/), você terá que sacrificar coisas sofisticadas do seu site e seguir rigorosamente as regras, cumprir as diretrizes e validar as páginas. Parece muito o que fazer, não é?

Felizmente, se você criou seu site com o WordPress, pode aplicar o AMP em um piscar de olhos usando um plug-in chamado AMP-WP.

Você pode ainda olhar o código fonte do AMP no [Github](https://github.com/ampproject).

O plug-in fornece várias ações e filtros que oferecem flexibilidade para personalizar os estilos, o conteúdo da página e até a marcação **HTML** da página AMP como um todo.

## Customizando os Estilos do AMP

Tenho certeza de que isso é algo que você deseja alterar imediatamente após a ativação do plug-in, como alterar a cor do plano de fundo do cabeçalho, a família da fonte e o tamanho da fonte para melhor corresponder à marca e à personalidade do seu site.

Para isso, podemos empregar a ação `amp_post_template_css` no arquivo functions.php do nosso tema.

```php
<?php function theme_amp_custom_styles() { ?>
  nav.amp-wp-title-bar {
    background-color: orange;
  }
<?php }
add_action( 'amp_post_template_css', 'theme_amp_custom_styles' );
```

Se examinarmos o Chrome DevTools, esses estilos serão anexados no elemento `<style amp-custom>` e substituirão as regras de estilo anteriores. Portanto, a cor de fundo laranja agora é aplicada ao cabeçalho.

Você pode continuar escrevendo as regras de estilo normalmente. Porém, lembre-se de algumas restrições e mantenha os tamanhos de estilo abaixo de 50 KB. Em caso de dúvida, consulte nosso artigo anterior sobre como validar suas páginas AMP.

## Customizando Templates no AMP

Se você precisar alterar muito além do estilo, convém personalizar o modelo inteiro. O plugin amp-wp [fornece vários modelos internos](https://github.com/ampproject/amp-wp/tree/develop/templates), a saber:

- `header-bar.php`
- `meta-author.php`
- `meta-taxonomy.php`
- `meta-time.php`
- `single.php`
- `style.php`

[Esses modelos são muito parecidos com a hierarquia de modelos padrão do WordPress](https://developer.wordpress.org/themes/basics/template-hierarchy/).

Cada um desses modelos pode ser assumido fornecendo um arquivo com o mesmo nome na pasta / amp / no tema.

Quando esses arquivos estiverem no lugar, o plug-in os buscará em vez dos arquivos padrão e nos permitirá alterar completamente a saída desses modelos.

```text
twentytwelve
├── 404.php
├── amp
│   ├── meta-author.php
│   ├── meta-taxonomy.php
│   ├── single.php
│   └── style.php
```

Você pode reescrever os estilos inteiros através do arquivo `style.php` ou modificar toda a estrutura da página AMP conforme sua necessidade com o `single.php`. Ainda assim, você precisará manter a alteração para cumprir os regulamentos da AMP.

## Usando os Hooks and Filters do AMP

Como mencionado anteriormente, este plug-in é fornecido com várias ações e filtros. Não é possível abordar cada um neste artigo mas vou apresentar um resumo e os snippets necessários que eu já utilizei:

O `amp_init();` ação é útil para plug-ins que dependem do [AMP](https://www.luizeof.com.br/amp-para-wordpress/) para que seu plug-in funcione; é executado quando o plug-in já está iniciado.

```php
<?php
function amp_customizer_support_init() {
  require_once dirname( __FILE__ ) . '/amp-customizer-class.php';
}
add_action( 'amp_init', 'amp_customizer_support_init' );
?>
```

Semelhante à ação `wp_head`, podemos usar `amp_post_template_head` para incluir elementos adicionais na tag head nas páginas AMP, como `meta`, estilos ou scripts.

```php
<?php
function theme_amp_google_fonts() { ?>
  <link rel="stylesheet" href="https://fonts.googleapis.com/css?family=PT+Serif:400,400italic,700,700italic%7CRoboto+Slab:400,700&subset=latin,latin">
<?php }
add_action( 'amp_post_template_head', 'theme_amp_google_fonts' );
```

A action `amp_post_template_footer` é semelhante ao `wp_footer`.

```php
<?php
function theme_amp_end_credit() { ?>
  <footer class="amp-footer">
    <p>© Hongkiat.com 2016</p>
  </footer>
<?php }
add_action( 'amp_post_template_footer', 'theme_amp_end_credit' );
```

O `amp_content_max_width` é usado para definir a largura máxima da página AMP. A largura também será usada para definir a largura dos elementos incorporados, como um vídeo do Youtube.

```php
<?php
function theme_amp_content_width() {
  return 700;
}
add_filter( 'amp_content_max_width', 'theme_amp_content_width' );
```

O `amp_site_icon_url` é usado para definir o URL do ícone - favicon e ícone da Apple -. O padrão é a imagem carregada por meio da interface Site Icon, que foi introduzida na versão 4.3.

```php
<?php
function theme_amp_site_icon_url( ) {
    return get_template_directory_uri() . '/images/site-icon.png';
}
add_filter( 'amp_site_icon_url', 'theme_amp_site_icon_url' );
```

O filter `amp_post_template_meta_parts` é para quando você precisar personalizar a saída de metadados do post, como o nome do autor, a categoria e o carimbo de data e hora.

Através desse filtro, você pode embaralhar a ordem padrão ou remover uma das meta da página AMP.

```php
<?php
function theme_amp_meta( $meta ) {

  foreach ( array_keys( $meta, 'meta-time', true ) as $key ) {
    unset( $meta[ $key ] );
  }

  return $meta;
};
add_filter( 'amp_post_template_meta_parts', 'theme_amp_meta' );
```

O `amp_post_template_metadata` é para personalizar a estrutura de dados do [schema.org](https://schema.org/) nas páginas AMP.

O exemplo a seguir mostra como fornecemos o logotipo do site que será mostrado no carrossel AMP no resultado da pesquisa do Google e removemos o carimbo de data / hora modificado da página.

```php
<?php
function amp_schema_json( $metadata ) {
  unset( $metadata[ 'dateModified' ] );
  $metadata[ 'publisher' ][ 'logo' ] = array(
    '@type' => 'ImageObject',
    'url' => get_template_directory_uri() . '/images/logo.png',
    'height' => 60,
    'width' => 325,
  );
  return $metadata;
}
add_filter( 'amp_post_template_metadata', 'amp_schema_json', 30, 2 );
```

O `amp_post_template_file` essa é uma maneira alternativa de substituir arquivos de modelo. É útil se você preferir hospedar seus arquivos de modelo AMP personalizados em outro diretório que não seja `/amp/`.

```php
function theme_custom_template( $file, $type, $post ) {
    if ( 'meta-author' === $type ) {
        $file = get_template_directory_uri() . '/partial/amp-meta-author.php';
    }
    return $file;
}
add_filter( 'amp_post_template_file', 'theme_custom_template', 10, 3 );
```

O `amp_query_var` é usado para alterar o ponto de extremidade da página AMP quando o URL Link permanente está ativado. Por padrão, está definido como `/amp/`. Dado o seguinte, a página AMP agora está acessível adicionando `/m/` no URL.

```php
function custom_amp_endpoint( $amp ) {
    return 'm';
}
add_filter( 'amp_query_var' , 'custom_amp_endpoint' );
```

Com esses `hooks` e `filters` você poderá customizar o AMP no seu site.
