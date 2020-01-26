---
title: "O que é o HTTP/3? Disponível no Cloudflare, Chrome e Firefox"
date: "2019-11-20"
categories: [ wordpress ]
lang: "pt-BR"
image: "assets/images/category/wordpress-full.png"
---

HTTP/3 é a próxima grande mudança do protocolo HTTP que deixará seu site muito mais rápido e vai mudar muita coisa no desempenho da web nos próximos anos.

Fique por dentro do HTTP/3 nesse episódio do **[WordCast](https://www.luizeof.com.br/)**!

https://open.spotify.com/episode/1TJWEP45HZKkS4NGA1bDxK

Então, a partir de agora, a Cloudflare anunciou que todos os clientes (inclusive os gratuitos) poderão ativar uma opção em seus painéis e ativar o suporte a HTTP/3 para seus domínios.

Isso significa que sempre que os usuários visitarem um site hospedado no Cloudflare a partir de um cliente compatível com HTTP/3, a conexão é atualizada automaticamente para o novo protocolo, em vez de ser manipulada por versões mais antigas.

Em relação ao navegador, o Chrome Canary adicionou suporte para HTTP/3 no início deste mês. Se você usa esta versão do Chrome já vai notar um desempenho muito melhor na navegação e abertura dos sites.

Além disso, da mesma forma, a Mozilla também anunciou que lançaria suporte para HTTP/3. O fabricante do navegador está programado enviar o HTTP/3 em uma próxima versão do Firefox Nightly em breve.

## Mas afinal, o que é HTTP/3

O HTTP/3 é a próxima versão principal do HTTP, o protocolo através do qual o conteúdo é transferido dos servidores para os clientes, onde é exibido dentro de navegadores, aplicativos móveis ou outros aplicativos.

O HTTP/3 – é diferente de tudo que veio antes. É uma reescrita completa do HTTP que usa o protocolo QUIC em vez do TCP e também vem com suporte TLS (criptografia) embutido.

É uma junção de múltiplas tecnologias. Tudo destinado a tornar os sites carregados mais rapidamente e através de conexões criptografadas por padrão.

## Um pouco de história

O protocolo TCP foi projetado nos anos 70 e ninguém esperava que fosse usado para comunicações quase em tempo real, como é hoje.

Com o passar dos anos, os engenheiros de software começaram a entender que o TCP nunca foi projetado para velocidade e nós desenvolvedores temos que ficar driblando as limitações do HTTP o tempo todo.

Ao longo dos anos, várias equipes de engenheiros tentaram criar um protocolo de camada de transporte um pouco melhor. De todos, os engenheiros do Google foram os mais bem-sucedidos.

Eles criaram o **SPDY** (pronuncia-se speedy), um protocolo que corrigia alguns problemas do TCP, e mais tarde foi usado para o HTTP-over-SPDY. Este foi um protocolo que acabou se tornando o HTTP/2 oficial, agora usado em cerca de 40% de todos os sites da Internet.

Porém, o SPDY foi apenas mais uma melhoria no TCP. Os engenheiros do Google perceberam que poderiam fazer muito melhor se combinasse a confiabilidade do TCP e a velocidade do UDP, em um protocolo totalmente novo.

Foi assim que surgiu o **QUIC** (pronuncia-se Quick), ou “Conexões rápidas à Internet UDP”.

Como o próprio nome indica, este é um protocolo que mescla os melhores recursos do TCP e UDP, a fim de criar um protocolo de transporte de camada 4 mais rápido.

HTTP/3 é o QUIC implementado dentro de HTTP, substituindo TCP e SPDY no nível de transporte. Foi formalmente aprovado em outubro passado.

## Cloudflare vai impulsionar adoção do HTTP/3

O suporte inicial foi adicionado no Chrome 29 e Opera 16 e nos servidores LiteSpeed. Portanto, o suporte ao Chrome expandiu agora no final de 2019.

Porém, a grande novidade é que o Cloudflare disponibilizando o protocolo para todos os clientes de um modo bem transparente e fácil de implementar. Basta apertar um único botão.

A rede de entrega de conteúdo (CDN) do Cloudflare é um dos principais players da Web, alimentando cerca de 10% de todos os sites da Internet.

O Cloudflare foi um dos principais impulsionadores da adoção do HTTP/2, que lançou seu suporte para todos os clientes em dezembro de 2015.

Agora, a empresa acredita que é hora de mudar a Web para um protocolo ainda melhor, mais rápido, mas que também vem com suporte interno para TLS, o protocolo no coração do HTTPS.

Porém o HTTP/3 é atualmente usado por apenas 3% de todos os sites da Internet.

E você? vai habilitar o HTTP/3 no Cloudflare? Se vocÊ ainda não conhece o Cloudflare e quer configurar o seu site hoje mesmo, não perca meu [curso no Udemy: Configurando e Otimizando o Cloudflare para Wordpress](https://www.udemy.com/course/configurando-e-otimizando-o-cloudflare-com-wordpress/?referralCode=E6BECFD733FAAB376595).

Um abraço!
