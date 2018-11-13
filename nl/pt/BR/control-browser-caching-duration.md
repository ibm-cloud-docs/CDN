---

copyright:
  years: 2017, 2018
lastupdated: "2018-08-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# Usando o controle de cache `max-age` para controlar a duração de armazenamento em cache do navegador

Ao usar um CDN, há um tipo de armazenamento em cache para cada local no qual o armazenamento em cache pode ocorrer:
  * O **armazenamento em cache no nível do navegador** ocorre quando o navegador armazena em cache uma parte do conteúdo por um período de tempo específico.
  * O **armazenamento em cache no nível de borda** ocorre quando um servidor de borda do CDN armazena em cache uma parte do conteúdo da origem por um período de tempo específico, mas não necessariamente o mesmo.

Há alguns métodos para controlar por quanto tempo o conteúdo é armazenado em cache no nível do navegador. O método a ser usado depende dos fatores a seguir:
  * Se você tiver atualizado ou [atualizará os detalhes de configuração de seu CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details) com "Respeitar o cabeçalho: ativado".
  * Se o servidor de origem fornecer um valor `max-age` no cabeçalho Controle de cache para uma determinada parte do conteúdo. 

Independentemente de como esses fatores mudam, sua origem precisa fornecer um cabeçalho Controle de cache não vazio para seu conteúdo de destino. Se a sua origem não fornecer um cabeçalho Controle de cache não vazio para o servidor de borda, o servidor de borda não fornecerá seu próprio cabeçalho Controle de cache para o navegador.

Quando o servidor de borda envia um cabeçalho Controle de cache para o navegador, o valor `max-age` para esse cabeçalho Controle de cache corresponde ao período de tempo restante antes que o conteúdo no servidor de borda fique antigo e precise de revalidação da origem. 

## Respeitar cabeçalho: desativado
Quando Respeitar cabeçalho estiver configurado como **Desativado**, será possível controlar o tempo de cache no nível do navegador, configurando suas definições de TTL do CDN. Para cada arquivo ou diretório de arquivos, é necessário criar uma regra TTL para seu caminho e duração de cache desejada.

Essa configuração faz duas coisas:
  * A regra TTL informa ao servidor de borda para armazenar em cache o conteúdo do período de tempo especificado.
  * Quando o conteúdo é entregue por meio do servidor de borda, o servidor de borda envia seu próprio cabeçalho Controle de cache com um valor `max-age`, com base na duração de TTL, para o navegador.

## Respeitar cabeçalho: ativado
Quando **Respeitar cabeçalho** estiver configurado como **Ativado**, será possível optar por não configurar uma regra TTL para controlar o tempo de armazenamento em cache no nível do navegador para seu conteúdo. Nesse caso, é necessário configurar seu servidor de origem para fornecer um cabeçalho Controle de cache com um valor `max-age` para uma determinada parte do conteúdo. Como Respeitar cabeçalho está Ativado, o servidor de borda usa o valor `max-age` do Controle de cache da origem como a duração de TTL desse conteúdo específico. Se você já tiver uma entrada de regra TTL para seu CDN que cubra o conteúdo, o servidor de borda realmente ainda usará o valor `max-age` e sobrescreverá qualquer duração de armazenamento em cache no nível de borda existente para esse conteúdo.

Essa configuração faz duas coisas:
  * O valor da diretiva `max-age` do Controle de cache da origem informa ao servidor de borda para armazenar em cache o conteúdo pelo período de tempo especificado.
  * Quando o conteúdo é entregue por meio do servidor de borda, o servidor de borda envia seu próprio cabeçalho Controle de cache para o navegador com um valor `max-age` com base no valor `max-age` da origem.

Nenhum outro conteúdo coberto por essa regra TTL será afetado se você estiver usando o método de destino mostrado no exemplo.

Quando **Respeitar cabeçalho** estiver configurado como **Ativado**, a duração do armazenamento em cache no navegador poderá ser controlada por uma regra TTL, se o servidor de origem não enviar a diretiva `max-age` no cabeçalho Controle de cache. Se ele especificar um `max-age`, esse valor poderá substituir acidentalmente o valor de tempo na regra TTL.

## Resumo

|Respeitar cabeçalho|A origem fornece Controle de cache|Duração do armazenamento em cache do conteúdo específico no Servidor de borda|O Servidor de borda fornece Controle de cache|
|---|---|---|---|
|Ativado|Sim, especifica um `max-age`|Sobrescrito pelo valor `max-age` da Origem|Sim, `max-age` é o tempo antes de o conteúdo da borda precisar de revalidação da origem|
|Ativado|Sim, não especifica um `max-age`|Baseia-se na configuração de TTL do CDN|Sim, `max-age` é o tempo antes de o conteúdo da borda precisar de revalidação da origem|
|Ativado|Não|Baseia-se na configuração de TTL do CDN|Não|
|Desativado|Sim, especifica um `max-age`|Baseia-se na configuração de TTL do CDN|Sim, `max-age` é o tempo antes de o conteúdo da borda precisar de revalidação da origem|
|Desativado|Sim, não especifica um `max-age`|Baseia-se na configuração de TTL do CDN|Sim, `max-age` é o tempo antes de o conteúdo da borda precisar de revalidação da origem|
|Desativado|Não|Baseia-se na configuração de TTL do CDN|Não|

## Mais informações
* Como [gerenciar seu CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Controle de cache conforme definido na seção 14.9 da [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
