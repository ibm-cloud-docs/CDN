---

copyright:
  years: 2017, 2020
lastupdated: "2020-08-31"

keywords:

subcollection: CDN

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}  

# Token authentication container
{: #token-auth-container}

The `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth` collection contains the attributes that are used by our Token Authentication APIs. Each of the token authentication APIs returns a collection of this type.
{: shortdesc}

**class** `SoftLayer_Container_Network_CdnMarketplace_Configuration_Behavior_TokenAuth`  

* `uniqueId` - The unique ID of the mapping to which this token authentication belongs.
* `path` - The path, relative to the domain that should be accessed using token authentication.
* `tokenKey` - The key/shared secret used to validate the tokens. This setting must match the key that is used in the token generation code. This specifies an even number of hex digits for the token key. An entry can be up to 64 characters in length.
* `name` - Specify the name of token on the incoming client HTTP request: cookie name, query string parameter name, or HTTP header name. The default value is `__token__`.
* `tokenDelimiter` - Specifies a single character to separate the individual token fields. The default value is `~`.
* `aclDelimiter` - Specifies a single character to separate access control list (ACL) fields. The default value is `!`.
* `hmacAlgorithm` - The algorithm used for the HMAC (Hashed Message Authentication Code) to generate the token. This setting must match the method that is chosen in the token generation code. **Caution!** The algorithms from most to least secure are `SHA256`, `SHA1`, and `MD5`. You should not change the default of `SHA256` unless you have a specific reason to do so (for example, if you have speed requirements or computational limitations on generating tokens at the origin).
* `escapeTokenInputs` - Specify whether the token inputs are URL escaped before generating the token. By default, inputs are URL escaped. This setting must match the setting that is used in the token generation code. Possible values `0` and `1`. If set to `1`, input values are escaped before adding them to the token. Default value is `1`.
* `ignoreQueryString` - Specify whether the query string is included in the URL input into the token. By default, the query string is included. This setting must match the setting that is used in the token generation code. Possible values `0` and `1`. If set to `1`, query strings are removed from a URL when computing the token’s HMAC algorithm. Default value is `0`.
* `transitionKey` - The token transition key. It's an alternate key/shared secret to validate tokens, which you can use to avoid downtime when rotating keys. Either the primary or transition key setting must match the key that is used in the token generation code. This specifies an even number of hex digits for the token transition key. An entry can be up to 64 characters in length.
