---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, bandwidth, overview, hit ratio, edge server, cache, ingress, hits

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Métricas
{: #metrics}

Quando você seleciona seu CDN pela primeira vez na lista, a página Visão geral é aberta. Aqui é possível ver a Largura total da banda, o Total de ocorrências e a Taxa de acertos do período selecionado (o padrão é 30 dias).

  ![Visão geral das métricas](images/metrics-overview.png)

Diretamente abaixo da visão geral, você verá uma representação gráfica da Largura total da banda, da Largura da banda por região, do Total de ocorrências e de Ocorrências por tipo.

  ![Gráficos de medidas](images/metrics-graphs.png)

**NOTA**: após criar o seu CDN, pode demorar até 48 horas para as métricas aparecerem.

## Há um número mínimo de dias durante os quais eu posso visualizar as métricas? Há um máximo?

Há um número mínimo e um número máximo de dias para os quais é possível visualizar as métricas usando o {{site.data.keyword.cloud}} Content Delivery Network com a Akamai. As métricas podem ser reunidas por um mínimo de 7 dias. As métricas podem ser visualizadas por um máximo de 90 dias. Para aqueles que usam a API, recomenda-se usar 89 dias como o máximo, para considerar quaisquer diferenças em fusos horários.

## Por que a taxa de acertos é diferente de zero quando o total de ocorrências é zero?
A taxa de acertos representa a porcentagem de vezes que o conteúdo foi entregue no Cache Edge Server, em vez de ser entregue do Servidor de Origem. Ela é calculada da seguinte maneira:

`((Ocorrências do Edge - Ocorrências de ingresso)/Ocorrências do Edge) * 100`

em que:

_Ocorrências de borda_ são definidas como "Todas as ocorrências dos servidores de borda dos usuários finais".  
_Ocorrências de ingresso_ são definidas como "Ocorrências de origem ou ingresso são relativas ao tráfego de sua origem para servidores de borda da Akamai."

Como a taxa de acertos é calculada no nível de conta e não por CDN, a taxa de acertos será a mesma para todos os CDNs em sua
conta. Este fato também explica por que a Taxa de Acertos pode ser diferente de zero quando o número de ocorrências do Edge para um CDN particular é zero.

## As métricas são atualizadas em tempo real?

Não. As métricas são atualizadas a cada 24 horas.
