---

copyright:
  years: 2017
lastupdated: "2017-09-06"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Perguntas mais frequentes

## O que é Content Delivery Network (CDN)?

O Content Delivery Network (CDN) é uma coleção de servidores de borda distribuídos por várias partes do país ou do mundo. Seu conteúdo da web é entregue de um servidor de borda, que está localizado na área geográfica mais próxima do cliente que solicita o conteúdo. Essa técnica permite que os usuários recebam o conteúdo com menos atraso do que seria possível entregando o conteúdo de um local centralizado. Ele entrega uma experiência geral melhor para seus clientes.

## Como o CDN funciona?

O CDN atinge seu propósito armazenando em cache o conteúdo da web em servidores de borda ao redor do mundo. Quando um usuário solicitar conteúdo da web, a solicitação de conteúdo será roteada para o servidor de borda que estiver geograficamente mais próximo desse usuário. Reduzindo a distância que o conteúdo deve viajar, o CDN oferece rendimento otimizado, latência minimizada e aumento de desempenho. 

## Como minha conta do CDN é criada?

Sua conta é criada durante o processo de ordem do CDN, quando você clica em **Selecionar** depois de passar pelo menu "Seleção do fornecedor".

## O que fazer quando meu CDN estiver no status CNAME_CONFIGURATION?

Atualize seu registro DNS para que seu website aponte para o `CNAME` associado ao novo mapeamento de CDN. 
**Nota**: pode levar até 15 - 30 minutos para a atualização se tornar ativa. Verifique com seu provedor DNS para obter uma estimativa de tempo exata.

## Pelo que serei cobrado?

Você será cobrado pela largura da banda usada por CDN. Se nenhuma largura de banda for usada por seu CDN, não haverá encargos. Os preços da largura de banda variam, dependendo do local regional do servidor de borda.

## Quando serei cobrado?

O faturamento do CDN ocorre de acordo com o período de faturamento estabelecido em sua conta do {{site.data.keyword.BluSoftlayer_notm}}.

## Como visualizo as métricas e o uso?

É possível visualizar métricas e uso na página **Visão geral**, que pode ser acessada selecionando seu CDN na página **Content Delivery Networks**. **Nota**: depois de criar seu CDN, pode levar até 48 horas para que as métricas apareçam.

## Há um número mínimo de dias durante os quais eu posso visualizar as métricas? Há um máximo?

Sim. As métricas podem ser reunidas por um mínimo de 7 dias. As métricas podem ser visualizadas por um máximo de 90 dias. Para aqueles que usam a API, recomenda-se usar 89 dias como o máximo, para considerar quaisquer diferenças em fusos horários.

## Como o certificado curinga HTTPS funciona?

