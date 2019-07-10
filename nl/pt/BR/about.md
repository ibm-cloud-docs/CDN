---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-03"

keywords: content delivery network, web content, caching, edge servers, streaming content

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Sobre o Content Delivery Networks (CDN)
{: #about-content-delivery-networks-cdn-}

Um Content Delivery Network (CDN) é uma coleção de servidores de ponta distribuídos por várias partes do país ou do mundo. O conteúdo da web é entregue de um servidor de borda, que está localizado na área geográfica mais próxima do cliente
que solicita o conteúdo. Essa técnica permite que seus usuários finais recebam o conteúdo com menos atraso, o que fornece uma
experiência geral melhor para seus clientes.

## Como um CDN funciona?
{: #how-does-a-cdn-work}

Um CDN atinge o seu propósito armazenando em cache o conteúdo da web em servidores de ponta ao redor do mundo. Quando um
cliente solicita o conteúdo da web, a solicitação de conteúdo é roteada para o servidor de borda que estiver geograficamente mais
próximo a esse cliente. Reduzindo a distância que o conteúdo deve viajar, o CDN oferece rendimento otimizado, latência minimizada e aumento de desempenho. Usando o {{site.data.keyword.cloud}} Content Delivery Network com o Akamai, os provedores de conteúdo podem realizar entrega eficiente de conteúdo solicitado de todo o mundo, com configuração mínima.

![Diagrama CDN de alto nível](images/high-level-cdn-diagram.png)

## Recursos
{: #features}

Os recursos-chave do serviço {{site.data.keyword.cloud}} Content Delivery Network incluem:
  * Suporte de origem do servidor host
  * Suporte de origem do Object Storage
  * Suporte para múltiplas origens com caminhos distintos
  * Mapeamentos do CDN baseados em caminho
  * Limpe o conteúdo em cache
  * TTL (tempo de vida)
  * Métricas com visualizações gráficas
  * Suporte ao Protocolo HTTPS com os Certificados Curinga e SAN DV
  * Suporte do cabeçalho do host
  * Entregue conteúdo antigo
  * Args de Consulta de Chave de Cache
  * Compactação de conteúdo
  * Otimização de arquivo grande
  * Vídeo on demand
  * Controle de acesso geográfico
  * Hotlink Protection

Consulte [o documento de descrições do recurso](/docs/infrastructure/CDN?topic=CDN-feature-descriptions) para ver as descrições completas do recurso.

## Diagrama Arquitetural
{: #architectural-diagram}

O diagrama a seguir oferece uma visão geral esquemática da arquitetura de três camadas do IBM Cloud CDN.

![Architectural diagram](images/3-tier-architecture.png)
