---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-05"

keywords: faq, https, wildcard, certificate, san certificate, domain validation, redirect, domains, challenge

subcollection: CDN

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
{: #for-my-cdn-what-is-the-difference-between-https-with-wildcard-certificate-and-https-with-san-certificate}
{:faq}

Com Certificados curingas, todos os clientes usam o mesmo certificado implementado nas redes CDN do fornecedor. O CNAME, incluindo o sufixo IBM `.cdnedge.bluemix.net`, deve ser usado para o acesso ao serviço. Por exemplo, `https://www.example-cname.cdnedge.bluemix.net`

No caso do certificado SAN, múltiplos domínios do cliente compartilham um único certificado SAN
incluindo seus nomes de domínio nas entradas do SAN. O serviço pode, então, ser acessado usando o nome do host,
por exemplo, `https://www.example.com`

## Como a Validação de Domínio com redirecionamento é feita?
{: #how-is-domain-validation-with-redirect-accomplished}
{:faq}

Depende do seu servidor. O procedimento para concluir a Validação de domínio para servidores Apache e Nginx pode ser localizado na página [Concluindo a validação de controle de domínio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#redirect).

## Quanto tempo leva a Validação de Domínio?
{: #how-long-does-domain-validation-take}
{:faq}

A Validação de domínio normalmente leva de duas a quatro horas, mas varia de acordo com o método escolhido para validação. A DV com a validação do CNAME é a mais rápida, levando geralmente menos de uma hora. A DV usando
os métodos Padrão e Redirecionamento geralmente leva em média 4 horas, depois que o desafio é resolvido.

## Quanto tempo leva para criar e ativar o HTTPS para o meu CDN com um certificado SAN DV?
{: #how-long-does-it-take-to-create-and-enable-https-for-my-cdn-with-a-dv-san-certificate}
{:faq}

Uma solicitação normal para ativar o HTTPS leva em média de 3 a 9 horas, desde a solicitação inicial
até a execução.

## Quanto tempo leva para excluir um CDN com um certificado SAN DV?
{: #how-long-does-it-take-to-delete-a-cdn-with-a-dv-san-certificate}
{:faq}

A exclusão do CDN requer que seu domínio seja removido do certificado em todos os servidores de borda. Esse processo pode levar até 8 horas para ser concluído.

## Há algum custo adicional associado ao uso de um certificado SAN DV para meu CDN?
{: #is-there-any-additional-cost-associated-with-using-a-dv-san-certificate-for-my-cdn}
{:faq}

Não. As configurações do certificado SAN DV são fornecidas sem encargos adicionais, em
comparação com o HTTP ou HTTPS com o Certificado curinga.

## O meu CDN criado usando HTTPS com curinga pode ser atualizado para usar um certificado SAN DV?
{: #can-my-cdn-created-using-https-with-wildcard-be-updated-to-use-a-dv-san-certificate}
{:faq}

Não, um mapeamento de curinga não pode ser mudado para o certificado SAN.

## O que é uma Autoridade de Certificação?
{: #what-is-a-certificate-authority}
{:faq}

Uma Autoridade de Certificação (CA) é uma entidade que emite Certificados Digitais.

## Qual CA o serviço {{site.data.keyword.cloud}} CDN usa para emitir um certificado SAN DV?
{: #which-ca-does-ibm-cloud-cdn-service-use-for-issuing-a-dv-san-certificate}
{:faq}

O serviço do IBM Cloud CDN usa a Autoridade de Certificação LetsEncrypt.

## Quais certificados SSL são suportados para o IBM Cloud CDN?
{: #what-ssl-certificates-are-supported-for-ibm-cloud-cdn}
{:faq}

Os certificados SSL suportados são o certificado curinga e o Domain Validation (DV) Subject Alternate Name (SAN). O certificado SAN é compartilhado entre múltiplos clientes. O IBM
Cloud CDN não suporta o upload de certificados customizados.

## Recebi um e-mail pedindo para tratar de um desafio de Domain Validation relacionado ao meu CDN. O que faço agora?
{: #i-received-an-email-asking-me-to-address-a-domain-validation-challenge-related-to-my-cdn}
{:faq}

A Validação de Domínio pode ser resolvida de uma das três maneiras: CNAME, Padrão ou Direto.

Para obter detalhes sobre como resolver em qualquer uma dessas maneiras, consulte o documento
[Concluindo
Domain Control Validation para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

## O que acontecerá se eu não tratar do desafio para a validação de domínio do meu CDN?
{: #what-will-happen-if-i-dont-address-the-challenge-for-domain-validation-of-my-cdn}
{:faq}

Se o estado do mapeamento permanecer em DOMAIN_VALIDATION_PENDING por mais de 48 horas, a criação do
mapeamento será cancelada e o estado do mapeamento será CREATE_ERROR. Nesse estado, é possível optar por
tentar novamente a criação ou excluir o mapeamento.

## Um certificado curinga precisa validar um domínio para meu CDN?
{: #does-a-wildcard-certificate-need-to-validate-a-domain-for-my-cdn}
{:faq}

Não, mas é possível utilizar apenas o CNAME para recuperar o conteúdo de sua origem. ` https://www.example-cname.cdnedge.bluemix.net `

## Recebi um e-mail indicando que meus domínios não estão apontados para o CNAME do IBM CDN. O que faço agora?
{: #i-received-an-email-indicating-that-my-domain-is-not-pointed-to-IBM-CDN-CNAME}
{:faq}

Este e-mail significa que seu CDN não está sendo usado. Para usar o CDN e tornar os domínios ativos nos certificados, deve-se configurar os registros DNS de CNAME listados em seu sistema de provedor DNS.
Se você concluir essa ação dentro de sete dias, o tráfego HTTP e HTTPS será restaurado para o seu CDN e o CDN irá para o status RUNNING. Se o CDN ainda não for usado após sete dias, devemos desativar permanentemente o HTTPS para o seu domínio CDN para evitar que seu domínio não usado bloqueie a inclusão de novas solicitações de domínio CDN no certificado SAN compartilhado. O acesso ao tráfego HTTP por meio do CDN ainda pode ser restaurado posteriormente por meio da inclusão de um registro CNAME para seu domínio.
Para obter detalhes sobre como resolver essa situação, consulte o documento [Concluindo a validação de controle de domínio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#cname).

## Se eu usar o tipo de certificado SAN para meu CDN, ainda posso usar o CNAME para acesso ao meu serviço?
{: #if-i-use-a-san-certificate-type-for-my-cdn-can-i-still-use-the-cname-for-access-to-my-service}
{:faq}

Não. Para o certificado SAN, é possível usar apenas o domínio customizado para acessar o conteúdo por
meio da origem.

## Todos os meus domínios do IBM Cloud CDN são incluídos em um certificado?
{: #are-all-of-my-ibm-cloud-cdn-domains-added-into-one-certificate}
{:faq}

Não necessariamente. A seleção do certificado é manipulada pelo Akamai, a fim de assegurar que os
certificados estejam no estado mais eficiente. Os domínios são incluídos em diferentes certificados em uma
maneira bem proporcionada, portanto, não é possível garantir que todos os seus domínios estarão no mesmo
certificado.

## Por que é exibido "curinga" quando executo uma `dig` ou quando tento
acessar o conteúdo quando meu CDN está no status `Requesting Certificate`,
`Domain Validation pending` ou `Deploying Certificate`?
{: #why-do-i-see-wildcard}
{:faq}

Durante o processo de solicitação de certificado SAN DV, a cadeia de registro DNS para seu CDN é
encadeada para um certificado curinga, temporariamente. Até que o processo seja concluído, o conteúdo
é entregue temporariamente por meio desse certificado curinga. Depois que o processo de solicitação é concluído,
a cadeia de registro DNS é atualizada para ser encadeada ao seu certificado SAN DV do CDN.
