---

copyright:
  years: 2017
lastupdated: "2017-09-10"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Como pedir um CDN

Aqui você aprenderá como pedir um Content Delivery Network (CDN). Seu CDN pode ser pedido no [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/) ou no [Portal do catálogo do Bluemix](https://www.ibm.com/cloud-computing/bluemix/).

## No Portal de controle:

1. Para iniciar, efetue logon no [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/) usando suas credenciais exclusivas.

2. Na barra de navegação na parte superior do monitor, selecione **Rede -> CDN**.

   ![Opções do menu de rede](images/network-cdn.png)

3. Na página **Content Delivery Networks**, selecione o botão **Pedir CDN** no canto superior direito.

	![Selecionar pedir CDN](images/order-cdn-button.png)

## No Portal do Bluemix:

1. Efetue logon no [Portal do Bluemix](https://www.ibm.com/cloud-computing/bluemix/)

2. Clique em **Catálogo do IBM Bluemix**. Na barra de navegação esquerda, selecione **Rede**.

   ![Navegação no CDN do Bluemix](images/bluemix_navigation.png)

3. Clique no **Quadro CDN**, que o levará para a tela Seleção do fornecedor.

   ![Ícone CDN do Bluemix](images/bluemix_tile.png)

4. Na tela **Selecionar um provedor CDN**, escolha entre as opções de provedor CDN. Clique no botão **Selecionado** na parte inferior da tela para confirmar suas opções selecionadas e, em seguida, clique em **Iniciar provisão**, para iniciar o processo de fornecimento.

	![Selecionar provedor CDN](images/newReducedSizeVendorSelectAndProvision.png)
	
5. Preencha o campo **Configurar nome**: 
      * Especifique o _nome do host_ (**necessário**), que serve como o identificador primário para seu CDN (por exemplo, _example.testingcdn.net_).
      * Opcionalmente, é possível fornecer um _CNAME_ customizado (como _myfirstcdn.cdnedge.bluemix.net_). Se nenhum CNAME for fornecido, um será criado para você. <informações de validação a serem incluídas aqui>
      
      ![Configurar nome](images/configure-hostname-cname.png)
		
6. Preencha o campo **Configurar sua origem**: para configurar esse campo, deve-se selecionar a opção **Servidor** ou **Object Storage**. (A especificação de **Caminho** é opcional. <informações de validação>)
		
  * **A opção Servidor**: se você selecionar a opção **Servidor**, insira o nome do host ou o endereço IP do Servidor de origem do qual os dados devem ser armazenados em cache. 
      * Se você selecionar essa opção, deverá especificar o **Endereço IP** do Servidor de origem.
      * Também é possível fornecer uma **Porta HTTP**, uma **Porta HTTPS** ou ambas. (Esses campos indicam qual protocolo e número de porta podem ser usados para entrar em contato com o Servidor de origem.)

	   ![Configurar o servidor de origem](images/configure-origin-server.png)
		
  * **A opção Object Storage**: se você selecionar a opção **Object Storage**, deverá fornecer as informações a seguir:
      * o **Terminal** do qual buscar o Objeto,
      * o nome do **Depósito** no qual seu conteúdo está armazenado e
      * a **Porta HTTPS**.
      * Também é possível especificar as extensões de arquivo, separadas por vírgulas, que podem ser usadas no serviço CDN. (Se nenhuma extensão de nome de arquivo for especificada, todas as extensões de arquivo serão permitidas.)
      * Deve-se configurar a **Lista de controle de acesso** (ACL) de cada **Objeto** no **Depósito** como "leitura pública".
		
	   ![Configurar o armazenamento de objeto](images/configure-origin-object-storage.png)

7. Configure o campo **Outras opções**: esta seção contém opções de configuração para o campo **Entregar conteúdo antigo** e o campo **Respeitar cabeçalhos**.
    
     * O campo **Entregar conteúdo antigo**: **Entregar conteúdo antigo** é um valor booleano que indica se o conteúdo em cache que expirou deverá ser entregue, caso o servidor de origem não possa ser acessado. Em razão de uma limitação atual, a funcionalidade **Entregar conteúdo antigo** fica ativada por padrão, independentemente de ser configurada como **Ativada** ou **Desativada** por um usuário.
     * O campo **Respeitar cabeçalhos**: quando a opção **Respeitar cabeçalhos** estiver **Ativada**, as configurações de TTL definidas no cabeçalho pela Origem substituirão o TTL padrão do CDN. **Respeitar cabeçalhos** é configurada como **Ativada** por padrão, mas deve-se configurar esse campo.

		![Outras opções](images/other-options.png)
		
8. Selecione o botão **Criar** no canto inferior direito para criar seu CDN.
