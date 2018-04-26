---

copyright:
  years: 2017,2018
lastupdated: "2018-02-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Pedir um CDN

Aqui você aprenderá como pedir um Content Delivery Network (CDN). Seu CDN pode ser pedido no [Portal do Cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/) ou do [Portal do Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## No Portal de controle:

**Etapa 1:**

Para iniciar, efetue logon no [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/) usando suas credenciais exclusivas.

**Etapa 2:**

Na barra de navegação na parte superior do monitor, selecione **Rede -> CDN**.

   ![Opções do menu de rede](images/network-cdn.png)

**Etapa 3:**

Na página **Content Delivery Networks**, selecione o botão **Pedir CDN** no canto superior direito.

   ![Selecionar pedir CDN](images/order-cdn-button.png)

## No Portal do Bluemix:

**Etapa 1:**

Efetue logon no [Portal do Bluemix](https://www.ibm.com/cloud-computing/bluemix/)

**Etapa 2:**

Clique em [Catálogo do IBM Bluemix](https://console.bluemix.net/catalog/). Na barra de navegação esquerda, selecione **Rede**.

   ![Navegação no CDN do Bluemix](images/bluemix_navigation.png)

**Etapa 3:**

Clique no **Quadro CDN**, que o levará para a tela Seleção do fornecedor.

   ![Ícone CDN do Bluemix](images/bluemix_tile.png)


**Etapa 4:**

Na tela **Selecionar um provedor CDN**, escolha entre as opções de provedor CDN. Clique no botão
**Selecionar** para confirmar suas opções selecionadas e, em seguida, clique em **Iniciar
provisão** na parte inferior direita de sua tela, para começar o processo de fornecimento.  
       ![Selecionar provedor de CDN](images/Vendor_Select_And_Provision.png)
**Etapa 5:**

Preencha o campo **Configurar nome**:  

  * Especifique o **nome do host** (**necessário**), que serve como o
identificador primário para o seu CDN (por exemplo, _example.testingcdn.net_).  
  * Opcionalmente, é possível fornecer um **CNAME** customizado (como _myfirstcdn.cdnedge.bluemix.net_). Se nenhum CNAME for fornecido, um será criado para você. <informações de validação a serem incluídas aqui>  

       ![Configurar nome](images/configure-hostname-cname.png)  

    **Nota**: o uso de um CNAME inadequado pode levar à finalização dos serviços.

**Etapa 6:**

Preencha o campo **Configurar sua origem**: para configurar esse campo, deve-se selecionar a opção **Servidor** ou **Object Storage**.  

   * Especifique o **Cabeçalho do host** (opcional). Se um não for fornecido, ele será padronizado para o
**Nome do host**. Consulte a descrição do recurso para [Suporte do
cabeçalho do host](about.html#host-header-support-) para obter mais informações sobre o cabeçalho do host.
   
   * Forneça um **Caminho** (opcional). O caminho deve ser relativo ao **Nome do
host**. 
   
      ![Configurar origem](images/configure-origin.png)  

  * **A opção Servidor**: se você selecionar a opção **Servidor**, insira o nome do host ou o endereço IP do Servidor de origem do qual os dados devem ser armazenados em cache.
      * Deve-se especificar o **Endereço do Servidor de Origem** (nome do host ou endereço IPv4 do Servidor de Origem) se você selecionar esta opção. 
Se **Porta HTTPS** for selecionada, o **Endereço do servidor de origem** deverá ser o nome do host, e não um endereço IP.
      * Também é possível fornecer uma **Porta HTTP**, uma **Porta HTTPS** ou ambas. Esses campos indicam qual protocolo e número de porta podem ser usados para entrar em contato com o Servidor de Origem. Para números de porta não padrão, consulte [Perguntas Frequentes](faq.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter uma lista dos números de porta permitidos.

	     ![Configurar o servidor de origem](images/configure-origin-server.png)

  * **A opção Object Storage**: se você selecionar a opção **Object Storage**, deverá fornecer as informações a seguir:
      * o **Terminal** do qual buscar o Objeto,
      * o nome do **Depósito** no qual seu conteúdo está armazenado e
      * a **Porta HTTPS**.
      * Também é possível especificar as extensões de arquivo, separadas por vírgulas, que podem ser usadas no serviço CDN. (Se nenhuma extensão de nome de arquivo for especificada, todas as extensões de arquivo serão permitidas.)
      * Deve-se configurar a **Lista de controle de acesso** (ACL) de cada **Objeto** no **Depósito** como "leitura pública".

	     ![Configurar o armazenamento de objeto](images/configure-origin-object-storage.png)

**Etapa 7:**

Configure o campo **Outras opções**: essa seção contém opções de configuração para o
campo **Respeitar cabeçalhos**.

   * Quando a opção **Respeitar cabeçalhos** estiver **Ligada**, as configurações de
TTL definidas no cabeçalho pela origem substituirão o TTL padrão do CDN. **Respeitar cabeçalhos** é configurada como **Ativada** por padrão, mas deve-se configurar esse campo.  

        ![Outras opções](images/other-options.png)

**Etapa 8:**

Selecione o botão **Criar** no canto inferior direito para criar seu CDN.
