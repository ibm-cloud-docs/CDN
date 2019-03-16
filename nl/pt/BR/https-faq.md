---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-19"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}
{:faq: data-hd-content-type='faq'}

# FAQ para HTTPS
{: #faq-for-https}

## Para meu CDN, qual é a diferença entre HTTPS com certificado curinga e HTTPS com certificado SAN?
{:faq}

Com Certificados curingas, todos os clientes usam o mesmo certificado implementado nas redes CDN do fornecedor. O CNAME, incluindo o sufixo IBM `.cdnedge.bluemix.net`, deve ser usado para o acesso ao serviço. Por exemplo, `https://www.example-cname.cdnedge.bluemix.net`

No caso do certificado SAN, múltiplos domínios do cliente compartilham um único certificado SAN
incluindo seus nomes de domínio nas entradas do SAN. O serviço pode, então, ser acessado usando o nome do host,
por exemplo, `https://www.example.com`

## Como a Validação de Domínio com redirecionamento é feita?
{:faq}

Depende do seu servidor. O procedimento para concluir a Validação de domínio para servidores Apache e Nginx pode ser localizado na página [Concluindo a validação de controle de domínio para HTTPS](/docs/infrastructure/CDN/how-to-https.html#redirect).

## Quanto tempo leva a Validação de Domínio?
{:faq}

A Validação de domínio normalmente leva de duas a quatro horas, mas varia de acordo com o método escolhido para validação. A DV com a validação do CNAME é a mais rápida, levando geralmente menos de uma hora. A DV usando
os métodos Padrão e Redirecionamento geralmente leva em média 4 horas, depois que o desafio é resolvido.

## Quanto tempo leva para criar e ativar o HTTPS para meu CDN com um certificado SAN DV?
{:faq}

Uma solicitação normal para ativar o HTTPS leva em média de 3 a 9 horas, desde a solicitação inicial
até a execução.

## Quanto tempo leva para excluir um CDN com um certificado SAN DV?
{:faq}

A exclusão do CDN requer que seu domínio seja removido do certificado em todos os servidores de borda. Esse processo pode levar até 8 horas para ser concluído.

## Há algum custo adicional associado ao uso de um certificado SAN DV para meu CDN?
{:faq}

Não. As configurações do certificado SAN DV são fornecidas sem encargos adicionais, em
comparação com o HTTP ou HTTPS com o Certificado curinga.

## O meu CDN criado usando HTTPS com curinga pode ser atualizado para usar um certificado SAN DV?
{:faq}

Não, não é possível mudar um mapeamento de curinga para o certificado SAN neste momento.

## O que é uma Autoridade de Certificação?
{:faq}

Uma Autoridade de Certificação (CA) é uma entidade que emite Certificados Digitais.

## Qual CA o serviço {{site.data.keyword.cloud}} CDN usa para emitir um certificado SAN DV?
{:faq}

O serviço do IBM Cloud CDN usa a Autoridade de Certificação LetsEncrypt.

## Quais certificados SSL são suportados para o IBM Cloud CDN?
{:faq}

Os certificados SSL suportados são o certificado curinga e o Domain Validation (DV) Subject Alternate Name (SAN). O certificado SAN é compartilhado entre múltiplos clientes. O IBM
Cloud CDN não suporta o upload de certificados customizados.

## Recebi um e-mail pedindo para tratar de um desafio de Domain Validation relacionado ao meu CDN. O que faço agora?
{:faq}

A Validação de Domínio pode ser resolvida de uma das três maneiras: CNAME, Padrão ou Direto.

Para obter detalhes sobre como resolver em qualquer uma dessas maneiras, consulte o documento
[Concluindo
Domain Control Validation para HTTPS](/docs/infrastructure/CDN?topic=CDN-initial-steps-to-domain-control-validation).

## O que acontecerá se eu não tratar do desafio para a validação de domínio do meu CDN?
{:faq}

Se o estado do mapeamento permanecer em DOMAIN_VALIDATION_PENDING por mais de 48 horas, a criação do
mapeamento será cancelada e o estado do mapeamento será CREATE_ERROR. Nesse estado, é possível optar por
tentar novamente a criação ou excluir o mapeamento.

## Um certificado curinga precisa validar um domínio para meu CDN?
{:faq}

Não, mas é possível utilizar apenas o CNAME para recuperar o conteúdo de sua origem. ` https://www.example-cname.cdnedge.bluemix.net `

## Se eu usar o tipo de certificado SAN para meu CDN, ainda posso usar o CNAME para acessar meu serviço?
{:faq}

Não. Para o certificado SAN, é possível usar apenas o domínio customizado para acessar o conteúdo por
meio da origem. ` https://www.example.com `

## Todos os meus domínios do IBM Cloud CDN são incluídos em um certificado?
{:faq}

Não necessariamente. A seleção do certificado é manipulada pelo Akamai, a fim de assegurar que os
certificados estejam no estado mais eficiente. Os domínios são incluídos em diferentes certificados em uma
maneira bem proporcionada, portanto, não é possível garantir que todos os seus domínios estarão no mesmo
certificado.

## Por que é exibido "curinga" quando executo uma `dig` ou quando tento
acessar o conteúdo quando meu CDN está no status `Requesting Certificate`,
`Domain Validation pending` ou `Deploying Certificate`?
{:faq}

Durante o processo de solicitação de certificado SAN DV, a cadeia de registro DNS para seu CDN é
encadeada para um certificado curinga, temporariamente. Até que o processo seja concluído, o conteúdo
é entregue temporariamente por meio desse certificado curinga. Depois que o processo de solicitação é concluído,
a cadeia de registro DNS é atualizada para ser encadeada ao seu certificado SAN DV do CDN.
