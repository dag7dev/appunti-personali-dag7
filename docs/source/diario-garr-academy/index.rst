--- Lista dei giorni ---
========================

Giorno 1
    Primo giorno di corso.

    Argomenti
        - presentazioni
        - **shell**: principali comandi shell Linux e semplici esempi da provare.
        - **introduzione alle reti**: modello TCP/IP, principali protocolli sui vari livelli, indirizzi IP, subnetting, NAT e DNS.
        - **introduzione a Python3**: costrutti base (if, for, while...), variabili, esercizi semplici introduttivi ed esercizio con le classi lasciato come compito per il giorno seguente.

    Cose interessanti
        - ``CTRL+D`` sostituisce exit
        - ``cp -p`` preserva i permessi
        - ``apt purge`` toglie tutto, ``apt remove`` solo i pacchetti
        - ``sudo -u``: esegue un comando dall'utente specificato
        - ``tail -f``: se un'altra applicazione sta scrivendo su quel file "rimane in ascolto" (stile live changing)
        - ``tail -n``: numero di righe in un file
        - ``bmon``: monitor di rete
        - nell'ethernet il CAT indica la categoria di cavo. Man mano che si avanza di numerazione, aumenta sia lo spessore del cavo che la quantita di dati trasmessi
        - routing: rip -> dv, ogni router comunica la propria tabella di routing ogni 30 secondi agli altri router
        - ospf: tutti mandano a tutti
        - bgp: router di frontiera


Giorno 2
    -  **Git**: introduzione a Git, creazione chiave SSH, git push, commit, add, merge, branch, checkout
    -  **Jinja**: :ref:`jinja`
    -  **Vagrant**: :ref:`vagrant`
    -  **reverse proxy**: proxy non in maniera attiva
    -  **Api REST**: cosa sono, metodi HTTP
    -  **JSON e YAML**: cosa sono, come funzionano
    -  JSON: supporta i tipi: string, number, boolean
        -  ``json.loads(obj, **)`` -> trasforma una stringa in un dizionario python
        -  ``json.dumps(obj, **)`` -> viceversa
        -  ``json.load(fp, **)`` -> carica da file
        -  ``json.dump(fp, **)`` -> salva in file
    -  esercizio con i JSON ed Harry Potter


    Cose interessanti
        -  nel layout italiano il backtick puo essere inserito, non devo cambiare layout di tastiera
        -  ``git commit --amend`` sovrascrive il commit che era pronto per essere pushato
        -  vagrant serve per parametrizzare le VM e Jinja serve per scrivere templating html


Giorno 3
    - :ref:`ansible`: principi base, comandi principali, playbook, esercizio con playbook (recipe)
        - Ansible advanced: cicli, loop, crittazione
    - :ref:`yaml`
    - File **hosts**
    - **Vagrant**: due host con Vagrantfile
        - ripassino sull'**SSH**


    Cose interessanti
    - nel **file hosts** possiamo anche usare un **alias**, basta definirlo in una **colonna separata**
    - se pinghi l'host con alias o seconda colonna, e host connesso, pinga
    - in Ansible, quando fai gli inventory sugli ini, puoi usare una forma contratta tipo: `stage[1:2].garr.academy`


Giorno 4
    - :ref:`ansible`: Continuazione su Ansible: roles, collections, e Galaxy



Giorno 5
    - **Devops, cosa sono?**: Prima si aveva la concezione che sistemisti e developers fossero indipendenti tra loro, invece adesso si tende ad accorpare le due cose.
    - **Monoliti vs Microservizi**: I primi sono piu difficili da manutenere ma semplici da sviluppare.
    -  :ref:`container`
    -  :ref:`docker`
    - **Esercizio con Docker**: pullare e pushare, installare applicazioni in un container


Giorno 6
    Corso di sicurezza sul lavoro.


Giorno 7
    È venuto un rappresentante della CGIL che ci ha spiegato diverse cose comode: TFR, dei corsi di formazione Formatemp, della riqualificazione degli impiegati in caso di disoccupazione.
    
Giorno 8
    Oggi abbiamo visto Kubernetes e un orchestratore.

    **Deploy**: tutta quella serie di attività che ci permettono di rendere
    la nostra applicazione disponibile con il pubblico

    Un **orchestratore** automatizza alcuni processi, come il deploy, la
    scalabilità, il networking ecc

    **Kubernetes** è un orchestratore di container, uno dei più famosi in
    ambito cloud.

    Argomenti
        -  :ref:`kuber`
        -  :ref:`minikube`
        -  :ref:`servacc`
        -  :ref:`rbac`
        -  :ref:`chaos`

Giorno 9
    -  :ref:`monitoring`



Giorno 10
    Oggi abbiamo visto i principi della buona comunicazione e public speaking.


Giorno 12
    **Log**: timestamp - severity (warn, danger, info) - unit: messaggio chi fa cosa

    **Measurement**: "Se non lo misuri, non esiste"

    Durante i test **si usa pytest**.

    Abbiamo parlato di https://refactoring.guru/ e dei design pattern e antipattern, e poi ci siamo divertiti un sacco a creare e commentare dei codici complicati da noi (gruppi).

    È stata una delle cose più divertenti fatte fino ad adesso.

Giorno 13
    -  :ref:`cicd`

    Ci siamo creati un certificato e ce lo siamo firmati da soli, e poi abbiamo fatto in modo che cognome.com
    usasse un certificato.

    .. image:: https://i.kym-cdn.com/entries/icons/original/000/030/329/cover1.jpg
        :alt: obama medals himself
