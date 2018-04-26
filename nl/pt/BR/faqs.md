---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-29"

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

Sua conta é criada durante o processo de ordem do CDN quando você clica em **Selecionar** após navegar pelo menu "Seleção do fornecedor".

## O que fazer quando meu CDN estiver no status CNAME_CONFIGURATION?

Para CDN baseado em HTTP, atualize seu registro DNS para que o seu website aponte para o `CNAME` associado ao seu novo mapeamento de CDN. Para CDN baseado em HTTPS, a atualização desse DNS **NÃO** é necessária.

**Nota**: pode levar até 15 - 30 minutos para a atualização se tornar ativa. Verifique com seu provedor DNS para obter uma estimativa de tempo exata.

## Pelo que serei cobrado?

Você é cobrado apenas pela largura da banda usada por instância do IBM Cloud Content Delivery Network. Se nenhuma largura de banda for usada por seu CDN, não haverá encargos. Os preços da largura de banda variam, dependendo do local regional do servidor de borda. É possível ver a precificação da largura de banda por região geográfica na seção [Introdução](https://github.com/IBM-Bluemix-Docs/CDN/blob/master/getting-started.md#pricing-shown-in-usd) para esse serviço.

## Quando serei cobrado?

O faturamento do IBM Cloud Content Delivery Network ocorre de acordo com o período de faturamento estabelecido em sua conta do {{site.data.keyword.BluSoftlayer_notm}}.

## Como visualizo as métricas e o uso?

É possível visualizar métricas e uso na página **Visão geral**, que pode ser acessada selecionando seu CDN na página **Content Delivery Networks**. 
**NOTA**: após criar o seu CDN, pode demorar até 48 horas para as métricas aparecerem.

## Eu criei um CDN e havia tráfego de dados por meio do CDN. Por que minhas métricas não aparecem?

Após a criação de um CDN, são necessárias 48 horas para que as métricas apareçam.

## Há um número mínimo de dias durante os quais eu posso visualizar as métricas? Há um máximo?

Há um número mínimo e um número máximo de dias para os quais é possível visualizar as métricas usando o IBM Cloud Content
Delivery Network com o Akamai. As métricas podem ser reunidas por um mínimo de 7 dias. As métricas podem ser visualizadas por um máximo de 90 dias. Para aqueles que usam a API, recomenda-se usar 89 dias como o máximo, para considerar quaisquer diferenças em fusos horários.

## Por que a taxa de acertos é diferente de zero quando o total de ocorrências é zero?
A taxa de acertos representa a porcentagem de vezes que o conteúdo foi entregue no Cache Edge Server, em vez de ser entregue do Servidor de Origem. Ela é calculada da seguinte maneira:

```
((Ocorrências do Edge - Ocorrências de ingresso)/Ocorrências do Edge) * 100

em que:
Ocorrências do Edge são definidas como "Todas as ocorrências para os servidores do Edge dos usuários finais"
Ocorrências de ingresso são definidas como "Ocorrências de origem ou de ingresso são para tráfego de sua origem para os servidores Edge de Akamai"
```
 
Como a taxa de acertos é calculada no nível de conta e não por CDN, a taxa de acertos será a mesma para todos os CDNs em sua
conta. Este fato também explica por que a Taxa de Acertos pode ser diferente de zero quando o número de ocorrências do Edge para um CDN particular é zero.

## Se eu selecionar `delete` no menu overflow do CDN, isso excluirá minha conta?

Não, excluirá apenas esse CDN. Sua conta ainda existirá e será possível criar CDNs adicionais.

## O armazenando em cache usa push ou pull?

O armazenamento em cache de conteúdo é feito usando um modelo _pull de origem_. Pull de origem é um método pelo qual os dados são "puxados" pelo servidor de borda do Servidor de origem, em vez de fazer upload manualmente do conteúdo para o servidor de borda. 

## Há um valor máximo para o Tempo de vida? Um mínimo?

O valor máximo para Tempo de Vida é 2.147.483.647 segundos, o que equivale a cerca de 68 anos! O valor mínimo é 30 segundos.

## Há um limite no número de entradas de Origem e de TTL?

Sim, o limite combinado é 75 entradas por CDN.

## Há um limite no número de CDNs que eu posso ter?

Sim, é possível ter um limite de 10 CDNs por conta.

## Há um navegador recomendado para usar para a configuração de serviço do CDN?

Sim, o Firefox e o Chrome são os navegadores recomendados. É recomendado que você use suas versões mais recentes com o IBM
Cloud Content Delivery Network.

## Qual é o propósito de fornecer um caminho ao criar meu CDN?

Se você fornecer um caminho ao criar seu CDN, ele permitirá isolar os arquivos que podem ser entregues por meio do CDN de um
servidor de origem específico.

## Como sei que o meu CDN está funcionando?
Execute o seguinte comando 'curl', substituindo "origin.cdntesting.net/assets/ibm_3d.gif" pelo caminho do respectivo
arquivo em sua origem: 
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

## Onde localizo meu CNAME se eu não forneci um? 
Clique em seu CDN para acessar a página **Visão geral** no portal. No canto direito será possível ver uma seção **Detalhes** com as informações de `CName`.

## Há um limite no número de solicitações de Limpeza que podem ficar ativas ao mesmo tempo?
Sim. Pode haver apenas 5 solicitações de limpeza ativas de cada vez.

## Minha solicitação de Limpeza para um determinado caminho de arquivo está em Andamento. Posso enviar uma nova solicitação para o mesmo caminho de arquivo?
Não. Pode haver apenas uma solicitação de Limpeza ativa para um determinado caminho de arquivo de cada vez.

## O Protocolo da Internet versão 6 (IPv6) é suportado com o serviço do IBM Cloud Content Delivery Network? Como isto funciona?
IPv6 (ou suporte de pilha duplo) é suportado por servidores Edge de Akamai. É projetado para ajudar os clientes com IPv4 apenas origem a aceitar conexões de clientes IPv6, converter de IPv6 para IPv4 no Edge e seguir adiante com a origem com IPv4.

**NOTA:** a criação de um CDN do IBM Cloud usando um endereço IPv6 como o endereço do servidor de origem
não é suportado.

## Quais são as regras para o nome do host?
A sequência de entrada do `Hostname` **deve**:
  * consistir em caracteres alfanuméricos
  * ter menos que 254 caracteres
  * terminar com um nome de domínio de nível superior válido
  * **não** deve terminar em 'cdnedge.bluemix.net' (esse término é usado para CNAMES e é reservado)

Consulte o RFC 1035, seção 2.3.4 para obter mais detalhes.

## Quais são as convenções de nomenclatura customizadas do CNAME?
A sequência de entrada `CNAME` deve seguir as seguintes regras:
  * **deve** ser exclusivo (não pode estar em uso por nenhum outro CDN do IBM Cloud)
  * menos de 64 caracteres alfanuméricos
  * `caracteres especiais. @ # $ % ^ & *` **não** são permitidos
  * sublinhados `_` **não** são permitidos
  * pontos `.` **não** são permitidos
  * traços `-` são permitidos, mas o CNAME não pode começar ou terminar com um traço

## Quais são as regras para nomes de depósito?
Nomes de depósito:
  * devem ter pelo menos 1 caractere
  * não podem ter mais de 199 caracteres de comprimento
  * podem conter letras (duas letras maiúsculas e minúsculas são permitidas), números e hifens
  * **não** devem ser formatados como um endereço IP (por exemplo, 127.0.0.1)
  * **não podem** iniciar com um ponto (.)
  * podem terminar com um ponto (.)
  * apenas um ponto (.) é permitido entre os rótulos (por exemplo, my..ibmcloud.bucket não é permitido). 

**NOTA**: embora letras maiúsculas e pontos possam passar na validação, sugerimos que você sempre use nomes de depósito compatíveis com o DNS.

## Quais são as regras para a sequência de caminho para a origem?
O caminho é opcional ao criar seu CDN. No entanto, se fornecido, o caminho **deve**:
  * ter menos que 1000 caracteres de comprimento
  * começar com '/'

## Para o comando **Add Origin**, existem regras a seguir para a sequência de Caminho?
Para **Add Origin**, o caminho é obrigatório. Além disso, se o CDN for criado com um caminho, o caminho para **Incluir origem** deverá iniciar com o caminho do CDN como o prefixo. Por exemplo, se o caminho CDN foi especificado como `/storage`, para chamar **Add Origin** com um caminho chamado `/examplePath`, o caminho fornecido seria `/storage/examplePath`'.

## Em qual formato devo fornecer as extensões de arquivo para criar um tipo de origem de armazenamento de objetos com esse
serviço do CDN?

As extensões de arquivo devem ser separadas por vírgula. Por exemplo, a lista "txt, jpg, xml" é uma lista válida. Qualquer extensão particular não pode ser maior que 10 caracteres.

## Existem restrições sobre quais números de porta HTTP e HTTPS são permitidos para Akamai?
Sim. Para o fornecedor do Akamai, somente os números de porta a seguir são permitidos: 72, 80-89, 443, 488, 591, 777, 1080, 1088, 1111,
1443, 2080, 7001, 7070, 7612, 7777, 8000-9001, 9090, 9901-9908, 11080-11110, 12900-12949, 20410 e 45002.

## Qual URL deve ser usada para acesso aos dados no CDN ou Caminho de Origem? 
O caminho para um mapeamento CDN ou para a origem é tratado como um diretório. Portanto, os usuários que estão tentando acessar o caminho de origem devem acessá-lo como um diretório (com uma barra). Por exemplo, se CDN `www.example.com` for criado usando o caminho que inclui o diretório `/images`, a URL para atingi-lo deverá ser `www.example.com/images/`

Omitir a barra, por exemplo, usando `www.example.com/images` resultará em um erro **Página Não Localizada**.

## Para HTTPS, por que não posso conectar por meio de um comando curl ou navegador usando o Nome do Host?

Atualmente, o HTTPS é suportado apenas por meio de um certificado curinga. Como resultado dessa limitação, a conexão deve ser feita usando CNAME; tentar se conectar usando o Nome do Host resultará em falha.

## Qual deve ser o comportamento esperado ao carregar o CNAME ou o nome do host no seu navegador para os protocolos do CDN
suportados?

|URL do navegador| CDN com protocolo HTTP apenas| CDN com protocolo HTTPS apenas | CDN com ambos os protocolos, HTTP e HTTPS |
|-------|-----|-----|-----|
|http://hostname| Carregamento bem-sucedido | 301 Movido permanentemente | 301 Movido permanentemente |
|https://hostname | Acesso negado | Redireciona a página da web do IBM Cloud | Redireciona a página da web do IBM Cloud | 
|http://cname| 301 Movido permanentemente | Acesso negado | Carregamento bem-sucedido | 
|https://cname| Redireciona a página da web do IBM Cloud | Carregamento bem-sucedido | Carregamento bem-sucedido |

## Como configurar o Content Delivery Network para o IBM Cloud Object Storage (COS)?
[Aqui está um tutorial](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) sobre a criação de uma Rede de entrega de conteúdo para o IBM Cloud Object Storage.

## Por que meu nome do host não carrega no navegador quando escolho IBM Cloud Object Storage (COS) como a origem?

Quando o IBM Cloud CDN estiver configurado para usar o IBM COS como o armazenamento de objetos, o acesso direto ao website não funcionará. Deve-se especificar o caminho completo da solicitação na barra de endereço do navegador (por exemplo, `www.example.com/index.html`). Esse comportamento é causado pela limitação do documento de índice no IBM COS.

## Qual é o maior tamanho do arquivo que pode ser entregue por meio do CDN do Akamai?

As tentativas de recuperar ou entregar arquivos maiores que 1,8 GB receberão uma resposta `403 Access Forbidden` para a configuração de desempenho padrão. Se a otimização de arquivos grandes estiver ativada, downloads de arquivos até 320 GB serão possíveis.

## Recebi notificação de que meu certificado de origem está expirando. O que faço agora?

Siga as etapas descritas [nesse artigo](https://community.akamai.com/docs/DOC-7708) do Akamai.

## O que é uma solicitação de intervalo de bytes e como ela funciona com o CDN do Akamai?

Uma solicitação de intervalo de bytes é usada para recuperar conteúdo parcial de um servidor de origem. O cabeçalho da solicitação de HTTP de intervalo indica qual parte do conteúdo o servidor deve retornar. Várias partes podem ser solicitadas com um cabeçalho de intervalo de uma vez e o servidor pode enviar de volta esses intervalos em uma resposta com múltiplas partes. Se o servidor enviar os intervalos de volta, ele responderá com um status de 206 (conteúdo parcial).

Quando uma solicitação de intervalo de bytes é enviada usando o CDN do IBM Cloud com o Akamai, o usuário pode receber um código de resposta de 200 (OK) para a primeira solicitação e um código de resposta de 206 para todas as solicitações subsequentes. Isso porque o conteúdo da solicitação dos servidores de borda do Akamai da origem está no formato compactado. Então, quando um servidor de borda não tiver um objeto em seu cache, nem qualquer informação sobre o comprimento do conteúdo do objeto, ele será encaminhado para a origem e solicitará o objeto inteiro. Nesse caso, a origem entrega o objeto sem o cabeçalho de comprimento de conteúdo para o Akamai e ao usuário final será entregue o objeto inteiro, embora fosse uma solicitação de intervalo de bytes. Portanto, o código de status 200. Em pedidos subsequentes, o servidor de borda tem o objeto em seu cache e entregará o código de status 206.

Uma maneira de assegurar uma resposta 206, mesmo para a primeira solicitação de intervalo de bytes, é desativar
`Transfer-Encoding: chunked` em seu servidor de origem.

## Qual segurança é incluída com a solução CDN da IBM com Akamai?

Usando a plataforma Akamai distribuída, você obtém escala e resiliência sem precedentes com mais de 240 mil servidores em mais de 130 países. A Plataforma Akamai está entre sua infraestrutura e seus usuários finais e age como o primeiro nível de defesa para picos repentinos no tráfego. Akamai Intelligent Platform também é um proxy reverso que atende e responde a solicitações nas portas 80 e 443 apenas, o que significa que o tráfego em outras portas é descartado na borda sem ser encaminhado para a sua infraestrutura.
