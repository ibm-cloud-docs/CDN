---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-05-21"

keywords: faqs, content delivery network, cname, configuration, status, ports, hotlink protection, error state, file path, cloud object storage, security, console, main page, create

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}
{:DomainName: data-hd-keyref="DomainName"}

# Perguntas mais frequentes
{: #faqs}

## O que é um Content Delivery Network (CDN)?
{: #what-is-a-content-delivery-network-cdn}
{: faq}

Um Content Delivery Network (CDN) é uma coleção de servidores de ponta distribuídos por várias partes do país ou do mundo. Seu conteúdo da web é entregue de um servidor de borda, que está localizado na área geográfica mais próxima do cliente que solicita o conteúdo. Essa técnica permite que os usuários recebam o conteúdo com menos atraso do que seria possível entregando o conteúdo de um local centralizado. Ele entrega uma experiência geral melhor para seus clientes.

## Como um Content Delivery Network (CDN) funciona?
{: #how-does-a-content-delivery-network-cdn-work}
{: faq}

Um CDN atinge o seu propósito armazenando em cache o conteúdo da web em servidores de ponta ao redor do mundo. Quando um usuário solicitar conteúdo da web, a solicitação de conteúdo será roteada para o servidor de borda que estiver geograficamente mais próximo desse usuário. Reduzindo a distância que o conteúdo deve viajar, o CDN oferece rendimento otimizado, latência minimizada e aumento de desempenho.

## Como a minha conta de serviço do IBM Cloud Content Delivery Network foi criada?
{: #how-is-my-ibm-cloud-content-delivery-network-service-account-created}
{: faq}

Sua conta é criada durante o processo de pedido do CDN. Se você estiver criando um CDN por meio do portal Anterior, quando clicar no botão **Pedir CDN**, na **página Rede -> CDN**, sua conta será criada. Se você estiver criando um CDN por meio do portal do {{site.data.keyword.cloud}}, ao clicar no botão **Criar**, na página **Catálogo -> Rede -> Content Delivery Network**, sua conta será criada.

## O que fazer quando meu CDN estiver no status de configuração CNAME?
{: #what-do-i-do-when-my-cdn-is-in-cname-configuratione-status}
{: faq}

Para o CDN de HTTPS baseado em Certificados HTTP e SAN, atualize seu registro DNS para que seu website aponte para o `CNAME` associado ao novo mapeamento de CDN. Para o CDN de HTTPS baseado em Certificado curinga, essa atualização de DNS **NÃO** é necessária.

**Nota**: pode levar até 15 - 30 minutos para a atualização se tornar ativa. Verifique com seu provedor DNS para obter uma estimativa de tempo exata.

## Como incluo um registro CNAME para meu domínio CDN no DNS?
{: #how-do-i-add-a-cname-record-for-my-cdn-domain-in-dns}
{: faq}

Na página de configuração de DNS de seu domínio CDN, é possível criar um registro CNAME com o nome de Domínio CDN como o Host e o IBM CNAME usado para configurar o CDN, como o CNAME. O IBM CNAME termina com `cdnedge.bluemix.net`.

Um registro CNAME típico seria semelhante ao seguinte na página de configuração do DNS:

| **Tipo de recurso** | **Host** | **Pontos para (CNAME)** | **TTL** |
|------------------|---------|-------------|----------------|
| CNAME | www.example.com | example.cdnedge.bluemix.net | 15 minutos |


## O que será faturado a mim no meu CDN
{: #what-will-i-be-billed-for-in-my-cdn}
{: faq}

Você é cobrado apenas pela largura da banda usada por instância do IBM Cloud Content Delivery Network. Se nenhuma largura de banda for usada por seu CDN, não haverá encargos. Os preços da largura de banda variam, dependendo do local regional do servidor de borda. É possível ver a precificação de largura da banda por região geográfica no [documento de precificação](/docs/infrastructure/CDN?topic=CDN-pricing) para esse serviço.

## Quando vou ser faturado por meu CDN?
{: #when-will-i-be-billed-for-my-cdn}
{: faq}

O faturamento do IBM Cloud Content Delivery Network ocorre de acordo com o período de faturamento estabelecido em sua conta do {{site.data.keyword.BluSoftlayer_notm}}.

## Se eu selecionar `delete` no menu overflow do CDN, isso excluirá minha conta?
{: faq}

Não, excluirá apenas esse CDN. Sua conta ainda existirá e será possível criar CDNs adicionais.

## O armazenamento em cache de conteúdo usa push ou pull?
{: #does-content-caching-use-push-or-pull}
{: faq}

O armazenamento em cache de conteúdo é feito usando um modelo _pull de origem_. Pull de origem é um método pelo qual os dados são "puxados" pelo servidor de borda do Servidor de origem, em vez de fazer upload manualmente do conteúdo para o servidor de borda.

## Há um navegador recomendado para usar para a configuração de serviço do CDN?
{: #is-there-a-recommended-browser-to-use-for-cdn-service-configuration}
{: faq}

Sim, o Firefox e o Chrome são os navegadores recomendados. É recomendado que você use suas versões mais recentes com o IBM
Cloud Content Delivery Network.

## Qual é o propósito de fornecer um caminho ao criar meu CDN?
{: #what-is-the-purpose-of-providing-a-path-when-creating-my-cdn}
{: faq}

Se você fornecer um caminho ao criar seu CDN, ele permitirá isolar os arquivos que podem ser entregues por meio do CDN de um
servidor de origem específico.

## Meu CDN está em um estado de erro. O que faço agora?
{: faq}

Consulte as páginas [Resolução de problemas](/docs/infrastructure/CDN?topic=CDN-troubleshooting#troubleshooting) ou [Obtendo ajuda e suporte](/docs/infrastructure/CDN?topic=CDN-gettinghelp#gettinghelp) ou abra um chamado no [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/).

## Onde localizo o CNAME para meu CDN se eu não tiver fornecido um?
{: faq}

Clique em seu CDN para acessar a página **Visão geral** no portal. No canto direito será possível ver uma seção **Detalhes** com as informações de `CName`.

## Minha Solicitação de limpeza para um determinado caminho de arquivo está em andamento. Posso enviar uma nova solicitação para o mesmo caminho de arquivo?

Não. Pode haver apenas uma solicitação de Limpeza ativa para um determinado caminho de arquivo de cada vez.

## O Protocolo da Internet versão 6 (IPv6) é suportado com o serviço do IBM Cloud Content Delivery Network? Como isto funciona?
{: faq}

IPv6 (ou suporte de pilha duplo) é suportado por servidores Edge de Akamai. É projetado para ajudar os clientes com IPv4 apenas origem a aceitar conexões de clientes IPv6, converter de IPv6 para IPv4 no Edge e seguir adiante com a origem com IPv4.

**NOTA:** a criação de um CDN do IBM Cloud usando um endereço IPv6 como o endereço do servidor de origem não é suportado.

## Existem restrições sobre quais números de porta HTTP e HTTPS são permitidos para Akamai?
{: faq}

Sim. Para o fornecedor do Akamai, somente os números de porta a seguir são permitidos: 72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111,
1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 e 45002.

## Qual URL deve ser usada para acesso aos dados no CDN ou Caminho de Origem?
{: faq}

O caminho para um mapeamento CDN ou para a origem é tratado como um diretório. Portanto, os usuários que estão tentando acessar o caminho de origem devem acessá-lo como um diretório (com uma barra). Por exemplo, se CDN `www.example.com` for criado usando o caminho que inclui o diretório `/images`, a URL para atingi-lo deverá ser `www.example.com/images/`

Omitir a barra, por exemplo, usando `www.example.com/images` resultará em um erro **Página Não Localizada**.

## Como configurar o Content Delivery Network para o IBM Cloud Object Storage (COS)?
{: faq}

[Aqui está um tutorial](/docs/tutorials?topic=solution-tutorials-static-files-cdn) sobre a criação de um Content Delivery Network para o IBM Cloud Object Storage.

## Recebi notificação de que meu certificado de origem está expirando. O que faço agora?
{: faq}

Siga as etapas descritas [neste artigo ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://community.akamai.com/docs/DOC-7708) da Akamai.

## Qual segurança está incluída com a solução IBM CDN com Akamai?
{: faq}

Usando a plataforma Akamai distribuída, você obtém escala e resiliência incomparáveis com milhares de servidores em mais de 50 países. A Plataforma Akamai está entre sua infraestrutura e seus usuários finais e age como o primeiro nível de defesa para picos repentinos no tráfego. Akamai Intelligent Platform também é um proxy reverso que atende e responde a solicitações nas portas 80 e 443 apenas, o que significa que o tráfego em outras portas é descartado na borda sem ser encaminhado para a sua infraestrutura.

## Os cookies do servidor de origem são preservados pelo CDN do Akamai?
{: faq}

Para conteúdo não armazenável em cache ou qualquer conteúdo que não seja armazenado em cache, os cookies são preservados da origem. Para conteúdo armazenado em cache pelos servidores de borda, os cookies não são preservados.

## Como usar o IBM Cloud Console para conceder a outros usuários permissão para criar ou gerenciar um CDN?
{: faq}

O Usuário principal da conta pode fornecer a outros usuários a permissão para criar e gerenciar um CDN.

Na página principal do console do IBM Cloud, siga estas etapas para editar permissões:
 * Selecione a guia **Gerenciar**
 * Selecione **Acessar (IAM)**
 * Clique na guia **Usuários** na área de janela esquerda
 * Clique no **Usuário** desejado
 * Em seguida, selecione a guia **Infraestrutura clássica**
 * Em seguida, na guia **Permissões**, expanda a categoria **Serviços**
 * Selecione **Gerenciar a conta CDN**
 * Clique no botão **Salvar**

Na página principal do console anterior, siga estas etapas para editar permissões:
 * Selecione a guia **Conta**
 * Selecione **Usuários -> Lista de usuários**
 * Clique no **Nome de usuário** desejado
 * Em seguida, selecione a guia **Permissões do portal**
 * Selecione a guia **Serviços**
 * Selecione **Gerenciar a conta CDN**
 * Clique no botão **Editar permissões do portal**

## Por que o botão Criar não é mostrado ou está desativado na página https://cloud.ibm.com/catalog/infrastructure/cdn-powered-by-akamai?
{: faq}

Se você for o Usuário principal da conta, deverá fazer upgrade da conta para que o botão Criar apareça ou seja ativado nessa página. Na página principal do console do IBM Cloud, siga estas etapas como o Usuário principal da conta:
 * Abra a área de janela de navegação à esquerda clicando no ícone `Barra tripla` no canto superior esquerdo da página da web
 * Selecione **Infraestrutura clássica**
 * Clique no botão **Fazer upgrade de conta** e siga as instruções

Se você for um dos Usuários secundários da conta, o Usuário principal da conta deverá dar a você a permissão `Incluir/Fazer upgrade de serviços` para que o botão Criar apareça ou seja ativado nessa página. Na página do console IBM Cloud, o Usuário principal da conta deverá seguir estas etapas para editar suas permissões:
 * Selecione a guia **Gerenciar**
 * Selecione **Acessar (IAM)**
 * Clique na guia **Usuários** na área de janela esquerda
 * Clique no **Usuário** desejado
 * Em seguida, selecione a guia **Infraestrutura clássica**
 * Em seguida, na guia **Permissões**, expanda a categoria **Conta**
 * Selecione **Incluir/Fazer upgrade de serviços**
 * Clique no botão **Salvar**

## Por que não consigo acessar minha página da web por meio de meu CDN depois de configurar o Hotlink Protection com `protectionType` `ALLOW`?
{: faq}

Vamos considerar um exemplo em que o domínio de seu website para usuários finais esteja configurado para ser o nome do domínio/host de seu CDN: `cdn.example.com`. Quando alguém tenta alcançar uma página da web navegando diretamente da barra de navegação do navegador, o navegador geralmente não envia os cabeçalhos Referentes em sua solicitação de HTTP. Por exemplo, quando você navega diretamente dessa maneira para `https://cdn.example.com/`, seu CDN considera que a solicitação contém uma não correspondência com relação aos `refererValues` especificados. Quando o CDN avalia o efeito ou a resposta apropriados por meio do Hotlink Protection, ele determina que uma não correspondência ocorreu. Portanto, seu CDN negará acesso, em vez de 'ALLOW'.

## Posso usar o terminal privado de armazenamento de objeto nas configurações de CDN?
{: faq}

Não, o CDN somente pode se conectar ao Object Storage em terminais públicos.

## Posso usar o recurso Brotli no serviço CDN?
{: faq}

Não, o recurso Brotli não é suportado com o nosso serviço CDN com Akamai.

## Como criar um terminal CDN sem usar o domínio?
{: faq}

É possível criar um terminal CDN sem usar o Domínio, mas APENAS para um CDN do tipo **Curinga HTTPS**. Ao criar um CDN do tipo **HTTPS curinga**, o seu **CNAME** age como o terminal CDN e o **CNAME** é usado para entregar o tráfego.
