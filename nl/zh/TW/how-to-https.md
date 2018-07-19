---

copyright:
  years: 2018
lastupdated: "2018-06-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 完成 HTTPS 的網域控制驗證

## 網域控制驗證的起始步驟

**步驟 1：**

使用「DV SAN 憑證」訂購 CDN 之後，憑證要求處理程序即會開始。在此處理程序期間，IBM Cloud CDN 會向 Akamai 要求憑證。在憑證變成可用之後，Akamai 即會向「憑證管理中心 (CA)」發出要求。

  * 在此期間，CDN 狀態將會顯示為**要求憑證**。

    ![要求憑證狀態](images/requesting-cert.png)

**步驟 2：**

在 CA 收到要求之後，即會發出「網域驗證盤查」。

  * 發生此情況時，您 CDN 的狀態將會變更為**需要網域驗證**。

    ![需要網域驗證](images/domain-validation-needed.png)

**步驟 3：**

按一下需要驗證的 CDN 名稱。即會開啟「概觀」頁面，您可以在其中看到 CDN 的整體狀態。頁面頂端會有一個警示，提醒您需要驗證網域。選取**檢視網域驗證**按鈕來開啟一個視窗，其中顯示完成驗證處理程序所需的盤查資訊。

   ![需要網域驗證](images/view-domain-validation.png)

**步驟 4：**在完成有關如何處理「網域驗證盤查」之小節中的其中一個驗證步驟之後，CDN 就會移至**部署憑證**狀態。在此期間，Akamai 會將經過驗證的憑證配送至其 Edge Server。部署憑證可能需要 2 到 4 小時。

  * 此處理程序完成後，不論使用哪種驗證方法，所有網域都會移至 **CNAME 配置**狀態。