O certificado curinga é a maneira mais econômica para entregar conteúdo da web para seus usuários finais com segurança. Para usar o certificado curinga, o cliente deve usar o nome do host CNAME como o ponto de entrada do serviço (por exemplo, _https://example.cdnedge.bluemix.net_). Depois que o mapeamento de CDN é ativado para HTTPS usando o certificado curinga, o servidor de borda entra em contato com o Servidor de origem por meio de HTTPS. Se o Servidor de origem for especificado como um nome de host, o servidor de borda usará o domínio de origem como o cabeçalho Server Name Indication (SNI) para a negociação da Segurança da Camada de Transporte (TLS) com o Servidor de origem, por padrão. No entanto, se o domínio de origem for especificado como um endereço IP, o cabeçalho do host de origem precisará ser fornecido. Outra dica é que o certificado de origem deve ser assinado por uma Autoridade de certificação reconhecida (CA).

## Se eu selecionar `delete` no menu overflow do CDN, isso excluirá minha conta?

Não, excluirá apenas esse CDN. Sua conta ainda existirá e será possível criar CDNs adicionais.

## O armazenando em cache usa push ou pull?

O armazenamento em cache de conteúdo é feito usando um modelo _pull de origem_. Pull de origem é um método pelo qual os dados são "puxados" pelo servidor de borda do Servidor de origem, em vez de fazer upload manualmente do conteúdo para o servidor de borda. 

## Há um valor máximo para o Tempo de vida? Um mínimo?

O valor máximo para o Tempo de vida é 21.474.836.471 segundos, o que equivale a praticamente 681 anos! O valor mínimo é 30 segundos.

## O que é a opção Entregar antigo?

Se o tempo de armazenamento em cache expirar para um conteúdo, o servidor de borda tentará buscar o conteúdo do Servidor de origem. Se por algum motivo o Servidor de origem estiver desativado ou não puder ser contatado, o servidor de borda poderá entregar o conteúdo antigo, se essa opção estiver configurada, que é o que a maioria dos clientes prefere. Atualmente, nossos CDNs só podem ser configurados com essa opção definida como **Sim** como padrão. Mudar a opção para **Não** não terá nenhum impacto: a opção **Entregar conteúdo antigo** continuará configurada como **Sim**.

## Há um limite no número de entradas de Origem e de TTL?

Sim, o limite combinado é 75 entradas por CDN.

## Há um limite no número de CDNs que eu posso ter?

Sim, é possível ter um limite de 10 CDNs por conta.

## Há um navegador recomendado para usar para a configuração de serviço do CDN?

Sim, Firefox e Chrome são os navegadores recomendados, em suas versões mais recentes.

## Qual é o propósito de se fornecer um Caminho ao criar meu CDN?

Se você fornecer um caminho ao criar um CDN, ele permitirá isolar os arquivos que podem ser entregues por meio do CDN de um Servidor de origem específico.

## Como sei que o meu CDN está funcionando?
Execute o comando 'curl', substituindo "origin.cdntesting.net/assets/ibm_3d.gif" pelo caminho do respectivo arquivo em sua origem: 
```
curl -I -H "Pragma: akamai-x-cache-on, akamai-x-cache-remote-on, akamai-x-check-cacheable, akamai-x-get-cache-key, akamai-x-get-extracted-values, akamai-x-get-nonces, akamai-x-get-ssl-client-session-id, akamai-x-get-true-cache-key, akamai-x-serial-no" origin.cdntesting.net/assets/ibm_3d.gif
```
Se a saída do comando corresponder ao formato a seguir, o CDN estará funcionando conforme o esperado: 
```
HTTP/1.1 200 OK

Server: nginx/1.13.0

...

X-Cache: TCP_HIT from a199-117-103-28.deploy.akamaitechnologies.com (AkamaiGHost/9.0.2.1-20488781) (-)

X-Cache-Key: /L/1363/535014/1d/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

X-True-Cache-Key: /L/origin.cdntesting.net.ibm.com/assets/ibm_3d.gif

...

...

X-Akamai-Session-Info: name=WSD_BEST_PRACTICES_TD_TYPE; value=PATTERNS_BASED_PARENT_MAP

X-Serial: 1363

Connection: keep-alive

X-Check-Cacheable: YES
```

## Meu CDN está em um estado de erro. O que faço agora?
Consulte a página ['Obtendo ajuda e suporte'](https://console.stage1.bluemix.net/docs/infrastructure/CDN/getting-help.html#gettinghelp) ou abra um chamado no [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/).

## Onde localizo meu CName se não tiver fornecido um? 
Clique no CDN para chegar à página **Visão geral**. No canto direito será possível ver uma seção **Detalhes** com as informações de `CName`.

## Há um limite no número de solicitações de Limpeza que podem ficar ativas ao mesmo tempo?
Sim. Pode haver apenas 5 solicitações de limpeza ativas de cada vez.

## Minha solicitação de Limpeza para um determinado caminho de arquivo está em Andamento. Posso enviar uma nova solicitação para o mesmo caminho de arquivo?
Não. Pode haver apenas uma solicitação de Limpeza ativa para um determinado caminho de arquivo de cada vez.
