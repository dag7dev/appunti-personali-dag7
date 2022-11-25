.. _dockerfile:

Dockerfile
==========

Un file di testo che racconta come l'immagine deve essere creata.

Esempio:

.. code::

    FROM debian:bullseye-slim
    RUN apt-get update && apt-get install -y apache2
    RUN date > /var/www/html/index.html
    CMD["apache2ctl", "-D", "FOREGROUND"]

RUN: esegue i comandi **durante** la creazione dell'immagine.

CMD: quando il container è avviato dai questo comando