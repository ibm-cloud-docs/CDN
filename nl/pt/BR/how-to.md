---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Gerenciar seu CDN
{: #manage-your-cdn}

Este documento descreve tarefas comuns para gerenciar seu CDN.

## Configurando o tempo de armazenamento em cache de conteúdo usando "Tempo de vida"

Depois que seu CDN estiver em execução, será possível configurar o tempo de armazenamento em cache de conteúdo usando o Tempo de vida (TTL). O Tempo de vida de um determinado caminho de arquivo ou diretório indica quanto tempo esse conteúdo deve ficar armazenado em cache. Quando você criou o Mapeamento de CDN, um TTL global padrão de 3.600 segundos (1 hora) foi criado.

**Etapa 1:**  

Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.

**Etapa 2:**  

É possível ajustar o tempo usando as setas ou inserindo um novo tempo. O valor de tempo é especificado em segundos. Por exemplo, 3.600 segundos é igual a 1 hora. O menor valor para `timeToLive` que pode ser escolhido é 0 segundos, enquanto o maior é 2147483647 segundos (aproximadamente 24.855 dias). Selecione o botão **Salvar** para configurar o tempo de armazenamento em cache de conteúdo.

  ![Incluindo TTL](images/adding-path.png)

**Etapa 3:**

Depois de salvar, será possível **Editar** ou **Excluir** a configuração TTL usando as opções do menu overflow. (**NOTA**: o Caminho para TTL não pode ser mudado. Se o caminho de mapeamento mudar, o caminho do TTL será atualizado automaticamente.)

  ![Editar ou excluir TTL](images/edit-delete-ttl-setting.png)  

  * Quando o conteúdo corresponder a múltiplas regras, a configuração incluída mais recentemente terá precedência.

  * Os valores de TTL podem ser configurados apenas para um nome de arquivo ou um diretório específico. As expressões comuns não são suportadas, porque podem criar um comportamento imprevisível.

## Incluindo detalhes do caminho de origem

Quando seu CDN estiver no status *CNAME_Configuration* ou *Em execução*, será possível incluir detalhes do Caminho de origem. É possível optar por fornecer conteúdo de múltiplos Servidores de origem. Por exemplo, é possível entregar fotos de um servidor diferente de vídeo. A Origem pode se basear em um Servidor host ou um Object Storage.

**Nota:** O CDN faz uma transformação de URL para o servidor de origem. Por exemplo, se a origem `xyz.example.com` for incluída com o caminho `/example/*` quando um usuário abrir URL `www.example.com/example/*`, o servidor de borda CDN recuperará o conteúdo de `xyz.example.com/*`.

**Etapa 1:**

Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.  

**Etapa 2:**

Selecione a guia **Origens** e, em seguida, selecione o botão **Incluir origem**. Essa etapa abre uma nova janela de diálogo, na qual é possível configurar sua origem.  

   ![Incluir origem de Origens](images/add-origins.png)

**Etapa 3:**

*Deve-se* fornecer um caminho. Você pode fornecer opcionalmente um cabeçalho do host.  

   ![Incluir origem de Origens](images/add-origin-path.png)

**Etapa 4:**

Selecione **Servidor** ou **Object Storage**.

  * Se você tiver selecionado **Servidor**, insira o Endereço do servidor de origem como endereço IPv4 ou o _nome do host_. É recomendado fornecer o nome do host e um nome completo do domínio (FQDN). Dependendo de qual protocolo você selecionou durante a criação do CDN, também forneça uma porta HTTP, uma porta HTTPS ou ambas. Se você usar uma porta HTTPS, o endereço do servidor de origem **deverá** ser um _nome do host_ e não um endereço IP.

       ![Incluir servidor de origem](images/add-origin-server-default.png)

  * Se você tiver selecionado **Object Storage**, forneça o Terminal, o nome do Depósito e a porta HTTPS. Opcionalmente, especifique as extensões de arquivo que podem ser usadas no serviço CDN. Se nada for especificado, todas as extensões de arquivo serão permitidas.

       ![Incluir armazenamento de objeto de origem](images/add-origin-object-storage.png)

  * As opções **Otimização** e **Chave de cache** são as mesmas para as configurações
de servidor e Object Storage.

    * Escolha opções de **Otimização** no menu suspenso. **Entrega geral da web**
é a opção padrão ou é possível escolher as otimizações **Arquivo grande** ou **Vídeo on demand**. **Entrega geral da web** permite que o CDN entregue conteúdo até 1,8 GB, enquanto que a otimização
**arquivo grande** permite downloads de arquivos de 1,8 GB até 320 GB. **Vídeo on demand**
otimiza seu CDN para entrega de formatos de fluxo segmentado. As descrições de recurso para
[Otimização de arquivo grande](/docs/infrastructure/CDN/feature-descriptions.html#large-file-optimization) e [Vídeo
on Demand](/docs/infrastructure/CDN/feature-descriptions.html#video-on-demand) fornecem informações adicionais.

        ![Opções de configuração de desempenho](images/performance-config-options.png)

    * Escolha as opções de **Chave de cache** no menu suspenso. A opção padrão é **Include-all**. Se você selecionar **Incluir especificado** ou **Ignorar especificado**, **deverá** inserir sequências de consulta a serem incluídas ou ignoradas, separadas por um espaço. Por exemplo, insira `uuid=123456` para uma sequência de consulta única ou `uuid=123456 issue=important` para duas sequências de consultas.  É possível descobrir mais sobre os [Argumentos de consulta de chave de cache](/docs/infrastructure/CDN/feature-descriptions.html#cache-key-query-args) na descrição do recurso.

        ![Opções da chave de cache](images/cache-key-options.png)

**NOTA**: as opções de protocolo e de porta mostradas pela UI corresponderão ao que foi selecionado durante o pedido do CDN. Por exemplo, se **Porta HTTP** foi selecionada como parte do pedido de um CDN, apenas a opção de **Porta HTTP** será mostrada como parte de Incluir origem.

**Etapa 5:**

Selecione o botão **Incluir** para incluir seu Caminho de origem.

  **Nota**: ao fornecer extensões de arquivo para um caminho de origem do Object Storage, a configuração de TTL com a mesma URL como o caminho de origem terá o escopo definido para incluir todos os arquivos que tenham essas extensões de arquivo especificadas. Por exemplo, se você criar um caminho de origem de `/example` e especificar extensões de arquivo de "jpg png gif", o valor de TTL do caminho de TTL `/example` terá um escopo que incluirá todos os arquivos JPG/PNG/GIF sob o diretório `/example` e seus subdiretórios.

**Etapa 6:**

Depois de incluir, será possível **Editar** ou **Excluir** a Origem usando as opções do menu overflow.

  ![Editar ou excluir Origem](images/edit-delete-origin.png)

## Limpando conteúdo em cache

Depois que seu CDN estiver em execução, será possível limpar o conteúdo em cache do servidor do Fornecedor.

**Etapa 1:**

Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.

**Etapa 2:**

Selecione a guia **Limpar**.

   ![Página Limpar](images/purge_tab.png)

**Etapa 3:**

Insira a sintaxe de caminho Unix padrão para indicar qual arquivo você gostaria de limpar e, em seguida, selecione o botão **Limpar**. A limpeza é permitida somente para um único arquivo neste momento. Consulte a página
[Regras e
convenções de nomenclatura](/docs/infrastructure/CDN/rules-and-naming-conventions.html#what-are-the-rules-for-the-path-string-for-purge-) para obter mais detalhes sobre qual sintaxe é
permitida para o caminho de Limpeza.

**Etapa 4:**

Após a limpeza, a atividade é listada sob **Limpar atividade**. É possível **Refazer a limpeza** ou **Favorito** do caminho usando as opções do menu overflow.

   ![Limpar atividade](images/purge-activity.png)

   **NOTA:** se houver mais de 15 limpezas, Limpar atividade será cortada a cada 15 dias, automaticamente.

## Atualizando os detalhes de configuração do CDN

Depois que seu CDN estiver em execução, será possível atualizar os detalhes de configuração do CDN.

**Etapa 1:**

Na página CDN, selecione seu CDN, que o levará à página **Visão geral**.

**Etapa 2:**

Selecione a guia **Configurações**. Os detalhes de configuração do CDN são exibidos.

   ![Guia Configurações](images/settings-tab.png)  
   **NOTA**: o Certificado SSL é exibido somente quando seu CDN é configurado com HTTPS.

Para **Servidor**, os campos a seguir podem mudar:
  * Cabeçalho do host
  * Endereço do servidor de origem
  * Porta HTTP/HTTPS
  * Entregar conteúdo antigo
  * Respeitar cabeçalhos
  * Opções de otimização
  * Consulta de cache    

Para **Object Storage**, os campos a seguir podem mudar:
  * Cabeçalho do host
  * Ponto de Extremidade
  * Nome do depósito
  * Porta HTTPS
  * Extensões de arquivo permitidas
  * Entregar conteúdo antigo
  * Respeitar cabeçalhos
  * Opções de otimização
  * Consulta de cache

**Etapa 3:**

Atualize os detalhes de **Origem** ou de **Outras opções** se necessário e, em seguida, clique no botão **Salvar** no canto inferior direito para atualizar os detalhes de configuração do CDN.

   ![Botão Salvar](images/save-button.png)

## Configure o IBM Cloud Object Storage para CDN

Para usar os objetos armazenados no IBM Cloud Object Storage, deve-se configurar o valor da propriedade "acl" (isto é, a lista de controle de acesso) para cada objeto em seu depósito para acesso de "leitura pública".

Para instalar quaisquer clientes ou ferramentas necessários, consulte a [seção do IBM Cloud Object Storage Developer](https://{DomainName}/docs/services/cloud-object-storage/basics/developers.html#for-developers). Este guia supõe que você tenha instalado a interface da linha de comandos oficial do AWS, que é compatível com a API S3 do IBM Cloud Object Storage.

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
