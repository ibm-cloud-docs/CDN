---

copyright:
  years: 2017, 2018
lastupdated: "2018-03-23"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Sobre o Content Delivery Networks (CDN)

Um Content Delivery Network (CDN) é uma coleção de servidores de ponta distribuídos por várias partes do país ou do mundo. 
O conteúdo da web é entregue de um servidor de borda, que está localizado na área geográfica mais próxima do cliente
que solicita o conteúdo. Essa técnica permite que seus usuários finais recebam o conteúdo com menos atraso, o que fornece uma
experiência geral melhor para seus clientes.

## Como um CDN funciona?

Um CDN atinge o seu propósito armazenando em cache o conteúdo da web em servidores de ponta ao redor do mundo. Quando um
cliente solicita o conteúdo da web, a solicitação de conteúdo é roteada para o servidor de borda que estiver geograficamente mais
próximo a esse cliente. Reduzindo a distância que o conteúdo deve viajar, o CDN oferece rendimento otimizado, latência minimizada e aumento de desempenho. 
Usando o IBM Cloud Content Delivery Network com o Akamai, os provedores de conteúdo podem realizar a entrega eficiente do conteúdo
solicitado de todo o mundo, com configuração mínima.

Os recursos-chave do serviço IBM Cloud Content Delivery Network incluem:
  * Suporte de origem do servidor host
  * Suporte de origem do Object Storage
  * Suporte para múltiplas origens com caminhos distintos
  * Mapeamentos do CDN baseados em caminho
  * Limpe o conteúdo em cache
  * TTL (tempo de vida)
  * Métricas com visualizações gráficas
  * Suporte HTTPS com certificado curinga
  * Suporte do cabeçalho do host
  * Entregue conteúdo antigo
  * Recurso "Ignorar Consultar argumentos na chave de cache"
  * Compactação de conteúdo
  * Otimização de arquivo grande
  * Vídeo on demand

## Descrições do recurso

### Suporte de origem do servidor host

