---

copyright:
  years: 2017
lastupdated: "2017-09-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Gerenciar seu CDN

Saiba como gerenciar a configuração de seu CDN seguindo estas diretrizes.
 
## Deixando o CDN no status de execução

Depois de ter criado um CDN, ele aparecerá no painel CDN. Aqui você verá o nome do CDN, a Origem, o Provedor e o status.  

 ![Mapeando a captura de tela de lista](images/mapping_list_cname.png)

1. Depois de ter pedido um CDN, você precisará configurar o **CNAME** com seu provedor DNS. A maioria dos provedores DNS pode fornecer instruções sobre como configurar ou mudar o CNAME.
   
   * Durante esse tempo, o status do seu CDN será mostrado como **Configuração do CNAME**. Verifique com seu provedor DNS quando as mudanças se tornarão ativas.

   ![Configuração do CNAME](images/cname-config.png)  
   
2. A qualquer momento, depois de ter configurado o CNAME com seu provedor DNS, será possível verificar o status selecionando **Obter status** no menu overflow à direita do status do CDN.
   
  ![getStatus do CNAME](images/cname-getstatus.png)  
    
3. Quando o encadeamento do CNAME estiver concluído, o status da conta mudará para *RUNNING* e o CDN estará pronto para uso.
   	  
Parabéns! Agora seu CDN está em execução.  
	  
## Parando o CDN
Um CDN poderá ser interrompido apenas quando estiver no status 'Em execução'. 

1. Clique em 'Parar CDN' no menu Overflow (3 pontos verticais à direita do status CDN).
![Menu Overflow](images/stop_cdn.png)

2. Aparece uma janela de diálogo maior pedindo para confirmar que você deseja iniciar o serviço. Selecione **Confirmar** para continuar.
  
3. Depois de aproximadamente 5 a 15 segundos, o status deve mudar para 'Interrompido'


## Iniciando o CDN
Um CDN poderá ser iniciado apenas quando estiver no status 'Interrompido'.
1. Clique em 'Iniciar CDN' no menu Overflow, que aparece como três pontos do lado direito da linha CDN.
![Menu Overflow](images/start_cdn.png)

2. Aparece uma janela de diálogo maior pedindo para confirmar que você deseja iniciar o serviço. Selecione **Confirmar** para continuar.

3. Se a ação tiver sido bem-sucedida, aparecerá uma caixa de diálogo no canto superior direito de sua tela, permitindo que você saiba que ela foi bem-sucedida, junto ao horário.
  
4. Isso mudará o status para 'Configuração do CNAME'

5. Clique em 'Obter status' no menu Overflow. Isso mudará o status para 'Em execução'. E o CDN se tornará operacional.

## Excluindo o CDN

Para excluir um CDN, siga as etapas abaixo:
**NOTA**: a seleção de `Delete` no menu overflow exclui apenas o CDN; ele não excluirá sua conta.

1. Clique em 'Excluir' no menu overflow.

 ![Excluir CDN no Menu Overflow](images/delete_cdn.png)

2. Aparece uma janela de diálogo maior pedindo para confirmar que você deseja excluir. Clique em **Excluir** para continuar

3. Isso mudará o status para 'Excluindo'. Clique em 'Obter status' no menu overflow. Isso removerá a linha da lista CDN.

## Configurando o tempo de armazenamento em cache de conteúdo usando "Tempo de vida"

Depois que seu CDN estiver em execução, será possível configurar o tempo de armazenamento em cache de conteúdo usando o Tempo de vida (TTL). O Tempo de vida de um determinado caminho de arquivo ou diretório indica quanto tempo esse conteúdo deve ficar armazenado em cache. Quando você criou o Mapeamento de CDN, um padrão TTL global de 3.600 segundos foi criado. 

1. Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.

2. É possível ajustar o tempo usando as setas ou inserindo um novo tempo. O valor de tempo é especificado em segundos. Por exemplo, 3.600 segundos é igual a 1 hora. O menor valor para `timeToLive` que pode ser escolhido é 30 segundos, enquanto o maior é 21.474.836.471 segundos. Selecione o botão **Salvar** para configurar o tempo de armazenamento em cache de conteúdo. 

  ![Incluindo TTL](images/adding-path.png)

3. Depois de salvar, será possível **Editar** ou **Excluir** a configuração TTL usando as opções do menu overflow. (**NOTA**: o Caminho para TTL não pode ser mudado. Se o caminho de Mapeamento for mudado, o caminho de TTL será atualizado automaticamente.)

  ![Editar ou excluir TTL](images/edit-delete-ttl-setting.png)  
	
  * Quando o conteúdo corresponder a múltiplas regras, a configuração incluída mais recentemente terá precedência.
  
  * Os valores de TTL podem ser configurados apenas para um nome de arquivo ou um diretório específico. As expressões comuns não são suportadas, porque podem criar um comportamento imprevisível.
	
## Incluindo detalhes do caminho de origem

Quando seu CDN estiver no status *CNAME_Configuration* ou *Em execução*, será possível incluir detalhes do Caminho de origem. É possível optar por fornecer conteúdo de múltiplos Servidores de origem. Por exemplo, é possível entregar fotos de um servidor diferente de vídeo. A Origem pode se basear em um Servidor host ou um Object Storage.

1. Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.
	
2. Selecione a guia **Origens** e, em seguida, selecione o botão **Incluir origem**.
	
3. *Deve-se* fornecer um caminho. Selecione **Servidor** ou **Object Storage**.
	
   ![Incluir origem de Origens](images/add-origin.png)
		
  * Se você tiver selecionado **Servidor**, insira o endereço IP do servidor ou o _nome do host_. Forneça uma porta HTTP, uma porta HTTPS, ou ambas, dependendo de qual protocolo tiver sido selecionado durante a criação do CDN; ele deve corresponder a esse mapeamento.
	
  ![Incluir servidor de origem](images/add-origin-server.png)
	
  * Se você tiver selecionado **Object Storage**, forneça o Terminal, o nome do Depósito e a porta HTTPS. Opcionalmente, especifique as extensões de arquivo que podem ser usadas no serviço CDN. Se nada for especificado, todas as extensões de arquivo serão permitidas.
	
  ![Incluir armazenamento de objeto de origem](images/add-origin-object-storage.png)
		
4. Selecione o botão **Incluir** para incluir seu Caminho de origem.

  **Nota**: quando você fornecer extensões de arquivo para um caminho de origem do Object Storage, a configuração de TTL que tiver a mesma URL do caminho de origem terá o escopo definido para incluir todos os arquivos que tiverem essas extensões de arquivo especificadas. Por exemplo, se você criar um caminho de origem de "/example" e especificar extensões de arquivo de "jpg png gif", o valor de TTL do caminho de TTL "/example" terá um escopo incluindo todos os arquivos JPG/PNG/GIF sob o diretório "/example" e seus subdiretórios.

5. Depois de incluir, será possível **Editar** ou **Excluir** a Origem usando as opções do menu overflow.

  ![Editar ou excluir Origem](images/edit-delete-origin.png)
	
## Limpando conteúdo em cache

Depois que seu CDN estiver em execução, será possível limpar o conteúdo em cache do servidor do Fornecedor.  

1. Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.
	
2. Selecione a guia **Limpar**.

	![Página Limpar](images/purge_tab.png)
	
3. Insira a sintaxe de caminho Unix padrão para indicar qual arquivo você gostaria de limpar e, em seguida, selecione o botão **Limpar**. Limpar é permitido apenas para um único arquivo neste momento.

4. Após a limpeza, a atividade é listada sob **Limpar atividade**. É possível **Refazer a limpeza** ou **Favorito** do caminho usando as opções do menu overflow.

	![Limpar atividade](images/purge-activity.png)
	
   **NOTA:** se houver mais de 15 limpezas, Limpar atividade será cortada a cada 15 dias, automaticamente.
	
## Atualizando os detalhes de configuração do CDN

Depois que seu CDN estiver em execução, será possível atualizar os detalhes de configuração do CDN.

1. Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.

2. Selecione a guia **Configurações**. Os detalhes de configuração do CDN são exibidos.

  Para **Object Storage**, os campos a seguir poderão ser mudados: 
   * Ponto de Extremidade
   * Nome do depósito
   * Porta HTTPS
   * Extensões de arquivo permitidas
   * Entregar conteúdo antigo
   * Respeitar cabeçalhos

  Para **Servidor**, os campos a seguir poderão ser mudados: 
   * Endereço do servidor de origem
   * Porta HTTP/HTTPS
   * Entregar conteúdo antigo
   * Respeitar cabeçalhos

3. Atualize os detalhes de **Origem** ou de **Outras opções** se necessário e, em seguida, clique no botão **Salvar** no canto inferior direito para atualizar os detalhes de configuração do CDN.

   ![Botão Salvar](images/save-button.png)


## Configure o IBM Cloud Object Storage para CDN

Para usar os objetos armazenados no IBM Cloud Object Storage, deve-se configurar o valor da propriedade "acl" (isto é, a lista de controle de acesso) para cada objeto em seu depósito para acesso de "leitura pública". 

Consulte a seção Ferramentas do IBM Cloud Object Storage Developer Center (https://developer.ibm.com/cloudobjectstorage/) para instalar os clientes ou as ferramentas necessárias. Este guia supõe que você tenha instalado a interface da linha de comandos oficial do AWS, que é compatível com a API S3 do IBM Cloud Object Storage.

O código de exemplo abaixo mostra como configurar o acesso de "leitura pública" para todos os objetos em seu depósito, usando a interface da linha de comandos.

```
$ export ENDPOINT="YOUR_ENDPOINT"
$ export BUCKET="YOUR_BUCKET"
$ KEYS=( "$(aws --endpoint-url "$ENDPOINT" s3api list-objects --no-paginate --query 'Contents[].{key: Key}' --output text --bucket "$BUCKET")" )
$ for KEY in "${KEYS[@]}"
  > do
  >   aws --endpoint-url "$ENDPOINT" s3api put-object-acl --bucket "$BUCKET" --key "$KEY" --acl "public-read"
  > done
```
