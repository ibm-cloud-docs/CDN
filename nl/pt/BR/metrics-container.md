---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-25"

keywords: metrics, class, container, collection, attributes, array, pie chart, API

subcollection: CDN

---

# Classe de contêiner para métricas
{: #container-class-for-metrics}

A coleção `SoftLayer_Container_Network_CdnMarketplace_Metrics` contém os atributos utilizados por
nossas APIs de métricas. Há três valores possíveis para o atributo `type`: PIECHART, LINEGRAPH ou
TOTALS. A saída para o restante dos atributos nesse contêiner depende de `type`, que é descrito em mais detalhes
abaixo. Todos os atributos, exceto `type` são matrizes. Nem todos os atributos são utilizados para todos os tipos.

**class** `SoftLayer_Container_Network_CdnMarketplace_Metrics`:
* `type`
* `names`
* `totals`
* `percentage`
* `hora`
* `xaxis`
* `yaxis`
* `yaxis2`
* `yaxis3`
* `yaxis4`
* `yaxis5`
* `yaxis6`
* `yaxis7`
* `yaxis8`
* `yaxis9`
* `yaxis10`
* `yaxis11`
* `yaxis12`
* `yaxis13`
* `yaxis14`
* `yaxis15`
* `yaxis16`
* `yaxis17`
* `yaxis18`
* `yaxis19`
* `yaxis20`

O tipo PIECHART é para dados que serão enviados para um gráfico de pizza. Nesse caso, os únicos atributos usados são:
`names`, `xaxis` e `yaxis`. O atributo`names` é uma matriz que
contém os nomes de região. O atributo `xaxis` contém o uso em GB e `yaxis1` é a porcentagem
de teste.


O tipo LINEGRAPH é para dados que serão enviados para um gráfico de linhas. Nesse caso, a matriz `names`
conterá os nomes das métricas que estão localizadas nos atributos `xaxis`, `yaxis`,
`yaxis2`...`yaxis20` e os dados reais estarão nas matrizes correspondentes.


Se `type` for TOTALS, a matriz `names` conterá os nomes das métricas localizadas na matriz
`totals` correspondente e os dados reais estarão contidos na matriz `totals`.
