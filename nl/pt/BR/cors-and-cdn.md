---

copyright:
  years: 2018, 2019
lastupdated: "2019-05-21"

keywords: cross origin resource sharing, CORS, CORS request, same-origin policy, simple request, preflighted request

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# CORS e solicitações de CORS por meio do CDN
{: #cors-and-cors-requests-through-your-cdn}

O Cross Origin Resource Sharing (CORS) é um mecanismo usado pelos navegadores, principalmente para validar permissões para acesso ao conteúdo de uma origem diferente.

## O que é CORS?
{: #what-is-cors}

Quando um navegador carrega uma página da web, ele aplica a **Política de mesma origem**, o que significa que ele só permite que o conteúdo seja buscado da mesma origem que o da página da web. No entanto, em alguns casos, uma página da web pode precisar de acesso a ativos de várias origens que confiam nesse website. Aqui é onde o CORS entra. 

Esse mecanismo de segurança existirá apenas se um aplicativo tiver ou for um cliente HTTP e se implementar o CORS. Quase todos os navegadores modernos, como Chrome, Firefox e Safari, implementam o CORS.

Para esclarecer, **uma origem com relação ao CORS não precisa ser a mesma que uma origem de CDN**. Uma origem no CORS é definida por um esquema de URI, um domínio e qualquer número de porta possível. Por exemplo, `https://www.example.com:1443` é uma origem diferente de `http://www.example.com`. E assim, um CDN também pode ser considerado uma origem do CORS na perspectiva de um navegador.

## Como o CORS funciona

O CORS pode manipular dois tipos de solicitações: _solicitações simples_ e _solicitações simuladas_, que são mais complexas.

### Solicitações simples
{: #simple-requessts}

**Primeira solicitação (acesso ao recurso):**

![cors simples](/images/cors-simple.png)

[Solicitações simples ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.w3.org/TR/cors/#simple-cross-origin-request) por meio de CORS são solicitações `GET` ou `POST` da página da web de uma origem que está tentando obter acesso à URL de outra origem para recursos.

Ao fazer essa solicitação, o navegador automaticamente configura os cabeçalhos de solicitação do CORS. Principalmente, ele configura a origem da página da web fazendo a solicitação no cabeçalho HTTP `Origin` automaticamente. A solicitação de CORS também pode conter um conjunto de [certos cabeçalhos HTTP padrão ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.w3.org/TR/cors/#simple-header), enquanto mantém seu status como uma solicitação de CORS simples, da perspectiva do navegador.

O servidor que recebe a solicitação de CORS a processa e pode (ou não) enviar um conjunto de cabeçalhos de resposta do CORS de volta para o navegador, com o conteúdo solicitado. Esses cabeçalhos de resposta do CORS contêm valores que especificam se a página da web atual tem permissão de acesso a esses recursos, se os cabeçalhos enviados são aceitáveis para essa solicitação e assim por diante.

Se o navegador não conseguir ver sua solicitação do CORS atendida pelos cabeçalhos de resposta do CORS, ele evitará automaticamente o acesso e o carregamento do conteúdo. Caso contrário, ele verá que a origem do CORS está fornecendo permissão para usar o recurso e permitirá o acesso e o carregamento do conteúdo solicitado.

### Solicitações simuladas
{: #preflighted-requests}

**Primeira solicitação (simulação):**

![simulação de cors](/images/cors-preflight.png)

Segunda solicitação (acesso ao recurso):

![cors após simulação](/images/cors-after-preflight.png)

Para comunicação de CORS mais complexa entre o navegador e uma origem do CORS que é diferente da página da web de solicitação, uma [solicitação de simulação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0) seria necessária antes de um acesso de recurso real. Determinadas situações podem requerer solicitações de CORS de simulação, como métodos HTTP que não sejam métodos `GET` ou `POST`, ou usar cabeçalhos HTTP não padrão com a solicitação, mesmo se for uma solicitação `GET` ou `POST`, e assim por diante.

Caso uma solicitação de simulação seja necessária, eis como os eventos se desdobram:

* O navegador envia uma solicitação usando o método HTTP OPTIONS para o servidor com todos os cabeçalhos de solicitação de CORS desejados. 
* O servidor processa esses cabeçalhos de solicitação de CORS e pode responder com cabeçalhos de resposta do CORS que não contêm dados de conteúdo reais. 
* O navegador verifica os cabeçalhos de resposta do CORS para certificar-se de que a solicitação do CORS seja permitida. 
* Se o navegador perceber que a solicitação de recurso desejada deve ser permitida pelo servidor, ele fará uma segunda solicitação para o navegador com o método HTTP desejado, se `GET`, `POST`, `PUT` etc., com os mesmos cabeçalhos de solicitação do CORS.

Posteriormente, a comunicação entre o navegador e a origem do CORS (diferente daquele da página da web) continuará como se fosse uma solicitação de CORS simples. Semelhante a uma solicitação de CORS simples, o conteúdo e os recursos estão acessíveis e poderão ser carregados se essa segunda solicitação de CORS também for permitida.

## Como configurar o CORS em sua origem
{: #how-to-set-up-cors-at-your-origin}

Conforme mostrado nos diagramas anteriores, o CORS é iniciado pelo cliente HTTP solicitante. No entanto, os efeitos dependem da origem solicitada. Para que seu conteúdo esteja pronto para solicitações de CORS, a sua origem deverá estar configurada corretamente para responder com os cabeçalhos de resposta corretos do CORS e as permissões de acesso corretas.

O exemplo a seguir mostra uma configuração básica do CORS para um servidor Nginx:

```nginx
http {
    # some http context configs

    server {
        # some server context configs

        # URI path to some content
        location /my-static-content {
        
            # some location context configs
        
            # Handle simple requests
            #
            # Consider only "HTTP GET" requests (content fetching)
            if ($request_method = 'GET') {
                
                # Allows the browser to access data from this server during CORS,
                # only if the request comes from a webpage from the following origin
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
            }
            
            # Handle preflight requests
            if ($request_method = 'OPTIONS') {
                
                # Allows the browser to access data from this server during CORS,
                # only if the request comes from a webpage from the following origin
                add_header 'Access-Control-Allow-Origin' 'https://www.example.com';
                
                # Allows only GET requests 
                add_header 'Access-Control-Allow-Methods' 'GET';
                
                # Allows the following headers in the browser's request headers
                # that may have been added by anything other than what the browser had added automatically
                add_header 'Access-Control-Allow-Headers' 'pragma';
                
                # Specifies to the browser how long it should cache this preflight response
                add_header 'Access-Control-Max-Age' 1728000;
                
                # HTTP 204 response code means success,
                # but also that it should expect no content from this (preflight) response
                return 204;
            }
            
            # more location context configs
            
            # If it is not a complex CORS situation requiring a preflight response,
            # then finally attempt to serve the file whose path falls under this location block's URI path match
            #
            # If no such file is found, then present an HTTP 404 code (not found)
            try_files $uri =404;
        }

        # more server context configs
    }

    # more http context configs
}
```
{: screen}

Geralmente, o navegador deverá carregar livremente o conteúdo quando ele vir um `Access-Control-Allow-Origin: *` nos cabeçalhos de resposta de CORS do servidor de acordo com a [especificação w3 referente a esse valor curinga ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.w3.org/TR/cors/#security). No entanto, nem todos os navegadores suportam `Access-Control-Allow-Origin: *`.

Se o servidor tiver que suportar o acesso de várias páginas da web, cada uma entregue de uma origem diferente, um único valor de origem para `Access-Control-Allow-Origin` deverá ser gerado dinamicamente por solicitação. A seguir está um exemplo básico de tal caso de uso para um servidor Nginx:

```nginx
http {
    # some http context config

    ######
    # Input from $http_origin,          the value in Origin HTTP header
    # Output to $cors_allowed_origin,   a string
    #
    # Full string match on the Origin header value
    #
    # Attempt to find a match for value from $http_origin using regex on left column.
    # If a match is found, then evaluate $cors_allowed_origin to the same value as $http_origin
    # Else, default evaluate $cors_allowed_origin to an empty string, so browsers will disallow the CORS request
    ######
    map $http_origin $cors_allowed_origin {
        default '';
        ~^http(s)?://(www|www2|cdn|dev)\.example\.com$ $http_origin;
    }
    
    server {
        # some server context configs

        # URI path to some content
        location /my-static-content {
        
            # some location context configs
        
            # Handle simple requests
            if ($request_method = 'GET') {
                add_header 'Access-Control-Allow-Origin' $cors_allowed_origin;
            }
            
            # Handle preflight requests
            if ($request_method = 'OPTIONS') {
                add_header 'Access-Control-Allow-Origin' '$cors_allowed_origin;
                add_header 'Access-Control-Allow-Methods' 'GET';
                add_header 'Access-Control-Allow-Headers' 'pragma';
                add_header 'Access-Control-Max-Age' 1728000;
                
                return 204;
            }
            
            # more location context configs

            try_files $uri =404;
        }

        # more server context configs
    }

    # more http context configs
}
```
{: screen}

No exemplo anterior, a diretiva `map` é usada para evitar o uso excessivo da instrução `if` do Nginx. Agora, quando uma solicitação de CORS é feita para esse servidor e corresponde a esse caminho de URI, o servidor responde com o cabeçalho `Access-Control-Allow-Origin` que contém o valor `http://www.example.com`, `https://cdn.example.com` ou `http://dev.example.com`, etc., quando o conteúdo é solicitado de `http://www.example.com`, `https://cdn.example.com`, `http://dev.example.com` e assim por diante.

## Como configurar o CORS para CDN
{: #how-to-set-up-cors-for-cdn}

![cors por meio do cdn](/images/cors-through-cdn.png)
O CDN é amplamente transparente para a configuração de CORS da Origem, portanto, ele não requer uma configuração de CDN específica. Se a borda do CDN não puder localizar uma resposta em cache para a primeira solicitação de algum conteúdo, ela encaminhará a solicitação para o Host de origem. Se o Host de origem estiver configurado para manipular solicitações de CORS e essa solicitação tiver o cabeçalho `Origin`, ele deverá responder de volta à borda com um cabeçalho do CORS de `Access-Control-Allow-Origin` e o valor associado. A resposta geral, incluindo esse cabeçalho e o valor, será armazenada em cache no CDN. Qualquer solicitação subsequente para o objeto no mesmo caminho de URI é atendida por meio do cache e inclui o valor do cabeçalho `Access-Control-Allow-Origin` que foi originalmente recebido da Origem.

## Resolução de problemas de CORS e de solicitações de CORS
{: #troubleshooting-cors-and-cors-requests}

Se o servidor Origem estiver configurado para CORS e você não vir o cabeçalho `Access-Control-Allow-Origin` retornado para a solicitação do navegador, será possível que o cabeçalho de resposta armazenado em cache no CDN tenha sido para uma solicitação que não tinha o cabeçalho Origem na solicitação. O CDN armazena em cache cabeçalhos de resposta do Host de origem. No entanto, os cabeçalhos em cache se baseiam na solicitação que acionou a solicitação para Origem. Nesse caso, os cabeçalhos de resposta podem não incluir os cabeçalhos do CORS. Limpe o cache no CDN para esse caminho usando a funcionalidade de Limpeza do CDN e tente a solicitação novamente por meio do cliente.

O cabeçalho de resposta `Vary` de seu servidor Origem também pode causar um comportamento inesperado ao buscar conteúdo por meio de seu CDN. Exceto uma exceção muito específica, o CDN não armazenará conteúdo em cache (e seu cabeçalho de resposta associado) de seu servidor Origem, se o servidor responder com um cabeçalho `Vary`. Atualmente, nosso serviço removerá o cabeçalho Variar da Origem para renderizar o objeto armazenável em cache, se o objeto for um dos tipos de arquivo a seguir: `aif, aiff, au, avi, bin, bmp, cab, carb, cct, cdf, class, css, doc, dcr, dtd, exe, flv, gcf, gff, gif, grv, hdml, hqx, ico, ini, jpeg, jpg, js, mov, mp3, nc, pct, pdf, png, ppc, pws, swa, swf, txt, vbs, w32, wav, wbmp, wml, wmlc, wmls, wmlsc, xsd, zip, webp, jxr, hdp, wdp, pict, tif, tiff, mid, midi, ttf, eot, woff, otf, svg, svgz, jar, woff2, json`. Se o objeto a ser armazenado em cache não for um desses tipos de arquivo, remova o cabeçalho `Vary` da resposta do servidor Origem desse objeto e tente novamente depois de usar a funcionalidade de Limpeza do CDN.

## Mais informações
{: #more-information}

* [https://www.w3.org/TR/cors/ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo") ](https://www.w3.org/TR/cors/)
* [https://w3c.github.io/webappsec-cors-for-developers/ ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://w3c.github.io/webappsec-cors-for-developers/)
