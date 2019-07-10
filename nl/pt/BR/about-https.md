---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: wildcard certificate, https, san certificate

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:note .note}
{:download: .download}

# Sobre HTTPS
{: #about-https}

O {{site.data.keyword.cloud}} oferece duas maneiras de proteger seu CDN com HTTPS: o certificado curinga e o certificado SAN de validação de domínio (DV). Ambas as opções de HTTPS podem ser configuradas selecionando **Porta HTTPS** ao configurar seu CDN. A Porta HTTPS padrão é 443, ou é possível escolher um número de porta diferente por meio da qual rotear o tráfego HTTPS. Uma lista de números de portas permitidos pode ser localizada na [FAQ](docs/infrastructure/CDN?topic=CDN-faqs#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

Para decidir entre o uso de **Certificado curinga** e **Certificado SAN** para HTTPS, responda a esta pergunta: Deseja entregar o tráfego HTTPS por meio do CNAME do CDN ou do Nome de domínio do CDN? Se quiser entregar o tráfego HTTPS por meio do CNAME, selecione **Certificado curinga**. Se quiser entregar o tráfego HTTPS por meio do Nome de domínio do CDN, selecione **Certificado SAN**.

## Suporte a Certificado Curinga
{: #wildcard-certificate-support}

**Nota**:
novos mapeamentos de CDN com o Certificado curinga NÃO são suportados neste momento.

O certificado curinga é a maneira mais simples de entregar conteúdo da web para seus usuários finais com segurança. O CDN CNAME completo, incluindo o sufixo de certificado Curinga, **deve**
ser usado como o ponto de entrada de serviço (por exemplo, `https://example.cdnedge.bluemix.net`) para o uso do certificado curinga.

O IBM Cloud CDN usa o certificado curinga `*.cdnedge.bluemix.net`. O CNAME que termina com o sufixo `*.cdnedge.bluemix.net`, independentemente de ter sido criado para você ou fornecido por você, é incluído no certificado curinga que é mantido no servidor CDN Edge. Assim, o CNAME se torna a única maneira pela qual os usuários finais podem usar HTTPS para seu CDN.

![Diagrama para HTTP e curinga](images/state-diagram-wildcard.png)

## Suporte do Certificado Subject Alternate Name (SAN)
{: #san-certificate-suport}

O Subject Alternative Name (SAN) é um certificado SSL digital que permite que múltiplos domínios ou nomes de host sejam protegidos por um único certificado.

Com o certificado SAN para HTTPS, seu Nome do host CDN primário é incluído em um certificado que foi
emitido por uma Autoridade de certificação. Isso permite que seus usuários acessem seu serviço
de forma segura por meio do nome do host, não pelo CNAME; por exemplo, `https://www.example.com`.

Quando o pedido do CDN é feito usando o certificado SAN HTTPS, ele passa pelos processos de
solicitação de um certificado e de criação de uma Domain Control Validation (DCV). A DCV é o processo que uma
Autoridade de certificação usa para estabelecer que você está autorizado a acessar e a controlar o domínio. Sua ação é necessária para concluir esta etapa. Depois que o controle é estabelecido, o certificado é
implementado nos servidores de borda do CDN em todo o mundo. Depois que o certificado é implementado com sucesso, a
renovação do certificado é manipulada automaticamente. Mais informações sobre esse recurso podem ser
localizadas na [descrição do recurso](/docs/infrastructure/CDN?topic=CDN-feature-descriptions#https-protocol-support). Os métodos de Domain Control Validation são explicados com mais detalhes na página
[Concluindo a Validação de Controle de
Domínio para HTTPS](/docs/infrastructure/CDN?topic=CDN-completing-domain-control-validation-for-https-with-dv-san#initial-steps-to-domain-control-validation).

Depois que o CDN atingir o status RUNNING, o registro CNAME do nome do host do CDN deverá ser mantido em seu DNS. Se o registro CNAME for removido, o Nome do host do CDN poderá ser removido do certificado SAN no prazo de 3 dias. Se isso acontecer, o tráfego HTTPS não será mais entregue com esse Nome do host do CDN.
{:note}

![Diagrama para o certificado HTTPS com SAN](images/state-diagram-san.png)
