---

copyright:
  years: 2017, 2018, 2019
lastupdated: "2019-02-25"

keywords: hotlink, protection, class, behavior, API, valid string

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Classe Hotlink Protection
{: #hotlink-protection-class}

A classe `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection` contém os atributos utilizados por nossas APIs do Hotlink Protection. Esse objeto é usado para configurar o comportamento do Hotlink Protection de um CDN chamando a API.  Ele também é retornado pelas APIs do Hotlink Protection após uma chamada API bem-sucedida.

**class**`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_HotlinkProtection`:

* `://` no primeiro rótulo da URL **não** é suportado.
   * **Válido**: `http*www.example.com`
   * **Inválido**: `http://www.example.com`

* `protectionType`: especifica para permitir ou negar acesso ao conteúdo quando uma solicitação de HTTP tiver um valor Referer Header que corresponda a um dos termos em `refererValues`. O oposto ocorrerá quando não houver correspondência.
  * Valor possível para protectionType:
    * `ALLOW`
    * `DENY`
* `refererValues`: é uma lista separada por um único espaço de URLs referentes, que também pode ter correspondência de curinga, na forma de uma sequência.
  * Alguns exemplos de uma sequência **válida** para `refererValues`:
    * `alternate-domain.example.com`
    * `www1.example.com www2.example.com www3.example.com`
    * `www.example.com www.example.com/ www.example.com/path`
    * `*.example.com`
    * `www.example.*`
    * `*.example.com *.example.net`
    * `https*www.example.com`
  * Alguns exemplos de uma sequência **inválida** para `refererValues`:
   
      |**Descrição de exemplo inválido**| Exemplo
      |-------|-----|
      | Contém mais de 2.100 caracteres no total| `www1.example.com www2.example.com www3.example.com www4.example.com www5.example.com`...|
      |Contém pelo menos um termo de correspondência de URL com mais de 255 caracteres | `www1.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.example.com www.example.org` |
      |Contém duplicatas na lista | `domain1.example.com domain1.example.com`|
      |RefererValues vazio | ` `|
      |Usando caracteres não especificados no [RFC-3986 ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://tools.ietf.org/html/rfc3986#section-2) | `domain1.exa}mple.com domain1.example.com`|
      |O caractere `&` não é suportado| `www.example.com/path&`|
      |Uma sequência `refererValues` com pelo menos um termo de correspondência de URL que tenha um conjunto de caracteres na frente do primeiro caractere `.` que contém `://` não é suportada| `www.example.org http://www.example.com`|


