---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: features, descriptions, metrics, multiple, origins, mapping, time to live, purge, cached, content, key, stale, https, geographical, access, protocol, compression, video, hotlink

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Descrições do recurso
{: #feature-descriptions}

Esta página destaca muitos dos recursos incluídos com o {{site.data.keyword.cloud}} CDN desenvolvido com a Akamai.

## Suporte de origem do servidor host
{: #host-server-origin-support}

O IBM Cloud Content Delivery Network (CDN) pode ser configurado para entregar conteúdo de uma origem de servidor de host fornecendo
o nome do host de origem, protocolo, número da porta e, opcionalmente, o caminho do qual entregar o conteúdo. O caminho padrão é  ` / * `. O protocolo pode ser HTTP, HTTPS ou ambos. Somente determinados números de porta são suportados pelo Akamai. Consulte as [Perguntas frequentes](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números/intervalos de porta suportados.

## Suporte de origem do Object Storage
{: #object-storage-file-support}

O IBM Cloud CDN pode ser configurado para entregar conteúdo por meio de um terminal do Object Storage
fornecendo o terminal, o nome do Depósito, o protocolo e a porta. Opcionalmente, uma
lista de extensões de arquivo pode ser especificada para permitir apenas armazenamento em cache para arquivos com essas extensões. Todos os objetos no depósito precisam ser configurados com acesso de leitura anônimo ou público.

Este tutorial sobre
[Como
configurar o IBM Cloud Object Storage com o CDN](/docs/tutorials?topic=solution-tutorials-static-files-cdn#static-files-cdn) fornece informações mais detalhadas.

## Suporte para múltiplas origens com caminhos distintos
{: #support-for-multiple-origins-with-distinct-paths}

Em alguns casos, você pode querer entregar determinado conteúdo de um servidor de origem diferente. Por exemplo, você pode
querer que determinadas fotos ou vídeos sejam entregues de diferentes servidores de origem. O IBM Cloud CDN fornece a opção para configurar
múltiplos servidores de origem com múltiplos caminhos. Isso permite flexibilidade com relação a como e onde os dados são
armazenados. O caminho fornecido para o servidor de origem deve ser exclusivo com relação ao CDN. O servidor de origem em si não
precisa ser exclusivo.

## Mapeamentos do CDN baseados em caminho
{: #path-based-cdn-mappings}

O serviço do IBM Cloud CDN pode ser restrito a um caminho do diretório específico no servidor de origem fornecendo o caminho ao
criar o CDN. Um usuário final pode acessar apenas aqueles conteúdos no caminho do diretório. Por exemplo, se um CDN `www.example.com` for criado com o Caminho `/videos`, ele estará acessível **somente** por meio de `www.example.com/videos/*`.

## Limpe o conteúdo em cache
{: #purge-cached-content}

O IBM Cloud CDN fornece a capacidade de remover de forma rápida e conveniente, ou "limpar", o conteúdo em cache dos servidores de borda. Nesse momento, a limpeza é permitida apenas para um caminho de arquivo. Atualmente, a implementação de limpeza com o
Akamai limpa o arquivo em cerca de cinco segundos. A UI também fornece a capacidade de visualizar seu histórico de limpeza e marcar caminhos de limpeza específicos como Favoritos.

## TTL (tempo de vida)
{: #time-to-live-ttl}

O tempo de vida indica a quantia de tempo (em segundos) que o servidor Edge manterá o conteúdo armazenado em cache para esse arquivo ou caminho de diretório específico. Quando um CDN é criado pela primeira vez, um Tempo de Vida (TTL) global é criado para o caminho
`/\*` com um tempo padrão de 3.600 segundos. O valor mínimo para TTL é 0 segundos e o valor máximo é 2147483647 segundos. Para cada entrada, o caminho de TTL deve ser exclusivo para o CDN. Se múltiplos caminhos corresponderem a um determinado conteúdo, a correspondência de caminho configurada mais recentemente se aplicará a esse conteúdo. Por exemplo, considere dois TTLs, `/example/file` criados primeiro com um valor de tempo de vida de 3000 segundos e `/example/*` criado posteriormente, com um valor de 4000 segundos. Embora `/example/file` seja
mais específico, `/example/*` foi criado mais recentemente, de modo que o TTL para `/example/file`
será de 4.000 segundos. Depois de criadas, as entradas de TTL poderão ser editadas para o caminho e/ou horário. Eles podem ser
excluídos também.

## Métricas com visualizações gráficas
{: #metrics-with-graphical-views}

Métricas para um CDN individual são fornecidas na guia Visão geral do portal do cliente para esse mapeamento do CDN. Dois tipos de métricas são calculados do uso do CDN: aquelas que mostram as métricas ao longo de um período de tempo como um gráfico e aquelas que são mostradas como valores agregados.

Para métricas que exibem a mudança ao longo de um período de tempo como um gráfico, é possível ver três gráficos de linhas e um gráfico de pizza. Os três gráficos de linhas são: **Largura da banda**, **Ocorrências por mapeamento**e **Ocorrências por tipo**. Eles exibem a atividade em uma base diária ao longo do curso de seu prazo especificado. Os gráficos para **Largura da banda** e **Ocorrências por mapeamento** são gráficos de linha única, enquanto que o detalhamento de **Ocorrências por tipo** mostra uma linha para cada um dos tipos de ocorrência fornecido. O gráfico de pizza exibe um detalhamento regional da largura da banda para um mapeamento de CDN em uma base porcentagem.

As métricas mostradas para valores agregados incluem **Uso de largura da banda** em GB, **Total de ocorrências** para o servidor de borda do CDN e a **Taxa de acertos**. A Taxa de acertos indica a porcentagem de vezes que o conteúdo é entregue pelo servidor
Edge, _não_ por meio de sua Origem. A taxa de acertos atualmente é mostrada como uma função de todos os mapeamentos de CDN, não apenas aquele que está sendo visualizado.

Por padrão, ambos os números agregados e os gráficos padronizam a exibição de métricas para os últimos 30 dias, mas você pode mudar isso por meio do [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/). As categorias são capazes de exibir métricas para períodos de 7, 15, 30, 60 ou 90 dias.

## Suporte do cabeçalho do host
{: #host-header-support}

O servidor Edge usa o **Cabeçalho do host** ao comunicar-se com o host de Origem. Este recurso fornece flexibilidade em como o
serviço da web é configurado no host de origem. Especificamente, ele ativa um caso de uso em que um cliente tenha vários servidores da web configurados no mesmo Host de origem. Se a entrada do cabeçalho do host não for fornecida, o serviço usará o nome do host
do servidor de origem como Cabeçalho do host HTTP padrão se o servidor de origem for especificado como nome do host (em vez de como um
endereço IP). Se o cabeçalho do host não for fornecido como entrada E o servidor de origem for fornecido como um endereço IP, o nome
do host CDN (também chamado de nome de domínio CDN) será usado como o cabeçalho do host HTTP padrão.

## Suporte ao Protocolo HTTPS
{: #https-protocol-support}

O CDN pode ser configurado para usar o protocolo HTTPS para entregar o conteúdo com segurança aos
usuários finais. Essa configuração requer que um certificado SSL seja definido como parte da
configuração do CDN. Dois tipos de opções de certificado SSL estão disponíveis para HTTPS:
[Certificado curinga](/docs/infrastructure/CDN?topic=CDN-about-https#wildcard-certificate-support) e
[Certificado Subject Alternative
Name (SAN) de Domínio Validado (DV)](/docs/infrastructure/CDN?topic=CDN-about-https#san-certificate-suport). Esse tipo será referido também como um certificado SAN nesta documentação.

O tipo de Certificado SSL a ser usado é uma consideração importante para o CDN HTTPS. A definição da
configuração do certificado curinga é rápida, mas com a desvantagem de que o CDN é acessível apenas por
meio de um CNAME. O processo de certificado SAN leva de 4 a 8 horas para ser concluído, mas permite
usar o CDN com o Domínio do CDN (ou seja, o Nome do host). O Certificado SAN também requer uma etapa adicional
de [**Domain Control Validation** ](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san) durante
a configuração. Nenhum custo está associado ao uso de qualquer um desses certificados. Consulte o
[Documento
de resolução de problemas](/docs/infrastructure/CDN?topic=CDN-troubleshooting#what-is-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-protocols-) para entender a implicação da seleção de um determinado tipo de
Certificado.

O Host de origem também deve ter seu próprio certificado SSL para o nome do host do CDN e deve ser
assinado por uma Autoridade de Certificação (CA) reconhecida.

Como uma melhor prática do segmento de mercado, o Akamai somente confia em certificados raiz e não em certificados intermediários porque o conjunto de certificados intermediários que é confiável muda frequentemente. É possível localizar os certificados confiáveis do Akamai
[na
web neste local](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

## Respeitar cabeçalhos
{: #respect-headers}

A opção **Respeitar cabeçalhos** permite que a configuração do Cabeçalho de HTTP
na Origem substitua a configuração do CDN. Esse
recurso é LIGADO por padrão, mas pode ser DESLIGADO.

## Entregue conteúdo antigo
{: #serve-stale-content}

Quando o servidor de borda do CDN recebe uma solicitação do usuário e o conteúdo solicitado não está armazenado em cache, o
servidor de borda alcança o host de origem para buscar o conteúdo. Esse conteúdo é, então, armazenado em cache para a duração do
tempo de vida (TTL) especificada para o conteúdo. Se uma solicitação do usuário for recebida após a expiração do TTL, o
servidor de borda alcançará o host de origem para buscar o conteúdo. Se o servidor de origem não puder ser alcançado por
alguma razão (por exemplo, o host de origem está inativo ou há um problema de rede), o servidor de borda entregará o conteúdo expirado
(antigo) para a solicitação. Esse recurso é suportado pelo Akamai e **não pode** ser desligado.

## Otimização da chave de cache
{: #cache-key-optimization}

Os servidores Edge do Akamai armazenam em cache o conteúdo em um **Armazenamento de
cache**. Para usar o conteúdo do **Armazenamento em cache**, os
servidores Edge usam uma **Chave de cache**. Geralmente, uma **Chave de cache**
é gerada com base em uma parte da URL de um usuário final. Em alguns casos, a URL contém argumentos da função de consulta que são diferentes para
usuários individuais, mas o conteúdo entregue é o mesmo. Por padrão, o Akamai utiliza os argumentos da função de consulta para gerar a chave de cache e, portanto, para gerar uma chave de cache exclusiva para cada usuário. Este método não é ideal porque ele faz com
que o servidor de borda entre em contato com o servidor de origem para conteúdo que já está em cache, mas usando uma chave de cache diferente. O recurso **Otimização de chave de cache** permite especificar quais argumentos de consulta incluir ou ignorar ao gerar uma chave de cache. Esse recurso se aplica a qualquer `create` ou
`update` de uma configuração de mapeamento de CDN, bem como para qualquer
`create` ou `update` de um caminho de origem.

O valor de **Otimização de chave de cache** pode ser configurado na guia **Configurações** após a criação de um mapeamento de CDN. Para o caminho de Origem, esse valor pode ser configurado durante as operações `create`
ou `update` de um caminho de Origem.
{: note}

## Compactação de conteúdo
{: #content-compression}

A **Compactação de conteúdo** é ativada no CDN do Akamai por padrão para os tipos
de conteúdo a seguir:
* text/html*
* text/css*
* text/xml*
* text/json
* text/javascript*
* text/plain*
* application/x-javascript*
* application/json
* application/xml*

Quando a compactação é manipulada pelo servidor de borda, o conteúdo deve ser de pelo menos 10 kB.  Em alguns casos, a
compactação é tratada pelo servidor de origem e, nesses casos, não há limite no tamanho dos arquivos a serem compactados. Se o
conteúdo já estiver sendo compactado pelo servidor de origem, ele não será compactado novamente. Para ativar a compactação de
conteúdo, o cabeçalho da solicitação deve definir `Accept-Encoding: gzip`.

## Otimização de arquivo grande
{: #large-file-optimization}

A otimização de arquivos grandes permite que a rede CDN otimize a entrega de conteúdo maior que 10 MB. Essa ativação aumenta o
desempenho para arquivos grandes e reduz os tempos de latência e download. Sem esse recurso, o CDN não pode atender arquivos com mais de 1,8 GB em tamanho. Esse recurso permite downloads de arquivos maiores que 1,8 GB, até um máximo de 320 GB.

Para que a otimização de arquivo grande funcione, as solicitações de intervalo de bytes **devem** ser ativadas no servidor de origem. O CDN do Akamai emprega uma técnica chamada de _Armazenamento em cache de objeto parcial_ para essa otimização. Quando um arquivo grande é solicitado, o servidor de borda verifica se o arquivo atende aos requisitos de tamanho. Isso significa que os arquivos maiores que 10 MB serão solicitados do servidor de origem em chunks. Quando o chunk chegar ao servidor de borda, ele será armazenado em cache e imediatamente entregue ao usuário. O próximo chunk é buscado previamente em paralelo pelo servidor de borda, reduzindo a latência. Esse processo continua até que o arquivo inteiro seja recuperado ou a conexão seja finalizada.

Quando esse recurso é ativado, há um pequeno custo de desempenho associado ao conteúdo de entrega menor que 10 MB. Portanto, esse recurso é recomendado apenas para entregar arquivos grandes. Um caso de uso típico seria criar um novo caminho de
origem na configuração do CDN e ativar a otimização de arquivo grande para esse caminho.

## Vídeo on demand
{: #video-on-demand}

**Vídeo on Demand** otimização de desempenho fornece fluxo de alta qualidade em uma variedade de
tipos de rede. Ao utilizar as configurações de controle de cache pré-configuradas e a capacidade da rede distribuída para distribuir a carga dinamicamente, o IBM Cloud CDN com o Akamai fornece a capacidade de escalar rapidamente para grandes públicos, quer você tenha planejado isso ou não.

**Vídeo On Demand** é otimizado para distribuição de formatos de fluxo segmentado como HLS, DASH, HDS e HSS. A transmissão de vídeo em tempo real **não** é suportada neste momento. É possível ativar o recurso **Vídeo on Demand** selecionando a opção do menu suspenso em **Otimizar para** na guia Configurações ou ao criar um novo caminho de origem. É necessário ativar esse recurso apenas ao otimizar a entrega de arquivos de vídeo.

## Controle de acesso geográfico
{: #geographical-access-control}

O Controle de acesso geográfico é um comportamento baseado em regra que permite configurar o parâmetro `access-type` para um grupo de usuários, com base em sua localização geográfica. Dois tipos de comportamentos estão disponíveis: **Allow** e **Deny**.

O tipo de acesso `Allow` deixa permitir especificamente o tráfego para regiões selecionadas, com base no tipo de região. Permitir tráfego para regiões específicas bloqueia implicitamente o tráfego para todas as outras. Por exemplo, você pode optar por usar `Allow` para permitir tráfego para continentes selecionados, como Europa e Oceania, que bloqueia o acesso a todos os outros continentes.

Por outro lado, o comportamento `Deny` bloqueia o acesso a seu serviço para o grupo especificado, mas permite acesso a todas as outras regiões não especificadas. Por exemplo, se você configurar o tipo de acesso Controle de acesso geográfico como `Deny` para os continentes da Europa e Oceania, os usuários nesses continentes **não** poderão usar seu serviço, enquanto os usuários em todos os outros continentes terão acesso a ele.

Esse recurso é acessível na página **Configurações** de sua Configuração do CDN.

## Hotlink Protection
{: #hotlink-protection}

Hotlink Protection é um comportamento baseado em regras que permite controlar se determinados websites têm permissão ou não para acessar seu conteúdo por meio do CDN. O navegador geralmente inclui um cabeçalho `Referer` quando uma solicitação de HTTP é feita por meio de um link em uma página da web e quando esse link aponta para um ativo remoto. O link que um website usa para acesso a um ativo de outro website é chamado de _hotlink_.  Dois tipos de comportamentos estão disponíveis: **ALLOW** e **DENY**.

Se o `protectionType` estiver configurado como `ALLOW`:
* Se o valor do cabeçalho `Referer` em uma solicitação enviada para seu CDN corresponder a um de seus `refererValues`especificados, seu CDN **entregará** o conteúdo solicitado.
* Caso contrário, seu CDN **não** entregará o conteúdo.

Se o `protectionType` estiver configurado como `DENY`:
* Se o valor do cabeçalho `Referer` em uma solicitação enviada para seu CDN corresponder a um de seus `refererValues`especificados, seu CDN **não** entregará o conteúdo solicitado.
* Caso contrário, seu CDN entregará o conteúdo.
