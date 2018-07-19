---

copyright:
  years: 2018
lastupdated: "2018-06-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Obtendo seu CDN para Status em Execução

Saiba como colocar seu CDN em um estado RUNNING seguindo estas diretrizes. Este documento também
informa como iniciar e parar o CDN.

## Obter para Em Execução

Depois de ter criado um CDN, ele aparecerá no painel CDN. Aqui você verá o nome do CDN, a Origem, o Provedor e o status.  

 ![Mapeando a captura de tela de lista](images/mapping_list_cname.png)


Se você solicitou seu CDN com HTTP ou HTTPS com o certificado curinga, será possível continuar
com a Etapa 1.

Se você criou um CDN com HTTPS com o certificado SAN DV, etapas adicionais podem ser necessárias
para verificar seu domínio e podem ser localizadas na página
[Concluindo Domain Control Validation para HTTPS](how-to-https.html#completing-domain-control-validation-for-https).

**Etapa 1:**

Depois de ter pedido um CDN, você precisará configurar o **CNAME** com seu provedor DNS. A maioria dos provedores DNS pode fornecer instruções sobre como configurar ou mudar o CNAME.

   * Durante esse tempo, o status do seu CDN será mostrado como **Configuração do CNAME**. Verifique com seu provedor DNS quando as mudanças se tornarão ativas.

   ![Configuração do CNAME](images/cname-config.png)  

**Etapa 2:**

A qualquer momento, depois de ter configurado o CNAME com seu provedor DNS, será possível verificar o status selecionando **Obter status** no menu overflow à direita do status do CDN.

  ![getStatus do CNAME](images/cname-getstatus.png)  

**Etapa 3:**

Quando o encadeamento CNAME estiver concluído, a seleção de **Obter status**
mudará o status para *RUNNING* e o CDN estará pronto para uso.

Parabéns! Agora seu CDN está em execução. A partir de agora, a página
[Gerenciar seu CDN](how-to.html#manage-your-CDN) tem informações adicionais sobre como
configurar opções, como [Tempo de
vida](how-to.html#setting-content-caching-time-using-time-to-live-), [Limpando conteúdo em cache](how-to.html#purging-cached-content) e
[Incluindo detalhes do Caminho de Origem](how-to.html#adding-origin-path-details).

## Iniciando o CDN

Um CDN poderá ser iniciado apenas quando estiver no status 'Interrompido'.  

**Etapa 1:**

Clique em 'Iniciar o CDN' no menu Overflow, que aparece como três pontos no lado direito da linha do
CDN.

  ![Overflow menu](images/start_cdn.png)

**Etapa 2:**

Aparece uma janela de diálogo maior pedindo para confirmar que você deseja iniciar o serviço. Selecione **Confirmar** para continuar.

**Etapa 3:**

Se a ação tiver sido bem-sucedida, aparecerá uma caixa de diálogo no canto superior direito de sua tela, permitindo que você saiba que ela foi bem-sucedida, junto ao horário.

**Etapa 4:**

Essa etapa muda o Status para 'Configuração do CNAME'

**Etapa 5:**

Clique em 'Obter status' no menu Overflow. Essa etapa muda o status para 'Em execução'. Seu CDN se torna operacional.

## Parando o CDN

Um CDN poderá ser interrompido apenas quando estiver no status 'Em execução'.

**Etapa 1:**

Clique em 'Parar CDN' no menu Overflow (3 pontos verticais à direita do status CDN).
![Menu Overflow](images/stop_cdn.png)

**Etapa 2:**

Aparece uma janela de diálogo maior, solicitando que você confirme se deseja parar o serviço. Selecione **Confirmar** para continuar.

**Etapa 3:**

Depois de aproximadamente 5 a 15 segundos, o status deve mudar para 'Interrompido'

## Excluindo o CDN

Para excluir um CDN, siga estas etapas:

**NOTA**: selecionar `Excluir` no menu overflow exclui apenas o CDN; ele não exclui sua
conta.

**Etapa 1:**

Clique em 'Excluir' no menu overflow.

 ![Excluir CDN no Menu Overflow](images/delete_cdn.png)

**Etapa 2:**

Aparece uma janela de diálogo maior pedindo para confirmar que você deseja excluir. Clique em **Excluir**
para continuar.

**NOTA**: se seu CDN estiver configurado usando HTTPS com o Certificado SAN DV, poderá
levar até 5 horas para concluir o processo de exclusão.

  ![Delete with Warning](images/delete-with-warning.png)

**Etapa 3:**

Depois de concluir as etapas 1 e 2, o status de seu CDN será `Deleting`.
Quando o processo de exclusão estiver concluído, clicar novamente em 'Obter status' no menu overflow
removerá a linha da lista do CDN. Se o processo de exclusão não tiver sido concluído, esta ação não terá efeito.
