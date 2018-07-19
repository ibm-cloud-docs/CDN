---

copyright:
  years: 2018
lastupdated: "2018-05-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Métricas

## Como visualizo as métricas e o uso?

É possível visualizar métricas e uso na página **Visão geral**, que pode ser acessada selecionando seu CDN na página **Content Delivery Networks**. **NOTA**: após criar o seu CDN, pode demorar até 48 horas para as métricas aparecerem.

## Eu criei um CDN e havia tráfego de dados por meio do CDN. Por que minhas métricas não aparecem?

Após a criação de um CDN, são necessárias 48 horas para que as métricas apareçam.


## Há um número mínimo de dias durante os quais eu posso visualizar as métricas? Há um máximo?

Há um número mínimo e um número máximo de dias para os quais é possível visualizar as métricas usando o IBM Cloud Content
Delivery Network com o Akamai. As métricas podem ser reunidas por um mínimo de 7 dias. As métricas podem ser visualizadas por um máximo de 90 dias. Para aqueles que usam a API, recomenda-se usar 89 dias como o máximo, para considerar quaisquer diferenças em fusos horários.

## Por que a taxa de acertos é diferente de zero quando o total de ocorrências é zero?
A taxa de acertos representa a porcentagem de vezes que o conteúdo foi entregue no Cache Edge Server, em vez de ser entregue do Servidor de Origem. Ela é calculada da seguinte maneira:

> ((Ocorrências do Edge - Ocorrências de ingresso)/Ocorrências do Edge) * 100

em que: ocorrências do Edge são definidas como "Todas as ocorrências dos servidores de borda dos usuários
finais"  
As ocorrências de ingresso são definidas como "Ocorrências de origem ou de ingresso são para o tráfego de sua
origem para os servidores de borda Akamai"

Como a taxa de acertos é calculada no nível de conta e não por CDN, a taxa de acertos será a mesma para todos os CDNs em sua
conta. Este fato também explica por que a Taxa de Acertos pode ser diferente de zero quando o número de ocorrências do Edge para um CDN particular é zero.

