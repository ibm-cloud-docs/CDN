---

copyright:
  years: 2018, 2019
lastupdated: "2019-04-04"

keywords: rules, naming, conventions, hostname, cname, RFC, 1033, 1035, bucket, path, origin, purge, alphanumeric, top-level domain, valid, string

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

# Regras e Convenções de Nomenclatura
{: #rules-and-naming-conventions}

## Quais são as regras para o Nome do host do CDN?
{: #what-are-the-rules-for-the-cdn-hostname}

A sequência de entrada `Hostname` do CDN **deve**:
  * consistir em caracteres alfanuméricos
  * ter menos que 254 caracteres
  * terminar com um nome de domínio de nível superior válido
  * **não** deve conter mais de 10 rótulos
  * **não** deve terminar em `cdnedge.bluemix.net`
(esse final é usado para o CNAMES e é reservado)

Consulte o RFC 1035, seção 2.3.4 para obter mais detalhes. 

Além disso, é altamente recomendável usar um nome de domínio completo como seu Nome do host do CDN. Escolha um nome no formato `www.example.com` em vez de um nome de domínio-raiz (também conhecido como domínio Zone Apex ou Naked), do formato `example.com`. Será necessário criar um Registro CNAME para o Nome do host do CDN que você usar e a RFC 1033 do DNS requererá que o registro de domínio-raiz seja um Registro A, não um CNAME. É fornecido mais esclarecimento na RFC 2181, seção 10.1

## Quais são as convenções de nomenclatura customizadas do CNAME?
{: #what-are-the-custom-cname-naming-conventions}

A sequência de entrada `CNAME` deve seguir as seguintes regras:
  * **deve** ser exclusivo (não pode estar em uso por nenhum outro CDN do IBM Cloud)
  * menos de 64 caracteres alfanuméricos
  * `caracteres especiais. @ # $ % ^ & *` **não** são permitidos
  * sublinhados `_` **não** são permitidos
  * pontos `.` **não** são permitidos
  * traços `-` são permitidos, mas o CNAME não pode começar ou terminar com um traço

## Quais são as regras para nomes de depósito?
{: #what-are-the-rules-for-bucket-names}

Nomes de depósito:
  * ** deve **  ter pelo menos 1 caractere
  * deve ter menos de 200 caracteres
  * pode conter letras (letras maiúsculas e minúsculas são permitidas), números e hifens
  * **não** devem ser formatados como um endereço IP (por exemplo, 127.0.0.1)
  * **não** pode iniciar com um ponto (.)
  * pode terminar com um ponto (.)
  * somente um ponto (.) é permitido entre os rótulos (por exemplo, my..ibmcloud.bucket não é permitido).

Embora as letras maiúsculas e os períodos possam passar na validação, sugerimos que você sempre use nomes de depósito compatíveis com o DNS.
{: note}

## Quais são as regras para a sequência de caminho para a origem?
{: #what-are-the-rules-for-the-path-string-for-the-origin}

O caminho é opcional ao criar seu CDN. No entanto, se fornecido, o caminho **deve**:
  * ter menos que 1000 caracteres de comprimento
  * começar com  ` / `

## Quais são as regras para a sequência Caminho para a Limpeza?
{: #what-are-the-rules-for-the-path-string-for-purge}

O caminho de Limpeza:
  * deve ter menos de 1.000 caracteres de comprimento
  * deve começar com  ` / `
  * não pode terminar com  ` / `
  * não pode terminar com um ponto (.)
  * não pode conter `*`

A limpeza é permitida apenas para arquivos únicos. A limpeza em nível de diretório não é suportada neste momento.
{; note}

## Para o comando **Add Origin**, existem regras a seguir para a sequência de Caminho?
{: #for-the-add-origin-command-are-there-any-rules-to-follow-for-the-path-string}

Para **Incluir origem**, o caminho é **obrigatório**. Além disso, se o CDN for criado com um caminho, o caminho para **Incluir origem** deverá iniciar com o caminho do CDN como o prefixo. Por exemplo, se o caminho CDN foi especificado como `/storage`, para chamar
**Incluir origem** com um caminho chamado `/examplePath`,
o caminho fornecido é `/storage/examplePath`.

## Quais são as regras para fornecer extensões de arquivo?
{: #what-are-the-rules-for-providing-file-extensions}

Ao criar uma Origem com o Object Storage, as extensões de arquivo devem ser separadas por
vírgulas. Por exemplo, `txt, jpg, xml` é uma lista válida. Nenhuma extensão particular pode ser maior que 10 caracteres.
