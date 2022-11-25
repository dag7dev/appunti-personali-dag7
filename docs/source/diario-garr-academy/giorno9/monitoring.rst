.. _monitoring:
Ottenere dati.

TIG
===

Acronimo che si divide in tre strumenti principali: - **T**\ elegraf:
collettore - **I**\ nfluxDB: un db fatto apposta per storare dati
temporanei - **G**\ rafana uno strumento di visualizzazione dei dati.

La caratteristica di questi strumenti è che sono programmabili tramite
API, è possibile fare prove con Ansible.

Ingredienti del sistema di monitoring
=====================================

-  oggetto fisico o logico - che ha un sensore: un frigorifero che ha un
   termometro - la metrica è la temperatura

   -  collettore: parla col sensore attraverso un protocollo

      -  si interfaccia con un db
      -  possiamo poi elaborare i dati e visualizzarli come più ci
         piace.

Vedi: :ref:`tagvfield`