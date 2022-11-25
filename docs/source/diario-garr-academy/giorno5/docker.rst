.. _docker:

Docker
======
Tool che pacchettizza un microservizio. Se vuoi puoi ridistribuirlo e chi lo vuole se lo tira giù (c'è una repo condivisa).

**RUNNA SEMPRE QUESTO:**

.. code::

    newgrp docker


Installazione
-------------

Installazione
^^^^^^^^^^^^^
Modo semplice per installare Docker:

.. code::
    https://docs.docker.com/engine/install/ubuntu/#install-using-the-convenience-script


Task manager
------------

Task manager docker
^^^^^^^^^^^^^^^^^^^
Container in esecuzione al momento:

.. code::

    ps aux


History container passati e presenti:

.. code::
    
    docker ps -a


Rimozione fisica dalla history

.. code::

    docker ps -a <nome>


Container
---------

Container live
^^^^^^^^^^^^^^

.. code:: bash

    docker run -it ubuntu


Container: background
^^^^^^^^^^^^^^^^^^^^^

.. code:: bash

    docker run -it -d ubuntu


Entrare nei container docker fisicamente
----------------------------------------

.. code:: bash

    docker exec -it <nome> bash


Killare container - terminare container
---------------------------------------

.. code:: bash

    docker kill <nome>


Opzioni:

- `-d`: detachable, vai in background
- `-it`: ci va
- `--name <nome>`: nome custom container
- `-p 8080:80`: mapping porta mia:porta sua , aprire le porte di un container

Immagini
--------

Listare le immagini che ho - list images
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

    docker image list


Pullare un immagine (senza installare un container)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 

    docker pull <nome>


Es.

.. code:: 
    
    docker pull nginx


nginx demos / hello
-------------------
Si pulla l'immagine nginxdemos/hello

Si esegue:

.. code:: 
    
    docker run -d -p 8080:80 --name sito-web nginxdemos/hello


Parametro `-p` -> locale (mia) : la sua

Log
---

Log del sistema operativo
^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 
    
    docker logs <nome-container> 


Live log / log live / log in tempo reale
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code:: 

    docker logs <nome-container> 


C'è anche l'inspector, log noioso con cose utili:

.. code:: 
    
    docker inspect <nome-container>


Come costruire immagini
-----------------------
Basta usare un :ref:`dockerfile`

Install from dockerfile
-----------------------

.. code:: 

    docker build -t my-first-docker .
