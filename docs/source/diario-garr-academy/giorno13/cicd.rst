.. _cicd:

CI - CD
=======

Continuous Integration, Continuous Deployment

È un insieme di pratiche che si basano su un concetto fondamentale:
**automazione** -> nello sviluppo e nel deploy

Non devo preoccuparmi di eseguire i test e vedere se è tutto ok, mi
basta committare e parte un processo di test automatico.

CI e CD sono dei concetti: la parte più pratica è implementata dalla
pipeline.

Pipeline
--------

È un insieme di istruzioni che vengono eseguite a seconda di eventi o
condizioni che andiamo a definire.

Vantaggi
--------

1. riduce processi manuali
2. rende i processi ripetibili
3. nessun intervento manuale
4. integrazione di modifiche e rilascio stabile di nuove funzionalità
   (grazie ai controlli automatici)

Infrastructure data code
------------------------

Ambienti che posso automatizzare attraverso del codice

Elementi della pipeline
-----------------------

1. **Pipeline**: implementazione della CI/CD
2. **Stage**: raggruppa piu job e crea piu pipeline - macrofasi
3. **Job**: sequenza di azioni
4. **Executor**: ambiente di esecuzione dei job (Docker)
5. **Runner**: infrastruttura che ospita lexecutor

Stage
~~~~~

Stage è il macroelemento della pipeline, definisce la struttura.

All'interno di uno stage sono definiti i **job** e sono **normalmente in
sequenza**: se uno stage fallisce tutta la pipeline fallisce.

Job
~~~

Unità elementare della pipeline. Definisce le operazioni come 'sequenza
di comandi che andiamo a definire nella direttiva "script". Vengono
definite anche le condizioni di esecuzione.

Executor
--------

Ambiente del runner in cui è fisicamente eseguita la pipeline Es.
Docker - k8s

Tipici runner: - singola VM con Docker engine - Cluster k8s

Esempio classico
----------------

1. **Compile**: compilazione, inizializzazione, installazione,
   dipendenze
2. **Test**: unità, integrazione, sistema
3. **Build**: packaging
4. **Deploy**: messa in staging o in produzione

Non eseguire la pipeline quando pusho
-------------------------------------

::

   git push -o ci.skip
