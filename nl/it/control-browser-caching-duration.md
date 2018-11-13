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

# Utilizzo di `max-age` di Cache Control per controllare la durata della memorizzazione in cache del browser

Quando si utilizza una CDN, c'è un singolo tipo di memorizzazione nella cache per ogni ubicazione dove può verificarsi la memorizzazione in cache:
  * La **memorizzazione in cache a livello del browser** si verifica quando il browser memorizza in cache del contenuto per uno specifico lasso di tempo.
  * La **memorizzazione in cache a livello edge** si verifica quando un server edge CDN memorizza in cache del contenuto dall'origine per uno specifico, ma non necessariamente uguale, lasso di tempo.

Ci sono alcuni metodi per controllare la durata della memorizzazione in cache del contenuto a livello del browser. Il metodo da utilizzare dipende dai seguenti fattori:
  * Se hai aggiornato o [aggiornerai i dettagli di configurazione della tua CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html#updating-cdn-configuration-details) con “Respect Header: On”.
  * Se il server di origine fornisce un valore `max-age` nell'intestazione Cache-Control per del determinato contenuto. 

Indipendentemente da come variano questi fattori, la tua origine deve fornire un'intestazione Cache-Control non vuota per il tuo contenuto oggetto dell'operazione. Se la tua origine non fornisce un'intestazione Cache-Control non vuota per il server edge, quest'ultimo non fornirà la sua intestazione Cache-Control al browser.

Quando il server edge invia un'intestazione Cache-Control al browser, il valore `max-age` per questa intestazione Cache-Control corrisponde al lasso di tempo residuo prima che il contenuto sul server edge venga ritenuto non aggiornato e richieda una riconvalida dall'origine. 

## Respect Header: Off
Quando Respect Header è impostato su **Off**, puoi controllare il tempo di memorizzazione in cache a livello del browser configurando le impostazioni TTL della tua CDN. Per ogni file o directory di file, devi creare una regola TTL per il relativo percorso e la durata di memorizzazione in cache desiderata.

Questa impostazione fa due cose:
  * La regola TTL indica al server edge di memorizzare in cache il contenuto per il lasso di tempo specificato.
  * Quando il contenuto è gestito dal server edge, quest'ultimo invia la sua intestazione Cache-Control con un valore `max-age`, basato sulla durata TTL, al browser.

## Respect Header: On
Quando **Respect Header** è impostato su **On**, puoi scegliere di non configurare una regola TTL per controllare il tempo di memorizzazione in cache a livello del browser per il tuo contenuto. In tal caso, devi configurare il tuo server di origine per fornire un'intestazione Cache-Control con un valore `max-age` per del determinato contenuto. Poiché Respect Header è On, il server edge utilizza il valore `max-age` di Cache-Control dall'origine come durata TTL per tale specifico contenuto. Se già hai una voce di regola TTL per la tua CDN che copre il contenuto, il server edge effettivamente usa ancora il valore `max-age` e sovrascrive qualsiasi durata di memorizzazione in cache a livello di edge esistente per tale contenuto.

Questa impostazione fa due cose:
  * Il valore della direttiva `max-age` di Cache-Control dall'origine indica al server edge di memorizzare in cache il contenuto per il lasso di tempo specificato.
  * Quando il contenuto viene gestito dal server edge, quest'ultimo invia la sua intestazione Cache-Control al browser con un valore `max-age` basato sul valore `max-age` dall'origine.

Nessun altro contenuto coperto da tale regola TTL è interessato se stai utilizzando il metodo mirato mostrato nell'esempio.

Quando **Respect Header** è impostato su **On**, la durata della memorizzazione in cache sul browser può essere controllata da una regola TTL, se il server di origine non invia la direttiva `max-age` nell'intestazione Cache-Control. Se specifica un valore `max-age`, tale valore può accidentalmente sovrascrivere il valore temporale nella regola TTL.

## Riepilogo

|Respect Header|L'origine fornisce Cache-Control|Durata della memorizzazione in cache del contenuto specifico sul server edge|Il server edge fornisce Cache-Control|
|---|---|---|---|
|On|Sì, specifica un `max-age`|Sovrascritto con il valore `max-age` dell'origine|Sì, `max-age` è il tempo prima che il contenuto dell'edge richieda la riconvalida dall'origine|
|On|Sì, non specifica un `max-age`|Basato sulla configurazione TTL della CDN|Sì, `max-age` è il tempo prima che il contenuto dell'edge richieda la riconvalida dall'origine|
|On|No|Basato sulla configurazione TTL della CDN|No|
|Off|Sì, specifica un `max-age`|Basato sulla configurazione TTL della CDN|Sì, `max-age` è il tempo prima che il contenuto dell'edge richieda la riconvalida dall'origine|
|Off|Sì, non specifica un `max-age`|Basato sulla configurazione TTL della CDN|Sì, `max-age` è il tempo prima che il contenuto dell'edge richieda la riconvalida dall'origine|
|Off|No|Basato sulla configurazione TTL della CDN|No|

## Ulteriori informazioni
* Come [gestire la tua CDN](https://console.bluemix.net/docs/infrastructure/CDN/how-to.html)
* Cache-Control come definito nella sezione 14.9 della [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt)
