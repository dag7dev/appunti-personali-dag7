.. _telegraf:
Telegraf
========

È scritto in Go.

Funzionamento
-------------

Fa due cose semplicissime: 1. Legge i dati attraverso un input plugin 2.
Scrive i dati attraverso un output plugin

Può essere usato in due modi: 1. Istanza di telegraf: che gira su una
macchina e legge scrive tutto su una macchina 2. Istanza di telegraf che
gira su un server e legge da più macchine.

Pipeline
--------

IPAO: input - process - aggregate - output

Plugin
------

Sono tutti qui:https://docs.influxdata.com/telegraf/v1.24/plugins/
