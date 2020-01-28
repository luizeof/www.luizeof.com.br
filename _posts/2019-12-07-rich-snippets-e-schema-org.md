---
title: "Como melhorar a pesquisa com o Rich Snippets e o Schema.org"
date: "2019-12-07"
categories: [ wordpress ]
lang: "pt-BR"
featured: true
image: "assets/images/category/wordpress-full.webp"
---

Os dados estruturados com **Rich Snippets** e **schema.org** permitem apresentar os resultados da pesquisa de uma maneira mais visual e atraente.

De acordo com um estudo da [**Searchmetrics**](http://www.searchmetrics.com/news-and-events/schema-org-in-google-search-results/), a adição de marcação estruturada de dados a um site leva a um aumento de 36% nos resultados de pesquisa do Google.

A marcação de dados estruturados é renderizada no **Google**, **Bing** e outros mecanismos de pesquisa como _rich snippets_.

Os _rich snippets_ são [resultados de pesquisa aprimorados](https://developers.google.com/search/docs/guides/intro-structured-data?hl=pt-br) que adicionam informações extras, como uma foto, o preço, as classificações do usuário e outras características aos `snippets` do mecanismo de pesquisa.

![Como melhorar a pesquisa com o Rich Snippets e o Schema.org](/assets/images/Rich-Snippets-Serp.webp)

Os dados estruturados levam a melhores resultados de pesquisa por dois motivos principais:

- Ele estende a semântica do HTML, **tornando o conteúdo mais compreensível para os mecanismos de pesquisa**.
- **Rich snippets** - a apresentação do mecanismo de pesquisa de dados estruturados - têm uma taxa de cliques melhor que os resultados de pesquisa regulares e menos informativos.

Veja essa mesma pesquisa da imagem acima com cards de conteúdo:

![Como melhorar a pesquisa com o Rich Snippets e o Schema.org](/assets/images/Rich-Snippets-Cards.webp)

## Marcações do schema.org

Se você deseja ter rich snippets para o seu próprio site, é necessário adicionar dados estruturados à sua marcação HTML.

Os dados estruturados usam o vocabulário schema.org que permite que você informe os mecanismos de pesquisa sobre a natureza do seu conteúdo.

O [schema.org](https://schema.org/) é uma iniciativa do Google, Bing e Yahoo que visa fornecer um conjunto de esquemas para descrever diferentes tipos de conteúdo da Web, para que os mecanismos de pesquisa possam entendê-lo melhor.

O Schema.org está organizado em uma hierarquia de dois níveis:

- **Tipos:** tipos diferentes de conteúdo da web, eles são organizados em sua própria hierarquia;
- **Propriedades**: cada tipo tem um certo número de propriedades.

### Primeiro nível de tipo de schema

Coisa é o item mais genérico no vocabulário schema.org, é o ancestral de todos os outros tipos.

### Segundo nível de tipo de schema

O segundo nível de Tipos é um pouco mais específico e contém os tipos de evento, ação, intangível, trabalho criativo, local, organização, produto e pessoa.

Há também uma coisa de segundo nível separada, disponível como uma extensão do schema.org; é o tipo MedicalEntity.

### Terceiro nível de tipo de schema

Cada tipo de segundo nível contém alguns ou muitos tipos de terceiro nível, por exemplo, um dos subtipos do `CreativeWork` é o tipo de revisão.

Observe que tipos mais específicos (segundo e terceiro nível) herdam as propriedades de seus pais (e avós no caso de terceiro nível).

A imagem abaixo foi publicada no blog oficial do esquema e visualiza o hierarquia do schema.org:

![Marcações do schema.org](/assets/images/schema-org-hierarchy.webp)

## Encontre o schema que você precisa

Navegue no vocabulário para encontrar o esquema necessário. Por exemplo, para Rich Snippets do tipo [Recipe](https://schema.org/Recipe), você precisa usar o Tipo de receita, que é filho do `CreativeWork`. Possui muitas propriedades, como `cookTime`, `cookingMethod`, `recipesIngredient` e outras, além de herdar as propriedades de seu pai (`CreativeWork`) e avô (`Thing`).

Você pode validar os Dados Estruturados na [Ferramenta de Teste de Dados Estruturados no Google](https://search.google.com/structured-data/testing-tool?hl=pt-BR).

O schema.org é um projeto comunitário, é frequentemente estendido e novas versões são lançadas regularmente. Se você não encontrar o esquema que precisa, poderá propor à Comunidade Schema.org e também contribuir com o projeto do Github.

## Adicionar dados estruturados à sua marcação

Então, como você adiciona seus esquemas ao código de front-end? Schema.org pode usar três formatos diferentes. Você precisa escolher um para adicionar a marcação de dados estruturados ao seu site. Embora, em teoria, você possa usar mais de um formato no mesmo site, isso prejudica a legibilidade e a manutenção do código, portanto, não é uma boa prática.

Os três principais formatos de marcação de dados estruturados são os seguintes:

**Microdados:** é um padrão da web que estende o HTML introduzindo novos atributos, como itemprop. O site oficial do schema.org tem um ótimo tutorial sobre como usar microdados. Portanto, se você já conhece o HTML, esse formato pode ser uma boa opção.

**RDFa:** o formato longo de RDFa é Resource Description Framework in Attributes, e é uma recomendação do W3C que estende documentos HTML, XML e SVG com a ajuda de um conjunto de atributos específicos.

O Open Graph Protocol do Facebook também é baseado em RDFa, então provavelmente você já o encontrou. Existe uma versão RDFa Lite para iniciantes e também uma versão completa que oferece muitas opções para trabalhar com dados estruturados de maneira elaborada.

**JSON-LD:** Enquanto as duas outras opções estendem a marcação HTML, o JSON-LD usa a sintaxe JSON. JSON-LD significa Notação de objeto JavaScript para dados vinculados, e este é o formato que o Google Developers recomenda, de acordo com a visão deles, "a marcação de dados estruturada é mais facilmente representada no formato JSON-LD".

Você não precisa ser um especialista em JavaScript para usar o JSON, pois é um sistema de notação simples usando pares nome-valor.

Você pode comparar facilmente os três formatos com a ajuda de uma guia prática na parte inferior de cada página de tipo schema.org.

![Adicionar dados estruturados à sua marcação](/assets/images/schema-tab.webp)

Examinando os exemplos, você pode entender facilmente como cada formato funciona e usar um deles em seu próprio site.

A marcação de dados estruturados que você precisa adicionar ao seu código é baseada no vocabulário schema.org. Se você escolher microdados ou RDFa, precisará adicionar atributos extras às tags HTML regulares.

Por exemplo, com microdados, você adiciona o nome do `Type` ao contêiner usando o atributo `itemscope itemtype=""` e cada propriedade com o atributo `itemprop`.

Aqui está um exemplo muito simples:

```markup
<div itemscope itemtype="http://schema.org/Recipe">
  <h2 itemprop="name">My Recipe</h2>
  <img itemprop="image" src="image.jpg" alt="Recipe Image">
  <p itemprop="description">Description of Recipe</p>
</div>
```

E o mesmo exemplo usando `RDFa`, vale a pena prestar atenção nos diferentes atributos que você precisa usar aqui:

```markup
<div vocab="http://schema.org/" typeof="Recipe">
  <h2 property="name">My Recipe</h2>
  <img property="image" src="image.jpg" alt="Recipe Image">
  <p property="description">Description of Recipe</p>
</div>
```

Se você escolher o formato `JSON-LD`, precisará inserir o código `<script type="application/ld+json"></script>` na tag no cabeçalho da sua página HTML. O exemplo acima terá a seguinte aparência:

```javascript
<script type="application/ld+json">
{
  "@context": "http://schema.org",
  "@type": "Recipe",
  "name": "My Recipe",
  "image": "image.jpg",
  "description": "Description of Recipe"
}
</script>
```

## Dicas para usar dados estruturados

### Teste sua marcação de dados estruturados

Antes de adicionar a marcação de dados estruturados ao seu site, você pode testá-la rapidamente usando a Ferramenta de teste de dados estruturados do **Google**. Dessa forma, você pode encontrar rapidamente os problemas, se houver.

### Aproveite os gráficos do Google Knowledge Cards

O Google não usa apenas dados estruturados para rich snippets, mas se você é uma autoridade para um determinado tipo de conteúdo, seu conteúdo também pode aparecer em um dos Cartões de gráficos de conhecimento exibidos no lado direito de algumas páginas de resultados do mecanismo de pesquisa .

Observe que você não pode fazer o Google exibir um cartão de gráfico de conhecimento para o seu conteúdo, mas se a sua marcação de dados estruturada estiver configurada corretamente, você poderá.

![Aproveite os gráficos do Google Knowledge Cards](/assets/images/facebook-knowledge-graph.webp)

## Finalizando

Em maio de 2016, o Google introduziu Rich Cards que são "a atualização do formato atual de Rich Snippets" e fornece resultados de pesquisa para dispositivos móveis atraentes apresentados em carrosséis que podem ser navegados rolando para a esquerda e para a direita.

Os Rich Cards do Google também usam marcação de dados estruturada e o vocabulário schema.org.

Não deixe de usar os Rich Snippets no seu site!
