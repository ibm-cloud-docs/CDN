---

copyright:
  years: 2018
lastupdated: "2018-06-28"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Sobre HTTPS

O IBM Cloud oferece duas maneiras de proteger seu CDN com HTTPS: usando o Certificado curinga e o Certificado Domain Validation (DV) SAN. Ambas as opções de HTTPS podem ser configuradas selecionando **Porta HTTPS** ao configurar seu CDN. A Porta HTTPS padrão é 443, ou é possível escolher um número de porta diferente por meio da qual rotear o tráfego HTTPS. Uma lista de números de portas permitidos pode ser localizada na [FAQ](faqs.html#are-there-any-restrictions-on-what-http-and-https-port-numbers-are-allowed-for-akamai-).

## Suporte a Certificado Curinga
>O certificado curinga é a maneira mais simples de entregar conteúdo da web para seus usuários finais com segurança. O CDN CNAME completo, incluindo o sufixo de certificado Curinga, **deve**
ser usado como o ponto de entrada de serviço (por exemplo, `https://example.cdnedge.bluemix.net`) para o uso do certificado curinga.
>
>O IBM Cloud CDN usa o certificado curinga `*.cdnedge.bluemix.net`.
O CNAME que termina com o sufixo `*.cdnedge.bluemix.net`, independentemente de ter sido criado para você ou fornecido por você, é incluído no certificado curinga que é mantido no servidor CDN Edge.
Assim, o CNAME se torna a única maneira pela qual os usuários finais podem usar HTTPS para seu CDN.

![Diagram for Http and Wildcard](images/state-diagram-wildcard.png)

## Suporte do Certificado Subject Alternate Name (SAN)

>O Subject Alternative Name (SAN) é um certificado SSL digital que permite que múltiplos domínios ou nomes de host sejam protegidos por um único certificado.
>
>Com o certificado SAN para HTTPS, seu Nome do host CDN primário é incluído em um certificado que foi
emitido por uma Autoridade de certificação. Isso permite que seus usuários acessem seu serviço
de forma segura por meio do nome do host, não pelo CNAME; por exemplo, `https://www.example.com`.
>
>Quando o pedido do CDN é feito usando o certificado SAN HTTPS, ele passa pelos processos de
solicitação de um certificado e de criação de uma Domain Control Validation (DCV). A DCV é o processo que uma
Autoridade de certificação usa para estabelecer que você está autorizado a acessar e a controlar o domínio. 
Sua ação é necessária para concluir esta etapa. Depois que o controle é estabelecido, o certificado é
implementado nos servidores de borda do CDN em todo o mundo. Depois que o certificado é implementado com sucesso, a
renovação do certificado é manipulada automaticamente. Mais informações sobre esse recurso podem ser
localizadas na [descrição do recurso](about.html#https-protocol-support).
Os métodos de Domain Control Validation são explicados com mais detalhes na página
[Concluindo a Validação de Controle de
Domínio para HTTPS](how-to-https.html#initial-steps-to-domain-control-validation).

![Diagrama para o certificado HTTPS com SAN](images/state-diagram-san.png)
