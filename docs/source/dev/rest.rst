.. _rest:

REST
====

Cosa è
------
REST è l'acronimo di Representational State Transfer ed è uno stile architetturale.

È un sistema di **transmissione dati basato interamente su HTTP**.

Funzionamento
-------------
**Struttura di un URL ben definita** che identifica una risorsa, e un insieme di metodi HTTP specifici (GET, POST...)

Principi
--------
Interfaccia uniforme!

Le modalità di interazione del sistema tra i componenti è regolata attraverso una serie di convenzioni uniformi.

1. **Hypermedia**: ogni risorsa deve fornire l'accesso ad altre risorse collegate

2. **Identificazione delle risorse**: definire un modo per individuare le risorse in un sistema.

3. **Manipolazione risorse tramite rappresentazione**: non vengono accedeute direttamente ma tramite rappresentazioni.

4. **Messaggi Autodescrittivi**

Spesso quando si progetta delle API, le si chiama "API Rest".

Ciò non è quasi mai vero: non definiamo quasi mai link ad altre risorse collegate, perciò è più corretto chiamarle "WebAPI" o semplicemente "API".

Risorse
-------
Le **risorse** sono **oggetti** o **rappresentazioni di qualcosa** che ha dei dati e un insieme di metodi per effettuare operazioni (tipicamente insert, update e delete).

Per **collections** si intende una insieme di risorse; ad esempio ``companies`` indica un insieme di ``company``.

**URL**: percorso col quale una risorsa viene specificata.

Per operare sulle risorse dobbiamo utilizzare le API.


Note generali
-------------
* Le **risorse devono sempre essere plurali** e se vogliamo accedere a solo una di loro dobbiamo poterlo fare tramite un ID.

Esempi:

  * ``GET     /companies``: lista di tutte le compagnie

  * ``DELETE  /companies/34``: elimina l'azienda 34

  * ``GET     /companies/34/employees/2``: fetch dell'impiegato numero 2 dell'azienda 34

* I metodi HTTP specificano il tipo di operazione andrà ad essere effettuata

  * 2xx: 200 ok, 201 created (post), 204 no content (delete)

  * 3xx: 304 not modified (cache)

  * 4xx: 400 bad req, 401 unauth, 403 forbidden, 404 not found, 410 gone

  * 5xx: 500 ISE, 503 service unavailable

* **Consistenze nel naming**

  * endpoint: con il _ tra una parola e l'altro

  * JSON: camelCase o snake_case

* Tipiche operazioni fatte con il GET

  * sorting
  
  * filtering
  
  * searching

* **Versioning**: parti da V1. Si va avanti se e solo se c'è un breaking change, ossia:
  
  * cambio di risposta significativo es. un post anziché un put, oppure un intero che diventa un float

  * si rimuove un pezzo di funzionalità

  * se aggiungi nuovi endpoint o funzionalità **non devi preoccuparti**!

Sicurezza nelle API
-------------------
1. Mettiti nei panni dell'attaccante, **pianificando mosse in anticipo**.

2. Non reinventare nulla ma **preferisci strumenti consolidati**

3. Usa **HTTPS**

4. **Autenticazione forte**

  * librerie crittografia

  * token invalidati dopo tot di tempo
  
  * gli utenti non dovrebbero accedere a determinate parti
  
  * credenziali MAI nell'url

5. **Ruoli-permessi**

6. **Rate limiting**: c'è meno possibilità di essere DDoSsato

7. **WAF**: firewall per l'applicazione

Vedi anche :ref:`api`