您可以在[開始執行](basic-functions.html#get-to-running)頁面中，找到有關完成「CNAME 配置」以及監督 CDN 的其他資訊。


## 網域驗證盤查

「網域驗證盤查」會證明您是網域擁有者。您可以使用下面三種方法來處理「網域驗證盤查」。

**附註**：如果您未在 48 小時內回應「網域驗證」要求，則要求將到期，並且需要重新開始訂購處理程序。

### CNAME

如果您的 CNAME 記錄已在訂購 CDN 之前新增至 DNS 提供者，則不需要執行其他任何動作。IBM Cloud、Akamai 及 CA 會自動處理「網域驗證」。驗證可能需要 2 到 4 小時。

  * 如果您尚未使用 DNS 提供者來配置 CNAME，則目前需要這樣做。大部分 DNS 提供者都會指示您如何設定或變更 CNAME。

   ![網域驗證 CNAME](images/domain-validation-cname.png)

**附註**：如果您的 CDN **不**負責處理即時資料流量，則**僅**建議使用此方法。如果您的網域負責處理即時資料流量，則建議使用「標準」方法來驗證網域。

---
### 標準

如果您選擇「網域驗證」的「標準」方法，則「網域驗證」視窗會顯示**盤查 URL** 及**盤查回應**。若要完成「網域驗證」處理程序，您必須將提供的**盤查回應**新增至原點伺服器。這可讓 CA 使用**盤查 URL** 中所指定的 URL，從原點伺服器擷取**盤查回應**。正確配置原點伺服器之後，「網域驗證」可能需要 2 到 4 小時。

   ![網域驗證盤查標準](images/domain-validation-standard.png)

若要透過「標準」方法順利完成「網域驗證」，您需要使用特定方式來配置原點伺服器。Apache 及 Nginx 伺服器的範例程序概述如下。

**範例狀況**
* 原點伺服器：`www.example.com`
* CDN 網域：`cdn.example.com`
* 盤查 URL：`http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* 盤查回應：`examplechallenge`

#### Apache 配置

  * **步驟 1：**登入執行 Apache2 伺服器的機器。

  * **步驟 2：**在網站內容目錄的 `.well-known/acme-challenge/` 下，建立適用於盤查回應的盤查回應檔。Apache2 網站內容的預設位置是 `/var/www/html/`。在此範例中，盤查回應將會放在 `/var/www/html/.well-known/acme-challenge/` 目錄中。

      ```
      mkdir -p /var/www/html/.well-known/acme-challenge
      printf "examplechallenge" > /var/www/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **步驟 3：**必要的話，請開啟 Apache2 伺服器配置檔。`/etc/apache2/apache2.conf` 及 `/etc/apache2/sites-enabled/` 是配置檔的預設位置。

  * **步驟 4：**必要的話，請將 CDN 網域新增為原點之虛擬主機的其他 **ServerAlias**。

  * **步驟 5：**如果您必須修改 Apache2 伺服器配置，請使用下列指令來重新啟動 Apache2 伺服器，且具有最小的關閉時間：

      ```
      apachectl -k graceful
      ```

  * **步驟 6：**在 CDN 網域與原點伺服器 IP 位址之間的 DNS 中建立一筆 A 記錄。

#### Nginx 配置

  * **步驟 1：**登入執行 Nginx 伺服器的機器。

  * **步驟 2：**在網站內容目錄的 `.well-known/acme-challenge/` 下，建立適用於盤查回應的盤查回應檔。Nginx 網站內容的預設位置是 `/usr/share/nginx/html/`。在此範例中，盤查回應將會放在 `/usr/share/nginx/html/.well-known/acme-challenge/` 目錄中。
      ```
      mkdir -p /usr/share/nginx/html/.well-known/acme-challenge
      printf "examplechallenge" > /usr/share/nginx/html/.well-known/acme-challenge/examplechallenge-fileobject
      ```

  * **步驟 3：**必要的話，請開啟 Nginx 伺服器配置檔。`/etc/nginx/nginx.conf` 及 `/etc/nginx/conf.d/` 是配置檔的預設位置。

  * **步驟 4：**必要的話，請將 CDN 網域新增為原點之伺服器區塊的其他 **server_name**。

  * **步驟 5：**如果您必須修改 Nginx 伺服器配置，請使用下列指令來重新啟動 Nginx 伺服器，且具有最小的關閉時間：

      ```
      nginx -s reload
      ```

  * **步驟 6：**在 CDN 網域與原點伺服器 IP 位址之間的 DNS 中建立一筆 A 記錄。

#### 驗證 CA 已可使用此標準方法來處理網域驗證

* 若要驗證此方法可透過 `curl` 運作，請執行「盤查 URL」的指令。
    ```
    curl -v http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```
* 若要驗證此方法可透過瀏覽器運作，請嘗試從瀏覽器存取「盤查 URL」。

在任一情況下，您都應該能夠擷取原點伺服器上所儲存的「網域驗證盤查」檔案物件副本。

#### 標準方法的清除

在您的 CDN 已達到**憑證部署**狀態之後，請執行下列動作：
1. 移除 `examplechallenge-fileobject` 檔案。（選用）
1. 必要的話，從伺服器配置中移除新增的 ServerAlias (Apache2) 或 server_name (Nginx)。（選用）
1. 移除 CDN 網域與原點伺服器 IP 之間的 A 記錄。

---
### 重新導向

按一下**重新導向**標籤，即會顯示透過重新導向來處理「網域驗證」所需的所有資訊。此資訊容許 CA 透過原點伺服器，從 Akamai 擷取**盤查回應**的副本。正確配置伺服器之後，「網域驗證」可能需要 2 到 4 小時。

   ![網域驗證盤查重新導向](images/domain-validation-redirect.png)

若要透過「重新導向」方法順利完成「網域驗證」，您可能需要使用特定方式來配置 Web 伺服器。Apache 及 Nginx 伺服器的範例程序概述如下。

**範例狀況**
* 原點伺服器：`www.example.com`
* CDN 網域：`cdn.example.com`
* 盤查 URL：`http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject`
* URL 重新導向：`http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject`

#### Apache 重新導向配置

  * **步驟 1：**登入執行 Apache2 伺服器的機器。

  * **步驟 2：**開啟 Apache2 伺服器配置檔。`/etc/apache2/apache2.conf` 及 `/etc/apache2/sites-enabled/` 是配置檔的預設位置。

  * **步驟 3：**在配置檔的適當位置中，新增重新導向陳述式。必要的話，請將 CDN 網域新增為原點之虛擬主機的其他 **ServerAlias**。
    ```
    Redirect http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject http://dcv.akamai.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

  * **步驟 4：**使用下列指令來重新啟動 Apache2 伺服器，且具有最小的關閉時間：

    ```
    apachectl -k graceful
    ```

  * **步驟 5：**在 CDN 網域與原點伺服器 IP 位址之間的 DNS 中建立一筆 A 記錄。

#### Nginx 重新導向配置

  * **步驟 1：**登入執行 Nginx 伺服器的機器。

  * **步驟 2：**開啟 Nginx 伺服器配置檔。`/etc/nginx/nginx.conf` 及 `/etc/nginx/conf.d/` 是配置檔的預設位置。

  * **步驟 3：**此步驟有兩個同樣有效的方法。

    * 選項 1：（建議）新增含 `return` 指引的 `location` 區塊，以在適當的 `server` 區塊內執行重新導向。必要的話，請將 CDN 網域新增為原點之伺服器區塊的其他 **server_name**。

    ```
    server {
      listen 80;
      server_name www.example.com cdn.example.com;

      # Some server configuration directives
      # ...

      location = /.well-known/acme-challenge/examplechallenge-fileobject  {
          return 302 http://dcv.akamai.com$request_uri;
      }

      # Some more server configuration directives
      # ...
    }
    ```

   * 選項 2：在 `server` 區塊內，新增 `rewrite` 指引。必要的話，請將 CDN 網域新增為原點之伺服器區塊的其他 **server_name**。

    ```
    server {
      listen 80;
      server_name www.exmaple.com cdn.example.com;

      # Some server configuration directives
      # ...

      rewrite ^/(\.well-known/acme-challenge/examplechallenge-fileobject)$ http://dcv.akamai.com/$1 redirect;

      # Some more server configuration directives
      # ...
    }
    ```

  * **步驟 4：**使用下列指令來重新啟動 Nginx 伺服器，且具有最小的關閉時間：

    ```
    nginx -s reload
    ```

  * **步驟 5：**在 CDN 網域與原點伺服器 IP 之間的 DNS 中建立一筆 A 記錄。

#### 驗證重新導向正在進行

完成這些步驟，只會將特定「盤查 URL」的資料流量重新導向至「URL 重新導向」。您可以驗證重新導向是透過 `curl` 還是瀏覽器適當地運作。

* 若要驗證重新導向是透過 `curl` 運作，請執行「盤查 URL」的指令。

    ```
    curl -vL http://cdn.example.com/.well-known/acme-challenge/examplechallenge-fileobject
    ```

* 若要驗證重新導向是透過瀏覽器運作，請嘗試從瀏覽器存取「盤查 URL」。

在任一情況下，您都應該能夠從原始要求會重新導向至的 dcv.akamai.com 網域的 Akamai 中擷取「網域驗證盤查」檔案物件的副本。

#### 重新導向方法的清除

在您的 CDN 已達到**憑證部署**狀態之後，請執行下列動作：
1. 從配置檔中移除重新導向陳述式或區塊。（選用）
1. 必要的話，從伺服器配置中移除新增的 ServerAlias (Apache2) 或 server_name (Nginx)。（選用）
1. 移除 CDN 網域與原點伺服器 IP 之間的 A 記錄。
