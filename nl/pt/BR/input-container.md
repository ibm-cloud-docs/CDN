---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-04-04"

keywords: input, container, class, API, mapping, origin, path, provider, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Contêiner de entrada
{: #input-container}

O contêiner de entrada é uma coleção utilizada por ambas as classes, mapeamento e caminho (origem). Ele fornece um conjunto consistente de atributos de entrada para ambas as classes.

* `vendorName`: o nome de um provedor do {{site.data.keyword.cloud}} CDN válido.
* `oldPath`: usado por updateOriginPath(). Essa propriedade armazena o nome do caminho atual ou 'antigo'.

Os atributos a seguir são comuns para as classes de mapeamento e caminho (origem):
* `originType`: O tipo de Host de origem, atualmente 'HOST_SERVER' ou 'OBJECT_STORAGE'.
* `origin`: endereço do servidor de origem (ou o nome do host ou o endereço IPv4 do servidor de origem), o
terminal do qual buscar conteúdo ou o nome do depósito onde o conteúdo é armazenado. Deve ser menor que 511 caracteres.
* `httpPort`: número da porta usada para o protocolo HTTP. A Akamai tem certas limitações em números de porta para as portas de HTTP e HTTPS. Consulte as [Perguntas frequentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter uma lista de números de porta permitidos.
* `httpsPort`: número da porta usada para o protocolo HTTPS. A Akamai tem certas limitações em números de porta para as portas de HTTP e HTTPS. Consulte as [Perguntas frequentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter uma lista de números de porta permitidos.
* `status`: o status do mapeamento ou do caminho. O status pode ser CNAME_CONFIGURATION, SSL_CONFIGURATION, RUNNING, STOPPED, DELETED ou ERROR.
* `path`: caminho por meio do qual o conteúdo em cache será entregue. O caminho padrão é /\* Quando usados
pela API `updateOriginPath`, esse atributo se refere ao novo caminho a ser incluído.
* `performanceConfiguration`: especificações para a configuração de desempenho do mapeamento.
* `cacheKeyQueryRule`: as opções a seguir estão disponíveis para configurar o comportamento da Chave de cache:
  * `include-all`: inclua todos os argumentos de consulta
  * `ignore-all`: ignore todos os argumentos de consulta
  * `ignore: space separated query-args`: ignora esses argumentos de consulta específicos. Por exemplo, `ignore: query1 query2`
  * `include: space separated query-args`: inclui esses argumentos de consulta específicos. Por exemplo, `include: query1 query2`
* `geoblockingRule`

Os atributos a seguir são específicos para a classe de mapeamento:

* `uniqueId`: um identificador de 10 dígitos, gerado pelo sistema, que é exclusivo para cada mapeamento. Gerada quando um mapeamento é criado.
* `domain`: nome do CDN primário. Também referido como nome do host.
* `protocol`: protocolo usado para configurar os serviços. Pode ser HTTP, HTTPS ou uma combinação dos dois,
HTTP_AND_HTTPS.
* `cname`: registro de nome canônico cria alias do nome do host. Pode ser fornecido pelo usuário ou
gerado pelo sistema. Se fornecido pelo usuário, ele deve ter menos que 64 caracteres alfanuméricos e deve ser exclusivo. Se não fornecido, um será gerado quando o mapeamento for criado.
* `certificateType`: tipo de certificado que está sendo usado por um mapeamento. Pode ser `WILDCARD_CERT` ou `SHARED_SAN_CERT`. O valor
será 0 para mapeamentos de HTTP.
* `respectHeaders`: um Valor booleano que, se configurado como 'true', fará as configurações do TTL na Origem substituírem as configurações do TTL do CDN.
* `cabeçalho`: especifica informações do cabeçalho do host usadas pela origem.

Os atributos a seguir são relacionados ao Cloud Object Storage (COS):  
* `bucketName`: o nome exclusivo de seu depósito para armazenamento de objetos S3.  
* `fileExtension`: extensões de arquivo que são permitidas.

O atributo a seguir está relacionado à configuração do Hotlink Protection:
* `hotlinkProtection`: consulte a [classe de proteção do hotlink](/docs/infrastructure/CDN?topic=CDN-hotlink-protection-class) para obter mais detalhes.
