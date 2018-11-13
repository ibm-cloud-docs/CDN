---

copyright:
  years: 2017, 2018
lastupdated: "2018-10-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Perguntas mais frequentes

## O que é um Content Delivery Network (CDN)?

Um Content Delivery Network (CDN) é uma coleção de servidores de ponta distribuídos por várias partes do país ou do mundo. Seu conteúdo da web é entregue de um servidor de borda, que está localizado na área geográfica mais próxima do cliente que solicita o conteúdo. Essa técnica permite que os usuários recebam o conteúdo com menos atraso do que seria possível entregando o conteúdo de um local centralizado. Ele entrega uma experiência geral melhor para seus clientes.

## Como um Content Delivery Network (CDN) funciona?

Um CDN atinge o seu propósito armazenando em cache o conteúdo da web em servidores de ponta ao redor do mundo. Quando um usuário solicitar conteúdo da web, a solicitação de conteúdo será roteada para o servidor de borda que estiver geograficamente mais próximo desse usuário. Reduzindo a distância que o conteúdo deve viajar, o CDN oferece rendimento otimizado, latência minimizada e aumento de desempenho.

## Como a minha conta de serviço do IBM Cloud Content Delivery Network foi criada?

Sua conta é criada durante o processo de pedido do CDN. Se você estiver criando um CDN por meio do portal Anterior, quando clicar no botão **Pedir CDN**, na **página Rede -> CDN**, sua conta será criada. Se estiver criando um CDN por meio do portal me nuvem da IBM, quando clicar no botão **Criar**, na página **Catálogo -> Rede -> Content Delivery Network**, sua conta será criada.

## O que fazer quando meu CDN estiver no status de configuração CNAME?

Para o CDN de HTTPS baseado em Certificados HTTP e SAN, atualize seu registro DNS para que seu website aponte para o `CNAME` associado ao novo mapeamento de CDN. Para o CDN de HTTPS baseado em Certificado curinga, essa atualização de DNS **NÃO** é necessária.

**Nota**: pode levar até 15 - 30 minutos para a atualização se tornar ativa. Verifique com seu provedor DNS para obter uma estimativa de tempo exata.

Um registro CNAME típico seria semelhante ao seguinte na página de configuração do DNS:

| **Tipo de recurso** | **Host** | **Pontos para (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutos |


## Pelo que serei cobrado?

Você é cobrado apenas pela largura da banda usada por instância do IBM Cloud Content Delivery Network. Se nenhuma largura de banda for usada por seu CDN, não haverá encargos. Os preços da largura de banda variam, dependendo do local regional do servidor de borda. É possível ver a precificação da largura de banda por região geográfica na seção [Introdução](getting-started.html#cdn-bandwidth-pricing-rates-shown-in-usd-) para esse serviço.

## Quando serei cobrado?

O faturamento do IBM Cloud Content Delivery Network ocorre de acordo com o período de faturamento estabelecido em sua conta do {{site.data.keyword.BluSoftlayer_notm}}.

## Se eu selecionar `delete` no menu overflow do CDN, isso excluirá minha conta?

Não, excluirá apenas esse CDN. Sua conta ainda existirá e será possível criar CDNs adicionais.

## O armazenando em cache usa push ou pull?

O armazenamento em cache de conteúdo é feito usando um modelo _pull de origem_. Pull de origem é um método pelo qual os dados são "puxados" pelo servidor de borda do Servidor de origem, em vez de fazer upload manualmente do conteúdo para o servidor de borda.

## Há um navegador recomendado para usar para a configuração de serviço do CDN?

Sim, o Firefox e o Chrome são os navegadores recomendados. É recomendado que você use suas versões mais recentes com o IBM
Cloud Content Delivery Network.

## Qual é o propósito de fornecer um caminho ao criar meu CDN?

Se você fornecer um caminho ao criar seu CDN, ele permitirá isolar os arquivos que podem ser entregues por meio do CDN de um
servidor de origem específico.

## Meu CDN está em um estado de erro. O que faço agora?

Consulte as páginas [Resolução de problemas](troubleshooting.html#troubleshooting) ou [Obtendo ajuda e suporte](getting-help.html#gettinghelp) ou abra um chamado no [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/).

## Onde localizo meu CNAME se eu não forneci um?

Clique em seu CDN para acessar a página **Visão geral** no portal. No canto direito será possível ver uma seção **Detalhes** com as informações de `CName`.

## Minha solicitação de Limpeza para um determinado caminho de arquivo está em Andamento. Posso enviar uma nova solicitação para o mesmo caminho de arquivo?

Não. Pode haver apenas uma solicitação de Limpeza ativa para um determinado caminho de arquivo de cada vez.

## O Protocolo da Internet versão 6 (IPv6) é suportado com o serviço do IBM Cloud Content Delivery Network? Como isto funciona?

IPv6 (ou suporte de pilha duplo) é suportado por servidores Edge de Akamai. É projetado para ajudar os clientes com IPv4 apenas origem a aceitar conexões de clientes IPv6, converter de IPv6 para IPv4 no Edge e seguir adiante com a origem com IPv4.

**NOTA:** a criação de um CDN do IBM Cloud usando um endereço IPv6 como o endereço do servidor de origem não é suportado.

## Existem restrições sobre quais números de porta HTTP e HTTPS são permitidos para Akamai?

Sim. Para o fornecedor do Akamai, somente os números de porta a seguir são permitidos: 72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111,
1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 e 45002.

## Qual URL deve ser usada para acesso aos dados no CDN ou Caminho de Origem?
O caminho para um mapeamento CDN ou para a origem é tratado como um diretório. Portanto, os usuários que estão tentando acessar o caminho de origem devem acessá-lo como um diretório (com uma barra). Por exemplo, se CDN `www.example.com` for criado usando o caminho que inclui o diretório `/images`, a URL para atingi-lo deverá ser `www.example.com/images/`

Omitir a barra, por exemplo, usando `www.example.com/images` resultará em um erro **Página Não Localizada**.

## Como configurar o Content Delivery Network para o IBM Cloud Object Storage (COS)?

[Aqui está um tutorial](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sobre a criação de um Content Delivery Network para o IBM Cloud Object Storage.

## Recebi notificação de que meu certificado de origem está expirando. O que faço agora?

Siga as etapas descritas [nesse artigo](https://community.akamai.com/docs/DOC-7708) do Akamai.

## Qual segurança está incluída com a solução IBM CDN com Akamai?

Usando a plataforma Akamai distribuída, você obtém escala e resiliência incomparáveis com mais de 240.000 servidores em mais de 130 países. A Plataforma Akamai está entre sua infraestrutura e seus usuários finais e age como o primeiro nível de defesa para picos repentinos no tráfego. Akamai Intelligent Platform também é um proxy reverso que atende e responde a solicitações nas portas 80 e 443 apenas, o que significa que o tráfego em outras portas é descartado na borda sem ser encaminhado para a sua infraestrutura.

## Os cookies do servidor de origem são preservados pelo CDN do Akamai? 

Para conteúdo não armazenável em cache ou qualquer conteúdo que não seja armazenado em cache, os cookies são preservados da origem. Para conteúdo armazenado em cache pelos servidores de borda, os cookies não são preservados.

## Como usar o IBM Cloud Console para conceder a outros usuários permissão para criar ou gerenciar um CDN?

No IBM Cloud Console, o usuário principal da conta pode fornecer a outros usuários a permissão para criar e gerenciar um CDN. Na página principal do IBM Cloud Console, siga estas etapas para editar permissões:
 * Selecione a guia **Conta**
 * Selecione **Usuários -> Lista de usuários**
 * Clique no **Nome de usuário** desejado
 * Em seguida, selecione a guia **Permissões do portal**
 * Selecione a guia **Serviços**
 * Selecione **Gerenciar a conta CDN**
 * Clique no botão **Editar permissões do portal**
 * Configure as permissões necessárias.

