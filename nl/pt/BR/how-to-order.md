---

copyright:
  years: 2017,2018, 2019
lastupdated: "2019-05-21"

keywords: order, create, configure, console, origin, preparation, bucket

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}

# Pedir um CDN
{: #order-a-cdn}

Aqui você aprenderá como pedir um Content Delivery Network (CDN). Seu CDN pode ser pedido por meio do [Console do {{site.data.keyword.cloud}}](https://cloud.ibm.com/login).

## Preparação para pedir
{: #preparation-for-ordering}

Veja aqui como navegar para a página do CDN para fazer seu pedido.

**Etapa 1**

* Efetue login na sua conta por meio do [Console do IBM Cloud](https://cloud.ibm.com/login)

**Etapa 2**

Clique em  [ IBM Cloud Catalog ](https://cloud.ibm.com/catalog/). Na barra de navegação esquerda, selecione **Rede**.

   ![Navegação no IBM Cloud CDN](images/bluemix_navigation.png)

**Etapa 3**

Clique no **Quadro CDN**.

   ![Ícone do IBM Cloud CDN](images/bluemix_tile.png)


## Pedir um novo CDN
{: #order-a-new-cdn}

Depois de estar na página de pedido, aqui está como proceder para criar e configurar seu CDN.

### Etapa 1: Criar sua conta do CDN
{: #create-your-cdn-account}

Clique em **Criar** na parte inferior direita, que criará sua conta do CDN se você ainda não tiver uma e redirecionará você para a tela Configuração do CDN.

   ![Visão geral do CDN](images/content-delivery.png)

### Etapa 2: Nomear seu CDN
{: #step-2-name-your-cdn}

Preencha o campo **Configurar nome**:  

  * Especifique o **nome do host** (**necessário**), que serve como o
identificador primário para o seu CDN (por exemplo, `example.testingcdn.net`).  
  * Opcionalmente, é possível fornecer um **CNAME** customizado (como `myfirstcdn.cdnedge.bluemix.net`). Se nenhum CNAME for fornecido, um será criado para você. O sufixo `cdnedge.bluemix.net` é anexado automaticamente ao CNAME. O uso de um CNAME inapropriado pode levar à finalização dos serviços.

       ![Configurar nome](images/configure-hostname-cname.png)  

Depois de fornecer o novo CDN, você **deverá** configurar o CNAME com seu provedor DNS.
{: note}
### Etapa 3: Configurar sua origem
{: #step-3-configure-your-origin}

Preencha o campo **Configurar sua origem**: para configurar esse campo, deve-se selecionar a opção **Servidor** ou **Object Storage**.  

  * **Etapa 3, Opção 1: a opção de Servidor**

     ![Configurar origem](images/configure-origin-server.png)

      * Deve-se especificar o **Endereço do servidor de origem** (nome do host ou endereço IPv4 do Servidor de origem). Se **Porta HTTPS** também for selecionada, o **Endereço do servidor de origem** deverá ser o nome do host e não um endereço IP.

      * Especifique o **Cabeçalho do host** (opcional). Se nenhum for fornecido, ele será padronizado para o **Nome do host**. Consulte a descrição do recurso para [Suporte do
cabeçalho do host](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support) para obter mais informações sobre o cabeçalho do host.

      * Forneça um **Caminho** no qual o conteúdo possa ser recuperado da Origem (opcional). Consulte a descrição do recurso para [Mapeamentos de CDN baseados em caminho](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) para entender as implicações da inclusão de um caminho neste momento.

      * Também é possível fornecer uma **Porta HTTP**, uma **Porta HTTPS** ou ambas. Esses campos indicam qual protocolo e número de porta podem ser usados para entrar em contato com o Servidor de Origem. Para números de porta não padrão, consulte [Perguntas Frequentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter uma lista dos números de porta permitidos.

      * **Certificado SSL** - Essa opção aparece _somente_ quando a Porta HTTPS é selecionada. Se você selecionar **Porta HTTPS** para o Servidor ou o Object Storage, será possível escolher **Curinga** ou **Certificado DV SAN** como a opção de Certificado SSL. Ambos oferecem a segurança aprimorada fornecida por HTTPS.
        * O **Certificado curinga** permite o tráfego HTTPS apenas ao usar o
**CNAME** e não requer nenhuma ação adicional de sua parte
        * O **Certificado SAN DV** permite o tráfego HTTPS sobre seu domínio, mas
requer etapas adicionais para a verificação. Consulte a página [Concluindo a Domain Control Validation para HTTPS](/docs/infrastructure/CDN/how-to-https.html#completing-domain-control-validation-for-https) para entender as etapas necessárias e as restrições de tempo envolvidas na escolha dessa opção.

	     ![Configurar o servidor de origem](images/ssl-cert-options.png)

  * **Etapa 3, Opção 2: a Opção Object Storage**

    ![Configurar o Object Storage](images/configure-origin-object-storage.png)

      * **Deve-se** especificar o **Terminal** do qual buscar o objeto.

      * Especifique o **Cabeçalho do host** (opcional). Se nenhum for fornecido, ele será padronizado para o **Nome do host**. Consulte a descrição do recurso para [Suporte do
cabeçalho do host](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#host-header-support) para obter mais informações sobre o cabeçalho do host.

      * Forneça um **Caminho** no qual o conteúdo possa ser recuperado da Origem (opcional). Consulte a descrição do recurso para [Mapeamentos de CDN baseados em caminho](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#path-based-cdn-mappings) para entender as implicações da inclusão de um caminho aqui.

      * **Deve-se** fornecer o nome do **Depósito** no qual seu conteúdo é armazenado.

      * Também é possível fornecer uma **Porta HTTP**, uma **Porta HTTPS** ou ambas. Esses campos indicam qual protocolo e número de porta podem ser usados para entrar em contato com o Servidor de Origem. Para números de porta não padrão, consulte [Perguntas Frequentes](/docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter uma lista dos números de porta permitidos.

      * **Certificado SSL** - Essa opção aparece _somente_ quando a Porta HTTPS é selecionada. Se você selecionar **Porta HTTPS** para o Servidor ou o Object Storage, será possível escolher **Curinga** ou **Certificado DV SAN** como a opção de Certificado SSL. Ambos oferecem a segurança aprimorada fornecida por HTTPS.
        * O **Certificado curinga** permite o tráfego HTTPS apenas ao usar o
**CNAME** e não requer nenhuma ação adicional de sua parte
        * O **Certificado DV SAN** permite o tráfego HTTPS sobre seu domínio, mas requer etapas adicionais para verificação. Consulte a página [Concluindo a Domain Control Validation para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#completing-domain-control-validation-for-https) para entender as etapas necessárias e as restrições de tempo envolvidas na escolha dessa opção.

        ![Configure HTTPS](images/ssl-cert-options.png)

Deve-se configurar a **Lista de controle de acesso** (ACL) para cada objeto em seu depósito para "public-read" com seu provedor de armazenamento de objeto de nuvem.
{: note}
      
### Etapa 4
{: #step-4}

* Deve-se selecionar **Eu li o Contrato de serviço principal e concordo com seus termos** na parte inferior direita, acima do botão **Criar**.

* Em seguida, selecione o botão **Criar** no canto inferior direito para criar seu CDN.