O IBM Cloud Content Delivery Network (CDN) pode ser configurado para entregar conteúdo de uma origem de servidor de host fornecendo
o nome do host de origem, protocolo, número da porta e, opcionalmente, o caminho do qual entregar o conteúdo. O caminho padrão é '/\*'. 
O protocolo pode ser HTTP, HTTPS ou ambos. Somente determinados números de porta são suportados pelo Akamai. Consulte as [Perguntas frequentes](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-) para obter os números/intervalos de porta suportados. Atualmente, HTTPS é suportado usando o certificado curinga,
o que significa que o serviço será acessível apenas por meio do CNAME terminando com `.cdnedge.bluemix.net`. 
Consulte a descrição do recurso [HTTPS com certificado curinga](about.html#https-with-wildcard-certificate-) para obter mais informações.

### Suporte de origem do Object Storage

O IBM Cloud CDN pode ser configurado para atender conteúdo de um terminal de armazenamento de objetos fornecendo o
_terminal_, o _nome do depósito_, o _protocolo_ e a _porta_. Opcionalmente, uma
lista de extensões de arquivo pode ser especificada para permitir apenas armazenamento em cache para arquivos com essas extensões. 
Todos os objetos no depósito precisam ser configurados com acesso de leitura anônimo ou público. Este tutorial sobre
[Como
configurar o IBM Bluemix Object Storage com o CDN](https://console.bluemix.net/docs/tutorials/static-files-cdn.html#accelerate-delivery-of-static-files-using-a-cdn) fornece mais informações detalhadas.

### Suporte para múltiplas origens com caminhos distintos

Em alguns casos, você pode querer entregar determinado conteúdo de um servidor de origem diferente. Por exemplo, você pode
querer que determinadas fotos ou vídeos sejam entregues de diferentes servidores de origem. O IBM Cloud CDN fornece a opção para configurar
múltiplos servidores de origem com múltiplos caminhos. Isso permite flexibilidade com relação a como e onde os dados são
armazenados. O caminho fornecido para o servidor de origem deve ser exclusivo com relação ao CDN. O servidor de origem em si não
precisa ser exclusivo.

### Mapeamentos do CDN baseados em caminho

O serviço do IBM Cloud CDN pode ser restrito a um caminho do diretório específico no servidor de origem fornecendo o caminho ao
criar o CDN. Um usuário final pode acessar apenas aqueles conteúdos no caminho do diretório. Por exemplo, se um www.example.com do CDN
for criado com o caminho '/videos', ele será acessível apenas por meio do `www.example.com/videos/`.

### Limpe o conteúdo em cache

O IBM Cloud CDN fornece a capacidade de remover de forma rápida e conveniente, ou "limpar", o conteúdo em cache dos servidores de borda. Nesse momento, a limpeza é permitida apenas para um caminho de arquivo. Atualmente, a implementação de limpeza com o
Akamai limpa o arquivo em cerca de cinco segundos. A UI também fornece a capacidade de visualizar seu histórico de limpeza e marcar caminhos de limpeza específicos como Favoritos.

### TTL (tempo de vida)

O tempo de vida indica a quantidade de tempo (em segundos) que o servidor de borda levará para armazenar em cache o conteúdo para esse caminho específico de arquivo ou diretório. Quando um CDN é criado pela primeira vez, um tempo de vida global (TTL) é criado para o caminho ('/ \*') com um tempo padrão de 3600 segundos. O valor mínimo para o TTL é 30 segundos e o valor máximo é 2147483647 segundos. Para cada entrada, o caminho de TTL deve ser exclusivo para o CDN. Se múltiplos caminhos corresponderem a um determinado conteúdo, a correspondência de caminho configurada mais recentemente se aplicará a esse conteúdo. Por exemplo, considere dois TTLs, `/example/file` criados primeiro com um valor de tempo de vida de 3000 segundos e `/example/*` criado posteriormente, com um valor de 4000 segundos. Embora `/example/file` seja
mais específico, `/example/*` foi criado mais recentemente, de modo que o TTL para `/example/file`
será de 4.000 segundos. Depois de criadas, as entradas de TTL poderão ser editadas para o caminho e/ou horário. Eles podem ser
excluídos também.

### Métricas com visualizações gráficas

Métricas para um CDN individual são fornecidas na guia Visão geral do portal do cliente para esse mapeamento do CDN. Dois tipos de métricas são calculados do uso do CDN: aquelas que mostram as métricas ao longo de um período de tempo como um gráfico e aquelas que são mostradas como valores agregados.

Para métricas que exibem a mudança ao longo de um período de tempo como um gráfico, é possível ver três gráficos de linhas e um gráfico de pizza. Os três gráficos de linhas são: **Largura da banda**, **Ocorrências por mapeamento**e **Ocorrências por tipo**. Eles exibem a atividade em uma base diária ao longo do curso de seu prazo especificado. Os gráficos para **Largura da banda** e **Ocorrências por mapeamento** são gráficos de linha única, enquanto que o detalhamento de **Ocorrências por tipo** mostra uma linha para cada um dos tipos de ocorrência fornecido. O gráfico de pizza exibe um detalhamento regional da largura da banda para um mapeamento de CDN em uma base porcentagem.

As métricas mostradas para valores agregados incluem **Uso de largura da banda** em GB, **Total de ocorrências** para o servidor de borda do CDN e a **Taxa de acertos**. A taxa de acertos indica a porcentagem de vezes que o conteúdo é entregue pelo servidor de borda, não por meio de sua origem. A taxa de acertos atualmente é mostrada como uma função de todos os mapeamentos de CDN, não apenas aquele que está sendo visualizado.

Por padrão, ambos os números agregados e os gráficos padronizam a exibição de métricas para os últimos 30 dias, mas você pode mudar isso por meio do [Portal do cliente ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/). As categorias são capazes de exibir métricas para períodos de 7, 15, 30, 60 ou 90 dias.

### Suporte HTTPS com certificado curinga

O certificado curinga é a maneira mais econômica para entregar conteúdo da web para seus usuários finais com segurança. Esse recurso é ativado selecionando `Porta HTTPS` ao configurar seu CDN ou caminho de origem. Depois que o mapeamento do CDN é ativado para HTTPS usando o certificado curinga, o servidor de borda entra em contato com o servidor de origem por meio de HTTPS. Se o servidor de origem for especificado como um nome de host, o servidor de borda usará o nome do host de origem como o cabeçalho Server Name Indication (SNI) para a negociação da Segurança da Camada de Transporte (TLS) com o servidor de origem por padrão. No entanto, se o servidor de origem for especificado como um endereço IP, o nome do host do CDN será usado como o
cabeçalho SNI. O certificado de origem deve ser assinado por uma Autoridade de Certificação (CA) reconhecida.

Ao usar o certificado curinga, um usuário final **somente** tem acesso ao serviço usando o CNAME. Por exemplo, se o domínio for `example.com` e o CNAME for `example.cdnedge.bluemix.net`, seu CDN de serviço será acessível **somente** por meio de `https://example.cdnedge.bluemix.net`. Consulte as [Perguntas frequentes](faqs.html#what-should-be-the-expected-behavior-when-loading-the-cname-or-hostname-on-your-browser-for-the-supported-cdn-protocols-) para entender as implicações de usar HTTPS com certificado curinga.

Como uma melhor prática do segmento de mercado, o Akamai somente confia em certificados raiz e não em certificados intermediários porque o conjunto de certificados intermediários que é confiável muda frequentemente. Para propósitos de segurança, o Akamai inclui apenas as autoridades de certificação raiz no armazenamento confiável do Akamai. Portanto, a origem deve enviar a cadeia de certificados inteira, incluindo o certificado leaf, e todos os certificados intermediários (não incluindo o certificado raiz) como parte do handshake SSL com o servidor de borda. É possível localizar os certificados confiáveis do
Akamai[aqui](https://community.akamai.com/docs/DOC-4447-ssltls-certificate-chains-for-akamai-managed-certificates).

### Suporte do cabeçalho do host

O servidor de borda usa o cabeçalho do host ao se comunicar com o host de origem. Este recurso fornece flexibilidade em como o
serviço da web é configurado no host de origem. Se a entrada do cabeçalho do host não for fornecida, o serviço usará o nome do host
do servidor de origem como Cabeçalho do host HTTP padrão se o servidor de origem for especificado como nome do host (em vez de como um
endereço IP). Se o cabeçalho do host não for fornecido como entrada E o servidor de origem for fornecido como um endereço IP, o nome
do host CDN (também chamado de nome de domínio CDN) será usado como o cabeçalho do host HTTP padrão.

### Respeitar cabeçalhos

A opção Respeitar cabeçalhos permite que a configuração do cabeçalho de HTTP na origem sobrescreva a configuração do CDN. Esse
recurso é LIGADO por padrão, mas pode ser DESLIGADO.

### Entregue conteúdo antigo

Quando o servidor de borda do CDN recebe uma solicitação do usuário e o conteúdo solicitado não está armazenado em cache, o
servidor de borda alcança o host de origem para buscar o conteúdo. Esse conteúdo é, então, armazenado em cache para a duração do
tempo de vida (TTL) especificada para o conteúdo. Se uma solicitação do usuário for recebida após a expiração do TTL, o
servidor de borda alcançará o host de origem para buscar o conteúdo. Se o servidor de origem não puder ser alcançado por
alguma razão (por exemplo, o host de origem está inativo ou há um problema de rede), o servidor de borda entregará o conteúdo expirado
(antigo) para a solicitação. Esse recurso é suportado pelo Akamai e **não pode** ser desligado.

### Recurso "Ignorar Consultar argumentos na chave de cache"

Os servidores de borda do Akamai armazenam conteúdo em cache em um assim chamado "Armazenamento em cache". Para usar o
conteúdo do "armazenamento em cache", os servidores de borda usam uma "chave de cache". Geralmente, uma chave de cache é gerada com
base em uma parte da URL de um usuário final. Em alguns casos, a URL contém argumentos da função de consulta que são diferentes para
usuários individuais, mas o conteúdo entregue é o mesmo. Por padrão, o Akamai utiliza os argumentos da função de consulta para gerar a chave de cache e, portanto, para gerar uma chave de cache exclusiva para cada usuário. Este método não é ideal porque ele faz com
que o servidor de borda entre em contato com o servidor de origem para conteúdo que já está em cache, mas usando uma chave de cache diferente. 
O recurso **Ignorar argumentos de consulta na chave do cache** permite especificar se os argumentos da consulta
devem ser ignorados ao gerar uma chave de cache. Esse recurso se aplica a qualquer `create` ou
`update` de uma configuração de mapeamento de CDN, bem como para qualquer
`create` ou `update` de um caminho de origem.

**Nota:** os argumentos de consulta de chave de cache podem ser configurados na guia
Configurações após a criação de um mapeamento de CDN. Para o caminho de origem, eles podem ser configurados durante o
`create` ou `update` de um caminho de origem.

### Compactação de conteúdo

A compactação de conteúdo é ativada no Akamai CDN por padrão para os tipos de conteúdo a seguir:
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

### Otimização de arquivo grande

A otimização de arquivos grandes permite que a rede CDN otimize a entrega de conteúdo maior que 10 MB. Essa ativação aumenta o
desempenho para arquivos grandes e reduz os tempos de latência e download. Sem esse recurso, o CDN não pode atender arquivos com mais de 1,8 GB em tamanho. Esse recurso permite downloads de arquivo maiores que 1,8 GB até um máximo de 320 GB.

Para que a otimização de arquivos grandes funcione, solicitações de intervalo de bytes **devem** ser ativadas no servidor de origem. O Akamai CDN emprega uma técnica chamada de armazenamento em cache de objeto parcial para essa otimização. Quando um arquivo grande é solicitado, o servidor de borda verifica se o arquivo atende aos requisitos de tamanho. Isso significa que arquivos maiores que 10 MB serão solicitados do servidor de origem em chunks. Quando o chunk chegar ao servidor de borda, ele será armazenado em cache e imediatamente entregue ao usuário. O próximo chunk é buscado previamente em paralelo pelo servidor de borda, reduzindo a latência. Esse processo continua até que o arquivo inteiro seja recuperado ou a conexão seja finalizada.  

Quando esse recurso é ativado, há um pequeno custo de desempenho associado ao conteúdo de entrega menor que 10 MB. 
Portanto, esse recurso é recomendado apenas para entregar arquivos grandes. Um caso de uso típico seria criar um novo caminho de
origem na configuração do CDN e ativar a otimização de arquivo grande para esse caminho.

### Vídeo on demand

**Vídeo on Demand** otimização de desempenho fornece fluxo de alta qualidade em uma variedade de
tipos de rede. Aproveitando a capacidade da rede distribuída para distribuir a carga dinamicamente, o IBM Cloud CDN com o Akamai
oferece a capacidade de escalar rapidamente para grandes públicos, independentemente de você ter ou não se planejado para eles.

**Vídeo On Demand** é otimizado para distribuição de formatos de fluxo segmentado como HLS, DASH, HDS e HSS. A transmissão de vídeo em tempo real **não** é suportada neste momento. É possível ativar o recurso **Vídeo on Demand** selecionando a opção do menu suspenso em **Otimizar para** na guia Configurações ou ao criar um novo caminho de origem. É necessário ativar esse recurso apenas ao otimizar a entrega de arquivos de vídeo.
